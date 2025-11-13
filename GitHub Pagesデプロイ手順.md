# GitHub Pagesデプロイ手順

**プロジェクト**: Todo App  
**リポジトリ**: https://github.com/J1921604/todo-app  
**デプロイURL**: https://j1921604.github.io/todo-app/

---

## 📋 目次

1. [自動デプロイ（GitHub Actions）](#自動デプロイgithub-actions)
2. [手動デプロイ（ローカル）](#手動デプロイローカル)
3. [初回設定](#初回設定)
4. [トラブルシューティング](#トラブルシューティング)
5. [デプロイ検証](#デプロイ検証)

---

## 🤖 自動デプロイ（GitHub Actions）

### 前提条件

- GitHubリポジトリが作成済み
- リポジトリのSettings → Pagesが有効化済み

### 自動デプロイの仕組み

GitHub Actionsワークフローが以下のブランチへのプッシュを検知して、自動的にビルド・デプロイを実行します：

- `main` ブランチ
- `deploy` ブランチ

### ワークフローの実行手順

#### 1. コードをプッシュ

```powershell
# 変更をコミット
git add .
git commit -m "feat: 新機能を追加"

# mainまたはdeployブランチにプッシュ
git push origin main
# または
git push origin deploy
```

#### 2. GitHub Actionsの確認

1. GitHubリポジトリのページを開く
2. 「Actions」タブをクリック
3. 最新のワークフロー実行を確認

ワークフローは以下のステップを実行します：

```mermaid
flowchart LR
    A[コードチェックアウト] --> B[Node.js セットアップ]
    B --> C[依存関係インストール]
    C --> D[テスト実行]
    D --> E{テスト成功?}
    E -->|はい| F[ビルド実行]
    E -->|いいえ| G[❌ デプロイ中止]
    F --> H[成果物アップロード]
    H --> I[GitHub Pagesへデプロイ]
    I --> J[✅ デプロイ完了]
    
    style A fill:#e1f5ff
    style D fill:#fff4e1
    style F fill:#e1ffe1
    style I fill:#f5e1ff
    style J fill:#e1fff4
    style G fill:#ffe1e1
```

#### 3. デプロイ完了の確認

- ワークフローが正常に完了すると、GitHub PagesのURLが更新されます
- デプロイURL: https://j1921604.github.io/todo-app/
- 通常、2-5分でデプロイが完了します

### 手動トリガー（workflow_dispatch）

GitHub UIから手動でワークフローを実行できます：

1. GitHubリポジトリの「Actions」タブを開く
2. 左側のメニューから「Deploy to GitHub Pages」を選択
3. 「Run workflow」ボタンをクリック
4. ブランチを選択して「Run workflow」を実行

---

## 🖥️ 手動デプロイ（ローカル）

### 前提条件

- Node.js 18以上がインストール済み
- GitHubリポジトリへのプッシュ権限がある

### 手動デプロイ手順

#### 1. 依存関係のインストール

```powershell
npm install
```

#### 2. テストの実行

```powershell
npm test -- --run
```

すべてのテストが通過することを確認してください。

#### 3. ビルドの実行

```powershell
npm run build
```

`dist/` ディレクトリにビルド成果物が生成されます。

#### 4. ローカルでプレビュー（オプション）

```powershell
npm run preview
```

http://localhost:4173 でビルド成果物をプレビューできます。

#### 5. 手動デプロイ（gh-pagesブランチ）

```powershell
# gh-pagesライブラリを使用してデプロイ
npx gh-pages -d dist -b gh-pages
```

このコマンドは以下を実行します：

1. `dist/` ディレクトリの内容を `gh-pages` ブランチにコミット
2. GitHubリポジトリの `gh-pages` ブランチにプッシュ
3. GitHub Pagesが自動的に更新を検知してデプロイ

---

## ⚙️ 初回設定

### 1. GitHubリポジトリの作成

```powershell
# 既存のリポジトリをクローン
git clone https://github.com/J1921604/todo-app.git
cd todo-app

# または、新規リポジトリを作成
git init
git remote add origin https://github.com/J1921604/todo-app.git
```

### 2. GitHub Pages設定の有効化

#### 方法A: GitHub UI から設定（推奨）

1. GitHubリポジトリのページを開く
2. 「Settings」タブをクリック
3. 左側のメニューから「Pages」を選択
4. 「Source」で「GitHub Actions」を選択
5. 保存

#### 方法B: リポジトリ設定ファイル（.github/workflows/deploy.yml）

すでに作成されたワークフローファイルが以下の設定を含んでいます：

```yaml
permissions:
  contents: read
  pages: write
  id-token: write
```

### 3. vite.config.ts のbase設定確認

`vite.config.ts` で正しいベースパスが設定されていることを確認：

```typescript
export default defineConfig({
  base: process.env.NODE_ENV === 'production' ? '/todo-app/' : '/',
  // ...
})
```

**重要**: リポジトリ名が `todo-app` と一致していることを確認してください。

### 4. 初回デプロイ

```powershell
# mainブランチにプッシュして自動デプロイをトリガー
git add .
git commit -m "chore: initial deployment setup"
git push origin main
```

---

## 🔧 トラブルシューティング

### 問題1: デプロイ後、ページが404エラーになる

**原因**: `base` パスの設定が間違っている

**解決策**:
1. `vite.config.ts` の `base` 設定を確認
2. リポジトリ名と一致しているか確認
3. リポジトリ名が `todo-app` の場合、`base: '/todo-app/'` とする

### 問題2: GitHub Actionsワークフローが失敗する

**原因**: テストが失敗している、またはビルドエラー

**解決策**:
1. ローカルでテストを実行: `npm test -- --run`
2. ローカルでビルドを実行: `npm run build`
3. エラーメッセージを確認して修正
4. 再度プッシュ

### 問題3: デプロイは成功したが、最新の変更が反映されない

**原因**: ブラウザキャッシュ、またはGitHub Pagesの更新遅延

**解決策**:
1. ブラウザのキャッシュをクリア（Ctrl+Shift+R）
2. 5-10分待ってから再度確認
3. シークレットモードで開いて確認

### 問題4: GitHub Pagesの設定が見つからない

**原因**: リポジトリが公開設定になっていない、または権限不足

**解決策**:
1. リポジトリが「Public」になっているか確認
2. Settings → Pagesが表示されない場合、リポジトリの可視性を「Public」に変更
3. 組織のリポジトリの場合、GitHub Pages機能が有効化されているか管理者に確認

### 問題5: `gh-pages` ブランチへのプッシュに失敗する

**原因**: 権限不足、またはブランチ保護ルール

**解決策**:
1. GitHubリポジトリへのプッシュ権限があるか確認
2. Settings → Branches で `gh-pages` ブランチの保護ルールを確認
3. 必要に応じて保護ルールを無効化または例外を追加

---

## ✅ デプロイ検証

### 自動デプロイの検証手順

1. **ワークフローの成功確認**
   - GitHub Actions タブで最新のワークフローが緑色のチェックマークになっているか確認

2. **デプロイURLへのアクセス**
   - https://j1921604.github.io/todo-app/ を開く
   - ホームページが正しく表示されるか確認

3. **機能の動作確認**
   - サイドバーからページを追加
   - タスクの追加・完了切り替え・削除が動作するか確認
   - フィルター機能が動作するか確認
   - ページをリロードしてデータが保持されるか確認

4. **ブラウザコンソールのエラー確認**
   - F12を押して開発者ツールを開く
   - Consoleタブでエラーがないか確認

### 手動デプロイの検証手順

1. **ビルドの成功確認**
   ```powershell
   npm run build
   # エラーがないことを確認
   ```

2. **ローカルプレビュー**
   ```powershell
   npm run preview
   ```
   - http://localhost:4173 で動作確認

3. **デプロイ実行**
   ```powershell
   npx gh-pages -d dist -b gh-pages
   ```
   - 正常に完了したことを確認

4. **デプロイ後の動作確認**
   - 上記「自動デプロイの検証手順」のステップ2-4と同じ

---

## 📊 デプロイステータス

### 現在の設定

| 項目 | 設定値 |
|------|--------|
| リポジトリ | https://github.com/J1921604/todo-app |
| デプロイURL | https://j1921604.github.io/todo-app/ |
| 自動デプロイ | ✅ 有効（main, deploy ブランチ） |
| テスト実行 | ✅ デプロイ前に自動実行 |
| ベースパス | `/todo-app/` |
| Node.jsバージョン | 18 |

### デプロイ履歴の確認

GitHubリポジトリの「Actions」タブで過去のデプロイ履歴を確認できます：

- 成功したデプロイ: 緑色のチェックマーク ✅
- 失敗したデプロイ: 赤色のXマーク ❌
- 実行中のデプロイ: 黄色の円 🟡

---

## 🚀 デプロイのベストプラクティス

### 1. デプロイ前のチェックリスト

- [ ] すべてのテストが通過している
- [ ] ローカルでビルドが成功している
- [ ] コードレビューが完了している
- [ ] 変更内容をコミットメッセージに明記している

### 2. ブランチ戦略

- `main`: 本番環境に反映される安定版
- `deploy`: デプロイテスト用（必要に応じて）
- `feature/*`: 機能開発用（自動デプロイされない）

### 3. デプロイのタイミング

- 新機能の実装完了後
- バグ修正後
- ドキュメント更新後

### 4. ロールバック手順

問題が発生した場合、以前のコミットに戻す：

```powershell
# 前回のコミットに戻す
git revert HEAD
git push origin main

# 特定のコミットに戻す
git revert <commit-hash>
git push origin main
```

---

## 📚 関連ドキュメント

- [GitHub Pages 公式ドキュメント](https://docs.github.com/ja/pages)
- [GitHub Actions 公式ドキュメント](https://docs.github.com/ja/actions)
- [Vite デプロイガイド](https://ja.vitejs.dev/guide/static-deploy.html)
- [プロジェクトREADME](../README.md)

---

## 🆘 サポート

デプロイに関する問題が発生した場合：

1. このドキュメントのトラブルシューティングセクションを確認
2. GitHub Actionsのログを確認
3. ローカルでビルド・テストを実行してエラーを特定
4. 必要に応じて開発チームに相談

---

**最終更新**: 2025年11月13日  
**作成者**: GitHub Copilot  
**バージョン**: 1.0.0
