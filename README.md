# BeautySalon nanairo｜デモHP

静的HTMLのみで構成されたデモサイトです。ビルド不要・依存パッケージなしで、そのままブラウザで開けます。

## ページ構成（10ページ）

| ページ | ファイル | 主な内容 |
|---|---|---|
| 01 TOP | `TOP.dc.html`（`index.html` から自動遷移） | HERO（縦長3カラム／mask切替）、ABOUT nanairo、SERVICES、SALONS、OUR PHILOSOPHY、RESERVATION、FOOTER |
| 02 SERVICES | `SERVICES.dc.html` | ページタイトル型HERO＋大型アコーディオンのSERVICE INDEX |
| 03 ESTHETIC | `ESTHETIC.dc.html` | HERO／CONCEPT（sticky photo）／TREATMENT／AVAILABLE SALONS／RESERVATION |
| 04 NAIL | `NAIL.dc.html` | 同共通システム。10メニューのタイポグラフィ主体リスト |
| 05 EYE | `EYE.dc.html` | 同共通システム。まつ毛パーマ／まつ毛エクステ／アイブロウ |
| 06 MEN'S BEAUTY | `MENS-BEAUTY.dc.html` | 同共通システム。写真は小面積・余白主体 |
| 07 JAGUA TATTOO | `JAGUA-TATTOO.dc.html` | HERO／CONCEPT／DESIGN（エディトリアルグリッド）／AVAILABLE SALONS／RESERVATION |
| 08 SALONS | `SALONS.dc.html` | SHIIZAKO／KATASHIMAを大型章立て＋ACCESS（地図・住所） |
| 09 ABOUT | `ABOUT.dc.html` | HERO（言葉主役）／OUR PHILOSOPHY／TOTAL BEAUTY（スタッキング）／COMPANY（エディトリアルテーブル） |
| 10 RESERVE NAVIGATOR | `RESERVE.dc.html` | 3ステップ（SERVICE → SALON → BOOKING） |

ページ間は相対リンク。`index.html` は `TOP.dc.html` への入口です。

## 起動方法

- ローカル：`index.html` をブラウザで開く（ビルド不要）。
  地図の埋め込みやフォント読み込みを含むため、簡易サーバー経由が確実です：
  `python3 -m http.server 8000` → `http://localhost:8000/`
- 公開：このフォルダ一式をそのまま静的ホスティング（GitHub Pages 等）へ配置。ビルドステップはありません。

## build方法

ビルド不要（No build step）。バンドラ・npm依存はありません。ページは HTML＋インラインCSS＋バニラJSのみで動作します。

## 主要アニメーション

- **REVEAL**：セクションラベルの横方向clip reveal、見出しの行単位mask reveal（`translateY(112%) → 0`）、本文は4〜12pxの微小移動＋opacity、主要画像は mask reveal＋`scale(1.06) → 1`
- **HERO**：3カラムそれぞれが左上→右下方向の `clip-path` トランジション（約1.45s）で切替、画像内部は `scale(1.03) → 1`。表示時間は約5.2秒（TOPのTweaksで3.5〜9秒に変更可）
- **STICKY**：SALONSの店舗名sticky、サービス詳細ページの左画像sticky、OUR PHILOSOPHYの横スクロール変換（1024px以上）、ABOUTのスタッキングカード
- **INTERACTION**：SERVICESのマウス追従フローティング画像（他行を薄く、英字を右へ、矢印を右上へ）、RESERVATIONの写真reveal hover、メニューhoverでの左画像の縦方向mask切替、テキストリンクのunderline draw、写真カードの`scale 1 → 1.03`
- **TRANSITION**：ページ遷移時にアイボリーのフルスクリーンmask＋遷移先名称（約1.0〜1.4秒）
- `prefers-reduced-motion: reduce` では全モーションを無効化し、静的表示にフォールバックします。

## Responsive対応

- Desktop（1440/1280px）：3カラムHERO、マウス追従画像、sticky、横スクロール変換あり
- Tablet（768〜1024px）：追従画像を無効化、横スクロール変換を解除、カラム段組みを再構成
- Mobile（375/390/430px）：SERVICESは常時写真の縦型グリッドへ切替、sticky解除、`100svh` 使用、タップ領域を44px以上確保、画像は端末別 `object-position` で顔・目元・手元が切れないよう調整

## 使用技術

HTML＋インラインCSS＋バニラJS（IntersectionObserver / requestAnimationFrame / CSS transitions のみ）。外部ライブラリなし。フォントは Cormorant Garamond（英字ディスプレイ）、Shippori Mincho（和文見出し）、Zen Kaku Gothic New（本文）の3ファミリー。

## 画像・ロゴassetsの扱い

- 原本は `uploads/`、実装で読み込むのは `assets/`（WebP最適化版＋ロゴ）。`width`/`height`／`aspect-ratio` 指定でレイアウトシフトを抑制、HERO画像のみ preload、他は `loading="lazy"`。
- **ロゴ**：提供された正式ロゴ（`assets/logo.jpg`）のみを使用。文字による再現・別デザイン生成・装飾追加は行っていません。白背景のJPEGのため、淡色背景上のヘッダー／フッターに限定して配置しています。
- **HERO画像4枚**：HERO専用素材として扱い、他セクションには再利用していません。4枚それぞれ顔・髪・手・余白を確認し、個別の `object-position` を設定（例：hero-3 は左の余白を残して `63% 33%`）。
- **施術写真**：メニューと写真内容が一致するものだけを割り当て。写真のないメニュー（脱毛／バストケア／フット／長さ出し／オフ／痩身・脱毛・爪ケア）は画像を差し替えず、キャプションで「準備中」を明示。
- **アイブロウ**：専用写真がないため、EYEカテゴリー全体のイメージとしてのみ表示し、「アイブロウ施術写真」としては掲載していません。
- **MEN'S写真**：解像度が弱いため大型表示せず、小面積＋余白＋タイポグラフィで構成。
- **店舗写真**：`店内内装1/2` はどちらの拠点か資料上特定できないため、店舗の実写として断定せず「nanairoブランドのサロンイメージ」と明記。片島（KATASHIMA）には椎迫の写真を店舗写真として使用せず、取り扱いサービスの写真＋タイポグラフィ＋余白で構成しています。
- 画像枚数を埋めることを目的にせず、似た写真は最適な1枚のみを採用しています。詳細な素材台帳と選定理由は `image-library.md` を参照。

## 要確認情報（正式公開前）

- 椎迫の郵便番号、営業時間、定休日、駐車場台数、NAIL&EYEの個室構造（資料間で差異あり）
- MEN'S美容の実施拠点（公式グループ：椎迫／Instagram：椎迫・下郡） → 拠点を断定せず「正式公開前に確認」表記のまま掲載
- 料金（変動情報のため掲載せず、各予約ページへ誘導）
- スタッフ情報（媒体間で人数差があるため掲載せず）
- ロゴの透過PNG版／横組み版、各専門ブランドの個別ロゴ、店舗外観・看板・駐車場写真、脱毛・バストケア・アイブロウ単独の施術写真（いずれも未受領）

## 未実装事項

- 地図は Google Maps の埋め込み（住所クエリ）と外部リンクで構成しています。正式なプレイスIDでの埋め込み・APIキー運用は公開時にご指定ください。
- WebPのみを出力しています（AVIFは未生成）。
- ドメイン公開用の `sitemap.xml` / `robots.txt` / OGP画像は未作成です。
