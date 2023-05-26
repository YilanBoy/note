# Google Cloud CLI

紀錄 Google Cloud CLI 安裝流程

## 安裝 Google Cloud CLI

詳細可以看[官方文件](https://cloud.google.com/sdk/docs/install)，下面以我在 WSL (Windows Subsystem for Linux) 中安裝為例

新增一個 gcloud 資料夾

```shell
mkdir gcloud
```

下載安裝檔案到剛剛的資料夾中

```shell
curl -O --output-dir gcloud/ https://dl.google.com/dl/cloudsdk/channels/rapid/downloads/google-cloud-cli-428.0.0-linux-x86_64.tar.gz
```

進入資料夾並解壓縮，解壓縮之後會出現一個 `google-cloud-sdk` 的資料夾

```shell
cd gcloud

tar -xf google-cloud-cli-428.0.0-linux-x86_64.tar.gz
```

執行安裝 script 檔案

```shell
./google-cloud-sdk/install.sh
```

安裝過程會詢問各種問題，是否同意傳送匿名的報告協助他們改善用戶體練以及設定環境變數並修改你的 Shell 初始檔案 (.bashrc 或是 .zshrc)

```text
Welcome to the Google Cloud CLI!

To help improve the quality of this product, we collect anonymized usage data
and anonymized stacktraces when crashes are encountered; additional information
is available at <https://cloud.google.com/sdk/usage-statistics>. This data is
handled in accordance with our privacy policy
<https://cloud.google.com/terms/cloud-privacy-notice>. You may choose to opt in this
collection now (by choosing 'Y' at the below prompt), or at any time in the
future by running the following command:

    gcloud config set disable_usage_reporting false

Do you want to help improve the Google Cloud CLI (y/N)?  y


Your current Google Cloud CLI version is: 428.0.0
The latest available version is: 428.0.0

┌────────────────────────────────────────────────────────────────────────────────────────────────────────────────┐
│                                                   Components                                                   │
├───────────────┬──────────────────────────────────────────────────────┬──────────────────────────────┬──────────┤
│     Status    │                         Name                         │              ID              │   Size   │
├───────────────┼──────────────────────────────────────────────────────┼──────────────────────────────┼──────────┤
│ Not Installed │ App Engine Go Extensions                             │ app-engine-go                │  4.5 MiB │
│ Not Installed │ Appctl                                               │ appctl                       │ 21.0 MiB │
│ Not Installed │ Artifact Registry Go Module Package Helper           │ package-go-module            │  < 1 MiB │
│ Not Installed │ Cloud Bigtable Command Line Tool                     │ cbt                          │ 10.5 MiB │
│ Not Installed │ Cloud Bigtable Emulator                              │ bigtable                     │  6.9 MiB │
│ Not Installed │ Cloud Datastore Emulator                             │ cloud-datastore-emulator     │ 35.1 MiB │
│ Not Installed │ Cloud Firestore Emulator                             │ cloud-firestore-emulator     │ 41.8 MiB │
│ Not Installed │ Cloud Pub/Sub Emulator                               │ pubsub-emulator              │ 66.4 MiB │
│ Not Installed │ Cloud Run Proxy                                      │ cloud-run-proxy              │  9.0 MiB │
│ Not Installed │ Cloud SQL Proxy                                      │ cloud_sql_proxy              │  7.8 MiB │
│ Not Installed │ Cloud Spanner Emulator                               │ cloud-spanner-emulator       │ 29.7 MiB │
│ Not Installed │ Cloud Spanner Migration Tool                         │ harbourbridge                │ 22.3 MiB │
│ Not Installed │ Google Container Registry's Docker credential helper │ docker-credential-gcr        │  1.8 MiB │
│ Not Installed │ Kustomize                                            │ kustomize                    │  4.3 MiB │
│ Not Installed │ Log Streaming                                        │ log-streaming                │ 13.9 MiB │
│ Not Installed │ Minikube                                             │ minikube                     │ 33.8 MiB │
│ Not Installed │ Nomos CLI                                            │ nomos                        │ 25.2 MiB │
│ Not Installed │ On-Demand Scanning API extraction helper             │ local-extract                │ 14.3 MiB │
│ Not Installed │ Skaffold                                             │ skaffold                     │ 22.0 MiB │
│ Not Installed │ Terraform Tools                                      │ terraform-tools              │ 61.7 MiB │
│ Not Installed │ anthos-auth                                          │ anthos-auth                  │ 20.4 MiB │
│ Not Installed │ config-connector                                     │ config-connector             │ 56.7 MiB │
│ Not Installed │ enterprise-certificate-proxy                         │ enterprise-certificate-proxy │  6.6 MiB │
│ Not Installed │ gcloud Alpha Commands                                │ alpha                        │  < 1 MiB │
│ Not Installed │ gcloud Beta Commands                                 │ beta                         │  < 1 MiB │
│ Not Installed │ gcloud app Java Extensions                           │ app-engine-java              │ 64.6 MiB │
│ Not Installed │ gcloud app Python Extensions                         │ app-engine-python            │  8.4 MiB │
│ Not Installed │ gcloud app Python Extensions (Extra Libraries)       │ app-engine-python-extras     │ 26.4 MiB │
│ Not Installed │ gke-gcloud-auth-plugin                               │ gke-gcloud-auth-plugin       │  7.7 MiB │
│ Not Installed │ kpt                                                  │ kpt                          │ 14.0 MiB │
│ Not Installed │ kubectl                                              │ kubectl                      │  < 1 MiB │
│ Not Installed │ kubectl-oidc                                         │ kubectl-oidc                 │ 20.4 MiB │
│ Not Installed │ pkg                                                  │ pkg                          │          │
│ Installed     │ BigQuery Command Line Tool                           │ bq                           │  1.6 MiB │
│ Installed     │ Bundled Python 3.9                                   │ bundled-python3-unix         │ 63.4 MiB │
│ Installed     │ Cloud Storage Command Line Tool                      │ gsutil                       │ 15.5 MiB │
│ Installed     │ Google Cloud CLI Core Libraries                      │ core                         │ 20.3 MiB │
│ Installed     │ Google Cloud CRC32C Hash Tool                        │ gcloud-crc32c                │  1.2 MiB │
└───────────────┴──────────────────────────────────────────────────────┴──────────────────────────────┴──────────┘
To install or remove components at your current SDK version [428.0.0], run:
  $ gcloud components install COMPONENT_ID
  $ gcloud components remove COMPONENT_ID

To update your SDK installation to the latest version [428.0.0], run:
  $ gcloud components update


Modify profile to update your $PATH and enable shell command completion?

Do you want to continue (Y/n)?  Y

The Google Cloud SDK installer will now prompt you to update an rc file to bring the Google Cloud CLIs into your
environment.

Enter a path to an rc file to update, or leave blank to use [/home/<username>/.zshrc]:
No changes necessary for [/home/<username>/.zshrc].

For more information on how to get started, please visit:
  https://cloud.google.com/sdk/docs/quickstarts
```

安裝好之後可以重新啟動終端機或是使用 `source ~/.zshrc` 來使用更新後的環境變數，這時候我們就可以使用 `gcloud` 指令了

初始化 `gcloud` 並登入帳戶

```shell
gcloud init
```

## 設定地區 (Region)

查看可以設定的地區有哪些

> 帳戶需要先設定付款方式才能使用

```shell
gcloud compute regions list
```

修改地區為台灣

```shell
gcloud config set compute/region asia-east1
```

查看地區是否修改成功

```shell
gcloud config list compute/region
```

## 更新 CLI 組件 (Component)

查看目前的 Google Cloud CLI 有安裝那些組件

```shell
gcloud components list
```

安裝組件

```shell
gcloud components install COMPONENT_ID
```

更新所有組件

```shell
gcloud components update
```

## 生成 Credentials

登入並設定

```shell
gcloud auth application-default login
```

登入之後會生成一個 `/home/<username>/.config/gcloud/application_default_credentials.json`，第三方的 library 或是 Terraform 都會直接使用這個 Credentials

如果要更換帳戶，可以撤銷原來的 credentials

```shell
gcloud auth application-default revoke
```

## 參考資料

- [StackOverflow - How to change Region / Zone in Google Cloud?](https://stackoverflow.com/questions/45125143/how-to-change-region-zone-in-google-cloud)
- [Google Cloud 亞太地區據點](https://cloud.google.com/about/locations?hl=zh-tw#asia-pacific)
