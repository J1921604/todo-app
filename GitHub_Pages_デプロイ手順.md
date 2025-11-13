# GitHub Pagesデプロイ手順# GitHub Pagesデプロイ手順



このドキュメントでは、Todo AppをGitHub Pagesに手動でデプロイする方法を説明します。## 目次

1. [自動デプロイ（推奨）](#自動デプロイ推奨)

## 前提条件2. [手動デプロイ](#手動デプロイ)

3. [初回セットアップ](#初回セットアップ)

- GitHubアカウント4. [トラブルシューティング](#トラブルシューティング)

- リポジトリへのプッシュ権限

- Node.js 18以上のインストール---



## デプロイ手順## 自動デプロイ（推奨）



### 1. 最新のコードを取得### 前提条件

- GitHubリポジトリ: `https://github.com/J1921604/todo-app`

```powershell- GitHub Actions が有効化されている

git checkout main

git pull origin main### 手順

```

#### 1. deployブランチの確認と切り替え

### 2. プロダクションビルドを実行

```powershell

```powershell# 現在のブランチを確認

npm installgit branch

npm run build

```# deployブランチに切り替え

git checkout deploy

ビルドが成功すると、`dist/`フォルダに以下のファイルが生成されます:```

- `index.html`

- `assets/index-[hash].js`**重要**: deployブランチは既に作成済みです。このブランチにpushすると、GitHub Actionsが自動的に：

- `assets/index-[hash].css`1. テストを実行

2. ビルドを実行

### 3. gh-pagesブランチを作成・更新3. **gh-pagesブランチを自動作成**

4. ビルド結果（dist/）をgh-pagesブランチにデプロイ

```powershell

# 一時フォルダにビルド成果物をコピー#### 2. 変更をプッシュ（必要な場合のみ）

Copy-Item -Path dist\* -Destination ..\temp-dist -Recurse -Force

```powershell

# gh-pagesブランチに切り替え（初回は新規作成）# 変更がある場合のみ

git checkout --orphan gh-pagesgit add .

# または既存の場合: git checkout gh-pagesgit commit -m "feat: Update todo app"



# すべてのファイルを削除# GitHubにプッシュ（自動デプロイが開始される）

git rm -rf .git push origin deploy

```

# ビルド成果物をコピー

Copy-Item -Path ..\temp-dist\index.html -Destination . -Force**注意**: 既にpush済みの場合は「Everything up-to-date」と表示されます。

New-Item -Path assets -ItemType Directory -Force

Copy-Item -Path ..\temp-dist\assets\* -Destination assets\ -Force#### 3. GitHub Actionsの確認



# 一時フォルダを削除1. GitHubリポジトリページを開く: https://github.com/J1921604/todo-app

Remove-Item -Path ..\temp-dist -Recurse -Force2. 「Actions」タブをクリック

```3. 「Deploy to GitHub Pages」ワークフローを確認

4. 最新のワークフロー実行をクリック

### 4. 変更をコミットしてプッシュ5. すべてのステップが成功（✅ グリーンチェック）になるまで待つ（通常2-3分）



```powershell**ワークフローが実行されているはずの手順**:

git add .- ✅ Checkout

git commit -m "Deploy to GitHub Pages"- ✅ Setup Node.js

git push -f origin gh-pages- ✅ Install dependencies

```- ✅ Run tests

- ✅ Build

### 5. mainブランチに戻る- ✅ Deploy to GitHub Pages（これがgh-pagesブランチを自動作成）



```powershell**トラブルシューティング**:

git checkout main- ワークフローが表示されない場合 → Actionsが有効化されているか確認

```- ワークフローが失敗している場合 → ログを確認して原因を特定



### 6. GitHub Pagesの設定を確認#### 4. GitHub Pagesの設定（初回のみ）



初回デプロイ時のみ必要です:1. リポジトリの「Settings」タブを開く

2. 左サイドバーから「Pages」をクリック

1. GitHubリポジトリページ（https://github.com/J1921604/todo-app）を開く3. **Source**セクションで:

2. 「Settings」タブをクリック   - **Source**: Deploy from a branch

3. 左サイドバーから「Pages」を選択   - **Branch**: `gh-pages` / `/(root)`

4. 「Source」セクションで以下を設定:   - **Save**ボタンをクリック

   - Source: `Deploy from a branch`

   - Branch: `gh-pages`#### 5. デプロイ完了確認

   - Folder: `/ (root)`

5. 「Save」をクリック数分後、以下のURLでアプリが公開されます:



### 7. デプロイ完了の確認```

https://j1921604.github.io/todo-app/

2-3分待ってから、以下のURLにアクセス:```



```ブラウザでアクセスして動作確認を行ってください。

https://j1921604.github.io/todo-app/

```---



アプリケーションが正常に表示されれば、デプロイ完了です。## 手動デプロイ



## トラブルシューティング### 前提条件

- Node.js 16以上

### ページが404エラーになる- npm 8以上

- `gh-pages` パッケージがインストール済み

- GitHub PagesのSettings → Pagesで、SourceがDeployブランチ`gh-pages`に設定されているか確認

- 2-3分待ってから再度アクセス### 手順



### ページが表示されるが真っ白#### 1. ビルド



- `vite.config.ts`の`base`設定が`/todo-app/`になっているか確認```powershell

- ビルドを再実行してから再度デプロイnpm run build

```

### assetsフォルダが見つからない

#### 2. デプロイ

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
