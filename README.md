# simple-html

HTML と CSS だけの静的ページ。VSCode の中だけで「Git 管理 → GitHub → GitHub Pages / Vercel デプロイ」を一通り練習するための教材用リポジトリ。

## ファイル構成

```
simple-html/
├── index.html   ページ本体
├── style.css    スタイル
├── .gitignore
└── README.md
```

ビルド不要。`index.html` をブラウザで開けばそのまま表示される。

## VSCode でのプレビュー

`index.html` を右クリック →「Open with Live Server」（拡張機能 Live Server を入れている場合）。
入れていなければ Finder から `index.html` をダブルクリックでも確認できる。

## 手順 1：Git（初期化済み）

このフォルダはすでに `git init` 済みで、初回コミットも入っている。
以降は VSCode 左側の **ソース管理**（Ctrl/Cmd + Shift + G）から操作する。

1. ファイルを編集して保存する
2. ソース管理パネルで変更ファイルの `+` を押してステージする
3. メッセージ欄にコミットメッセージを書く
4. `✓ コミット` を押す

## 手順 2：GitHub に上げる

VSCode だけで完結する方法。

ソース管理パネルの一番上にある青いボタンは、**状況によって表示が変わる**。

| いまの状態 | 青いボタンの表示 |
|---|---|
| コミットしていない変更がある | `✓ コミット` |
| 変更なし・リモート未設定 | **`Publish Branch` / `ブランチの発行`** ← 手順2はここ |
| リモート設定済み | `同期` |

1. 変更をすべてコミットして、パネルに変更が残っていない状態にする
2. 青いボタンが **`Publish Branch`（ブランチの発行）** に変わっているので押す
3. 画面上部に選択肢が出るので、どちらかを選ぶ
   - `Publish to GitHub private repository`
   - `Publish to GitHub public repository`
   - Vercel の無料プランは private でもデプロイできる
   - ただし **GitHub Pages（手順3）を使うなら public が必要**（無料アカウントの場合）
4. 初回は GitHub へのサインインを求められるので、ブラウザで承認する
5. 完了すると GitHub 上にリポジトリが**自動で作られる**。以降は `同期` ボタンで push できる

> ボタンが見当たらないときは、コマンドパレット（`Cmd + Shift + P`）で `Publish to GitHub` と入力しても同じことができる。
>
> なお **「Publish to GitHub」という名前のボタン**は、まだ `git init` していないフォルダを開いたときにソース管理パネルへ出るもの。このリポジトリは init 済みなので、その表示にはならない。

## 手順 3：GitHub Pages で公開する

GitHub だけで完結する公開方法。Vercel を使わず、リポジトリの中身をそのまま Web サイトとして配信する。

### 前提：リポジトリが public であること

無料アカウントでは **GitHub Pages は public リポジトリでしか使えない**（private で使うには GitHub Pro 以上が必要）。

private で発行してしまった場合の変更手順:

1. GitHub のリポジトリページ → **Settings**
2. 一番下の **Danger Zone** → **Change repository visibility** → **Change to public**
3. 確認のためリポジトリ名を入力して確定

### 公開設定

1. リポジトリページ → **Settings** → 左メニューの **Pages**
2. **Build and deployment** の **Source** を `Deploy from a branch` にする
3. **Branch** を `main`、フォルダを `/ (root)` にして **Save**
4. 1〜2 分待つと、ページ上部に公開 URL が表示される

URL はこの形になる:

```
https://<GitHubユーザー名>.github.io/simple-html/
```

### 反映の確認

- push するたびに自動で再デプロイされる
- 進行状況はリポジトリの **Actions** タブ、`pages build and deployment` で確認できる
- 反映まで数十秒〜数分かかる。表示が変わらないときはスーパーリロード（`Cmd + Shift + R`）

> `style.css` は相対パスで読み込んでいるので、`/simple-html/` のようなサブパス配信でもそのまま動く。
> ファイル名やフォルダ名を `_` で始めていないため、`.nojekyll` ファイルは不要。

## 手順 4：Vercel でデプロイ

ブラウザ版（推奨・最短）:

1. https://vercel.com にログイン（GitHub アカウントでログインすると連携が早い）
2. **Add New… → Project**
3. 先ほど発行したリポジトリを **Import**
4. Framework Preset は **Other**、Build Command と Output Directory は **空のまま**
   （ビルド不要な静的サイトなので設定はいらない）
5. **Deploy** を押す

数十秒で `https://＜プロジェクト名＞.vercel.app` が発行される。

VSCode の中だけで済ませたい場合は Vercel CLI を使う:

```bash
npm i -g vercel     # 初回のみ
vercel login
vercel              # プレビュー環境へデプロイ
vercel --prod       # 本番へデプロイ
```

## 手順 5：更新が反映されるか確認する

GitHub Pages も Vercel も、**push するたびに自動で再デプロイ**される。

1. `index.html` の `<p class="badge">v1</p>` を `v2` に書き換える
2. 保存 → ソース管理でコミット → 同期（push）
3. 公開 URL を開いて、表示が `v2` に変われば成功
   - GitHub Pages のビルド状況: リポジトリの **Actions** タブ
   - Vercel のビルド状況: Vercel のダッシュボード

ここまで確認できれば、デプロイの一連の流れは押さえたことになる。

## GitHub Pages と Vercel の違い

| | GitHub Pages | Vercel |
|---|---|---|
| 料金 | 無料（public のみ） | 無料枠あり（private も可） |
| 設定場所 | リポジトリの Settings | Vercel のダッシュボード |
| URL | `<ユーザー名>.github.io/<リポジトリ名>/` | `<プロジェクト名>.vercel.app` |
| 反映速度 | やや遅い（数十秒〜数分） | 速い（十数秒〜） |
| プレビュー環境 | なし | ブランチごとに自動生成 |
| 向き | 静的サイト専用 | 静的サイト＋動的な処理も可 |

今回のような HTML と CSS だけのページなら、どちらでも同じ結果になる。両方試すと違いが分かりやすい。
