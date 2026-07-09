# セットアップ手順（GitHub版・完全無料）

サーバー契約もFTPもcronの設定も不要です。GitHubのアカウントを1つ作るだけで完結します。
所要時間の目安：30分程度（+ APIキーの審査待ち5営業日）。

---

## 0. 全体の流れ

1. APIキーを申請する（数日かかるので最初に）
2. GitHubアカウントを作る（無料）
3. リポジトリ（プロジェクトの保管場所）を作り、ファイル一式をアップロード
4. APIキーを「Secrets」に安全に登録する
5. 自動実行（GitHub Actions）を1回試しに動かす
6. 公開設定（GitHub Pages）をONにする
7. 完成したURLを確認する

---

## 1. APIキーを申請する

1. https://www.reinfolib.mlit.go.jp/api/request/ から申請
2. 審査完了まで目安5営業日。届いたAPIキーは後で使うのでメモしておく
   （**待っている間に2〜3を進めておけます**）

---

## 2. GitHubアカウントを作る

1. https://github.com/ にアクセスし、「Sign up」からメールアドレスで無料登録
2. ログインできる状態にしておく

---

## 3. リポジトリを作り、ファイルをアップロードする

1. GitHub右上の「+」→「New repository」をクリック
2. リポジトリ名を入力（例：`estate-search`）
3. 「**Private**」を選択（第三者に中身を見られたくない場合。Publicでも動作は同じです）
4. 「Create repository」をクリック
5. 作成後の画面で「**uploading an existing file**」というリンクをクリック
6. お渡しした一式（`config.php`、`lib`フォルダ、`batch`フォルダ、`.github`フォルダ、`index.html`、
   `cache`フォルダ）を、パソコンからそのまま**ドラッグ＆ドロップ**でアップロード
   - Gitのコマンドやツールは一切不要です。ブラウザの操作だけで完結します
   - `.github`フォルダのような「.」から始まるフォルダも、ドラッグ＆ドロップならそのまま
     認識されます（フォルダごと選択してドラッグしてください）
7. 画面下部の「Commit changes」をクリックしてアップロードを確定

---

## 4. APIキーを安全に登録する（Secrets）

1. リポジトリ画面上部の「**Settings**」タブを開く
2. 左メニューの「**Secrets and variables**」→「**Actions**」を開く
3. 「**New repository secret**」をクリック
4. 以下の通り入力：
   - Name: `REINFOLIB_API_KEY`
   - Secret: （届いたAPIキーを貼り付け）
5. 「Add secret」をクリック

これでAPIキーは暗号化されて保管され、実行時にだけ使われます。リポジトリの中身を見ても
キーの値は表示されません（Publicリポジトリにしても安全です）。

---

## 5. 自動実行（GitHub Actions）を試しに動かす

1. リポジトリ上部の「**Actions**」タブを開く
2. 左側に「相場データの更新」というワークフローが表示されるのでクリック
3. 右側の「**Run workflow**」ボタン→再度「Run workflow」で手動実行
4. 数十秒〜数分待ち、実行結果が緑のチェックマーク（✓）になれば成功
   - 赤い×になった場合は、クリックしてログを開き、エラーメッセージを確認してください
     （多くの場合、Secretsの名前が `REINFOLIB_API_KEY` と完全に一致しているか、
     APIキーの審査がまだ完了していないか、のどちらかが原因です）
5. 成功すると、`cache/` フォルダの中に `14203.json` のようなファイルが自動的に
   コミットされます（リポジトリの「Code」タブから確認できます）

このワークフローは、これ以降**毎月1日の朝5時（日本時間）に自動的に再実行**されます。
手動で今すぐ更新したいときは、いつでも同じ「Run workflow」ボタンで実行できます。

---

## 6. GitHub Pagesを有効にする（ページを公開する）

1. 「Settings」タブ →左メニューの「**Pages**」を開く
2. 「Build and deployment」の「Source」で「**Deploy from a branch**」を選択
3. 「Branch」で「**main**」、フォルダは「**/ (root)**」を選択して「Save」
4. 数分待つと、ページ上部に「Your site is live at https://（あなたのアカウント名）.github.io/（リポジトリ名）/」
   と表示されます。これが検索ページのURLです

---

## 7. 完成したURLを確認する

`https://あなたのアカウント名.github.io/estate-search/` のようなURLで、実際に
検索ページが開けることを確認してください。市区町村・エリア・種別を選んで
「相場を調べる」を押し、数値が表示されればセットアップ完了です。

---

## 8. いえらぶクラウドへの反映

いえらぶの担当者に確認した結果に応じて、以下のどちらかで対応してください。

- **iframe埋め込みが可能な場合**：上記のURLをiframeのsrcに指定
  ```html
  <iframe src="https://あなたのアカウント名.github.io/estate-search/"
          style="width:100%; height:900px; border:none;"></iframe>
  ```
- **外部リンクのみ可能な場合**：メニューやボタンのリンク先に上記URLを設定

---

## 9. （任意）自社ドメインのサブドメインで見せたい場合

`estate.your-domain.jp` のような自社ドメインで見せたい場合は、以下の対応も可能です。

1. ドメインを管理しているサービス（お名前.comなど）でCNAMEレコードを追加
   - ホスト名：`estate`（お好きな文字列）
   - 値：`あなたのアカウント名.github.io`
2. GitHubリポジトリの「Settings」→「Pages」の「Custom domain」に `estate.your-domain.jp` を入力して保存

これで `https://estate.your-domain.jp/` として公開できます（DNS反映まで数時間かかる場合があります）。

---

## よくあるトラブル

**Q. Actionsの実行が赤い×で失敗する**
→ 「Actions」タブ→失敗した実行をクリック→ログを開いてエラー内容を確認してください。
`REINFOLIB_API_KEY` が空、またはAPIキーがまだ有効化されていない、というケースが多いです。

**Q. ページを開いても「データがまだありません」と出る**
→ 手順5のActionsがまだ1度も成功していない可能性があります。Actionsタブで実行結果を確認してください。

**Q. cache/フォルダにJSONファイルが増えない**
→ ワークフローの最後の「更新結果をコミット」ステップが失敗していないか確認してください。
リポジトリの権限設定で「Settings」→「Actions」→「General」→「Workflow permissions」が
「Read and write permissions」になっているか確認してください（初期設定では読み取り専用に
なっている場合があります）。
