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

