# AWS構成図（`s-aws`）

`s-aws` スライドの `.badge` には、各AWSサービスを表すインラインSVGアイコンを入れます。

## バッジとサービス枠

この案ではサービスごとに色を振り分けません。バッジは一律で1px罫の白抜きにし、**朱赤で塗るのは `.svc.compute` の1つだけ**です（「自分でコードを書く場所」を1箇所だけ示す）。

- `.svc` — サービス1つ分の枠。`.badge`（56角のアイコン枠）＋ `.sname` ＋ `.sdesc`。
- `.svc.compute` — バッジを朱赤ベタ、アイコンを紙色の線に反転する（template.html の `.s-aws .svc.compute .badge`）。**1枚に1つまで。**
- `.svc.ext` — 枠線を破線にする。AWS の外側にあるもの（利用者、外部サービス）に使う。
- `.svc-stack` — 複数のサービスを縦に積んで1カラムに収める（DynamoDB と S3 のような並列の保存先）。
- サービスの追加・削除は `.svc` と `.aws-arrow` をセットで増減する。

## 公式アイコンへの差し替え

テンプレートには簡略化したグリフが入っていますが、**正式なAWS構成図を作る場合は、AWS公式の「AWS Architecture Icons」から該当サービスのSVGをダウンロードして差し替えてください。**

- 配布元: AWS Architecture Icons（<https://aws.amazon.com/architecture/icons/>）の公式アセットパックをダウンロードする。
- パック内から使用するサービスのSVG（例: Amazon CloudFront、Elastic Load Balancing、AWS Lambda、Amazon RDS、Amazon S3）を選ぶ。
- 該当SVGの中身を `<div class="badge">` の中に貼り付けて、テンプレートの簡略アイコンと置き換える。公式アイコンは独自の背景色を持つため、`.badge` の1px罫と二重に見える場合は `.badge` 側を `border:0` にする。`.svc.compute` に公式アイコンを入れると朱赤ベタと競合するので、その場合は `compute` クラスを外す。
- 公式アイコンには利用規約があるため、配布・登壇用途では AWS の商標・アイコン利用ガイドラインに従ってください。
- ライセンスや時間の都合で公式SVGを使わない場合は、テンプレート同梱の簡略グリフ（単色の線画）をそのまま使って構いません。その旨を構成図の注記に残すと誤解がありません。
