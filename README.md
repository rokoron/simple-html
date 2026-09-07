# simple-html

HTML と CSS だけの静的ページ。VSCode の中だけで「Git 管理 → GitHub → Vercel デプロイ」を一通り練習するための教材用リポジトリ。

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

1. ソース管理パネルの **「Publish to GitHub」**（GitHub に発行）を押す
2. GitHub アカウントへのサインインを求められたらブラウザで承認する
3. リポジトリ名を確認し、**public / private** を選ぶ
   - Vercel の無料プランは private でもデプロイできる
4. 発行が終わると、以降は `同期` ボタンで push できる

> 「Publish to GitHub」が出ない場合は、拡張機能 **GitHub Pull Requests** を入れると表示される。

## 手順 3：Vercel でデプロイ

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

## 手順 4：更新が反映されるか確認する

GitHub 連携が済んでいれば、**push するたびに Vercel が自動で再デプロイ**する。

1. `index.html` の `<p class="badge">v1</p>` を `v2` に書き換える
2. 保存 → ソース管理でコミット → 同期（push）
3. Vercel のダッシュボードでビルドが走り、公開 URL の表示が `v2` に変われば成功

ここまで確認できれば、デプロイの一連の流れは押さえたことになる。
