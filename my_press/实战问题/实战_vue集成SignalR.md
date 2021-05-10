---
title: vue集成signalR配置
category: 实战
tag:
  - signalR
comment: false
---
# 安装 signalr 及 messagePack优化包
```
    npm install @microsoft/signalr @microsoft/signalr-protocol-msgpack --save
```

# 配置
```javascript
import * as SignalR from '@microsoft/signalr'
import 'msgpack5'
import * as SignalProtocol from '@microsoft/signalr-protocol-msgpack'

const hub = new SignalR.HubConnectionBuilder().withUrl(`http://10.160.2.250:8001/chat?accessToken=${token}`, {
    accessTokenFactory: () => token
}).withHubProtocol(new SignalProtocol.MessagePackHubProtocol()) // 使用messagePack优化
this.connection = hub.build()
console.log(this.connection)
this.connection.on('event', (...args) => {
    console.log(args)
})
this.connection.start().catch((err) => console.log(err))
```

