# hatenablog-workflows

## Usage

サンプル用の[Template リポジトリ](https://github.com/hatena/Hatena-Blog-Workflows-Boilerplate)をご確認ください

## Workflows

### `initialize.yaml`

- 初めてはてなブログと GitHub を連携するときに利用するアクションです
- はてなブログからすべての記事データ（公開済みおよび下書き）を取得します
  - 公開記事は`entries`ディレクトリ, 下書き記事は`draft_entries`ディレクトリにそれぞれ配置されます
  - `Date`および`URL`は公開時に設定されることを想定しているため, 下書き記事ではこれらのメタデータは削除した状態で配置されます
- 本アクションで作成されたプルリクエストはマージしてもはてなブログへの同期(push)は行われません

### `create-draft.yaml`

- 下書きファイルを新規作成するときに利用するアクションです
- タイトルの文字列を受け取って記事の新規作成, はてなブログへの同期とプルリクエストの作成を行います

### `pull-draft.yaml`

- はてなブログから特定のタイトルの下書き記事を取得するときに利用するアクションです
- 引数にタイトルを渡して実行してください
  - 同一の下書きタイトルが複数ある場合, 意図しない挙動になる可能性がありますの一意の名前をつけてください
- タイトルと一致する下書き記事が取得できると, `draft_entries`に当該の下書き記事ファイルが追加されたプルリクエストが作成されます

### `pull.yaml`

- はてなブログから公開済みの記事のみを取得するアクションです
- `entries`配下に当該記事ファイルが追加, 更新されたプルリクエストが作成されます
  - 新たに追加, 更新のあった場合のみが対象です
  - 下書きファイルは取得対象外になっています
- 本アクションで作成されたプルリクエストはマージしてもはてなブログへの同期(push)は行われません

### `push-draft.yaml`

- 下書きファイルの更新結果をはてなブログに同期するためのアクションです
- `draft_entries`にある下書き記事の場合のみ, はてなブログに同期します
  - `draft_entries`に対して変更があるたびに実行することを想定しております

### `push-when-publishing-from-draft.yaml`

- 下書きファイルを初めて公開するときに利用するアクションです
- `draft_entries`にある下書き記事を公開する変更があった場合, はてなブログへ記事を公開します
- また, 同期した記事を`entries`に移動するプルリクエストを作成および自動マージします
  - main ブランチにマージされたときにのみ実行することを想定しています

### `push.yaml`

- 公開済みの記事を更新し, はてなブログに反映するときに利用するアクションです
- `entries`内の更新のあった記事をはてなブログに同期します
  - main ブランチにマージされたときにのみ実行することを想定しています
