# AGENTS.md

<!-- TODO: プロジェクト概要を1行で書く -->

このリポジトリで AI Agent（Codex, Cursor, etc.）が作業する時は、単にコードを足すのではなく、`何を作るか`, `どう確認するか`, `どう文書を更新するか` まで含めて進める。

## 基本方針

- raw-first を守る。観測した事実を保存し、特徴量や要約は後で `processed` や `exports` に作る。
- raw の保存形式は基本 `JSONL` にする。
- nested JSON は最小限に抑える。トップレベルに平らに置けるものは平らに置く。
- event-driven で取れるものは polling より event-driven を優先する。

## セッション開始時にやること

新しいセッションを開始したら、以下の順に読む:

1. `README.md` — repo 全体の案内
2. `AGENTS.md` — この文書（ルール確認）
3. `docs/implementation/handoff/agent_handoff_prompt.md` — 前セッションからの引き継ぎ
4. `docs/implementation/current_system_state.md` — 現在の実装状態
5. `docs/implementation/handoff/agent_backlog.md` — 自走でやれるタスク
6. `docs/testing/agent_checks.md` — 自動確認の状況
7. `docs/planning/future_required.md` — 必須タスク

前セッションの引き継ぎがある場合は **handoff を最優先で読む**。

## 設計→実装の順序

新しい機能・コンポーネント・データパイプラインを作る前に:

1. 入出力の仕様を決める（保存先パス、ファイル形式、1 行の schema）
2. 必要なら `docs/implementation/architecture/` に設計メモを残す
3. 確認方法（Agent が自走でできるもの / ユーザが手動でやるもの）を先に決める
4. その上で実装に入る

設計なしにいきなりコードを書かない。

## 実装時のルール

- 大きめの実装をしたら、`docs/implementation/current_system_state.md` を更新する。
- 大きめの実装をしたら、`docs/implementation/progress/YYYYMMDD_slug.md` に進捗エントリを書く。
- 新しい確認項目があれば、`docs/testing/agent_checks.md` に追記する。
- 人間の操作が必要な確認は `docs/testing/active_manual_checks.md` に追記する。
- 将来の作業が増えたら、`docs/planning/future_required.md` か `docs/planning/future_optional.md` に振り分ける。
- 自分が離席中に進めるための作業は `docs/implementation/handoff/agent_backlog.md` に書く。
- ユーザがやるべき確認と、Agent が自走でできる確認は分けて管理する。
- マジックナンバーは直接散らさない。閾値、周期、設定値は定数として名前付きで置く。
- fallback や除外対象を入れる時は、理由が分かる名前で定数化する。

## テスト・確認ループ

- 実装したら、自分でできる確認は必ず先に実行する（build, test, lint 等）。
- 実装して期待どおりの出力が出ない場合は、そのまま放置せず、原因を詰めて修正し、所望の出力が出るまで `実装 → 確認 → 修正` を回す。
- 確認が通ったら `docs/testing/agent_checks.md` のステータスを更新する。

### Agent ができる確認

- `uv run pytest`
- `uv run python scripts/...`
- build / lint / 型チェック
- 出力ファイルの存在・中身の確認

### ユーザがやる確認

- 実機操作が必要なもの
- 外部サービスとの連携確認
- 主観的な品質判断

## 完了定義

実装が「完了」と呼べる条件:

1. Agent ができる確認が全て通っている
2. `docs/testing/agent_checks.md` のステータスが更新されている
3. ユーザ確認が必要なものは `docs/testing/active_manual_checks.md` に手順が書かれている
4. `docs/implementation/current_system_state.md` が最新になっている

全条件を満たすまでは、その実装タスクは未完了とする。

## 失敗時の報告ルール

`実装 → 確認 → 修正` を **3 回** 繰り返しても解決しない場合:

1. 試行を止める
2. 以下を `docs/testing/agent_checks.md` または該当する場所に記録する:
   - 何をやろうとしたか
   - 何が起きたか（エラーメッセージ、ログ）
   - 試した修正とその結果（各試行）
   - 仮説（何が原因と思われるか）
3. ユーザに報告して判断を仰ぐ

黙って放置しない。解決できなかったことは必ず文書に残す。

## 禁止事項

- `main` ブランチへの直接 push（ユーザの明示的な指示がない限り）
- データの削除（`data/`, `runs/`, `results/` 配下）を確認なしに行うこと
- 不可逆な操作（`git push --force`, `git reset --hard` 等）をユーザの確認なしに行うこと
- `.env` やクレデンシャルを含むファイルのコミット
- テスト未実行での「完了」宣言

## セッション終了時にやること

セッションを終了する前に、必ず以下を更新する:

1. `docs/implementation/handoff/agent_handoff_prompt.md` — 現在の状態と次にやることを書く
2. `docs/implementation/handoff/agent_backlog.md` — 残タスクがあれば追記する
3. `docs/testing/agent_checks.md` — 確認のステータスを最新にする
4. `docs/implementation/current_system_state.md` — 実装を変更したなら更新する

次のセッションの Agent が、これらを読むだけで作業を継続できる状態にする。

## まず見るファイル

- repo 全体の案内:
  - `README.md`
- 前セッションからの引き継ぎ:
  - `docs/implementation/handoff/agent_handoff_prompt.md`
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
