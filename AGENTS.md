# AGENTS.md

<!-- TODO: プロジェクト概要を1行で書く -->

このリポジトリで AI Agent（Codex, Cursor, etc.）が作業する時は、単にコードを足すのではなく、`何を作るか`, `どう確認するか`, `どう文書を更新するか` まで含めて進める。

## 基本方針

- raw-first を守る。観測した事実を保存し、特徴量や要約は後で `processed` や `exports` に作る。
- raw の保存形式は基本 `JSONL` にする。
- nested JSON は最小限に抑える。トップレベルに平らに置けるものは平らに置く。
- event-driven で取れるものは polling より event-driven を優先する。

## 実装時のルール

- 大きめの実装をしたら、`docs/implementation/current_system_state.md` を更新する。
- 将来の作業が増えたら、`docs/planning/future_required.md` か `docs/planning/future_optional.md` に振り分ける。
- 自分が離席中に進めるための作業は `docs/implementation/handoff/agent_backlog.md` に書く。
- ユーザがやるべき確認と、Agent が自走でできる確認は分けて管理する。
- 実装したら、自分でできる確認は必ず先に実行する（build, test, lint 等）。
- 実装して期待どおりの出力が出ない場合は、そのまま放置せず、原因を詰めて修正し、所望の出力が出るまで `実装 → 確認 → 修正` を回す。
- 人間の操作が必要な確認だけを `docs/testing/active_manual_checks.md` に残す。
- マジックナンバーは直接散らさない。閾値、周期、設定値は定数として名前付きで置く。
- fallback や除外対象を入れる時は、理由が分かる名前で定数化する。

## テスト運用

- Agent ができる確認:
  - `uv run pytest`
  - `uv run python scripts/...`
  - build / lint / 型チェック
  - 出力ファイルの存在・中身の確認
- ユーザがやる確認:
  - 実機操作が必要なもの
  - 外部サービスとの連携確認
  - 主観的な品質判断

## まず見るファイル

- repo 全体の案内:
  - `README.md`
- 現在の実装状態:
  - `docs/implementation/current_system_state.md`
- ユーザがやる手動テスト:
  - `docs/testing/active_manual_checks.md`
- Agent が自走でやる確認:
  - `docs/testing/agent_checks.md`
- 必須の将来実装:
  - `docs/planning/future_required.md`
- optional の将来実装:
  - `docs/planning/future_optional.md`
- 離席中の継続作業:
  - `docs/implementation/handoff/agent_backlog.md`

## リポジトリ整理方針

- `docs/implementation/` は「実装状態・設計・handoff」を置く
- `docs/implementation/architecture/` は設計や保存 schema を置く
- `docs/implementation/progress/` は実装進捗を置く
- `docs/implementation/handoff/` は AI 継続作業文書を置く
- `docs/testing/` は「今やる確認」を置く
- `docs/planning/` は「これからやること」を置く
- `docs/research/notes/` は研究メモを置く
- `docs/research/review/` は文献レビューを置く
- `docs/research/progress/` は研究進捗を置く
- `docs/meeting/agenda/` は会議前メモを置く
- `docs/meeting/minutes/` は議事録を置く
- 古い日付ベースのメモは残してよいが、永続更新するファイルは上記の固定ファイル名に寄せる

## Project-Specific Rules

<!-- TODO: プロジェクト固有のルールをここに追記する -->
<!-- 例:
- Android collector 固有のルール
- 特定の API / SDK の使い方
- データ収集・保存に関する制約
-->
