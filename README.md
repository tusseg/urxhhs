# VLESS Heroku

## 概述

用于在 Heroku 上部署 vless+websocket+tls，每次部署自动选择最新的 alpine linux 和 xray core。  
vless 相比 vmess 性能更加优秀，占用资源更少，运行更加稳定。
以及xray支持UDP协议的FullCone，个人使用更推荐使用xray-core以便获取更好的体验。

## 感谢
感谢大佬GeekNAUer/vlessheroku做的项目，应为fork了很多其他的项目，太多重名的，所以我只是改个名字，方便自己使用。
https://github.com/GeekNAUer/vlessheroku

## 镜像

经测试本镜像占用内存资源较低，运行稳定。

[![Deploy](https://www.herokucdn.com/deploy/button.png)](https://dashboard.heroku.com/new?template=https://github.com/tusseg/urxhhs)

## 注意

### 路径

`WebSocket` 路径(配置文件中的 `path` )为 `/app` 。

### 端口

`端口` 为 `443` 。

### alterId

`alterId` 为 `0` 。 // xray，vless协议不需要使用此参数了。

### UUID

`UUID` 默认为 `3a53a3e5-da83-48d2-aee9-d88a498eb3dd` 可自行设置。

## 流量中转

可以使用cloudflare的workers来`中转流量`，注册两个账号就可以避免流量用太高的问题，配置为：  

const SingleDay = 'app1.herokuapp.com'
const DoubleDay = 'app2.herokuapp.com'
addEventListener(
    "fetch",event => {
    
        let nd = new Date();
        if (nd.getDate()%2) {
            host = SingleDay
        } else {
            host = DoubleDay
        }
        
        let url=new URL(event.request.url);
        url.hostname=host;
        let request=new Request(url,event.request);
        event. respondWith(
            fetch(request)
        )
    }
)
