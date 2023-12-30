# 创建命名空间
```shell
kubectl create namespace clusterstorage
```

# nfs storageClass 部署

修改storageclass-nfs.yaml server path 改成自己nfs 服务器配置, 例如
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
kubectl apply .
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

