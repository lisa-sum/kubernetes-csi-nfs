# Kubernetes分布式存储

## 前置
1. [在Linux部署NFS的服务](https://juejin.cn/post/7317260317620240447)
2. 至少2台机器

## 创建命名空间
```shell
kubectl create namespace clusterstorage
```

## nfs storageClass 部署

修改`storageclass-nfs.yaml`文件的`server`和`path`改成自己`nfs服务器`配置, 例如:
```yaml
...
parameters:
  # 填写为/etc/exports的IP
  server: 192.168.2.152
  # 填写为/etc/exports的挂载地址
  share: /mnt/data/
...
```

启动
```shell
kubectl apply -f .
```

查看
```shell
kubectl -n clusterstorage get pod -owide
```

示例的PV:
```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: data-consul-consul-server-0  # PV的名称
spec:
  capacity:
    storage: 10Gi  # 存储容量
  volumeMode: Filesystem
  accessModes:
    - ReadWriteOnce
  persistentVolumeReclaimPolicy: Retain  # 回收策略
  storageClassName: nfs-csi  # 存储类名称
```

# 修改csi-nfs-controller.yaml csi-nfs-node.yaml  pods-mount-dir registration-dir socket-dir kubelet-registration-path 等参数 修改路径对应kubelet volume-plugin-dir 参数
# 部署nfs storageClass kubectl apply -f .
# 测试 能自己创建pvc 并成功写入文件 kubectl apply -f test/.
# service nfslock start
# systemctl enable nfslock.service

