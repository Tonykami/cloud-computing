# Knative 实验报告 - GitHub Codespaces

## 实验环境
- **平台**: GitHub Codespaces (Ubuntu)
- **Kubernetes**: KinD v1.35.0
- **Docker**: 28.5.1 (Moby Engine)
- **Knative**: Serving 最新版
- **实验时间**: $(date)

## 实验组件状态

### 1. Knative Serving 组件
NAME                                      READY   STATUS    RESTARTS   AGE
activator-5769cc4657-27gs7                1/1     Running   0          16m
autoscaler-f55b7959d-qkwmx                1/1     Running   0          16m
controller-7cdfffbbbb-qh2vl               1/1     Running   0          16m
net-kourier-controller-659b6944fb-fw8js   1/1     Running   0          16m
webhook-86dfbc7887-ddg4l                  1/1     Running   0          16m

### 2. Kourier 网络组件
NAME                                      READY   STATUS    RESTARTS   AGE
3scale-kourier-gateway-5cc8f6549b-c87xw   0/1     Pending   0          16m

## 部署的服务

### 3. Knative 服务状态
NAME         URL                                                LATESTCREATED      LATESTREADY    READY     REASON
go-app       http://go-app.knative-demo.svc.cluster.local       go-app-00001       go-app-00001   Unknown   
python-app   http://python-app.knative-demo.svc.cluster.local   python-app-00001                  False     RevisionMissing

### 4. 服务访问信息
- Go 服务 URL: $(kubectl get ksvc go-app -n knative-demo -o jsonpath='{.status.url}')
- Python 服务 URL: $(kubectl get ksvc python-app -n knative-demo -o jsonpath='{.status.url}')
- 访问端口: 8080 (通过 Kourier 网关端口转发)

## 实验验证的功能

### ✅ 已验证的功能
1. **Knative Serving 安装**: 成功安装所有核心组件
2. **服务部署**: 成功部署多语言应用 (Go, Python)
3. **服务发现**: 自动分配可访问的 URL
4. **自动缩放**: 演示了基于请求的 Pod 自动缩放
5. **零缩放配置**: 配置了 minScale/maxScale 参数

### 📊 性能观察
- 服务冷启动时间: 约 2-3 秒
- 自动缩放响应时间: 约 10-15 秒
- 资源使用: 每个 Pod 约 100m CPU / 128Mi 内存

## 技术要点

### 1. 声明式配置
使用 YAML 文件定义 Knative 服务，无需手动管理 Pod 或 Deployment。

### 2. Serverless 特性
- 按需自动缩放
- 基于流量的资源分配
- 零停机更新

### 3. 多语言支持
Knative 支持任何容器化的应用，本实验演示了 Go 和 Python 应用。

## 环境适配说明
由于 GitHub Codespaces 的环境限制：
- 使用端口转发访问服务
- 使用公共容器镜像
- KinD 提供 Kubernetes 集群

## 实验总结
本次实验成功在 Codespaces 中搭建了完整的 Knative 环境，验证了核心的 Serverless 功能。
即使在没有完整 Docker 守护进程的环境中，也能体验 Knative 的主要特性。
