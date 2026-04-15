# Project Name

<!-- TODO: プロジェクト名に置き換える -->

> このリポジトリは [ResearchTemplate](https://github.com/YutoShimmyo/ResearchTemplate) から生成されました。

<!-- TODO: プロジェクトの1行説明 -->

この README は、人間と AI の両方が最初に読む運用仕様書です。
ここを読めば、少なくとも次が分かる状態にします。

1. この repo は何をする場所か
2. top-level directory tree と各ディレクトリの責務
3. 命名規則
4. 環境管理ルール
5. docs 更新ルール

## Project Overview

<!-- TODO: このリポジトリの目的を書く。2〜3 行。 -->

## Scope

この repo で扱うもの:

<!-- TODO: 箇条書きで列挙 -->

この repo で扱わないもの:

<!-- TODO: 箇条書きで列挙 -->

## Top-level Directory Tree

```text
.
├── README.md
├── AGENTS.md
├── pyproject.toml
├── uv.lock
├── apps/                  ← (optional) アプリ開発がある場合に追加
├── configs/
├── data/
├── docs/
│   ├── implementation/
│   ├── meeting/
│   ├── planning/
│   ├── research/
│   └── testing/
├── papers/
├── references/
├── results/
├── runs/
├── scripts/
├── src/
└── tests/
```

## Directory Responsibilities

### `configs/`

- 実験設定、解析設定、run 用 config を置く

### `data/`

- 研究データ整理用ディレクトリ
- `raw/` — 生データ（gitignore 済み、大きいデータはここ）
- `interim/` — 中間処理結果（gitignore 済み）
- `processed/` — 最終処理済みデータ
- `external_dataset/` — 外部データセット（gitignore 済み）

### `docs/`

repo 運用で最も重要な文書群。下記の責務分割を守る。

### `docs/implementation/`

- 設計、実装状態、実装進捗を置く
- `architecture/` — 設計メモ、schema 定義
- `progress/` — 実装進捗の時系列ログ
- `handoff/` — AI 向け handoff、agent backlog、継続メモ
- `current_system_state.md` — 今なにが動いているかの正本

### `docs/meeting/`

- `agenda/` — 会議前の準備メモ
- `minutes/` — 会議後の議事録

### `docs/planning/`

- 将来やることを固定ファイルで管理する
- `future_required.md` — 必須タスク
- `future_optional.md` — 任意タスク

### `docs/research/`

- `notes/` — 研究メモ、評価設計、実験アイデア
- `review/` — 文献レビュー、関連研究整理
- `progress/` — 研究進捗の時系列ログ

### `docs/testing/`

- 今やる確認だけを置く。終わったら整理し、永遠に肥大化させない
- `active_manual_checks.md` — ユーザが手動で行う確認
- `agent_checks.md` — AI Agent が自走でできる確認

### `papers/`

- 論文本文、図の素材、執筆支援メモを置く

### `references/`

- PDF や参考資料そのものを置く
- 研究メモの置き場ではない（それは `docs/research/` へ）

### `results/`

- 実験結果の図表や集計結果を置く

### `runs/`

- 実験 run ごとの出力を置く

### `scripts/`

- 実行スクリプトや補助ツール

### `src/`

- Python 側の本体ロジック
- `analysis/` — 解析コード
- `methods/` — 手法実装
- `preprocessing/` — 前処理
- `utils/` — ユーティリティ

### `tests/`

- テストコード

## Naming Conventions

### Files

- 日付付き文書: `YYYYMMDD_slug.md`（例: `20260413_research_direction.md`）
- 固定運用ファイル: `snake_case.md`（例: `current_system_state.md`）
- Python ファイル: `snake_case.py`

### Directories

- 単数形を基本にする（`meeting`, `implementation`, `research`, `testing`, `planning`）
- 役割が名前だけで分かるものを優先する
- 略語だけの名前は避ける

### General rules

- `.DS_Store` など不要ファイルは repo に残さない
- 日付だけのファイル名は避け、可能なら slug を付ける

## Environment Management

Python 側は `uv` を正規フローとします。

- 環境同期: `uv sync`
- コマンド実行: `uv run python ...` / `uv run pytest`
- 正本: `pyproject.toml` + `uv.lock`

`pip install -r ...` や ad-hoc な仮想環境運用は正規フローにしません。

<!-- TODO: Android, Node.js など追加の環境があればここに追記 -->

## Documentation Update Rules

### 会議前

- `docs/meeting/agenda/YYYYMMDD_slug.md` を更新する

### 会議後

- `docs/meeting/minutes/YYYYMMDD_slug.md` を更新する

### 研究方針や評価法を考えた時

- `docs/research/notes/` に書く

### 文献整理をした時

- `docs/research/review/` に書く

### 実装した時

最低限更新する場所:

- `docs/implementation/current_system_state.md`
- `docs/testing/agent_checks.md` — 新しい確認項目の追加 / 既存項目のステータス更新
- `docs/testing/active_manual_checks.md` — ユーザ確認が必要なら追記
- 必要なら `docs/implementation/architecture/`

### 大きめの実装をした時

- `docs/implementation/progress/YYYYMMDD_slug.md` に進捗エントリを書く

### 研究進捗を残す時

- `docs/research/progress/`

### 将来の必須・任意タスクを整理する時

- `docs/planning/future_required.md`
- `docs/planning/future_optional.md`

### AI がセッションを終了する時

必ず更新する場所（詳細は `AGENTS.md` を参照）:

- `docs/implementation/handoff/agent_handoff_prompt.md` — 現状と次のアクション
- `docs/implementation/handoff/agent_backlog.md` — 残タスク
- `docs/testing/agent_checks.md` — 確認ステータス
- `docs/implementation/current_system_state.md` — 実装変更があれば

## First Files to Read

### 人間向け

1. `README.md`
2. `docs/implementation/current_system_state.md`
3. `docs/planning/future_required.md`

### AI / Agent 向け

1. `README.md`
2. `AGENTS.md`
3. `docs/implementation/current_system_state.md`
4. `docs/testing/active_manual_checks.md`
5. `docs/testing/agent_checks.md`
6. `docs/planning/future_required.md`
7. `docs/planning/future_optional.md`
8. `docs/implementation/handoff/agent_handoff_prompt.md`
9. `docs/implementation/handoff/agent_backlog.md`
