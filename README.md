# 【黑马程序员】前端开发之电商管理后台



## vue_shop_server-master



1. 安装 MySQL 数据库
2. 安装 Node.js 环境
3. 配置项目相关信息
4. 启动项目
5. 使用 Postman 测试后台项目接口是否正常



环境搭好后，将 db 文件夹下 sql 文件导入数据库。同时修改 config 文件夹下的 default.json 配置文件，将其中 `db_config` 对象属性修改为自己本地数据库的连接信息。 

进入项目根目录，进入 cmd，先运行 npm install 安装依赖包，再用 node 命令执行 app.js，从而 api 接口项目启动起来。





## vue_shop_server



项目上线相关配置！！

### 项目上线相关配置



1. 通过 node 创建 web 服务器。

2. 开启 gzip 配置。

3. 配置 https 服务。

4. 使用 pm2 管理应用。

   

#### 1. 通过 node 创建 web 服务器



创建 node 项目，并安装 express，通过 express 快速创建 web 服务器，将 vue 打包生成的 dist 文件夹，托管为静态资源即可，关键代码如下：

```javascript
// 初始化一个包管理配置
npm init -y
// 安装相应包
npm i express -S
// 复制 生成的 dist 文件夹到 node 项目下
// 写入 app.js:
const express = require('express')
// 创建 web 服务器
var app = express()

// 托管静态资源
app.use(express.static('./dist'))

// 启动 web 服务器
app.listen(80, () => {
    console.log('web server running at http://127.0.0.1')
})

```



#### 2. 开启 gzip 配置



使用 <font color="red"> gzip </font> 可以减小文件体积，使传输速度更快。

可以通过服务器端使用 Express 做 gzip 压缩。其配置如下：

```javascript
// 安装相应包
npm install compression -D
// 导入包
const compression = require('compression')
// 启用中间件 (一定要把这一行代码，写到静态资源托管之前)
app.use(compression());
```



#### 3. 配置 HTTPS 服务



<font color="red"> 为什么要启用 HTTPS 服务? </font>

* 传统的 HTTP 协议传输的数据都是明文，不安全
* 采用 HTTPS 协议对传输的数据进行了加密处理，可以防止数据被中间人窃取，使用更安全



<font color="red"> 申请 SSL 证书(https://freessl.org) </font>

① 进入 https://freessl.cn/ 官网，输入要申请的域名并选择品牌。

② 输入自己的邮箱并选择相关选项。

③ 验证 DNS (在域名管理后台添加 TXT 记录)。

④ 验证通过之后，下载 SSL 证书 (full_chain.pem 公钥；private.key 私钥)。



在后台项目中导入证书

```javascript
const https=require('https');
const fs = require('fs');
const options = {
    cert: fs.readFileSync('./full_chain.pem'),
    key:fs.readFileSync('./private.key')
}
https.createServer(options,app).listen(443);
```



#### 4. 使用 pm2 管理应用



① 在服务器安装 pm2：<font color="red"> npm i pm2 -g </font>

② 启动项目：pm2 start 脚本 --name 自定义名称

③ 查看运行项目：pm2 ls

④ 重启项目：pm2 restart 自定义名称

⑤ 停止项目：pm2 stop 自定义名称

⑥ 删除项目：pm2 delete 自定义名称