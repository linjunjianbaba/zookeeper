建立持久卷（PersistentVolume，简称PV）
```
kubectl apply -f https://raw.githubusercontent.com/linjunjianbaba/zookeeper/master/zookeeper_pv.yaml
```
查看pv
```
kubectl get pv -o wide
```
部署statefulset zookeeper
```
kubectl apply -f https://raw.githubusercontent.com/linjunjianbaba/zookeeper/master/zookeeper.yaml
```
查看pod部署在哪一台宿主机
```
kubectl get pod -w -o wide
```
授目录权限(在宿主机上运行)
```
chown -R 1000:1000 /data/zookeeper
```
三个实例跑起来查看集群是否正常
```
for i in 0 1 2; do kubectl exec zk-$i zkServer.sh status; done
for i in 0 1 2; do echo "myid zk-$i";kubectl exec zk-$i -- cat /var/lib/zookeeper/data/myid; done
```
Ensemble 健康检查
最基本的健康检查是向一个 ZooKeeper 服务写入一些数据，然后从另一个服务读取这些数据。
使用 zkCli.sh 脚本在 zk-0 Pod 上写入 world 到路径 /hello。
```
kubectl exec zk-0 zkCli.sh create /hello world
```
这将会把 world 写入 ensemble 的 /hello 路径。
```
WATCHER::

WatchedEvent state:SyncConnected type:None path:null
Created /hello
```
从 zk-1 Pod 获取数据。
```
kubectl exec zk-1 zkCli.sh get /hello
```
你在 zk-0 创建的数据在 ensemble 中所有的服务上都是可用的。
```
WATCHER::

WatchedEvent state:SyncConnected type:None path:null
world
cZxid = 0x100000002
ctime = Thu Dec 08 15:13:30 UTC 2016
mZxid = 0x100000002
mtime = Thu Dec 08 15:13:30 UTC 2016
pZxid = 0x100000002
cversion = 0
dataVersion = 0
aclVersion = 0
ephemeralOwner = 0x0
dataLength = 5
numChildren = 0
```
