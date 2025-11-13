# GitHub Pages デプロイ手順書

## 目次

1. [デプロイ先URL](#デプロイ先url)
2. [自動デプロイの仕組み](#自動デプロイの仕組み)
3. [デプロイ手順](#デプロイ手順)
4. [デプロイ確認方法](#デプロイ確認方法)
5. [トラブルシューティング](#トラブルシューティング)
6. [手動デプロイ](#手動デプロイ緊急時)
7. [設定ファイル詳細](#設定ファイル詳細)

---

## デプロイ先URL

**本番環境：** https://j1921604.github.io/todo-app/

**テストページ：** https://j1921604.github.io/todo-app/test.html

---

## 自動デプロイの仕組み

### 概要

このプロジェクトは、**GitHub Actions**を使用してGitHub Pagesに自動デプロイされます。

### トリガー条件

以下のいずれかで自動デプロイが開始されます：

1. **mainブランチへのpush**
   ```bash
   git add .
   git commit -m "feat: Add new feature"
   git push origin main
   ```

2. **GitHub ActionsのUIから手動実行**
   - https://github.com/J1921604/todo-app/actions
   - "Deploy to GitHub Pages"ワークフローを選択
   - "Run workflow"ボタンをクリック

### ワークフローの流れ

`.github/workflows/deploy.yml`で定義されているステップ：

1. **Checkout** (約5秒)
   - リポジトリのコードをチェックアウト
   - 使用: `actions/checkout@v4`

2. **Setup Node.js** (約10秒)
   - Node.js 18をインストール
   - 使用: `actions/setup-node@v4`

3. **Install dependencies** (約30秒)
   - `npm install`を実行
   - すべての依存パッケージをインストール

4. **Run tests** (約5-10秒)
   - `npm test -- --run`を実行
   - **106個のテスト**が実行される
   - **失敗時はデプロイ中断**

5. **Build** (約5-10秒)
   - `npm run build`を実行
   - TypeScriptのコンパイル
   - Viteによる本番ビルド
   - `dist/`フォルダに成果物を生成

6. **Add .nojekyll** (約1秒)
   - `dist/.nojekyll`ファイルを作成
   - GitHub PagesでJekyll処理を無効化

7. **Deploy to GitHub Pages** (約10秒)
   - `peaceiris/actions-gh-pages@v3`を使用
   - `dist/`フォルダの内容を`gh-pages`ブランチにデプロイ
   - 強制的に孤立ブランチとして作成（`force_orphan: true`）

**合計所要時間：** 約1-2分

---

## デプロイ手順

### 通常の開発フロー

```bash
# 1. 機能を実装
# (コードを編集)

# 2. ローカルでテスト
npm test

# 3. ローカルでビルド確認
npm run build

# 4. 開発サーバーで動作確認
npm run dev
# → http://localhost:1234 で確認

# 5. 変更をコミット
git add .
git commit -m "feat: 新機能を追加"

# 6. mainブランチにpush（自動デプロイ開始）
git push origin main

# 7. GitHub Actionsの進捗を確認
# → https://github.com/J1921604/todo-app/actions

# 8. デプロイ完了後、本番サイトで確認
# → https://j1921604.github.io/todo-app/
```

### 緊急時の素早いデプロイ

テストをスキップする場合（**非推奨**）：

```bash
# ワークフローファイルを一時的に編集
# .github/workflows/deploy.yml の"Run tests"ステップをコメントアウト

git add .github/workflows/deploy.yml
git commit -m "chore: Temporarily skip tests"
git push origin main

# デプロイ完了後、元に戻す
```

---

## デプロイ確認方法

### 1. GitHub Actionsの実行状態を確認

**URL:** https://github.com/J1921604/todo-app/actions

**確認項目：**

- ✅ **成功（緑色のチェック）** → デプロイ完了
- ❌ **失敗（赤色のX）** → エラーあり（ログを確認）
- 🟡 **実行中（黄色の丸）** → 処理中（1-2分待つ）
- ⏸️ **待機中（灰色）** → キューに追加済み

**詳細ログの見方：**

1. 失敗したワークフローをクリック
2. "deploy"ジョブをクリック
3. 各ステップを展開してエラーメッセージを確認

### 2. gh-pagesブランチを確認

**ブランチ一覧：** https://github.com/J1921604/todo-app/branches

**gh-pagesブランチの内容：**

```
gh-pages/
├── .nojekyll          # Jekyll無効化ファイル
├── index.html         # メインHTML
├── test.html          # テストページ
└── assets/
    ├── index-[hash].js   # JavaScriptバンドル
    └── index-[hash].css  # CSSファイル
```

### 3. デプロイされたサイトを確認

**デプロイ完了後の待機時間：** 2-3分

**確認手順：**

```bash
# 1. メインページにアクセス
https://j1921604.github.io/todo-app/

# 2. テストページで基本動作確認
https://j1921604.github.io/todo-app/test.html

# 3. ブラウザのデベロッパーツールを開く（F12）
# - Consoleタブでエラー確認
# - Networkタブで404エラー確認

# 4. 強制リロード（キャッシュクリア）
# Ctrl + Shift + R (Windows)
# Cmd + Shift + R (Mac)
```

---

## トラブルシューティング

### 問題1: 404エラー "File not found"

**症状：**
```
404
File not found
The site configured at this address does not contain the requested file.
```

**原因と解決策：**

#### 原因1: GitHub Pages設定が未完了

**確認方法：**
1. https://github.com/J1921604/todo-app/settings/pages を開く
2. 以下を確認：
   - Source: "Deploy from a branch"
   - Branch: "gh-pages"
   - Folder: "/ (root)"

**解決策：**
1. Branchを"gh-pages"に設定
2. Folderを"/ (root)"に設定
3. "Save"をクリック
4. 2-3分待つ

#### 原因2: gh-pagesブランチが空

**確認方法：**
```bash
git fetch origin gh-pages
git checkout gh-pages
ls
```

**解決策：**
```bash
git checkout main
git push origin main  # 再デプロイをトリガー
```

### 問題2: ページが真っ白

**症状：**
- ページは開くが何も表示されない
- 404エラーは出ていない

**原因と解決策：**

#### 原因1: JavaScriptのエラー

**確認方法：**
1. F12キーでデベロッパーツールを開く
2. Consoleタブを確認
3. 赤いエラーメッセージを探す

**よくあるエラー：**
```
Failed to load module script: Expected a JavaScript module script but the server responded with a MIME type of "text/html"
```

**解決策：**
- `.nojekyll`ファイルが存在するか確認
- gh-pagesブランチに`touch .nojekyll`を追加

#### 原因2: assetsフォルダが見つからない

**確認方法：**
```bash
git checkout gh-pages
ls assets/
```

**解決策：**
- ビルドを再実行してデプロイ

### 問題3: GitHub Actionsが失敗

#### エラー1: "Tests failed"

**ログ例：**
```
✗ tests/unit/utils/performance.test.ts
AssertionError: expected 9.93 to be greater than or equal to 10
```

**解決策：**
```bash
# ローカルでテストを実行
npm test

# 失敗したテストを修正
# tests/unit/utils/performance.test.ts を編集

# 再度テスト
npm test

# コミット & プッシュ
git add .
git commit -m "fix: Fix flaky performance test"
git push origin main
```

#### エラー2: "Build failed"

**ログ例：**
```
error TS2304: Cannot find name 'React'
```

**解決策：**
```bash
# ローカルでビルド
npm run build

# エラーを修正
# TypeScriptのエラーを解決

# 再度ビルド
npm run build

# コミット & プッシュ
git add .
git commit -m "fix: Fix TypeScript errors"
git push origin main
```

#### エラー3: "Dependencies lock file is not found"

**解決策：**
- すでに`.github/workflows/deploy.yml`で`npm install`を使用するよう修正済み
- もし再発した場合は、ワークフローファイルを確認

### 問題4: キャッシュの問題

**症状：**
- 最新の変更が反映されない
- 古いバージョンが表示される

**解決策：**

```bash
# 1. 強制リロード
Ctrl + Shift + R (Windows)
Cmd + Shift + R (Mac)

# 2. キャッシュをクリア
# Chromeの場合：
# - F12 → Network タブ → "Disable cache"にチェック
# - ページをリロード

# 3. シークレットモードで開く
Ctrl + Shift + N (Chrome)
Ctrl + Shift + P (Firefox)

# 4. 別のブラウザで確認
# Edge、Firefox、Safariなど
```

---

## 手動デプロイ（緊急時）

GitHub Actionsが使用できない場合の手動デプロイ手順：

### 前提条件

- Node.js 18以上がインストール済み
- リポジトリへのプッシュ権限あり

### 手順

```bash
# 1. mainブランチで最新のコードを取得
git checkout main
git pull origin main

# 2. 依存関係をインストール
npm install

# 3. テストを実行（任意）
npm test

# 4. プロダクションビルドを実行
npm run build

# 5. ビルド成果物を一時フォルダにバックアップ
mkdir -p ../todo-app-dist-backup
cp -r dist/* ../todo-app-dist-backup/

# 6. gh-pagesブランチに切り替え
git checkout gh-pages

# 7. gh-pagesブランチの最新を取得
git pull origin gh-pages

# 8. 既存のファイルを削除（.gitフォルダは除く）
rm -rf assets index.html test.html

# 9. ビルド成果物をコピー
cp -r ../todo-app-dist-backup/* .

# 10. .nojekyllファイルを作成
touch .nojekyll

# 11. 変更をステージング
git add .

# 12. コミット
git commit -m "Manual deployment on $(date '+%Y-%m-%d %H:%M:%S')"

# 13. プッシュ
git push origin gh-pages

# 14. mainブランチに戻る
git checkout main

# 15. 一時フォルダを削除
rm -rf ../todo-app-dist-backup

# 16. 2-3分待ってからサイトを確認
# https://j1921604.github.io/todo-app/
```

### PowerShell版（Windows）

```powershell
# 1-4は同じ

# 5. ビルド成果物を一時フォルダにバックアップ
New-Item -Path ..\todo-app-dist-backup -ItemType Directory -Force
Copy-Item -Path dist\* -Destination ..\todo-app-dist-backup\ -Recurse -Force

# 6-7は同じ

# 8. 既存のファイルを削除
Remove-Item -Path assets,index.html,test.html -Recurse -Force -ErrorAction SilentlyContinue

# 9. ビルド成果物をコピー
Copy-Item -Path ..\todo-app-dist-backup\* -Destination . -Recurse -Force

# 10. .nojekyllファイルを作成
New-Item -Path .nojekyll -ItemType File -Force

# 11-14は同じ

# 15. 一時フォルダを削除
Remove-Item -Path ..\todo-app-dist-backup -Recurse -Force

# 16は同じ
```

---

## 設定ファイル詳細

### .github/workflows/deploy.yml

GitHub Actionsのワークフロー定義ファイル

**重要な設定：**

```yaml
on:
  push:
    branches:
      - main  # mainブランチへのpushでトリガー
  workflow_dispatch:  # 手動実行を許可

permissions:
  contents: write  # gh-pagesブランチへの書き込み権限

jobs:
  deploy:
    runs-on: ubuntu-latest  # Ubuntu環境で実行
```

**peaceiris/actions-gh-pages@v3の設定：**

```yaml
- name: Deploy to GitHub Pages
  uses: peaceiris/actions-gh-pages@v3
  with:
    github_token: ${{ secrets.GITHUB_TOKEN }}  # 自動提供されるトークン
    publish_dir: ./dist  # デプロイするディレクトリ
    publish_branch: gh-pages  # デプロイ先ブランチ
    force_orphan: true  # 履歴を残さない孤立ブランチとして作成
```

### vite.config.ts

Viteのビルド設定

**重要な設定：**

```typescript
export default defineConfig({
  base: process.env.NODE_ENV === 'production' 
    ? '/todo-app/'  // GitHub Pagesのサブパス
    : '/',          // ローカル開発用
  
  plugins: [react()],
  
  build: {
    outDir: 'dist',  // ビルド出力先
    sourcemap: false  // ソースマップを生成しない
  }
})
```

**base設定の重要性：**

- GitHub Pagesは`https://j1921604.github.io/todo-app/`というサブパスで公開される
- `base: '/todo-app/'`を設定しないと、assetsのパスが`/assets/...`となり404エラーになる
- 正しく設定すると、`/todo-app/assets/...`となり正常に読み込まれる

### package.json

**スクリプト：**

```json
{
  "scripts": {
    "dev": "vite --port 1234",        // 開発サーバー起動
    "build": "tsc && vite build",     // TypeScript + Viteビルド
    "preview": "vite preview",        // ビルド結果をプレビュー
    "test": "vitest",                 // テスト実行（watch mode）
    "test:watch": "vitest --watch",   // 明示的なwatch mode
    "test:coverage": "vitest --coverage",  // カバレッジ計測
    "type-check": "tsc --noEmit"      // 型チェックのみ
  }
}
```

---

## よくある質問（FAQ）

### Q1: デプロイにどのくらい時間がかかりますか？

**A:** 
- GitHub Actionsの実行：1-2分
- GitHub Pagesの反映：2-3分
- **合計：3-5分程度**

### Q2: デプロイを取り消すことはできますか？

**A:** 
はい、以前のコミットに戻すことで可能です：

```bash
# gh-pagesブランチで
git checkout gh-pages
git log  # コミット履歴を確認
git reset --hard <前のコミットハッシュ>
git push -f origin gh-pages
```

### Q3: 複数のブランチを同時にデプロイできますか？

**A:** 
いいえ、GitHub Pagesは1つのブランチのみです。ただし、以下の方法で複数環境を作ることは可能：

- 別のリポジトリを作成（例：todo-app-dev）
- Vercel、Netlifyなどの他のホスティングサービスを併用

### Q4: デプロイ時にテストをスキップできますか？

**A:** 
可能ですが**非推奨**です。緊急時のみ：

```yaml
# .github/workflows/deploy.yml
# "Run tests"ステップをコメントアウト
# - name: Run tests
#   run: npm test -- --run
```

### Q5: ローカルでビルド結果をプレビューできますか？

**A:** 
はい：

```bash
npm run build
npm run preview
# → http://localhost:4173 でプレビュー
```

---

## 参考リンク

- **リポジトリ：** https://github.com/J1921604/todo-app
- **GitHub Actions：** https://github.com/J1921604/todo-app/actions
- **GitHub Pages設定：** https://github.com/J1921604/todo-app/settings/pages
- **デプロイ先：** https://j1921604.github.io/todo-app/
- **テストページ：** https://j1921604.github.io/todo-app/test.html

### 公式ドキュメント

- **GitHub Actions：** https://docs.github.com/en/actions
- **GitHub Pages：** https://docs.github.com/en/pages
- **Vite：** https://vitejs.dev/
- **Vitest：** https://vitest.dev/
- **React：** https://react.dev/

