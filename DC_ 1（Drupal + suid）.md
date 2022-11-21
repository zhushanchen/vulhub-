# 端口发现
nmap -sP 192.168.31.0/24
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668248466506-282a4ae9-69c9-474d-b7cb-60dbfdfb4ba0.png#averageHue=%23262c38&clientId=u30a9a2b6-c095-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=338&id=u73748224&margin=%5Bobject%20Object%5D&name=image.png&originHeight=338&originWidth=1234&originalType=binary&ratio=1&rotation=0&showTitle=false&size=105121&status=done&style=none&taskId=u9c7fd3b4-bd0d-4bab-bb40-5cb08725bf4&title=&width=1234)
nmap -p- 192.168.31.142 或  namp -A 192.168.31.142
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668248489823-57a4e84c-ee90-4eef-ad28-b61b5f82c569.png#averageHue=%23262d39&clientId=u30a9a2b6-c095-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=526&id=ub6df12aa&margin=%5Bobject%20Object%5D&name=image.png&originHeight=526&originWidth=1237&originalType=binary&ratio=1&rotation=0&showTitle=false&size=161614&status=done&style=none&taskId=ud1367204-de1f-4949-8051-120bfe732b8&title=&width=1237)
# web
访问主页 80 端口
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668248626092-40dcee2e-5aca-4f86-940d-9b0fce7290a9.png#averageHue=%23eeebe4&clientId=u30a9a2b6-c095-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=638&id=u0b7b1d37&margin=%5Bobject%20Object%5D&name=image.png&originHeight=638&originWidth=1698&originalType=binary&ratio=1&rotation=0&showTitle=false&size=83433&status=done&style=none&taskId=u257c11a1-7b21-441e-ac2c-59744e47ca2&title=&width=1698)
看到主页已经知道 cms 是 Drupal
# msf
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668248808363-d3ef0e49-7013-4ee8-98df-9aceede24316.png#averageHue=%232f3440&clientId=u30a9a2b6-c095-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=413&id=u32e950a4&margin=%5Bobject%20Object%5D&name=image.png&originHeight=413&originWidth=1298&originalType=binary&ratio=1&rotation=0&showTitle=false&size=131056&status=done&style=none&taskId=u731f82e5-9a63-4c93-97c1-4db58d860d9&title=&width=1298)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668248820016-ca0a4e80-ac8f-4e3a-92af-e5f92cf6a6f6.png#averageHue=%232e313b&clientId=u30a9a2b6-c095-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=237&id=uebbe51cb&margin=%5Bobject%20Object%5D&name=image.png&originHeight=237&originWidth=1195&originalType=binary&ratio=1&rotation=0&showTitle=false&size=55959&status=done&style=none&taskId=u4b4b66d5-4558-4700-bcdc-6e0d4a4fe68&title=&width=1195)
键入 shell 来获取 shell
whoami ，经典 www-data
python 生成稳定的 shell
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668248937825-a60b2e86-e7d6-4f6e-adbe-bc594527527e.png#averageHue=%232e313b&clientId=u30a9a2b6-c095-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=128&id=ufab31c3f&margin=%5Bobject%20Object%5D&name=image.png&originHeight=128&originWidth=737&originalType=binary&ratio=1&rotation=0&showTitle=false&size=15514&status=done&style=none&taskId=ue9130396-4602-47bf-9ba4-f375fbf0bf0&title=&width=737)

# 提权
find / -perm -u=s -type f 2>/dev/null  或 find / -user root -perm -4000 -print 2>/dev/null 
可以看到 find 有 suid 权限，find 提权，cron.php 随便写，只要能搜到就行（os 里有）
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668249227019-e6039311-3e0e-4191-bab9-f42fb3b344ae.png#averageHue=%232d303a&clientId=ua00935a9-3144-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=475&id=u5764cac4&margin=%5Bobject%20Object%5D&name=image.png&originHeight=475&originWidth=1220&originalType=binary&ratio=1&rotation=0&showTitle=false&size=82774&status=done&style=none&taskId=uc13fb141-9a58-4499-b36d-59163361523&title=&width=1220)
拿到 root ，找 flag
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668249522505-987a146d-78ed-4bbf-9e9c-63519f47420c.png#averageHue=%232e323c&clientId=ua00935a9-3144-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=198&id=ub2ac9417&margin=%5Bobject%20Object%5D&name=image.png&originHeight=198&originWidth=764&originalType=binary&ratio=1&rotation=0&showTitle=false&size=32717&status=done&style=none&taskId=u054df72d-7c47-49c4-bca8-067594c7512&title=&width=764)
意思是没完，
drupal 使用 CVE-2014-3704 （34992.py）可以添加管理员账号
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668250031204-f447f387-aa20-4d80-a473-2c2f47c6ea87.png#averageHue=%232f323c&clientId=ua00935a9-3144-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=69&id=u367eefa7&margin=%5Bobject%20Object%5D&name=image.png&originHeight=69&originWidth=740&originalType=binary&ratio=1&rotation=0&showTitle=false&size=19634&status=done&style=none&taskId=u86d0f5fa-d17a-488b-bdc1-63e5f361c36&title=&width=740)
运行完毕后会给一个 url
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668250052670-1cde72b6-9d7b-4b79-9376-d41e2ae22ab2.png#averageHue=%232e313a&clientId=ua00935a9-3144-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=173&id=ufd083302&margin=%5Bobject%20Object%5D&name=image.png&originHeight=173&originWidth=1010&originalType=binary&ratio=1&rotation=0&showTitle=false&size=18774&status=done&style=none&taskId=u99c04197-0e3b-4923-a9a0-f892e6a8df8&title=&width=1010)
用自己设置的用户名和密码登录给定的 url
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668250087487-a8d2e69b-35d6-4b90-9103-b138ec3b13a2.png#averageHue=%23eae4da&clientId=ua00935a9-3144-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=571&id=u206682fc&margin=%5Bobject%20Object%5D&name=image.png&originHeight=571&originWidth=1701&originalType=binary&ratio=1&rotation=0&showTitle=false&size=74700&status=done&style=none&taskId=ucea9137f-432f-4f47-8fea-c07f2f79368&title=&width=1701)
获得 Drupal 的管理员账号
左上角的 find content 可以找出 flag3
![image.png](https://cdn.nlark.com/yuque/0/2022/png/23194752/1668250230668-d4c61142-c172-457b-9b8f-910c4c9a5493.png#averageHue=%23f3f0e8&clientId=ua00935a9-3144-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=519&id=ue6661b1b&margin=%5Bobject%20Object%5D&name=image.png&originHeight=519&originWidth=1698&originalType=binary&ratio=1&rotation=0&showTitle=false&size=46591&status=done&style=none&taskId=u59595f31-c9f3-4198-9a2c-f5c46352650&title=&width=1698)








