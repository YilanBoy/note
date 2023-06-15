# Kubernetes (K8s)

一直想學習如何使用 k8s，與其看書學習不如實際將自己的 side project 應用在上面看看

## 架構

與前輩討論後，便規劃了下面的架構

![k8s architecture](https://allen-files.s3.ap-northeast-1.amazonaws.com/images/k8s-architecture.jpg)

- 在一個 VPC 底下，有兩個 subnet，一個是可以與外部直接連線的 public subnet，一個是只能透過 NAT 與外部連線的 private subnet
- Public subnet 底下有一個用 nginx 做的 proxy server
- 外部如果想訪問 private subnet 底下的資源，必須透過 public subnet 底下的 proxy server
- 想使用 SSH 遠端連線到 private subnet 底下的機器時，需要透過 proxy server 當作 SSH 的跳板
- 因為 k8s 叢集不對外，如果想訪問叢集上的服務，會透過 proxy 做請求上的轉發
- 同上，想在本地端使用 `kubectl` 操作 k8s 叢集時，也會透過 proxy 做請求上的轉發

## 計畫

搭建好叢集之後預計做一些項目來練習如何使用 k8s

- 將自己的部落格搬移到 k8s 上
- 在 k8s 上面部署 Argo CD，可以透過 Argo CD 更新與部署部落格
