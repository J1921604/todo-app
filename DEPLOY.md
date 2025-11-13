# GitHub Pages デプロイ手順書

## デプロイ先

https://j1921604.github.io/todo-app/

## 自動デプロイ

mainブランチにpushすると、GitHub Actionsが自動的に以下を実行します：

1. 依存関係のインストール（npm install）
2. テストの実行（npm test）
3. ビルドの実行（npm run build）
4. gh-pagesブランチへのデプロイ

## デプロイ確認

### GitHub Actionsの確認

https://github.com/J1921604/todo-app/actions

ワークフローが成功（✅）していることを確認してください。

### サイトの確認

デプロイ完了後、2-3分待ってからアクセス：

https://j1921604.github.io/todo-app/

## トラブルシューティング

### 404エラーが表示される

**GitHub Pages設定を確認：**

1. https://github.com/J1921604/todo-app/settings/pages を開く
2. Source: "Deploy from a branch"
3. Branch: "gh-pages" / "/ (root)"
4. Saveをクリック

### ページが真っ白

**ブラウザコンソールを確認：**

1. F12キーを押してデベロッパーツールを開く
2. Consoleタブでエラーを確認
3. Networkタブで404エラーがないか確認

**キャッシュをクリア：**

- Ctrl + Shift + R（強制リロード）
- シークレットモードで開く

### GitHub Actionsが失敗

**ログを確認：**

1. https://github.com/J1921604/todo-app/actions を開く
2. 失敗したワークフローをクリック
3. エラーメッセージを確認

**よくあるエラー：**

- テスト失敗 → ローカルで `npm test` を実行して修正
- ビルド失敗 → ローカルで `npm run build` を実行して修正

## 設定ファイル

### .github/workflows/deploy.yml

GitHub Actionsのワークフロー設定

### vite.config.ts

```typescript
base: process.env.NODE_ENV === 'production' ? '/todo-app/' : '/'
```

GitHub Pagesのサブパス設定

## 手動デプロイ（緊急時）

```bash
# ビルド
npm install
npm run build

# gh-pagesブランチに切り替え
git checkout gh-pages

# 古いファイルを削除
rm -rf *
git checkout HEAD -- .git

# ビルド成果物をコピー
cp -r ../dist-backup/* .
touch .nojekyll

# プッシュ
git add .
git commit -m "Manual deploy"
git push origin gh-pages

# mainブランチに戻る
git checkout main
```
