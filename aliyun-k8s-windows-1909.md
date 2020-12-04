# 阿里云k8s windows节点无法获取pause-windows镜像 

## 现象

当我们的应用发布至 Windows 节点时，pod 无法启动，使用 kubectl describe 命令显示如下：

```shell
Events:
  Type     Reason                  Age                    From                               Message
  ----     ------                  ----                   ----                               -------
  Normal   Scheduled                             default-scheduler                  
  Warning  FailedCreatePodSandBox  4m48s (x111 over 29m)  kubelet, cn-hangzhou.10.243.0.196  Failed to create pod sandbox: rpc error: code = Unknown desc = failed pulling image "registry-vpc.cn-hangzhou.aliyuncs.com/acs/pause-windows:1909": Error response from daemon: manifest for registry-vpc.cn-hangzhou.aliyuncs.com/acs/pause-windows:1909 not found: manifest unknown: manifest unknown
```

我们基础环境配置如下：

1. kubernetes ACK 托管版 1.18.8 
2. docker   19.03.5 
3. worker节点操作系统  Windows Sever Core，version 1909

此时去阿里云官方镜像仓库查询确实没有1909版本的镜像，怀疑是阿里云BUG，提前将所需的版本功能发布出来。联系阿里云售后确认确实如此。


## 解决方案

将worker节点操作系统更换为 Windows 64 （带UI界面），此时检查发现worker使用的镜像为 registry-vpc.cn-hangzhou.aliyuncs.com/acs/pause-windows:1809  POD运行正常。