# k8s-install
install HA k8s with ansible and kubeadm
# 步骤
1. 配置 nginx 到 master 6443 的 tcp 代理
2. 安装 docker-ce
```shell
ansible-playbook install.yml -i host --tags=docker
```
3. 安装 kubeadm
```shell
ansible-playbook install.yml -i host --tags=kubeadm
```
4. 下载必须的镜像
```shell
ansible-playbook install.yml -i host --tags=sync-image
```
5. 关闭防火墙
```shell
ansible-playbook install.yml -i host --tags=system
```
6. 初始化集群
```shell
kubeadm init --control-plane-endpoint "10.249.216.99:443" --upload-certs --pod-network-cidr="192.168.0.0/16"
```
7. 将 join 的命令复制到 install.yml 中，执行 join　
```shell
ansible-playbook install.yml -i hosts --tags=join
```
8. 安装网络插件
```shell
kubectl apply -f "https://cloud.weave.works/k8s/net?k8s-version=$(kubectl version | base64 | tr -d '\n')&env.IPALLOC_RANGE=192.168.0.0/16"
```

# 参考
* https://blog.hellojukay.cn/2019/02/26/20190227/
* https://blog.hellojukay.cn/2019/04/12/20190413/
* https://kubernetes.io/zh/docs/setup/production-environment/tools/kubeadm/high-availability/
