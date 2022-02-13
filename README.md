# DDD
## ディレクトリ階層(DDD込み)

CommandServiceの中で、永続化させるモデルを生成するのがEntity

Entityによって生成されたモデルを永続化するのがRepository

QueryServiceが非永続化した情報を取得する処理をまとめる。実際に取得するのはRepository。

モデルを表示するだけならばモデルを返すが、複数のモデルと一緒に情報を表示する場合はDtoに集約して表示する

### Entity
引数はモデル生成の文字列たち、戻り値はモデル(モデル生成の役割)

Controllerでは呼び出されない。あくまでサービス内でインスタンス化される

### Dto
取得したモデルの整形を行う。Dtoにしなくてもモデルを直接返すのも可能。

複数モデルを利用した集約などに利用される。


Controllerでは呼び出されない。あくまでサービス内でインスタンス化される
### Service
引数はリクエスト、中でEntityの生成やRepositoryによるCrudを行う。戻り値はvoidやDto、Model

### Repository
Controllerでは呼ばないようにする。必ずサービスの中で。

```
docker/                       Dockerコンテナ群
src/
   ├─ app/                    メインコード
   │   ├─ Actions
   │   │   └─ Commands/       Fortify(Fortify認証 カスタマイズ用)
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
