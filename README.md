# demo-kotlin-spring-lambda-snapstart

Spring Cloud Function + AWS Lambda SnapStart のデモプロジェクト（Kotlin）

## 概要

このプロジェクトは、AWS Lambda の Java 21 ランタイムで Spring Cloud Function を動かし、SnapStart による高速起動を実現するデモです。

### 技術スタック

- **言語**: Kotlin
- **フレームワーク**: Spring Cloud Function
- **ランタイム**: AWS Lambda (Java 21 / Amazon Corretto)
- **ビルドツール**: Gradle (Kotlin DSL)
- **インフラ**: Terraform
- **タスクランナー**: Task
- **環境変数管理**: direnv

## 前提条件

### Codespaces / devcontainer を使う場合

devcontainer が自動的に必要なツールをセットアップします。特別な準備は不要です。

### ローカル環境の場合

以下のツールをインストールしてください:

- [Amazon Corretto 21](https://docs.aws.amazon.com/corretto/latest/corretto-21-ug/downloads-list.html)
- [AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)
- [Terraform](https://developer.hashicorp.com/terraform/downloads)
- [Task](https://taskfile.dev/installation/)
- [direnv](https://direnv.net/docs/installation.html)

## 環境セットアップ

### 1. AWS 認証情報の設定

`.envrc.example` をコピーして `.envrc` を作成し、AWS 認証情報を設定します:

```bash
cp .envrc.example .envrc
```

`.envrc` を編集:

```bash
export AWS_ACCESS_KEY_ID=your-access-key-id
export AWS_SECRET_ACCESS_KEY=your-secret-access-key
export AWS_REGION=ap-northeast-1
```

### 2. direnv の有効化

```bash
direnv allow
```

## ビルド

```bash
# プロジェクトのビルド
task build

# テストの実行
task test
```

## インフラ構築 (Terraform)

### 1. Terraform 初期化

```bash
task tf:init
```

### 2. 変更内容の確認

```bash
task tf:plan
```

### 3. インフラ構築

API Gateway と Lambda 関数を作成します:

```bash
task tf:apply
```

## デプロイと動作確認

### 1. Lambda 関数のデプロイ

```bash
task deploy
```

### 2. 動作確認

API Gateway エンドポイントに curl でリクエストを送信:

```bash
task curl
```

レスポンス例:

```json
{"message": "Hello, World!"}
```

## クリーンアップ

作成したインフラを削除します:

```bash
task tf:destroy
```

## アーキテクチャ

```
┌─────────────┐     ┌─────────────────────────────────┐
│ API Gateway │────▶│ Lambda (Java 21 + SnapStart)    │
│   (HTTP)    │◀────│ Spring Cloud Function (Kotlin)  │
└─────────────┘     └─────────────────────────────────┘
```

## ディレクトリ構成

```
.
├── src/
│   └── main/
│       └── kotlin/          # Kotlin ソースコード
├── terraform/               # Terraform ファイル
├── .devcontainer/           # devcontainer 設定
├── Taskfile.yml             # タスク定義
├── .envrc.example           # 環境変数のサンプル
├── .envrc                   # 環境変数 (gitignore)
├── build.gradle.kts         # Gradle 設定
└── README.md
```

## Task コマンド一覧

| コマンド | 説明 |
|---------|------|
| `task build` | プロジェクトのビルド |
| `task test` | テストの実行 |
| `task deploy` | Lambda 関数のデプロイ |
| `task curl` | API Gateway への動作確認リクエスト |
| `task tf:init` | Terraform 初期化 |
| `task tf:plan` | Terraform 変更内容の確認 |
| `task tf:apply` | Terraform でインフラ構築 |
| `task tf:destroy` | Terraform でインフラ削除 |

## ライセンス

Apache License 2.0
