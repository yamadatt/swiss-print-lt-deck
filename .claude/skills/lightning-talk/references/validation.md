# 検証・PDF出力リファレンス

流し込み後の品質チェックと、PDF自動出力の手順です。

## 1. はみ出しの自動チェック（Playwright MCPが使える場合）

1. `browser_navigate` で `file:///<絶対パス>/<slug>.html` を開く。
2. `browser_evaluate` で次のスクリプトを実行し、スライド枠からはみ出している要素を機械検出する。

   ```js
   () => {
     // 意図的に枠外へはみ出す装飾要素は除外する
     const SKIP = '.idx, .bg-photo, .bg-credit, .plate';
     const issues = [];
     document.querySelectorAll('deck-stage > section').forEach((sec, i) => {
       const sr = sec.getBoundingClientRect();
       if (!sr.width || !sr.height) return; // レイアウトされていないスライドはスキップ
       sec.querySelectorAll('*').forEach((el) => {
         if (el.closest(SKIP)) return;
         const r = el.getBoundingClientRect();
         if (!r.width || !r.height) return;
         if (r.right > sr.right + 1 || r.bottom > sr.bottom + 1 ||
             r.left < sr.left - 1 || r.top < sr.top - 1) {
           issues.push(`slide ${i + 1}: <${el.tagName.toLowerCase()} class="${el.className}"> ` +
             `text="${(el.textContent || '').trim().slice(0, 30)}"`);
         }
       });
     });
     return issues.length ? issues : 'OK: はみ出し検出なし';
   }
   ```

   - 非アクティブなスライドが `display:none` でレイアウトされない場合は、`browser_press_key` の `ArrowRight` で1枚ずつ送りながらアクティブスライドに対して同じ検査を繰り返す。
   - 装飾としてあえてはみ出させた要素が誤検出された場合は `SKIP` に追加して再実行する。

3. **全スライドのスクリーンショットを撮って目視確認する。** 機械検出は「枠からのはみ出し」しか拾えない。**不自然な改行位置・1文字だけの行・左右バランス**はスクリーンショットを読んで確認する。`ArrowRight` で1枚ずつ送りながら `browser_take_screenshot` を撮り、全枚数分を確認する。

Playwright MCPが使えない場合は、`open <slug>.html` でブラウザ表示し、全スライドを目視で確認する（矢印キー/Spaceで移動、`R` で先頭へ）。

## 2. 完成チェックリスト（出力フォルダ内で実行）

```bash
# サンプル文言・プレースホルダーの残り（ヒットしたら実内容に置き換える）
grep -nEi 'yourname|@handle|example\.com|あなたの名前|2026\.xx\.xx|SAMPLE|TODO|XXX' <slug>.html

# id の重複（image-slot の複製時に起きやすい。出力が空ならOK）
grep -o 'id="[^"]*"' <slug>.html | sort | uniq -d

# 参照している画像が img/ に実在するか（出力が空ならOK）
grep -o 'src="img/[^"]*"' <slug>.html | sed 's/src="//;s/"$//' | \
  while read p; do [ -f "$p" ] || echo "missing: $p"; done

# 必須ファイルが揃っているか
ls deck-stage.js image-slot.js
```

あわせて次も確認する:

- kicker（`06 / KEY POINTS`）と `s-section` の章番号（`.chapter` / `.idx`）が順序どおりか（`.pageno` は自動採番なので確認不要）。
- 表紙の時間バッジ（`5 MIN` 等）が実際の持ち時間と一致しているか。

## 3. PDF出力

### 自動（headless Chrome）

```bash
"/Applications/Google Chrome.app/Contents/MacOS/Google Chrome" \
  --headless=new --disable-gpu \
  --no-pdf-header-footer \
  --virtual-time-budget=10000 \
  --print-to-pdf="<slug>.pdf" \
  "file:///<絶対パス>/<slug>.html"
```

- `--virtual-time-budget` はWebフォントの読み込み待ちのため。
- 出力後、**ページ数がスライド枚数と一致するか**と、先頭・末尾ページの見た目を必ず確認する（フォント・背景が欠けることがある）。問題があれば手動出力に切り替える。

### 手動（ブラウザの印刷ダイアログ）

ブラウザで印刷 → 「PDFとして保存」を選び、余白なし、背景グラフィックONにする。テンプレートの `@media print` により、1スライド1ページで出力される。
