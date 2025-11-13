# GitHub Pages デプロイ手順# GitHub Pagesデプロイ手順# GitHub Pagesデプロイ手順



このドキュメントでは、Todo AppをGitHub Pagesに自動デプロイする方法を説明します。



## 概要このドキュメントでは、Todo AppをGitHub Pagesに手動でデプロイする方法を説明します。## 目次



- **デプロイ先**: https://j1921604.github.io/todo-app/1. [自動デプロイ（推奨）](#自動デプロイ推奨)

- **デプロイ方法**: GitHub Actionsによる自動デプロイ

- **トリガー**: mainブランチへのpushまたは手動トリガー## 前提条件2. [手動デプロイ](#手動デプロイ)



## 前提条件3. [初回セットアップ](#初回セットアップ)



- GitHubリポジトリ: `https://github.com/J1921604/todo-app`- GitHubアカウント4. [トラブルシューティング](#トラブルシューティング)

- Node.js 18以上

- npm- リポジトリへのプッシュ権限



## 自動デプロイの仕組み- Node.js 18以上のインストール---



### ワークフロー



`.github/workflows/deploy.yml`が以下の処理を自動実行します:## デプロイ手順## 自動デプロイ（推奨）



1. **Checkout**: ソースコードをチェックアウト

2. **Setup Node.js**: Node.js 18をセットアップ

3. **Install dependencies**: `npm ci`で依存関係をインストール### 1. 最新のコードを取得### 前提条件

4. **Run tests**: `npm test -- --run`でテストを実行

5. **Build**: `npm run build`でプロダクションビルドを実行- GitHubリポジトリ: `https://github.com/J1921604/todo-app`

6. **Add .nojekyll**: GitHub PagesでJekyll処理をスキップするファイルを追加

7. **Deploy**: `peaceiris/actions-gh-pages@v3`を使用してgh-pagesブランチにデプロイ```powershell- GitHub Actions が有効化されている



### デプロイトリガーgit checkout main



以下の操作でデプロイが自動実行されます:git pull origin main### 手順



1. **mainブランチへのpush**```

   ```bash

   git checkout main#### 1. deployブランチの確認と切り替え

   git add .

   git commit -m "feat: 新機能を追加"### 2. プロダクションビルドを実行

   git push origin main

   ``````powershell



2. **GitHub Actionsの手動トリガー**```powershell# 現在のブランチを確認

   - GitHubリポジトリページで「Actions」タブを開く

   - 「Deploy to GitHub Pages」ワークフローを選択npm installgit branch

   - 「Run workflow」ボタンをクリック

npm run build

## デプロイ手順

```# deployブランチに切り替え

### 1. コードの変更

git checkout deploy

```bash

# 最新のmainブランチを取得ビルドが成功すると、`dist/`フォルダに以下のファイルが生成されます:```

git checkout main

git pull origin main- `index.html`



# 変更を加える- `assets/index-[hash].js`**重要**: deployブランチは既に作成済みです。このブランチにpushすると、GitHub Actionsが自動的に：

# （ファイルを編集）

- `assets/index-[hash].css`1. テストを実行

# 変更をコミット

git add .2. ビルドを実行

git commit -m "feat: 機能を追加"

```### 3. gh-pagesブランチを作成・更新3. **gh-pagesブランチを自動作成**



### 2. mainブランチにプッシュ4. ビルド結果（dist/）をgh-pagesブランチにデプロイ



```bash```powershell

git push origin main

```# 一時フォルダにビルド成果物をコピー#### 2. 変更をプッシュ（必要な場合のみ）



これで自動的にGitHub Actionsが起動し、デプロイが開始されます。Copy-Item -Path dist\* -Destination ..\temp-dist -Recurse -Force



### 3. デプロイ状況の確認```powershell



1. https://github.com/J1921604/todo-app/actions にアクセス# gh-pagesブランチに切り替え（初回は新規作成）# 変更がある場合のみ

2. 最新のワークフロー実行をクリック

3. すべてのステップが✅（グリーンチェック）になることを確認git checkout --orphan gh-pagesgit add .



**ワークフロー実行時間**: 通常2-4分# または既存の場合: git checkout gh-pagesgit commit -m "feat: Update todo app"



### 4. デプロイ完了の確認



ワークフローが成功したら、2-3分待ってから以下のURLにアクセス:# すべてのファイルを削除# GitHubにプッシュ（自動デプロイが開始される）



```git rm -rf .git push origin deploy

https://j1921604.github.io/todo-app/

``````



アプリケーションが正常に表示されれば、デプロイ完了です。# ビルド成果物をコピー



## GitHub Pages設定Copy-Item -Path ..\temp-dist\index.html -Destination . -Force**注意**: 既にpush済みの場合は「Everything up-to-date」と表示されます。



初回デプロイ時のみ、以下の設定が必要です:New-Item -Path assets -ItemType Directory -Force



1. リポジトリの「Settings」タブを開くCopy-Item -Path ..\temp-dist\assets\* -Destination assets\ -Force#### 3. GitHub Actionsの確認

2. 左サイドバーから「Pages」を選択

3. 「Source」セクションで以下を設定:

   - Source: `Deploy from a branch`

   - Branch: `gh-pages`# 一時フォルダを削除1. GitHubリポジトリページを開く: https://github.com/J1921604/todo-app

   - Folder: `/ (root)`

4. 「Save」をクリックRemove-Item -Path ..\temp-dist -Recurse -Force2. 「Actions」タブをクリック



## トラブルシューティング```3. 「Deploy to GitHub Pages」ワークフローを確認



### ワークフローが失敗する4. 最新のワークフロー実行をクリック



**症状**: GitHub Actionsのステップが❌（レッドクロス）になる### 4. 変更をコミットしてプッシュ5. すべてのステップが成功（✅ グリーンチェック）になるまで待つ（通常2-3分）



**対処法**:

1. ワークフロー実行ページでエラーログを確認

2. エラー内容に応じて修正:```powershell**ワークフローが実行されているはずの手順**:

   - テスト失敗 → テストコードを修正

   - ビルド失敗 → TypeScriptエラーを修正git add .- ✅ Checkout

   - デプロイ失敗 → 権限設定を確認

git commit -m "Deploy to GitHub Pages"- ✅ Setup Node.js

### ページが404エラー

git push -f origin gh-pages- ✅ Install dependencies

**症状**: https://j1921604.github.io/todo-app/ にアクセスすると404エラー

```- ✅ Run tests

**対処法**:

1. GitHub Pages設定を確認（上記「GitHub Pages設定」参照）- ✅ Build

2. gh-pagesブランチが存在するか確認

   ```bash### 5. mainブランチに戻る- ✅ Deploy to GitHub Pages（これがgh-pagesブランチを自動作成）

   git fetch origin

   git branch -r

   ```

3. 2-3分待ってから再度アクセス```powershell**トラブルシューティング**:



### ページが表示されるが真っ白git checkout main- ワークフローが表示されない場合 → Actionsが有効化されているか確認



**症状**: ページは開くが何も表示されない```- ワークフローが失敗している場合 → ログを確認して原因を特定



**対処法**:

1. ブラウザの開発者ツールでコンソールエラーを確認

2. `.nojekyll`ファイルがgh-pagesブランチに含まれているか確認### 6. GitHub Pagesの設定を確認#### 4. GitHub Pagesの設定（初回のみ）

3. `vite.config.ts`の`base`設定が`/todo-app/`になっているか確認

4. ビルドファイルのパスが正しいか確認



### JavaScriptファイルが読み込めない初回デプロイ時のみ必要です:1. リポジトリの「Settings」タブを開く



**症状**: コンソールに「Failed to load resource」エラー2. 左サイドバーから「Pages」をクリック



**対処法**:1. GitHubリポジトリページ（https://github.com/J1921604/todo-app）を開く3. **Source**セクションで:

1. gh-pagesブランチに`.nojekyll`ファイルを追加:

   ```bash2. 「Settings」タブをクリック   - **Source**: Deploy from a branch

   git checkout gh-pages

   touch .nojekyll3. 左サイドバーから「Pages」を選択   - **Branch**: `gh-pages` / `/(root)`

   git add .nojekyll

   git commit -m "Add .nojekyll"4. 「Source」セクションで以下を設定:   - **Save**ボタンをクリック

   git push origin gh-pages

   ```   - Source: `Deploy from a branch`

2. ワークフローが`.nojekyll`を追加するステップを含んでいるか確認

   - Branch: `gh-pages`#### 5. デプロイ完了確認

## ローカルでのプレビュー

   - Folder: `/ (root)`

デプロイ前にローカルでビルド結果を確認できます:

5. 「Save」をクリック数分後、以下のURLでアプリが公開されます:

```bash

# プロダクションビルドを実行

npm run build

### 7. デプロイ完了の確認```

# ビルド結果をプレビュー

npm run previewhttps://j1921604.github.io/todo-app/

```

2-3分待ってから、以下のURLにアクセス:```

ブラウザで http://localhost:4173 を開いてプレビューします。



## 手動デプロイ（緊急時のみ）

```ブラウザでアクセスして動作確認を行ってください。

GitHub Actionsが利用できない場合の手動デプロイ手順:

https://j1921604.github.io/todo-app/

```bash

# ビルドを実行```---

npm run build



# gh-pagesブランチに切り替え

git checkout --orphan gh-pages-newアプリケーションが正常に表示されれば、デプロイ完了です。## 手動デプロイ



# すべてのファイルを削除

git rm -rf .

## トラブルシューティング### 前提条件

# ビルド結果をコピー

cp -r dist/* .- Node.js 16以上

touch .nojekyll

### ページが404エラーになる- npm 8以上

# コミットしてプッシュ

git add .- `gh-pages` パッケージがインストール済み

git commit -m "Manual deploy"

git branch -M gh-pages-new gh-pages- GitHub PagesのSettings → Pagesで、SourceがDeployブランチ`gh-pages`に設定されているか確認

git push -f origin gh-pages

- 2-3分待ってから再度アクセス### 手順

# mainブランチに戻る

git checkout main

```

### ページが表示されるが真っ白#### 1. ビルド

## 関連リンク



- **本番サイト**: https://j1921604.github.io/todo-app/

- **GitHubリポジトリ**: https://github.com/J1921604/todo-app- `vite.config.ts`の`base`設定が`/todo-app/`になっているか確認```powershell

- **GitHub Actions**: https://github.com/J1921604/todo-app/actions

- **GitHub Pages設定**: https://github.com/J1921604/todo-app/settings/pages- ビルドを再実行してから再度デプロイnpm run build



## 注意事項```



- デプロイ前に必ずテストを実行してください（`npm test`）### assetsフォルダが見つからない

- mainブランチへの直接pushは自動デプロイをトリガーします

- デプロイには2-5分程度かかります#### 2. デプロイ

- gh-pagesブランチは自動生成されるため、直接編集しないでください

- `dist/`フォルダの構造を確認

- `index.html`内のassets参照パスが正しいか確認```powershell

npm run deploy

## 更新時のデプロイ```



アプリケーションを更新した後は、上記の手順1-5を再度実行してください。#### 3. 確認



## 注意事項数分後、以下のURLでアプリが公開されます:

```

- `gh-pages`ブランチは本番環境用です。直接編集しないでくださいhttps://j1921604.github.io/todo-app/

- すべての開発は`main`ブランチで行ってください```

- デプロイ前に必ずテストが通ることを確認してください（`npm test`）

---

## 初回セットアップ

### GitHub Pagesの有効化

1. GitHubリポジトリページを開く
2. 「Settings」→「Pages」
3. 以下を設定:
   - **Source**: Deploy from a branch
   - **Branch**: `gh-pages`
   - **Folder**: `/(root)`
4. 「Save」をクリック

### GitHub Actionsの有効化

1. リポジトリの「Actions」タブを開く
2. 「I understand my workflows, go ahead and enable them」をクリック
3. `.github/workflows/deploy.yml` が有効化されることを確認

### 権限設定（必要な場合）

GitHub Actionsがリポジトリに書き込みできるように設定:

1. 「Settings」→「Actions」→「General」
2. 「Workflow permissions」セクションで:
   - **Read and write permissions** を選択
   - 「Save」をクリック

---

## トラブルシューティング

### Q: GitHub Actionsが失敗する

**A: ログを確認して原因を特定**

1. 「Actions」タブでワークフローをクリック
2. 失敗したステップのログを確認
3. よくある原因:
   - テストの失敗 → `npm test -- --run` をローカルで実行して修正
   - ビルドエラー → `npm run build` をローカルで実行して修正
   - 権限エラー → Settings → Actions → Workflow permissions を確認

### Q: GitHub Pagesにアクセスできない（404エラー）

**A: 以下を確認**

1. GitHub Pages設定が正しいか確認（Settings → Pages）
2. `gh-pages` ブランチが存在するか確認
3. デプロイ完了後、5-10分待ってから再アクセス
4. URLが正しいか確認: `https://j1921604.github.io/todo-app/`

### Q: ページは表示されるが、リソースが404になる

**A: ベースパスの設定を確認**

`vite.config.ts` の `base` が正しく設定されているか確認:

```typescript
base: process.env.NODE_ENV === 'production' ? '/todo-app/' : '/'
```

### Q: ローカルでは動くが、デプロイ後に動かない

**A: 本番ビルドをローカルでテスト**

```powershell
# 本番ビルドを実行
npm run build

# ビルドされたファイルをプレビュー
npm run preview
```

`http://localhost:4173/todo-app/` でアクセスして動作確認。

### Q: デプロイブランチを削除したい

**A: 以下のコマンドで削除**

```powershell
# ローカルのブランチを削除
git branch -d deploy

# リモートのブランチを削除
git push origin --delete deploy
```

---

## デプロイフロー図

```mermaid
graph LR
    A[コード変更] --> B[git commit]
    B --> C[git push origin deploy]
    C --> D[GitHub Actions起動]
    D --> E[npm ci]
    E --> F[npm test]
    F --> G[npm run build]
    G --> H[Deploy to gh-pages]
    H --> I[GitHub Pages更新]
    I --> J[https://j1921604.github.io/todo-app/]
    
    style A fill:#e1f5ff
    style D fill:#ffe1e1
    style J fill:#e1ffe1
```

---

## 参考情報

### 関連ドキュメント
- [plan.md](../specs/feature/impl-001-todo-app/plan.md) - 技術アーキテクチャ
- [quickstart.md](../specs/feature/impl-001-todo-app/quickstart.md) - 開発環境構築
- [README.md](../README.md) - プロジェクト概要

### 外部リンク
- [GitHub Pages公式ドキュメント](https://docs.github.com/ja/pages)
- [GitHub Actions公式ドキュメント](https://docs.github.com/ja/actions)
- [Vite公式ドキュメント - Static Deploy](https://vitejs.dev/guide/static-deploy.html)
- [gh-pages NPMパッケージ](https://www.npmjs.com/package/gh-pages)
