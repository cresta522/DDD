# DDD
## Laravel ディレクトリ階層(DDD込みCQRS込み)

### CommandService: 永続化
永続化処理をまとめるサービス

リポジトリをDIして、Entityの中のModelを引数として渡す。

#### Entity
永続化対象(Model)生成器

永続化対象をどんなRequestをもとに生成するか場合によって分かれるため静的関数で自身を生成する。

返り値は基本自分自身とし、`getModel`関数で永続化対象を返却する。

### QueryService: 非永続化(取得)
非永続化をまとめるサービス

リポジトリをDIしてクエリを作成し、必要なレコードの取得を行う。

取得したレコードはModelになっているのでDtoに引き渡し、返り値をDto、DtoCollectionとする

#### Dto (Data Transfer Object)

引数はModelとする。そのクラス内で必要なレコードの表示内容の変更を行う

- 姓名をまとめてフルネームを取得する関数を作成する
- 生年月日から年齢を取得する関数を作成する

など。

### Service: DBと関係ないもの

取得したModelを用いた表示内容の変更ならDtoの責務だが、例えばRequestをもとにレコードの存在を確認するか、
などの永続化・非永続化などに属さない処理をまとめる。

一番処理内容が少ないことが望ましい。

### Repository

- 渡されたデータを保存する
- 渡されたIDを削除する
- データ取得のためのビルダを提供する

のみを役割とする。

Controllerの中で呼ばないこと。DIをするのはサービス内でのみとする。

### 注意

Serviceの中でCommandServiceやQueryServiceを呼び、その中でServiceを再帰的に呼んでしまうと500になる。

### ディレクトリ構造
```
docker/                       Dockerコンテナ群
src/
   ├─ app/                    メインコード
   │   ├─ Actions
   │   │   └─ Commands/       Fortify(Fortify認証 カスタマイズ用 Laravel8-)
   │   ├─ Console/
   │   │   └─ Commands/       コマンド (Laravel標準)
   │   │
   │   ├─ Core/               共通機能
   │   │
   │   ├─ Domain/             業務ドメイン(DDD)
   │   │   ├─ Entity/         エンティティ（集約）
   │   │   │   └─ Validator/  エンティティバリデーター
   │   │   ├─ Event/          ドメインイベント
   │   │   ├─ Service/        ドメインサービス
   │   │   └─ ValueObjects/   値オブジェクト（エンティティ用）
   │   │
   │   ├─ Exception/          例外
   │   │
   │   ├─ Http/
   │   │   ├─ Controllers/    Webコントローラ (Laravel標準)
   │   │   │
   │   │   ├─ Middleware/     ミドルウェア (Laravel標準)
   │   │   └─ Requests/       フォームバリデーション (Laravel標準)
   │   │
   │   ├─ Jobs/               ジョブ (Laravel標準)
   │   ├─ Listeners/          イベントリスナー (Laravel標準)
   │   │
   │   ├─ Models/             Eloquentモデル (Laravel標準)
   │   │   ├─ Event/          Eloquentイベント
   │   │   └─ Validator/      Eloquentモデルバリデーター
   │   │
   │   ├─ Providers/          サービスプロバイダ (Laravel標準)
   │   │
   │   ├─ Repositories/       リポジトリ
   │   │   └─ Domain/         業務ドメイン/エンティティ（集約） 用リポジトリ
   │   │
   │   ├─ Rules/              バリデーションルール (Laravel標準)
   │   │
   │   ├─ Services/           アプリケーションサービス
   │   │   ├─ Command/           永続化処理
   │   │   │   └─ Entity/        モデルを直接変更しないようにEntity経由
   │   │   └─ Query/             非永続化処理
   │   │       └─ DTO/           Data Transfer Object
   │   │
   │   ├─ ValueObjects/       値オブジェクト
   │   └─ View/               ビュー用のヘルパー
   │
   ├─ bootstrap               起動
   │
   ├─ config                  設定ファイル
   │
   ├─ database
   │   └─ migrations          マイグレーション
   │
   ├─ resources               ビュー、js、sass
   │
   ├─ routes                  ルーティング
   │
   └─ tests/                  テスト
       ├─ Browser             E2Eテスト (Laravel Dusk)
       ├─ Feature             機能テスト (Feature test)
       └─ Unit                単体テスト (Unit test)
```
