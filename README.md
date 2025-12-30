# svg-part-editor

**svg-part-editor** is a lightweight, browser-only SVG WYSIWYG editor  
that treats `<g>` elements as the minimum editable parts.

SVGを **DOMとして扱い**、**`<g>`（group）をパーツの最小単位**として編集することに特化した、  
ミニマルなWebブラウザ向けSVGエディタです。

---

## Features

- 🧩 `<g>` を最小編集単位としたパーツ指向編集
- 🖱 Webブラウザ単体で動作（サーバ・ビルド不要）
- 🔍 ズーム / パン対応
- ✋ ドラッグによるパーツ移動
- 🎨 fill / stroke / stroke-width の編集
- ↩️ Undo / Redo
- 💾 編集結果をSVGとして保存

---

## Non-Goals（やらないこと）

svg-part-editor は **フル機能のSVGエディタを目指しません**。

以下はMVPでは意図的に対象外です：

- パス（ノード）編集
- グループ内部要素の個別編集
- フィルター / マスク / クリッピング
- 高度なテキスト編集
- Inkscape / Illustrator 互換を目指すこと

---

## Design Philosophy

### `<g>` as the Minimum Editable Unit

本プロジェクトでは、人が「1つの部品」と認識する単位を最優先します。

- 編集対象の最小単位は **`<g>` 要素**
- `<g>` が存在しない場合のみ、単一のSVG要素をパーツとして扱う
- 移動・変形・属性編集は **常にパーツ単位**で行う

```svg
<g id="icon">
  <path d="..." />
  <path d="..." />
</g>

この前提により：
	•	複数の線で構成されたアイコンを自然に扱える
	•	transform や属性管理が単純になる
	•	将来的な拡張（内部編集）を段階的に進められる

⸻

Transform Handling
	•	変形は SVG属性（transform）として反映
	•	CSS transform は使用しない
	•	transformは可能な限り <g> に集約する

これにより、保存・再読み込み・他ツールとの相互運用性を保ちます。

⸻

View vs Edit Separation
	•	ズーム・パンは viewBox操作
	•	編集操作（移動・属性変更）とは責務を分離

⸻

Tech Stack
	•	SVG.js — SVG構造・属性操作
	•	Interact.js — ドラッグ等のポインタイベント
	•	svg-pan-zoom — ズーム / パン制御

すべてCDN経由で利用可能です。

⸻

Getting Started

Run Locally
	1.	リポジトリをクローン or ダウンロード
	2.	index.html をブラウザで開く

# no build, no server required
open index.html


⸻

MVP Scope

Included
	•	SVGの読み込み・表示
	•	<g> 単位の選択
	•	ドラッグ移動
	•	fill / stroke / stroke-width 編集
	•	Undo / Redo
	•	SVGとして保存

Excluded (Future Work)
	•	パス編集
	•	グループ内部編集
	•	高度な整列・レイアウト機能

⸻

Project Status

🚧 Work in Progress (MVP)

設計思想の検証と、軽量SVG編集の実装実験を目的としたプロジェクトです。

⸻

License

Apache License 2.0
See LICENSE for details.
