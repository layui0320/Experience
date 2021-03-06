# 安装
官网下载解压
# 命令
## agent 启动agent
```sh
consul agent [参数]
```
参数|意义
-|-
-dev|开发模式
-ui|启动web UI
-coinfig-dir=目录|指定配置目录

* 配置目录下的每个json文件定义一个服务
```json
{
  "service": {
    "name": "名称",
    "tags": ["标签",...],
    "port": 端口
  }
}
```
## members 查看集群成员
```sh
consul members [参数]
```
参数|意义
-|-
-detailed|详细信息

members采用gossip协议，保证最终一致性；若要求强一致性，使用HTTP API
```sh
curl localhost:8500/v1/catalog/nodes
```
# DNS查询
```sh
dig @127.0.0.1 -p 8600 [标签.]名称.service.consul SRV
```
* 标签给定时，仅返回tags包含它的项
# HTTP查询
```sh
curl http://localhost:8500/v1/catalog/service/名称[?passing]
```
* passing表示只返回healthy节点的结果，这也是DNS查询的行为
# 更新
向consul进程发送SIGHUP
# 停止
## 优雅停止
通知其他节点此节点left，并退出catalog
## 强行停止
其他节点检测到此节点failed，但catalog保留其信息，health标记为critical，consul自动尝试重连
