# Kubernetes

[之前本人较为完善的k8s部署, 不过版本已经过时](https://github.com/shanwenhao1/k8s-learning): 后续准备全面拥抱helm chart进行实践

[本人最新版本安装教程](Lastest/README.md)
- **`声明`本示例`K8s集群`使用虚拟机进行搭建.**
- [Rancher](Rancher/README.md)

[官方中文文档](https://kubernetes.io/zh/docs/home/)

## [创建集群前准备](prepare/README.md)

## [kubernetes集群搭建](kubernetes-build/README.md)
- [Helm Chart使用](Helm/README.md)

## [kubernetes必要插件管理](kubernetes-plugin/README.md)

## [kubernetes相关知识](kubernetes-knowledge/README.md)

## 其他资料
- [dokcer-k8s-elk一站式](https://www.qikqiak.com/k8s-book/)
- [kubernetes指南](https://feisky.gitbooks.io/kubernetes/)


## 常用命令

```bash
# --------- 节点管理 ---------
# 查看所有节点信息
kubectl get nodes -o wide
# 查看节点详细信息(节点相关的信息可对照: https://kubernetes.io/zh/docs/concepts/architecture/nodes/#condition)
kubectl describe node k8s-node-1(node name)
# ---------  ---------


# 查看kubelet日志
journalctl -xeu kubelet
# 查看集群node数量
kubectl get nodes
# 获取node详细信息
kubectl describe node <node_name>
# 查看所有pod运行状态
kubectl get pods --all-namespaces
kubectl get pod --all-namespaces -o wide
# 获取pod的ip及暴露的端口信息
kubectl get ep -n kube-system
# 获取pod详细信息
kubectl describe pod your_pod_name -n your_namespace
kubectl describe pod coredns-fb8b8dccf-ngsh5 -n kube-system
# 获取pod日志
kubectl logs podName -n kube-system
# 删除pod
kubectl delete pod pod_name
# 创建token(默认过期时间是24h)
kubeadm token generate
kubeadm token create

# 删除某个节点, --ignore-daemonsets为master节点使用
kubectl drain ubuntu-node-1 --delete-local-data --ignore-daemonsets
kubectl delete node ubuntu-node-1

# 查看service信息
kubectl get svc kubernetes -o yaml

# 删除所有已退出的docker容器    谨慎使用
docker rm `docker ps -a|grep Exited|awk '{print $1}'`
# 配置外网访问
kubectl proxy --address='0.0.0.0'  --accept-hosts='^*$'
# 强制删除pod
kubectl delete pod --grace-period=0 --force node-exporter-srzkk -n monitoring
# 启动一个容器用以测试与其他容器的一些操作
kubectl run  -it --rm  cirror-$RANDOM --image=cirros -- /bin/sh
# 查看指定pod的部署yaml文件
kubectl get po 'pod-name' -o yaml
```


docker相关命令
```bash
# 删除tag为none的镜像elasticsearch-logging:9200
docker images|grep none|awk '{print $3}'|xargs docker rmi
# 批量删除状态为exited的容器
docker rm $(docker ps -q -f status=exited)
# docker查看latest版本号信息
docker image inspect drone/drone | grep -i version
```