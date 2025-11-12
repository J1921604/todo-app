# 実装計画: Todo App - template-no-delete.tsx ベースアプリケーション# Implementation Plan: [FEATURE]



**ブランチ**: `feature/impl-001-todo-app` | **日付**: 2025-11-13 | **仕様**: [specs/001-todo-app-spec/spec.md](../001-todo-app-spec/spec.md)  **Branch**: `[###-feature-name]` | **Date**: [DATE] | **Spec**: [link]

**入力**: 機能仕様書 `/specs/001-todo-app-spec/spec.md`**Input**: Feature specification from `/specs/[###-feature-name]/spec.md`



**注記**: この実装計画は `/speckit.plan` コマンドによって生成されました。**Note**: This template is filled in by the `/speckit.plan` command. See `.specify/templates/commands/plan.md` for the execution workflow.



## 概要## Summary



**主要要件**: 開発者が自分専用のTodoページを作成し、タスクの追加・完了切り替え・フィルタリング・削除ができるReact+TypeScriptアプリケーション。LocalStorageによるデータ永続化、ワンコマンド起動（start.ps1）、GitHub Pagesデプロイをサポート。[Extract from feature spec: primary requirement + technical approach from research]



**技術アプローチ**: ## Technical Context

- React 18.2.0のHooksベース（useState、useEffect）で状態管理

- TypeScript 4.9.3による型安全性確保<!--

- Vite 4.2.0による高速ビルドとHMR  ACTION REQUIRED: Replace the content in this section with the technical details

- Vitest 0.34.0によるテスト駆動開発（100%カバレッジ目標）  for the project. The structure here is presented in advisory capacity to guide

- LocalStorageによるクライアントサイド永続化  the iteration process.

- UIkit 3.16.10によるUIコンポーネント-->

- GitHub Pagesによる静的サイトホスティング

**Language/Version**: [e.g., Python 3.11, Swift 5.9, Rust 1.75 or NEEDS CLARIFICATION]  

## 技術コンテキスト**Primary Dependencies**: [e.g., FastAPI, UIKit, LLVM or NEEDS CLARIFICATION]  

**Storage**: [if applicable, e.g., PostgreSQL, CoreData, files or N/A]  

**言語/バージョン**: TypeScript 4.9.3、JavaScript ES2020  **Testing**: [e.g., pytest, XCTest, cargo test or NEEDS CLARIFICATION]  

**主要依存関係**: React 18.2.0、React Router 6.10.0、UIkit 3.16.10、Vite 4.2.0、Vitest 0.34.0  **Target Platform**: [e.g., Linux server, iOS 15+, WASM or NEEDS CLARIFICATION]

**ストレージ**: LocalStorage（ブラウザネイティブAPI、5MB制限）  **Project Type**: [single/web/mobile - determines source structure]  

**テスト**: Vitest 0.34.0 + @testing-library/react 14.1.2 + happy-dom 12.10.3  **Performance Goals**: [domain-specific, e.g., 1000 req/s, 10k lines/sec, 60 fps or NEEDS CLARIFICATION]  

**ターゲットプラットフォーム**: モダンブラウザ（Chrome、Firefox、Safari、Edge最新版）、GitHub Pages（静的サイトホスティング）  **Constraints**: [domain-specific, e.g., <200ms p95, <100MB memory, offline-capable or NEEDS CLARIFICATION]  

**プロジェクトタイプ**: Web Application（Single Page Application）  **Scale/Scope**: [domain-specific, e.g., 10k users, 1M LOC, 50 screens or NEEDS CLARIFICATION]

**パフォーマンス目標**: 

- 初期ページ読み込み: < 2秒## Constitution Check

- タスク追加・削除応答: < 100ms

- フィルタリング処理: < 1秒（10,000タスクまで）*GATE: Must pass before Phase 0 research. Re-check after Phase 1 design.*

- テストスイート実行: < 12秒（106テスト）

[Gates determined based on constitution file]

**制約条件**: 

- LocalStorage容量制限: 5MB（ブラウザ依存）## Project Structure

- ページ追加・編集・削除後はサーバー再起動が必要（userPages.ts手動編集のため）

- クライアントサイドのみ（APIサーバーなし）### Documentation (this feature)

- 削除されたページのタスクデータは復元不可

```text

**規模/スコープ**: specs/[###-feature]/

- ユーザーストーリー: 4つ（P1/P2/P3優先度付き）├── plan.md              # This file (/speckit.plan command output)

- 機能要件: 10要件（FR-001〜FR-010）├── research.md          # Phase 0 output (/speckit.plan command)

- テストケース: 106テスト（100%カバレッジ目標）├── data-model.md        # Phase 1 output (/speckit.plan command)

- コンポーネント数: 約15コンポーネント（Atomic Design原則）├── quickstart.md        # Phase 1 output (/speckit.plan command)

- 想定ページ数: 最大100ページ├── contracts/           # Phase 1 output (/speckit.plan command)

- 想定タスク数: 10,000タスク/ページで動作保証└── tasks.md             # Phase 2 output (/speckit.tasks command - NOT created by /speckit.plan)

```

## 憲法チェック

### Source Code (repository root)

*ゲート: Phase 0 研究前に合格必須。Phase 1 設計後に再チェック。*<!--

  ACTION REQUIRED: Replace the placeholder tree below with the concrete layout

### ✅ I. テスト駆動開発の徹底  for this feature. Delete unused options and expand the chosen structure with

  real paths (e.g., apps/admin, packages/something). The delivered plan must

| 要件 | ステータス | 詳細 |  not include Option labels.

|-----|----------|------|-->

| テストファースト | ✅ 合格 | テストコードを先に作成し、Red-Green-Refactorサイクルを実施 |

| 仕様対応テスト | ✅ 合格 | 機能要件（FR-001〜FR-010）すべてに対応するテストケース106件 |```text

| カバレッジ目標 | ✅ 合格 | 100%カバレッジ（単体テスト+統合テスト） |# [REMOVE IF UNUSED] Option 1: Single project (DEFAULT)

src/

**検証方法**: Vitestで`npm run test`実行、カバレッジレポート生成、すべてのテストがパスすることを確認├── models/

├── services/

### ✅ II. セキュリティファーストの原則├── cli/

└── lib/

| 要件 | ステータス | 詳細 |

|-----|----------|------|tests/

| 機密データ保護 | ✅ 合格 | LocalStorageには機密情報を保存しない（Todoタスクテキストのみ） |├── contract/

| XSS対策 | ✅ 合格 | Reactのデフォルトエスケープ機能に依存、`dangerouslySetInnerHTML`使用禁止 |├── integration/

| 入力バリデーション | ✅ 合格 | 空文字チェック、trim()処理、文字列長制限（タスク名500文字、ページ名50文字） |└── unit/

| HTTPS | ✅ 合格 | GitHub Pages自動HTTPS提供 |

| 依存関係脆弱性 | ✅ 合格 | `npm audit`を定期実行、脆弱性修正 |# [REMOVE IF UNUSED] Option 2: Web application (when "frontend" + "backend" detected)

backend/

**検証方法**: セキュリティレビューチェックリスト、npm audit実行、入力バリデーションテスト├── src/

│   ├── models/

### ✅ III. パフォーマンス定量化の原則│   ├── services/

│   └── api/

| 要件 | ステータス | 詳細 |└── tests/

|-----|----------|------|

| 成功基準定義 | ✅ 合格 | 10の測定可能な成功基準（SC-001〜SC-010）を定義 |frontend/

| パフォーマンス監視 | ✅ 合格 | Chrome DevTools Performance、Lighthouse監視 |├── src/

| 目標達成 | ⏳ Phase 1後確認 | 初期読み込み < 2秒、タスク操作 < 100ms、フィルタリング < 1秒 |│   ├── components/

│   ├── pages/

**検証方法**: Lighthouse スコア、Chrome DevTools Performance プロファイリング、成功基準検証テスト│   └── services/

└── tests/

### 制約チェック

# [REMOVE IF UNUSED] Option 3: Mobile + API (when "iOS/Android" detected)

| 制約項目 | 現状 | 合格基準 | ステータス |api/

|---------|------|---------|----------|└── [same as backend above]

| CS-001: プロジェクト数 | 1プロジェクト | ≤ 3 | ✅ 合格 |

| CS-002: 依存関係数 | 主要6依存関係 | ≤ 10 | ✅ 合格 |ios/ or android/

| CS-003: 抽象化レイヤー | 2層（components, utils） | ≤ 3 | ✅ 合格 |└── [platform-specific structure: feature modules, UI flows, platform tests]

| CS-007: ビルド時間 | < 30秒 | ≤ 5分 | ✅ 合格 |```

| CS-008: テスト実行時間 | < 12秒 | ≤ 1分 | ✅ 合格 |

**Structure Decision**: [Document the selected structure and reference the real

**総合判定**: ✅ すべての憲法要件を満たしていますdirectories captured above]



## プロジェクト構造## Complexity Tracking



### ドキュメント（この機能）> **Fill ONLY if Constitution Check has violations that must be justified**



```text| Violation | Why Needed | Simpler Alternative Rejected Because |

specs/feature/impl-001-todo-app/|-----------|------------|-------------------------------------|

├── plan.md              # このファイル（/speckit.plan コマンド出力）| [e.g., 4th project] | [current need] | [why 3 projects insufficient] |

├── research.md          # Phase 0 出力（/speckit.plan コマンド）| [e.g., Repository pattern] | [specific problem] | [why direct DB access insufficient] |

├── data-model.md        # Phase 1 出力（/speckit.plan コマンド）
├── quickstart.md        # Phase 1 出力（/speckit.plan コマンド）
├── contracts/           # Phase 1 出力（/speckit.plan コマンド） - LocalStorageベースのためN/A
└── tasks.md             # Phase 2 出力（/speckit.tasks コマンド - /speckit.planでは作成しない）
```

### ソースコード（リポジトリルート）

```text
todo-app/
├── src/
│   ├── App.tsx                    # メインアプリケーションコンポーネント
│   ├── main.tsx                   # エントリーポイント
│   ├── index.css                  # グローバルスタイル
│   ├── vite-env.d.ts             # Vite型定義
│   ├── components/
│   │   ├── atoms/                 # Atomic Design: 原子コンポーネント
│   │   │   ├── Button/
│   │   │   │   ├── index.tsx     # ボタンエクスポート
│   │   │   │   ├── Middle.tsx    # 中サイズボタン
│   │   │   │   └── Small.tsx     # 小サイズボタン
│   │   │   └── Input/
│   │   │       ├── index.tsx     # 入力エクスポート
│   │   │       └── Text.tsx      # テキスト入力
│   │   └── organisms/             # Atomic Design: 有機体コンポーネント
│   │       └── Sidebar.tsx        # サイドバー（ページ管理）
│   ├── config/
│   │   └── userPages.ts           # ユーザーページ設定
│   ├── pages/
│   │   ├── HomePage.tsx           # ホームページ
│   │   ├── DynamicTodoPage.tsx    # 動的Todoページ
│   │   └── TestUserTodo.tsx       # テストユーザーページ
│   ├── types/
│   │   └── todo.ts                # TodoItem、FilterType型定義
│   └── utils/
│       ├── localStorage.ts         # LocalStorage操作ユーティリティ
│       └── performance.ts          # パフォーマンス監視ユーティリティ
├── tests/
│   ├── setup.ts                   # テストセットアップ
│   ├── unit/
│   │   ├── components/            # コンポーネント単体テスト
│   │   │   ├── App.test.tsx
│   │   │   ├── DynamicTodoPage.test.tsx
│   │   │   ├── HomePage.test.tsx
│   │   │   ├── Sidebar.test.tsx
│   │   │   ├── TaskInput.test.tsx
│   │   │   └── TaskItem.test.tsx
│   │   ├── config/
│   │   │   └── userPages.test.ts
│   │   ├── types/
│   │   │   └── todo.test.ts
│   │   └── utils/
│   │       ├── localStorage.test.ts
│   │       └── performance.test.ts
│   └── integration/               # 統合テスト
│       ├── bulk-deletion.test.tsx
│       ├── data-persistence.test.tsx
│       ├── edge-cases.test.tsx
│       ├── personal-page-setup.test.tsx
│       ├── task-deletion.test.tsx
│       ├── task-filtering.test.tsx
│       └── task-operations.test.tsx
├── public/                        # 静的アセット
├── dist/                          # ビルド出力（.gitignore）
├── node_modules/                  # 依存関係（.gitignore）
├── package.json                   # npm設定
├── tsconfig.json                  # TypeScript設定
├── tsconfig.node.json             # Node.js用TypeScript設定
├── vite.config.ts                 # Vite設定
├── vitest.config.ts               # Vitest設定
├── start.ps1                      # ワンコマンド起動スクリプト
└── README.md                      # プロジェクト説明
```

**構造決定**: 既存のプロジェクト構造を活用し、Atomic Design原則（atoms、organisms）とReact Hooksベースのコンポーネント設計を採用。LocalStorageベースのため、バックエンドAPIは不要。

## 複雑性追跡

> **憲法チェック違反があり、正当化が必要な場合のみ記入**

現時点で憲法違反はありません。すべての制約を満たしています。

---

**バージョン**: 1.0.0  
**最終更新**: 2025-11-13  
**ステータス**: Phase 0 準備完了（研究フェーズへ進む）
