# KF English-Study HTML — Design System

共通HTMLテンプレートの設計規約・デザイントークン・コンポーネント集。
英語学習ドキュメントをHTMLで生成するときの「正本」となるルール集です。

- **テンプレート本体（必ず最初に取得）**: `https://raw.githubusercontent.com/kazukifujiwara/html-template/main/kf-english-study-template-20260601.html`
- 最終更新: 2026-06-01
- 路線: Apple / Distinction のミニマル（余白・文字組み主役、アクセント1系統、ライト/ダーク自動）

---

## 0. 使い方（生成手順）

1. 上記テンプレート本体を取得し、**`:root` のトークンと `<style>` ブロックをそのままコピー**して起点にする（CSSは再発明しない）。
2. `<body>` 内は、本書のコンポーネント・スニペットを**必要な分だけ複製・編集**して組み立てる。
3. 不要なセクションは削除してよい（各ブロックはコメントで区切り済み）。
4. 末尾の `<script>`（スクロール出現・開閉・印刷時全開）はそのまま残す。
5. 色を変えたいときは `:root` の `--grad-1` / `--grad-2` / `--em` / `--accent` だけ触る。

> 原則：**CSSはテンプレ本体からコピー**、本書は「どのHTML構造を使うか」と「文言・運用ルール」のリファレンス。

---

## 1. 設計の規律（5原則）

1. **余白を大きく取る**。1セクション = 1メッセージ。
2. **色の主役はアクセント（グラデ）1系統**。本文・構造はモノクロのまま。
3. **文字組みで見せる**（装飾より階層・字間・行間）。
4. **派手なのは1つだけ**（主ボタン＝グラデは1画面に1つが理想）。
5. **ライト/ダーク両対応**を常に確認（トークン差し替えで自動追従）。

---

## 2. デザイントークン

`:root` に定義。値の変更は原則アクセント系のみ。

### Color（モノクロ基調）
| トークン | Light | Dark | 用途 |
|---|---|---|---|
| `--bg` | `#ffffff` | `#0b0b0d` | 背景 |
| `--bg-alt` | `#f5f5f7` | `#19191c` | セクション地・カード |
| `--ink` | `#1d1d1f` | `#f5f5f7` | 本文 |
| `--ink-2` | `#6e6e73` | `#a1a1a6` | 補足・リード |
| `--ink-3` | `#86868b` | `#8a8a8f` | ラベル・キャプション |
| `--line` | `#d2d2d7` | `#2c2c30` | 罫線・境界 |

### Accent（見出しグラデ + 強調 + リンク）
| トークン | Light | Dark | 用途 |
|---|---|---|---|
| `--grad-1` | `#3730d6` | `#7d7bff` | グラデ開始（インディゴ） |
| `--grad-2` | `#e640fb` | `#ff74ea` | グラデ終了（マゼンタ） |
| `--accent-grad` | `linear-gradient(105deg,var(--grad-1) 0%,#7c3aed 42%,var(--grad-2) 100%)` | 明るめに差し替え | 見出し `.grad`・バッジ・主ボタン |
| `--em` | `#8b3ff0` | `#bca4ff` | 本文中の語句強調・clip非対応フォールバック |
| `--accent` | `#7c3aed` | `#bca4ff` | リンク |

### Typography
| トークン | 値 |
|---|---|
| `--font-en` | `-apple-system, BlinkMacSystemFont, "SF Pro Display", "SF Pro Text", "Helvetica Neue", Helvetica, Arial, sans-serif` |
| `--font-chart` | `var(--font-en)`（チャート/SVG文字。数字は `tabular-nums` で桁そろえ） |
| `--font-jp` | `"Hiragino Kaku Gothic ProN", "Hiragino Sans", "Yu Gothic", "Noto Sans JP", var(--font-en)` |
| `--font-mono` | `"SF Mono", ui-monospace, "JetBrains Mono", Menlo, Consolas, monospace` |
| `--font-ipa` | `"Charis SIL", "Gentium Plus", "Doulos SIL", "Hiragino Mincho ProN", Georgia, Cambria, "Times New Roman", serif` |

### Spacing（8ptグリッド）/ Radius / Layout
| 種別 | トークン |
|---|---|
| Spacing | `--s1:4` `--s2:8` `--s3:12` `--s4:16` `--s5:24` `--s6:32` `--s7:48` `--s8:64` `--s9:96` `--s10:128`（px） |
| Radius | `--r-sm:8px` `--r-md:14px` `--r-lg:22px` |
| Layout | `--maxw:780px`（本文カラム幅） |

---

## 3. 文言・言語ルール（i18n）

- **文言は出力ドキュメントの言語に統一**する（コンポーネント自体は言語非依存）。
- **バイリンガル資料**：日本語リードの直下に英語補足を `<p class="en-sub">` で添える。
- **英語のみの資料**：見出し・本文・例文・注釈をすべて英語にし、UI文字列は下表の英語版を使う。

| 日本語 | English |
|---|---|
| すべて開く / すべて閉じる | Expand all / Collapse all |
| タップで意味 | Tap to reveal |
| Word Review · 復習（kicker） | Word Review |
| 語彙の見出し | Key Vocabulary |

### IPA（発音記号）ルール
- 語彙には**既定で必ずIPAを付ける**。
- **GA（General American）= Dallas関連／米語文脈**。**RP = それ以外**。
- 表示フォントは `--font-ipa`（セリフ）。

---

## 4. コンポーネント（HTMLスニペット）

CSSはテンプレ本体に含まれるので、ここでは**body内に置くHTML**のみ。`reveal` クラスはスクロール出現用（任意）。

### 4.1 Hero
```html
<section class="hero wrap">
  <div class="reveal">
    <p class="kicker">Section Label</p>
    <h1 class="grad">見出し（gradで色がつく）</h1>
    <p class="lead">リード文。要点を1〜2文で。</p>
    <p class="en-sub">English sub-line (optional, for bilingual docs).</p>
  </div>
</section>
```

### 4.2 Top bar（任意・sticky）
```html
<header class="topbar">
  <div class="row">
    <span class="brand">Doc Title</span>
    <nav><a href="#sec1">Section</a><a href="#sec2">Section</a></nav>
  </div>
</header>
```

### 4.3 Word Entry ★看板コンポーネント
Anki対応：Word / IPA / POS / Meaning_JP / Meaning_EN / Example。
```html
<div class="entry reveal">
  <div class="head">
    <span class="word grad">headword</span>
    <span class="ipa">/ˈhɛdˌwɝd/</span>
    <span class="pos">noun</span>
  </div>
  <p class="mean">日本語の意味</p>
  <p class="mean-en en">English definition</p>
  <div class="ex">
    <p class="en" style="margin:0">Example sentence in English.</p>
    <p class="jp" style="margin:0">日本語訳。</p>
  </div>
  <div class="origin">
    <span class="l">Origin</span>
    語源・由来の解説（定着用）。
  </div>
</div>
```

### 4.4 Word Review / Key Vocabulary ★折りたたみ（アクティブリコール）
語彙まとめはこれで統一。`summary` をタップ → 意味が出る。`ri-body` の順序は **IPA → 意味 → 例文**。
```html
<section id="review" class="alt">
  <div class="wrap">
    <div class="reveal">
      <p class="kicker">Word Review</p>
      <h2 class="grad">復習</h2>
      <p class="lead">タップで意味が出る、アクティブ・リコール用リスト。</p>
      <div class="review-ctrl">
        <button class="btn btn-ghost btn-sm" id="openAll">すべて開く</button>
        <button class="btn btn-ghost btn-sm" id="closeAll">すべて閉じる</button>
      </div>
    </div>
    <div class="review reveal">
      <details class="ri">
        <summary>
          <span class="word en">touch base</span>
          <span class="hint">タップで意味</span>
          <svg class="chev" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M6 9l6 6 6-6"/></svg>
        </summary>
        <div class="ri-body">
          <p class="ipa">/tʌtʃ beɪs/</p>
          <p class="mean">（短く）連絡を取る</p>
          <p class="ex en">Let's touch base on Monday.<span class="jp">月曜にすり合わせましょう。</span></p>
        </div>
      </details>
      <!-- details.ri を必要数だけ複製 -->
    </div>
  </div>
</section>
```
- 軽い語彙集（用語＋短い定義のみ）なら `ri-body` は `.mean` だけでも可（IPA/例文は任意）。
- 英語資料では `hint="Tap to reveal"` / ボタンは `Expand all` `Collapse all`。
- 同一ページに複数リストを置くなら、開閉ボタンの対象をリスト単位に絞る。

### 4.5 Labeled Cards（番号バッジ + ラベル + テキスト）
```html
<div class="icards cols-3 reveal">
  <div class="icard">
    <div class="label">Label</div>
    <div class="body"><div class="badge">1</div><p class="t">カードの内容</p></div>
  </div>
  <!-- cols-2 / cols-3 で列数指定 -->
</div>
```

### 4.6 Note（補足・注意）
```html
<div class="note reveal">
  <span class="l">Note</span>
  <p>補足・ポイント。地色グレーで本文と区別しつつ主張しすぎない。</p>
</div>
```

### 4.7 Table ★モバイル対応：横長表は必ず `.table-wrap` で囲む
```html
<div class="table-wrap">
  <table>
    <thead><tr><th>列1</th><th>列2</th><th>列3</th></tr></thead>
    <tbody>
      <tr><td>見出しセル（自動で太字）</td><td class="mono">値</td><td>説明</td></tr>
    </tbody>
  </table>
</div>
```
- 狭い画面でははみ出さず横スクロール（両端の影で続きを示唆）。
- 列が多い／値が長い表は `.table-wrap` 必須。数値列は `class="mono"` でそろう。

### 4.8 Stat（数値の見せ場）
```html
<div class="stats reveal">
  <div class="stat"><div class="v grad">3</div><div class="k">ラベル</div></div>
  <div class="stat"><div class="v grad">8pt</div><div class="k">ラベル</div></div>
</div>
```

### 4.9 Buttons
```html
<a class="btn btn-primary" href="#">主ボタン</a>   <!-- グラデ・1画面に1つ -->
<a class="btn btn-soft" href="#">副ボタン</a>      <!-- 薄紫・控えめ -->
<a class="btn btn-ghost btn-sm" href="#">機能ボタン</a> <!-- 無彩色枠線・開閉等 -->
<a class="btn-text" href="#">テキストリンク <span class="ar">›</span></a>
```

### 4.10 Reference Links ★参考リンクは3型に統一
```html
<!-- ① 外部リンク：末尾↗・別タブ。外部は必ずこれ -->
<a class="reflink" href="https://example.com" target="_blank" rel="noopener">リンク名</a>

<!-- ② 本文中の引用 [n]：#ref-n へジャンプ -->
…という結果が報告されています<sup class="cite"><a href="#ref-1">1</a></sup>。

<!-- ③ 末尾の References リスト -->
<div class="refs">
  <ol class="ref-list">
    <li id="ref-1">
      <span class="ref-title">文献タイトル</span>
      <span class="ref-meta">著者 · 媒体・会議名 · 年</span>
      <a class="reflink" href="https://example.com" target="_blank" rel="noopener">リンク</a>
    </li>
  </ol>
</div>
```
**使い分け**：主要文献＝ヒーローに `.reflink` ＋ References登録／補足主張＝`[n]`／軽い言及＝インライン `.reflink` のみ。内部アンカー（TOC・引用ジャンプ）には ↗ を付けない。

### 4.11 Chart / SVG text
- チャート/図は `<div class="chart">…svg…</div>` または `<figure>…svg…</figure>` の中に置く。
- SVG内テキストは **CSSで `--font-chart` ＋ `tabular-nums` に自動上書き**される（JSで `font-family` を属性指定しても効かない環境対策）。数字の桁がそろう。
- 線・面の色はアクセント系トークン（`--accent` 等）を流用。

---

## 5. 印刷 / アクセシビリティ / ダーク

- **印刷（PDF化）**：グラデ見出しは単色（`--em`）にフォールバック、外部リンクはURLを併記、復習リストは自動で全開、`.ri` はページ途中で割れない。
- **アクセシビリティ**：`a` / `summary` / `button` に `:focus-visible` のアウトライン。`prefers-reduced-motion` で出現アニメ無効。
- **ダーク**：`@media (prefers-color-scheme: dark)` でトークンを差し替え、OS設定に自動追従。新規の色は両テーマで可読か確認。

---

## 6. 末尾スクリプト（そのまま残す）
```html
<script>
/* (a) スクロール出現 .reveal → .in / (b) すべて開く・閉じる / (c) 印刷時に全開 */
</script>
```
※ 完全版はテンプレ本体の末尾を参照。`#openAll` / `#closeAll` と `#review details` を前提にしている。

---

## 7. 生成時チェックリスト

- [ ] `:root` トークンと `<style>` をテンプレ本体からコピーした
- [ ] 文言は出力言語で統一（バイリンガルなら `.en-sub`、英語のみなら英語UI文字列）
- [ ] 語彙にIPAを付けた（GA=Dallas/米語、RP=その他）
- [ ] 横長になりうる表は `.table-wrap` で囲んだ
- [ ] 外部リンクは `.reflink`（↗・`target="_blank" rel="noopener"`）／引用は `[n]`／末尾に References
- [ ] チャート/図はラッパ内に置き、文字が `--font-chart`＋`tabular-nums` で表示される
- [ ] 主ボタン（グラデ）は1画面に1つに抑えた
- [ ] ライト/ダーク・印刷・スマホ幅で崩れないか確認した
