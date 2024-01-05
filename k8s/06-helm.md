# Helm

Helm 是一個 Kubernetes 的套件管理工具，可以用來部署應用程式到 Kubernetes 叢集中。

> [!NOTE]
>
> Helm 之於 Kubernetes，就像是 apt 或是 yum 之於 Linux。

一個 Helm 套件由三個部分組成：

- Chart : Helm 套件的主要，裡面包含了要部署的應用程式的描述檔。
- Repository : 用來存放 Chart 的地方，可以是本地端的檔案系統，也可以是遠端的 HTTP 伺服器。
- Release : 一個 Chart 的部署實例，一個 Chart 可以部署多個 Release。

## 常用指令

搜尋 Chart：

```shell
# 從 Artifact Hub 搜尋 有沒有 WordPress 的 Chart
helm search hub wordpress

# 從本地添加的 Repository 中搜尋有沒有 WordPress 的 Chart
helm search repo wordpress
```

加入一個 Repository：

```shell
# 添加一個名稱為 sealed-secrets 的 Repository
# Repository 的 URL 為 https://charts.helm.sh/stable
helm repo add sealed-secrets https://bitnami-labs.github.io/sealed-secrets
```

安裝 Chart：

```shell
# 安裝 wordpress 的 Chart，並將 release 名稱設定為 happy-panda
helm install happy-panda bitnami/wordpress
```

查看 Release 的狀態：

```shell
helm status happy-panda
```

Chart 通常會有預設的設定，這些設定是可以修改的，可以使用 `helm show values` 查看 Chart 的預設設定。

```shell
helm show values bitnami/wordpress
```

如果想要修改 Chart 的設定，可以建立一個 `values.yaml`，並寫上想要修改的設定。

```yaml
# values.yaml
mariadb:
  auth:
    database: user0db
    username: user0
```

使用 `helm install` 的 `-f` 參數載入檔案，覆蓋 Chart 的預設設定。

```shell
# 使用 --generate-name 參數，讓 helm 自動產生 release 名稱
helm install -f values.yaml bitnami/wordpress --generate-name
```

也可以直接使用 `--set` 參數修改設定。

```shell
helm install bitnami/wordpress --generate-name \
--set mariadb.auth.database=user0db,mariadb.auth.username=user0
```
