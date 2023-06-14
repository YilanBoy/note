# Sealed Secrets

Sealed secrets 是由 Bitnami 所開發，是一種幫助你管理 k8s 上 secrets 的工具

## 概念

Sealed secrets 由兩個部分組成

- A cluster-side controller / operator
- A client-side utility: `kubeseal`

它的原理是會在 k8s 叢集上面啟動一個 sealed secrets controller，用戶端可以使用 `kubeseal` 與該 controller 溝通並取得憑證，
`kubeseal` 會使用這個憑證來加密 `Secret` 資源，使其變成 `SealedSecret` 資源

原本的 `Secret` 設定檔案

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: mysecret
  namespace: mynamespace
data:
  foo: YmFy  # <- base64 encoded "bar"
```

經過加密後的 `SealedSecret` 設定檔案，**可以安心的將其放入版本控制中**

```yaml
apiVersion: bitnami.com/v1alpha1
kind: SealedSecret
metadata:
  name: mysecret
  namespace: mynamespace
spec:
  encryptedData:
    foo: AgBy3i4OJSWK+PiTySYZZA9rO43cGDEq.....
```

當你將 `SealedSecret` 部署到叢集時，sealed secrets controller 會自動將你的 `SealedSecret` 解密，並建立對應的 `Secret`

## 使用 Helm 部署 Sealed Secrets Controller

可以使用 Helm 部署 Sealed Secrets Controller

```shell
helm repo add sealed-secrets https://bitnami-labs.github.io/sealed-secrets

helm install sealed-secrets -n kube-system --set-string fullnameOverride=sealed-secrets-controller sealed-secrets/sealed-secrets
```

Sealed secrets controller 的預設名稱為 `sealed-secrets`

因為 `kubeseal` 預設會找尋名稱為 `sealed-secrets-controller` 的 controller，
因此這裡使用 `--set-string fullnameOverride=sealed-secrets-controller` 將 controller 的預設名稱修改為 `sealed-secrets-controller`

## 使用 Kubeseal 將 Secret 轉換成 SealedSecret

Mac 上可以使用 Homebrew 安裝 `kubeseal`

```shell
brew install kubeseal
```

安裝完成之後就可以使用 kubeseal 將原本的 `Secret` 轉換成 `SealedSecret`

```shell
# option 1
kubeseal <mysql-secrets.yaml > mysql-sealed-secrets.yaml

# option 2
cat mysql-secrets.yaml | kubeseal > mysql-sealed-secrets.yaml
```
