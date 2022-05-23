---
title: 为React项目本地服务设置HTTPS
date: "2020-04-01"
description: 为React项目本地服务设置 HTTPS 流程。
---
# 为React本地设置HTTPS
> 由于没有其他电脑，所以此教程在macOS中演示  
在项目package.json中添加HTTPS
```json
“script”: {
    “start”: “HTTPS=true react-scripts start”,
    “build”: “react-scripts build”,
    “test”: “react-scripts test”,
    “eject”: “react-scripts eject”,
},
```
在此步骤之后运行`yarn start`或者`npm start`将在浏览器中显示此屏幕：

![](/images/react-https/1-1.png)

在此阶段，已经使用了https. 但是没有有效的证书，因此连接被认为是不安全的。

## 创建 SSL 证书
获得证书的最简单方法就是通过 [mkcert](https://github.com/FiloSottile/mkcert) .
```bash
# 安装 mkcert 工具
brew install mkcert

# 设置 mkcert
mkcert -install
```

> 如果使用 Firefox 浏览器，需要单独安装 nss  `brew install nss`

完成以上操作后，就可以为 React 项目生成证书了。
```bash
# 在项目根目录下创建一个 .cert 文件夹
mkdir -p .cert

# 在项目根目录下生成证书
mkcert -key-file ./.cert/key.pem -cert-file ./.cert/cert.pem "localhost"
```
![](/images/react-https/2-1.png)

此时就可以看到根目录下创建好的新证书。

## 最后一步
接下来，回到 package.json文件中：
```json
“script”: {
    “start”: “HTTPS=true SSL_CRT_FILE=./.cert/cert.pem SSL_KEY_FILE=./.cert/key.pem react-scripts start”,
    “build”: “react-scripts build”,
    “test”: “react-scripts test”,
    “eject”: “react-scripts eject”,
},
```

再次使用`yarn start`或者`npm start` 运行时，应该就会看到“连接是安全的”。

![](/images/react-https/3-1.png)