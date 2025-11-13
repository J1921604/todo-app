# GitHub Pagesデプロイ手順

## 目次
1. [自動デプロイ（推奨）](#自動デプロイ推奨)
2. [手動デプロイ](#手動デプロイ)
3. [初回セットアップ](#初回セットアップ)
4. [トラブルシューティング](#トラブルシューティング)

---

## 自動デプロイ（推奨）

### 前提条件
- GitHubリポジトリ: `https://github.com/J1921604/todo-app`
- GitHub Actions が有効化されている

### 手順

#### 1. deployブランチの確認と切り替え

```powershell
# 現在のブランチを確認
git branch

# deployブランチに切り替え
git checkout deploy
```

**重要**: deployブランチは既に作成済みです。このブランチにpushすると、GitHub Actionsが自動的に：
1. テストを実行
2. ビルドを実行
3. **gh-pagesブランチを自動作成**
4. ビルド結果（dist/）をgh-pagesブランチにデプロイ

#### 2. 変更をプッシュ（必要な場合のみ）

```powershell
# 変更がある場合のみ
git add .
git commit -m "feat: Update todo app"

# GitHubにプッシュ（自動デプロイが開始される）
git push origin deploy
```

**注意**: 既にpush済みの場合は「Everything up-to-date」と表示されます。

#### 3. GitHub Actionsの確認

1. GitHubリポジトリページを開く: https://github.com/J1921604/todo-app
2. 「Actions」タブをクリック
3. 「Deploy to GitHub Pages」ワークフローを確認
4. 最新のワークフロー実行をクリック
5. すべてのステップが成功（✅ グリーンチェック）になるまで待つ（通常2-3分）

**ワークフローが実行されているはずの手順**:
- ✅ Checkout
- ✅ Setup Node.js
- ✅ Install dependencies
- ✅ Run tests
- ✅ Build
- ✅ Deploy to GitHub Pages（これがgh-pagesブランチを自動作成）

**トラブルシューティング**:
- ワークフローが表示されない場合 → Actionsが有効化されているか確認
- ワークフローが失敗している場合 → ログを確認して原因を特定

#### 4. GitHub Pagesの設定（初回のみ）

1. リポジトリの「Settings」タブを開く
2. 左サイドバーから「Pages」をクリック
3. **Source**セクションで:
   - **Source**: Deploy from a branch
   - **Branch**: `gh-pages` / `/(root)`
   - **Save**ボタンをクリック

#### 5. デプロイ完了確認

数分後、以下のURLでアプリが公開されます:

```
https://j1921604.github.io/todo-app/
```

ブラウザでアクセスして動作確認を行ってください。

---

## 手動デプロイ

### 前提条件
- Node.js 16以上
- npm 8以上
- `gh-pages` パッケージがインストール済み

### 手順

#### 1. ビルド

```powershell
npm run build
```

#### 2. デプロイ

```powershell
npm run deploy
```

#### 3. 確認

数分後、以下のURLでアプリが公開されます:
```
https://j1921604.github.io/todo-app/
```

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
