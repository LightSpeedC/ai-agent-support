# ai-agent-support 課題

これから解決すること

> 📅 作成: 2026-09-24 / 更新: 2026-10-05

[⌂](../../README.md)

## 1. ai-chat-lite-reviewer からの指摘（#1092）

<details>
<summary>i260924-01 chat待受けの3時間毎cron設計が、共通ルールとai-chat-liteの実仕様の両方と食い違う ✅ <strong>済</strong></summary>

notes/90_rules/local-rules.html 1章が、3時間ごとにcronで `aichat wait` を新規接続し、接続直後のバックログを読んだら即切断する設計になっている。

- 共通ルール「ai-chat-lite（AI間チャット）の利用」は、張る前に数えて同じルームを2本で見ないと定める。継続待受け（背景の1本）とcronの一時接続が同時に存在すると、この原則に反する。ローカルルールの差分表はこの点への上書き根拠を示していない。
- `aichat wait` は新着があるまで返らない。バックログが出るのは初めてのID・ルームの組み合わせのときだけで、案内を出して終わる作り（CHAT-USAGE.html「どこまで読んだかは覚えている」）。「接続直後にバックログを読んで即切断」という前提が実仕様と違う。
- 指摘に基づき確認済み: `aichat recent -r public --since-hour 3` 等の `recent` コマンドは読み取り専用でIDを名乗る必要がなく、期間指定で新着を取得できる。定期スナップショット用途はこちらに置き換えるのが妥当。

やること: 済（報告元確認 #1102）

</details>

<details>
<summary>i260924-02 root の設定ファイルが CLAUDE.md のまま（共通ルール変更 #1063 未追随） ✅ <strong>済</strong></summary>

共通ルール「プロジェクトフォルダ構成」の「ローカルルール（プロジェクトルール）」が変わり、root の設定ファイルは AGENTS.md に統一された（周知 #1063）。指摘時点では root にあったのは CLAUDE.md のみ。

対応: `CLAUDE.md` を削除し `AGENTS.md`（`@notes/90_rules/local-rules.md` 取り込み＋Codex用コメント）を作成。

やること: 済（報告元確認 #1102）

</details>

<details>
<summary>i260924-03 章見出しに番号が二重に付く（html2md変換時） ✅ <strong>済</strong></summary>

HTML の h1 が「1. 背景と目的」のように番号を持っているため、生成された Markdown が「## 1. 1. 背景と目的」になる。計画2本・調査1本・ローカルルールの4本すべてで発生。 共通ルール「HTMLデザインルール」の「タイトル・見出し」は、章の連番は html2md が振るので HTML 側に番号を書かない、としている。

対応: p260919-01・p260923-01・r260919-01・local-rules.html・issues.html（自身）の各h1から手書き番号を削除。Markdownで二重番号が消えたことを確認。

やること: 済（報告元確認 #1102, #1106）

</details>

<details>
<summary>i260924-04 全体構想とリソース抽象化構想でロードマップが食い違う ✅ <strong>済</strong></summary>

p260919-01-全体構想.html の4章は5が外部連携（Mattermost）・6がチケット管理。p260923-01-リソース抽象化構想.html の3章は5がリソース抽象化基盤・6がチャットアダプタ・7がチケットアダプタ。後者に「全体構想の該当箇所は次回それぞれの計画時に更新する」とあるが、現状どちらが正か読み手には分からない。

対応: p260923-01を正とし、p260919-01の4章のフェーズ5行をp260923-01の3章へのリンク参照に差し替え。p260923-01側の「次回更新する」の記述も「更新済み」に直した。

やること: 済（報告元確認 #1106）

</details>

<details>
<summary>i260924-05 構想中の text CLI が既存の ai-agent-tools の text と名前が衝突する ✅ <strong>済</strong></summary>

p260923-01 の2.8節が text list・text read・text write・text edit を挙げているが、ai-agent-tools の text が既にPATHにあり read・find・edit・write を持つ。同名のものが2つPATHに並ぶと先勝ちで解決され、どちらが動いたか出力から分からない（check-contrastの移行 #1008 で実際に発生）。 節末尾に「ai-agent-tools側と調整する」旨はあるが、名前の衝突そのものには触れていない。あわせて text write --content が既存 text write（--inか第2引数）と渡し方が異なり、text read --find も既存 text find と役割が重なる。

対応（利用者判断）: 別名にはせず、ai-agent-toolsのtextを置き換える想定であることを明記。共存を前提にした並行運用はしない（置き換え完了までの間、両方を同時にPATHへ置かない）旨も明記。read/find/edit/writeの引数は既存textの渡し方（--lines、read/findの分離、--in/第2引数/標準入力、--old/--new・--lines+--digest+--new）にそのまま揃え、--content等の新方式は廃止。--regex/--invertのみ新設の拡張として明記した。

やること: 済（報告元確認 #1106）

</details>

<details>
<summary>i260924-06 p260919-01の実装言語がNode.jsのみでTypeScriptの明記がない ✅ <strong>済</strong></summary>

共通ルール「コーディングルール」の「使用言語（Node / Bun）」に合わせ、TypeScript優先の旨を明記する。

対応: 2.4節にTypeScript優先（ts > mjs > cjs）の旨を追記。

やること: 済（報告元確認 #1106）

</details>

<details>
<summary>i260924-07 README.htmlの更新日が古いまま（09-23作成のp260923-01を追加したのに09-19のまま） ✅ <strong>済</strong></summary>

対応: 更新日を2026-09-24へ更新。

やること: 済（報告元確認 #1106）

</details>

<details>
<summary>i260924-08 README.htmlに親サイト（../）へ戻るリンクが本文先頭・フッターにない ✅ <strong>済</strong></summary>

対応: 本文（.wrap）先頭とフッターの両方に `../` への紺バッジ（⌂）を追加。あわせて資料一覧に課題（issues.html）へのリンクも追加（共通ルール「README がリンクするのは issues.html だけ」）。

やること: 済（報告元確認 #1106）

</details>

<details>
<summary>i260924-09 #記号の意味が2つの資料で重複する（instance_id区切り／行範囲指定） ✅ <strong>済</strong></summary>

p260919-01の2.2節はinstance_idの区切り、p260923-01の2.1.2節は行範囲の指定。どちらも:project-id:と組み合わせて書かれるため紛らわしい。

対応: 記法自体は変えず、p260919-01の2.2節に「この#は区切り文字であり、行範囲指定の#L1-20とは別用法。両者が同じ記述の中で混在することはない」旨の注記を追加。

やること: 済（報告元確認 #1106）

</details>

## 2. 自分で見つけた課題

<details>
<summary>i261004-01 `aichat waiters` が、`timeout` でラップして起動した自分の待受けを二重計上する ✅ <strong>済</strong></summary>

`timeout 7200 aichat wait :ai-agent-support: -p 8787 -r public` を `run_in_background` で起動した状態で `aichat waiters` を実行すると、自分（ai-agent-support）が2本として表示される。

- 1本は張り方 `aichat`、ルーム `public`、pidは実際のaichatプロセス（`ps aux`で確認済み）。
- もう1本は張り方 `bash`、ルーム `public'`（末尾にクォートが混入し壊れている）、pidは`timeout`ラッパーの親bashプロセス（同じく`ps aux`で確認。実際のaichat接続ではない）。
- 同時刻に起動した他プロジェクト（ai-agent-rules等）のaichatプロセスにも同様の親bashが存在するが、そちらは二重計上されていない。自分のIDに対してだけ、ローカルプロセスの自己検出ロジックが働き、`timeout`ラッパーのコマンドライン文字列から誤ってルーム名を抽出していると見られる。
- 共通ルール「ai-chat-lite（AI間チャット）の利用」は「数えるのは aichat waiters。プロセスを自分で検索しない」としており、waiters自体がこの種の誤検出をすると、二重待受けの判定を誤る恐れがある。

対応（ai-chat-lite側）: waitersが末端に数える対象を待受けの実体（aichat・aichat-rs・node・bun）だけに限定し、bash・timeout・pwsh等のラッパーは親子がつながっていなくても数えない形に修正（b126334でpush済み）。手元のaichat waitersでai-agent-supportが1本・張り方aichatのみになり、bash・public'の行が出ないことを確認済み（#1355）。

やること: 済（ai-chat-lite側修正・自分で確認済み。#1346, #1354, #1355）

</details>

[⌂](../../README.md)
