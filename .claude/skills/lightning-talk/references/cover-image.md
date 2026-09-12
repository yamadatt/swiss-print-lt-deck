# 表紙の背景画像（`s-title`）

既定の表紙は**写真なし**です。3pxの罫とタイポグラフィだけで組んであり、そのままで成立します。
写真を敷きたい場合は、この案の規律（グラデーション禁止）に合わせた専用の手順を使ってください。

## この案で `bg-shade` を使わない理由

ダークテーマのテンプレートは、写真の上に黒のグラデーションを重ねて文字の可読性を確保します。
Swiss Print は明るい紙色（`#F2F0EB`）が地で、**グラデーション・影・角丸を一切使わない**のが規律なので、この手は使えません。
代わりに、**写真の上に紙色のベタ面（`.plate`）を置き、その中にタイトルを組みます**。印刷物で写真に白いプレートを載せるのと同じやり方です。

## 手順

1. 画像を出力フォルダの `img/` に置く（例: `img/cover.jpg`）。1920×1080以上、横長。
2. `s-title` の `<section>` に `class="on-photo"` を足す。
3. コメントアウトされている `<img class="bg-photo">` を有効にし、`src` を差し替える。
4. `.frame` 直下のタイトルを包む `<div>` を `<div class="plate">` に変える。
5. 出典表記が要る素材なら `<div class="bg-credit">` を `.plate` の中に足す。

```html
<section class="s-title on-photo" data-screen-label="01 Title">
  <img class="bg-photo" src="img/cover.jpg" alt="">
  <div class="frame">
    <div class="topbar">
      <div class="kicker anim">Lightning Talk</div>
      <div class="kicker anim">5 min</div>
    </div>
    <div class="plate">
      <div class="hr anim d1"></div>
      <div class="lede anim d1">技術トークの<br><em>タイトル</em>をここに</div>
      <div class="meta anim d2"><b>あなたの名前</b><span>@handle</span><span>2026.xx.xx</span></div>
      <div class="bg-credit">Photo: 出典 / ライセンス</div>
    </div>
  </div>
</section>
```

- `.plate` は画面下部に置かれる紙色のベタ面です。**タイトルは必ずこの中に置く**（写真の上に直接文字を置かない）。
- `.on-photo` は `.frame::before`（12カラムのグリッド）を消します。写真の上にグリッドを重ねると汚れて見えるためです。
- `.topbar`（`Lightning Talk` / `5 min`）だけは写真の上に残り、`mix-blend-mode: difference` で明暗が自動反転します。**写真の上端が中間調（グレー一色）だと読みにくくなる**ので、その場合は `.topbar` も `.plate` の中に移してください。

## 画像選びの注意

- ライセンスに注意。商用・登壇可否を確認できる素材（Unsplash / Pexels / Wikimedia Commons など）か、自分で用意した画像を使う。生成画像を使う場合もその旨を意識する。
- トーク内容に合った画像にする。関係ない「とりあえずかっこいい」画像は避ける。
- `.plate` が画面下部を覆うので、**被写体は上半分〜中央**に来る構図を選ぶ。
- 同じ `.bg-photo` は章扉（`s-section`）でも使い回せますが、`s-section` は `.idx`（大きな章番号）が写真と競合します。写真を敷くなら `.idx` を削ってください。
