# Jrebel License Server

自建 JRebel & JetBrains 激活服务器，兼容 JRebel 2023.4.0+。

## 快速开始

### 1. 启动服务

```bash
mvn clean package -DskipTests
java -jar target/jrebel-0.0.1-SNAPSHOT.jar
```

默认监听 `8080` 端口，可在 `application.yml` 中修改。

### 2. 获取激活地址

浏览器访问 `http://<服务器IP>:8080/guid` 获取一个随机 GUID，或者自己生成一个 UUID。

### 3. 配置 JRebel 插件

打开 IDEA → **Settings** → **JRebel & XRebel** → **JRebel** → **Activation**：

| 配置项 | 值 |
|---|---|
| Activate | **License Server** |
| License Server URL | `http://<服务器IP>:8080/{GUID}` |
| Email | 任意邮箱 |

格式示例：`http://192.168.1.100:8080/a1b4aea8-b031-4302-b602-670a990272cb`

### 4. 激活 JetBrains IDE（可选）

在 IDE 的 License Server 中填入：

```
http://<服务器IP>:8080/
```

---

## 有效期说明

| 产品 | 离线模式 | 说明 |
|---|---|---|
| JRebel | 180 天 | 到期后重新激活即可 |
| JetBrains | ~19 年 | `prolongationPeriod = 607875500` |

---

## 服务端点

### JRebel 激活

| 端点 | 说明 |
|---|---|
| `/jrebel/leases` | 主激活接口 |
| `/jrebel/leases/1` | 激活状态确认 |
| `/jrebel/validate-connection` | 连接验证 |
| `/agent/leases` | XRebel 兼容（同 leases） |
| `/agent/leases/1` | XRebel 兼容（同 leases/1） |

### JetBrains 激活

| 端点 | 说明 |
|---|---|
| `/rpc/obtainTicket.action` | 获取激活 Ticket |
| `/rpc/ping.action` | 心跳保活 |
| `/rpc/releaseTicket.action` | 释放 Ticket |

### 辅助

| 端点 | 说明 |
|---|---|
| `/guid` | 生成随机 GUID |
| `/` | 跳转使用指南 |

---

## Docker 部署

```bash
docker-compose up -d
```

默认映射 `8080` 端口，可通过环境变量 `licenseUrl` 自定义对外地址。
