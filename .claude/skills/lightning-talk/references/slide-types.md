# スライド種別リファレンス（Swiss Print）

各スライドは自己完結した `<section class="s-...">` です。
`template.html` から該当ブロックをコピーしてください。
登場アニメーションを付ける要素には `anim` を付け、段階的に表示したい場合は `d1`〜`d4` を追加します。

## このデザインの規律

迷ったら次の5点に戻ってください。テンプレート全体がこれで組まれています。

1. **角丸 0 / グラデーション 0 / 影 0 / 絵文字 0。** 面を分けるのは罫線と余白だけ。
2. **アクセント（朱赤 `--accent`）は1枚に1〜2箇所まで。** 「ここだけ見てほしい」場所に使う。それ以外は `--text` と `--muted` で組む。
3. **ラベルに `//` を使わない。** kicker は `06 / KEY POINTS` の形。
4. **タイプスケールの8段（`--type-*`）から外れない。** 新しいサイズを足したくなったら、たいていは情報を削るほうが正しい。スライドは `overflow:hidden` なので、大きいサイズを直に書くと収まらない行が黙って切れる（枠内で切れるため、はみ出し検査には出てこない）。
5. **罫線は4段の濃度階層を守る。** `--grid`(.05) < `--line`(.14) < `--tick` / `--line-2`(.34)。縦罫と横罫を同じ濃さにすると交点が網目になって読めなくなる。

## 種別一覧（`<section>` のCSS class）

| Class | 用途 | 編集する主な項目 |
| --- | --- | --- |
| `s-title` | タイトルスライド（表紙） | `.kicker` 2つ（左＝イベント種別、右＝持ち時間。実際の時間に合わせる）、`.hr`（3pxの区切り罫）、`.lede`（アクセント語は `<em>`）、`.meta`（名前は `<b>`／ハンドル／日付）。背景写真を敷く場合は `references/cover-image.md` 参照 |
| `s-about` | 登壇者の自己紹介 | `.name`（アクセントは `<em>`）、`.role`、`.facts` の各行（`.k` ラベル／`.v` 値）、`<image-slot shape="rect">` 写真枠（固有の `id` を付ける。**この案では角丸にしない**） |
| `s-intro` | はじめに（本題前の導入） | `.lede`（116px の大見出し。アクセントは `<em>`）、`.hr`、`.sub`（背景・課題を2行）。`.lede` は自動折り返しに任せず `<br>` で行を確定させ、各行を**9〜12文字程度**に揃える |
| `s-agenda` | 目次 | `.title`、`.toc` の各 `li`（`.n` 番号、`.t` 章タイトル、`.meta` 右端の補足）。章数に合わせて `li` を増減 |
| `s-section` | 章区切り | `.chapter`（`Chapter 01 / 03` の進行表示）、`.idx`（右上に薄く出る300pxの番号）、`.label`、`.rule`、`.desc`。`.chapter` と `.idx` は自動採番されないので手動で合わせる |
| `s-stat` | 大きな数字・インパクト | `.num`（268px、朱赤）、`.unit`、`.caption`（強調は `<b>`）、`.ctx`。**数字の出典と計測条件を `.ctx` に必ず書く** |
| `s-code` | コード例 | `.head .t`、`.takeaway`、`.fname`、`<pre>` 本文。色は `c-key` / `c-fn` / `c-str` / `c-com` / `c-num`。注目箇所は `c-hl`（左の赤罫）。**`.c-hl` は `display:block` なので、複数行をまとめるときは1つの `.c-hl` の中に改行ごと入れる**（span を並べて間に改行を置くと空の行ボックスが増え、`overflow:hidden` で末尾が切れる） |
| `s-bullets` | 番号付きの3要点 | `.title`、各 `.row` の `.rt`（要点）と `.rd`（詳細） |
| `s-list` | 箇条書き（1〜2階層） | `.blist > li` の `.lead`、必要なら `.sub`。`.mk` は朱赤の正方形マーカー |
| `s-diagram` | フロー・構成図（3ノード） | `.node .nt` のラベル、`.node .ph` の説明。強調ノードに `class="fill"`（上辺が朱赤の4px罫）。矢印は `.arrow`（CSSで描画。グリフではない） |
| `s-aws` | AWS構成図 | `.aws-cloud` 内の各 `.svc`（`.badge` のアイコンSVG、`.sname`、`.sdesc`）。**`.svc.compute` だけバッジを朱赤で塗る＝「自分でコードを書く場所」**。詳細は `references/aws-diagram.md` |
| `s-shot` | スクリーンショット／デモ画面 | `.browser`（`.bar` にURL文字列、`<image-slot>` に画像）、`.notes`（右の注釈列：`.cap` ＋ `<ul>`）。**全幅で見せたい場合は `.notes` を丸ごと削除**。`<image-slot>` には固有の `id` を付ける |
| `s-table` | 比較表 | `<thead>` の各 `<th>`（1列目は空。おすすめ列に `class="pick"` ＋ `<span class="tag">おすすめ</span>`）、`<tbody>` の各行。**`.pick` は塗らず、左右の罫と朱赤の見出し罫で囲って示す**。良い値は `<span class="yes">`。列を増減するときは thead / tbody の `th`・`td` をセットで |
| `s-quote` | 引用・メッセージ | `.hr`（朱赤の短い罫）、`.q`（アクセントは `<em>`）、`.by` の出典 |
| `s-ba` | Before / After（数値の対比） | `.cond`（**比較の前提条件を必ず書く**：計測環境・期間・換算レート）、`.pane.before` / `.pane.after` の `.big` と `<ul>`。After 側だけ上辺が朱赤 |
| `s-cta` | まとめ・CTA | `.takeaways` の各 `li`（`.n`、`.t`、強調は `<b>`）、`.punch`（締めの一言。ハイライトは `<span class="hl">`） |
| `s-steps` | 手順・タイムライン（3〜5ステップ） | `.track`（上を通す1本の罫＝進行）、各 `.step` の `.sn`（`Step 01`。タイムラインなら年月に）、`.st`、`.sd`。現在地のステップに `class="now"`（四角が朱赤で一回り大きくなる）。5段を超えるなら2枚に割る |
| `s-chart` | 簡易棒グラフ（2〜5本） | 各 `.col` の `.val`（値）、`.bar` の `style="height:◯px"`（**最大値の棒を420pxにして比例配分**）、`.lab`（軸ラベル）。強調する棒の `.col` に `class="hot"`（朱赤）。右の `.notes` が不要なら丸ごと削除で全幅 |
| `s-shot2` | スクリーンショット2枚の比較 | 各 `.panel` の `.ptag`（Before / After）、`<image-slot>`（固有の `id`）、`.pcap`。強調側の `.panel` に `class="after"` |
| `s-dont` | Do / Don't | `.pane2.ng` / `.pane2.ok` の `.tag`、`.big`、`<ul>`。**この案では危険色（赤）を使わない**。Don't 側は見出しを `--muted` に落とし、Do 側に朱赤の上辺罫を付けて差をつける |
| `s-photo` | 全画面写真＋一言 | `<image-slot shape="rect">`（固有の `id`。ドラッグ＆ドロップ or `src="img/foo.jpg"`）、`.plate`（**紙色のベタ面。明るい案なのでグラデーションの覆いは使わない**）、`.cap`、`.bg-credit`（出典。不要なら削除） |
| `s-thanks` | 締め（Thank you / Q&A ＋ QR） | `.big`（アクセントは `<em>`）、`.sub`、`.contact` の各行（`.k` / `.v`）、QRの `<image-slot>`。QR不要なら `.qr` 列を丸ごと削除 |

## スライドの追加・削除

- **追加** する場合は、使いたい種別の `<section>` を複製し、適切な位置に置いてください。新しい `<image-slot>` には固有の `id` を付けてください。
- **削除** する場合は、対象の `<section>` 全体を削除してください。
- `.pageno`（`02 / 22` の表示）は `deck-stage.js` が表示時に自動採番するため、**編集不要**です。HTML上のサンプル値はそのままで構いません。
- kickerラベルの番号（`06 / KEY POINTS`）と `s-section` の章番号（`.chapter` / `.idx`）は自動採番されないため、増減後に手動で振り直してください。
