# GitHub Pages設定手順

## ✅ 完了済み項目

1. ✅ GitHub Actionsワークフロー作成済み (`.github/workflows/deploy.yml`)
2. ✅ Vite設定でGitHub Pagesのbaseパス設定済み (`/todo-app/`)
3. ✅ gh-pagesブランチ存在確認済み（リモートに存在）
4. ✅ mainブランチにプッシュ済み（自動デプロイトリガー）

## 🔧 手動でGitHub Pages設定が必要な手順

以下の手順でGitHub Pagesを有効化してください：

### ステップ1: GitHubリポジトリにアクセス

https://github.com/J1921604/todo-app

### ステップ2: Settings（設定）タブをクリック

リポジトリのトップページで「Settings」タブをクリック

### ステップ3: Pagesを選択

左サイドバーから「Pages」を選択

### ステップ4: ソース設定

**Build and deployment** セクションで以下を設定：

- **Source**: `Deploy from a branch`
- **Branch**: `gh-pages` を選択
- **Folder**: `/ (root)` を選択
- **Save** ボタンをクリック

### ステップ5: Actions権限設定（必要な場合）

もし権限エラーが発生した場合：

1. Settings > Actions > General に移動
2. **Workflow permissions** セクションを見つける
3. **Read and write permissions** を選択
4. **Allow GitHub Actions to create and approve pull requests** にチェック
5. **Save** をクリック

### ステップ6: デプロイ確認

1. **Actions** タブをクリック
2. 最新のワークフロー実行を確認
3. ✅ 緑色のチェックマーク = デプロイ成功
4. ❌ 赤色のXマーク = エラー（ログを確認）

### ステップ7: サイトにアクセス

約3-5分後、以下のURLでアクセス可能になります：

https://j1921604.github.io/todo-app/

---

## 🔍 トラブルシューティング

### 問題: 404エラーが表示される

**解決策**:
1. Settings > Pages で `gh-pages` ブランチが選択されているか確認
2. Actions タブでワークフローが成功しているか確認
3. 5-10分待ってから再アクセス

### 問題: GitHub Actionsが失敗する

**解決策**:
1. Actions タブでエラーログを確認
2. テストが失敗している場合: `npm test -- --run` でローカル確認
3. ビルドが失敗している場合: `npm run build` でローカル確認

### 問題: CSSが読み込まれない

**解決策**:
`vite.config.ts` の `base` パスを確認：
```typescript
base: process.env.NODE_ENV === 'production' ? '/todo-app/' : '/'
```

---

## 📝 現在のデプロイ状況

- **GitHub Actions**: 自動デプロイ設定完了 ✅
- **ワークフロー**: `.github/workflows/deploy.yml` 作成済み ✅
- **gh-pagesブランチ**: リモートに存在 ✅
- **最新コミット**: プッシュ済み ✅

**次のステップ**: 上記の手動設定を実施してGitHub Pagesを有効化してください。

---

## 🚀 今後のデプロイ方法

設定完了後は、以下のコマンドだけでデプロイできます：

```powershell
git add .
git commit -m "feat: 新機能を追加"
git push origin main
```

約3-5分後、自動的にGitHub Pagesに反映されます。

詳細は `GitHub Pagesデプロイ手順.md` を参照してください。
