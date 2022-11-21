# 端口发现
nmap -sP 192.168.31.0/24
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668314237812-41a5a2ae-2608-4563-ac24-61a61e877966.png#averageHue=%2329323e&clientId=u42955d50-d92b-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=337&id=ubeac31e8&margin=%5Bobject%20Object%5D&name=image.png&originHeight=337&originWidth=1084&originalType=binary&ratio=1&rotation=0&showTitle=false&size=98608&status=done&style=none&taskId=uc68a3676-3d9e-44c7-a490-9496a3fb137&title=&width=1084)
nmap -p- -A 192.168.31.147
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668314299736-dce00a7d-b795-41ad-a8bd-bccdb52d6397.png#averageHue=%2328303c&clientId=u42955d50-d92b-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=462&id=u72d18271&margin=%5Bobject%20Object%5D&name=image.png&originHeight=462&originWidth=1254&originalType=binary&ratio=1&rotation=0&showTitle=false&size=127782&status=done&style=none&taskId=u610bdb2e-acbf-4b59-88db-7838631b47b&title=&width=1254)
# web 突破
hosts 文件里加 dns 解析
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668314361905-3945eed8-606c-42cb-91bf-4007b6c94e35.png#averageHue=%232a3543&clientId=u42955d50-d92b-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=146&id=uabd42b11&margin=%5Bobject%20Object%5D&name=image.png&originHeight=146&originWidth=568&originalType=binary&ratio=1&rotation=0&showTitle=false&size=27011&status=done&style=none&taskId=ue8f8bdae-f836-415c-b5ec-32de8c4ffdb&title=&width=568)
访问主页 80 端口
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668315212756-0a73366b-9465-48c4-9472-3e674d2b23ec.png#averageHue=%23a9a192&clientId=u42955d50-d92b-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=970&id=u2ca400a1&margin=%5Bobject%20Object%5D&name=image.png&originHeight=970&originWidth=1919&originalType=binary&ratio=1&rotation=0&showTitle=false&size=879045&status=done&style=none&taskId=uca443faa-2e3d-4e04-860a-e15530cf32f&title=&width=1919)
经典 wordpress
使用 wpscan 来爆破用户名
wpscan --url http://wordy --enumerate u
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668315397245-ce84c23f-521a-4afa-a773-b38180380161.png#averageHue=%232d303a&clientId=u42955d50-d92b-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=434&id=u2deaf831&margin=%5Bobject%20Object%5D&name=image.png&originHeight=434&originWidth=1312&originalType=binary&ratio=1&rotation=0&showTitle=false&size=180298&status=done&style=none&taskId=u4c319d4a-169f-43a4-a17e-1c2140cfe94&title=&width=1312)
爆了好多用户名 admin、graham、mark、sarah、jens
wordpress 一般后台都有登陆页面 wp-admin
dirb 扫一下
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668315515430-1485096a-8bfb-47a9-99bb-90b2f23c3110.png#averageHue=%232e323c&clientId=u42955d50-d92b-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=444&id=udcb654c0&margin=%5Bobject%20Object%5D&name=image.png&originHeight=444&originWidth=1083&originalType=binary&ratio=1&rotation=0&showTitle=false&size=202833&status=done&style=none&taskId=uddc957cd-7694-45a4-9931-db8c2c451fe&title=&width=1083)
访问 wp-admin
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668315585900-1c28cbf8-5510-4f21-81f7-dacc8a2211aa.png#averageHue=%23d1d1d1&clientId=u42955d50-d92b-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=695&id=ud626203f&margin=%5Bobject%20Object%5D&name=image.png&originHeight=695&originWidth=1580&originalType=binary&ratio=1&rotation=0&showTitle=false&size=71682&status=done&style=none&taskId=u8b15b7d1-5be0-431c-ab3b-0cc8a7bb3d6&title=&width=1580)
直接爆破，将上面的用户名放一个字典，密码字典用 hydra 的就可以
wpscan 爆破 
wp-scan --ignore-main-redirect --url http://wordy -U 2.txt -P 1.txt --force
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668316027876-ec11d14b-f7b9-4cb1-bc0e-7b8bafbc9259.png#averageHue=%23282c38&clientId=u42955d50-d92b-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=66&id=ua36c5d48&margin=%5Bobject%20Object%5D&name=image.png&originHeight=66&originWidth=419&originalType=binary&ratio=1&rotation=0&showTitle=false&size=6555&status=done&style=none&taskId=u394e0c6d-eccd-4d8e-80bd-5786ced217b&title=&width=419)
mark:helpdesk01
登录 wordpress
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668316124656-fa0a7111-0693-4f65-802c-83b2bdd587d3.png#averageHue=%23e0e0df&clientId=u42955d50-d92b-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=796&id=u6310babf&margin=%5Bobject%20Object%5D&name=image.png&originHeight=796&originWidth=1685&originalType=binary&ratio=1&rotation=0&showTitle=false&size=162836&status=done&style=none&taskId=ub3cdfe58-3978-449b-a83e-0ea86e08a1a&title=&width=1685)
这次没有地方能看到页面内容，无法修改页面内容拿 shell
有个插件 Activity monitor 
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668316280043-ff3f9ebe-1153-41d0-a4f2-16e3d5474ef9.png#averageHue=%2330333d&clientId=u42955d50-d92b-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=284&id=u067bc476&margin=%5Bobject%20Object%5D&name=image.png&originHeight=284&originWidth=1255&originalType=binary&ratio=1&rotation=0&showTitle=false&size=87803&status=done&style=none&taskId=u2fdec7a6-120e-4d10-b829-7ab894c0ff8&title=&width=1255)
用 python 执行 50110.py（复制出来一份）
生成的这个shell 不太稳定，nc 反弹到 kali 一个shell
kali 监听 端口，python 优化 shell
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668317622143-2de9e04a-f531-43df-8712-d0e1a8832521.png#averageHue=%232c2f39&clientId=uda852f2e-d07c-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=226&id=u2a024267&margin=%5Bobject%20Object%5D&name=image.png&originHeight=226&originWidth=839&originalType=binary&ratio=1&rotation=0&showTitle=false&size=53373&status=done&style=none&taskId=ued1bb26f-56fc-4821-8d7e-1a437f4616d&title=&width=839)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668317550699-f2b460da-5b4d-4dab-9d35-148fa2b2a9f2.png#averageHue=%232e323d&clientId=uda852f2e-d07c-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=181&id=u8350e668&margin=%5Bobject%20Object%5D&name=image.png&originHeight=181&originWidth=705&originalType=binary&ratio=1&rotation=0&showTitle=false&size=53665&status=done&style=none&taskId=u96683fdd-636c-4204-86cd-8d254ad42b6&title=&width=705)
在 /home/mark/stuff 里有个 things-to-do.txt 文件，里面有 graham 的密码
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668317721056-122b1657-91a3-43ee-b54b-80cf02c9af07.png#averageHue=%232b2f3a&clientId=uda852f2e-d07c-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=196&id=u8c7ece51&margin=%5Bobject%20Object%5D&name=image.png&originHeight=196&originWidth=765&originalType=binary&ratio=1&rotation=0&showTitle=false&size=53437&status=done&style=none&taskId=ue05f98f0-4a11-4d33-814a-31b364ca49e&title=&width=765)
ssh graham@192.168.31.147
 查看 graham 的权限
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668317829511-31ff2b75-ffa4-4d2d-a355-b44628510e7b.png#averageHue=%232d313c&clientId=uda852f2e-d07c-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=411&id=u768885ce&margin=%5Bobject%20Object%5D&name=image.png&originHeight=411&originWidth=1152&originalType=binary&ratio=1&rotation=0&showTitle=false&size=132744&status=done&style=none&taskId=u56dbd52f-1c01-4838-a621-56926f2cf54&title=&width=1152)
graham 可以使用 jens 目录下的 backups.sh
在 backups.sh 中写入 /bin/bash
以 jens 的身份 运行 backups.sh
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668318097895-21dff512-983c-48c8-ad97-64152fe322f3.png#averageHue=%232a2e39&clientId=uda852f2e-d07c-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=259&id=ube5379f3&margin=%5Bobject%20Object%5D&name=image.png&originHeight=259&originWidth=838&originalType=binary&ratio=1&rotation=0&showTitle=false&size=72464&status=done&style=none&taskId=u3d9114b3-29ef-4e7d-b171-bed9bfebff2&title=&width=838)
切换到 jens 用户，sudo -l 查看 jens 的权限
可以免密使用 root 的 nmap 命令
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668318171262-5948b6e2-a288-4b35-919d-480f9ef04bde.png#averageHue=%232c2f3a&clientId=uda852f2e-d07c-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=139&id=u4df8c392&margin=%5Bobject%20Object%5D&name=image.png&originHeight=139&originWidth=1028&originalType=binary&ratio=1&rotation=0&showTitle=false&size=41064&status=done&style=none&taskId=u99d79831-6f95-4ffd-a167-a70138adc3a&title=&width=1028)
# 提权
写入脚本命名位 getShell，使用 nmap 调用 getShell 
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668318311782-4facea93-e1cf-45c8-8292-98627a4b3d34.png#averageHue=%232c313c&clientId=u0854bd5f-9435-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=124&id=u62182f63&margin=%5Bobject%20Object%5D&name=image.png&originHeight=124&originWidth=717&originalType=binary&ratio=1&rotation=0&showTitle=false&size=42298&status=done&style=none&taskId=u94834d47-97c5-42f1-92ae-e0bf7f2d372&title=&width=717)
报错建议说后缀是 .nse
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668318745167-76c784e9-e108-4a51-b3ae-29f94f07feb1.png#averageHue=%2327323f&clientId=u0854bd5f-9435-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=140&id=uab735e7c&margin=%5Bobject%20Object%5D&name=image.png&originHeight=140&originWidth=694&originalType=binary&ratio=1&rotation=0&showTitle=false&size=30723&status=done&style=none&taskId=u6ac398e9-c1ff-490c-b8bd-cdea29d10bc&title=&width=694)
拿 flag
这个 shell 不显示输入的命令
cd /root
ls
cat theflag.txt
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668318821433-5b87ef10-c7b5-4268-a35b-3a73dfe0c140.png#averageHue=%2327313d&clientId=ucef6bbab-ce10-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=394&id=ufa4d224c&margin=%5Bobject%20Object%5D&name=image.png&originHeight=394&originWidth=868&originalType=binary&ratio=1&rotation=0&showTitle=false&size=63503&status=done&style=none&taskId=ub70731c3-8b15-4deb-a65c-728c21bc6b5&title=&width=868)
