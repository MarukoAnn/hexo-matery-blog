---
title: npm私有服务器搭建
top: false
cover: false
toc: true
mathjax: true
date: 2021-01-25
password: false
summary: false
tags:
    - npm
    - 服务器
categories:
    - npm服务器
---

### 一、搭建npm私有服务器
1、首先安装 Node,然后输入以下命令，安装 verdaccio，

```
    npm install -g verdaccio
```

2、如果在安装过程中报 grywarn 的权限错的话，那么需要加上 --unsafe-perm, 如下命令

```
    npm install -g verdaccio --unsafe-perm
```

3、在命令行中输入 verdaccio 命令回车, 如下图:

- (1)、config file 表示配置文件所在的位置
  
  ![](https://static01.imgkr.com/temp/a1fe5b2609de42f6a203a91992289b64.png)

- > http address 表示运行的地址

4、打开上图显示的 http://localhost:4873 地址，显示以下页面表示安装成功

![](https://imgkr2.cn-bj.ufileos.com/18ad683c-83b1-42e9-860a-f61661053a6e.png?UCloudPublicKey=TOKEN_8d8b72be-579a-4e83-bfd0-5f6ce1546f13&Signature=UrTxP2i%252BIh6mAf7uukjBocu5ATM%253D&Expires=1611726293)

5、找到以上第三步显示的配置文件地址，修改配置文件，末尾新增 listen: 0.0.0.0:4873 ，通过访问本机 ip 地址访问。

> 如下是新增的，默认是没有的，只能在本机访问，添加完成后就可以在外网访问了~ 
>       listen: 0.0.0.0:4873

6、下载 nrm 管理 npm 镜像地址。

- (1)、安装命令：npm install -g nrm
  想要了解更多有关的 nrm 命令，可以使用 nrm --help, 会列出所有的命令行的
- (2)、添加私有镜像，设置别名（此处地址为我本地搭建地址，此处可以修改为自己的服务器地址）

> nrm add  verdaccio（此名字可自定义）http://192.168.6.98:4873

- (3)、nrm 查看全部镜像

```
  nrm  ls
```

![](https://imgkr2.cn-bj.ufileos.com/f7fb7dd7-3d96-4d4d-bcf4-a3032b570dc7.png?UCloudPublicKey=TOKEN_8d8b72be-579a-4e83-bfd0-5f6ce1546f13&Signature=n3s%252BlN42GFr6dGIKdvR%252Bpz0cxA0%253D&Expires=1611726454)
此处最后一项是我添加的私有镜像。

- (4)、切换镜像
  nrm use  镜像名称(verdaccio)
  如果不想使用私有 npm 上下载包的话，可以使用 nrm use xxx 命令，切换镜像

7、npm 发布包  
要在私有 npm 仓库中发布包首先需要注册或者登陆账号。如果我们还没有账号的话，通过输入命令 npm adduser，然后依次输入用户名，密码即可创建完毕，如果已有账号，通过 npm login，依次输入用户名密码即可登陆。然后进入我们需要上传的代码目录，执行发布命令即可
> 如果出现以下错误日志可以先忽略
![](https://imgkr2.cn-bj.ufileos.com/1e8be315-df95-4207-bd38-1185955cf993.png?UCloudPublicKey=TOKEN_8d8b72be-579a-4e83-bfd0-5f6ce1546f13&Signature=cbfzcQNQnwY7pWwqAyY%252FUVMND5o%253D&Expires=1611726576)

输入用户名、 密码、邮箱  ，出现登录再某某地址表示登录成功
![](https://imgkr2.cn-bj.ufileos.com/555471a5-5506-4415-a5e3-ea359d4f4823.png?UCloudPublicKey=TOKEN_8d8b72be-579a-4e83-bfd0-5f6ce1546f13&Signature=C6IXiwMnd44tuRs94jON93WxpU4%253D&Expires=1611726595)
8、进入需要组件库项目中，输入以下命令 上传组件库到私有服务器中
```
 npm publish --registry http://192.168.6.98:4873
```

![](https://imgkr2.cn-bj.ufileos.com/c517471c-e14b-4419-a333-66292b23e442.png?UCloudPublicKey=TOKEN_8d8b72be-579a-4e83-bfd0-5f6ce1546f13&Signature=lDOe1kbLBpTSEkZywEOYWzPoJCc%253D&Expires=1611726954)

![](https://static01.imgkr.com/temp/69b55129fb9c4bc3beb80a81807bc4d5.png)
上传成功后 ，进入刚才启动的cerdaccio服务，刷新页面。看到以下组件库名称，表示上传成功。

![](https://static01.imgkr.com/temp/5d263d0dbf1b41fca110e3e9dce1787c.png)
这样私有组件库就算搭建成功了。

### 二、私有服务器的组件下载使用

1、首先根据上面的nrm use 命令 切换npm 为私有组件库源镜像。

2、 创建一个vue 项目
```
   使用 npm install -s test_ui  
```
![](https://static01.imgkr.com/temp/2e35d790c65b452e9f58dbdc2af1dadc.png)
> 注意：此处的 test_ui 为上面上传到私有组件库服务的组件库名称

3、在main.js里面导入使用组件库

![](https://imgkr2.cn-bj.ufileos.com/2c81eaa1-9604-470a-b0c7-ec630e033fac.png?UCloudPublicKey=TOKEN_8d8b72be-579a-4e83-bfd0-5f6ce1546f13&Signature=DmyNjO7zkbz0gZUen4mOxWx%252BVsQ%253D&Expires=1611727080)

4、成功后 在组件中引用message组件

![](https://static01.imgkr.com/temp/cbcf0191f01e4ba088c3ea0cafe28f68.png)

5、npm run start 启动项目，出现以下字样表示引用成功。

![](https://static01.imgkr.com/temp/2558b4a84ba146eebfe82a5d87202dee.png)

这样组件库使用成功咯，是不是非常的简单~~

>本文适用于部分中小型公司搭建自己前端的生态体系的私有服务器，可以自己搭建，也可以直接去npm上购买私服，此方法只是其中一种，还有很多方式可以搭建私有npm组件库，仅供大家参考，有疑问随时与我联系，大家一起学习一起成长。
