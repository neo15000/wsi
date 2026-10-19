# GitHub Pages WSI教材ビューアー

## このパッケージについて
- `index.html`：教材一覧・OpenSeadragonビューアー・学習メモ
- `cases.json`：教材情報とDZIファイルのパス
- `images/demo.dzi` と `images/demo_files/`：動作確認用の合成デモ画像
- `images/demo_full.jpg`：DZI変換前のデモ画像（任意）

デモ画像は合成画像であり、患者画像・診断用画像ではありません。

## GitHub Pagesで公開する手順（初心者向け）
1. https://github.com/ にログインします。
2. 右上の `+` → `New repository` を選びます。
3. Repository name に `wsi-viewer` と入力します。
4. `Public` を選び、`Add a README file` にチェックして `Create repository` を押します。
5. リポジトリ画面で `Add file` → `Upload files` を選びます。
6. ZIPを解凍し、**中身のファイルとフォルダ**（`index.html`、`cases.json`、`images`、`README.md`）をまとめてアップロードします。ZIPファイルそのものだけをアップロードしないでください。
7. 下の `Commit changes` を押します。
8. `Settings` → 左メニューの `Pages` を開きます。
9. `Build and deployment` の `Source` は `Deploy from a branch`、`Branch` は `main`、フォルダは `/(root)` を選び `Save` します。
10. 1〜数分待って、表示されたURL（通常 `https://あなたのユーザー名.github.io/wsi-viewer/`）を開きます。

## 教材の追加方法
1. DZI形式のファイル一式を `images` フォルダ内に追加します。例：`images/case001.dzi` と `images/case001_files/`
2. `cases.json` を編集し、配列の中に症例情報を追加します。例：

```json
{
  "id": "case001",
  "title": "症例001",
  "organ": "子宮内膜",
  "stain": "HE染色",
  "magnification": "40倍",
  "description": "学習用の説明文",
  "dzi": "./images/case001.dzi"
}
```

3. 症例を複数登録するときは、各オブジェクトをカンマで区切ります。最後のオブジェクトの後にはカンマを付けません。
4. `cases.json` とDZIファイルをGitHubにアップロードしてコミットします。公開サイトに反映されるまで少し時間がかかることがあります。

## QuPath / OpenSlide / libvipsでDZIを作る例
libvipsがインストールされている環境で、入力WSIからDZIを作成します。入力ファイルの形式によってはOpenSlideなどの対応が必要です。

```bash
vips dzsave input.svs images/case001 --layout dz --tile-size 512
```

このコマンドは `images/case001.dzi` と `images/case001_files/` を作成します。出力先フォルダは事前に作成してください。出力ファイルとフォルダを両方GitHubに登録します。

## 注意点
- GitHub Pagesは静的サイトです。PHP、SQLite、サーバー側ログイン、共有データベースは動きません。
- 学習メモは `localStorage` に保存され、入力した端末・ブラウザにだけ残ります。別の端末とは同期しません。
- 公開リポジトリに登録した画像は公開されます。患者情報、氏名、受付番号、施設内限定画像、利用許諾のない画像をアップロードしないでください。
- DZIはタイル数が多くなるため、巨大なWSIを大量に置くとリポジトリ容量・通信量・読み込み時間が増えます。まず小さな匿名化済みテスト画像で確認してください。
- OpenSeadragon本体はCDNから読み込みます。インターネット接続が必要です。
- 公開URLは作成後に `Settings` → `Pages` に表示されます。ユーザー名が分からない状態ではURLを決め打ちせず、GitHubの表示を確認してください。
