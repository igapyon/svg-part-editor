# DEVELOPER_NOTES.md

本ドキュメントは開発の引き継ぎ事項をまとめたものです。

## スコープ

- 本プロジェクトはブラウザ単体で動作する軽量SVGエディタです。
- 最小編集単位は最も近い `<g>` 要素で、`<g>` が無い場合は要素自体を扱います。
- MVPではビルドやサーバは不要です。
- 本アプリは Static Web App（静的Webアプリ）として設計されています。単一HTMLで完結し、ビルドやサーバは不要です。

## 作業ファイル

- メインの作業ファイル: `index.html`
- ドキュメント: `README.md`, `docs/ISSUE_LIST.md`

## 設計制約

- 変形はSVG属性（transform）に反映し、CSS transformは使いません。
- ズーム / パンは表示操作（viewBox）に限定し、編集操作と分離します。
- 依存ライブラリは固定バージョンのCDNを使います。

## 選択バージョン

- SVG.js: 3.2.5
- Interact.js: 1.10.27
- svg-pan-zoom: 3.6.2

## 実装方針

- HTML内にCSS/JSを内包する（1ファイル完結）。
- テストデータは `test/data` に配置する。

## 開発フロー

- `index.html` をブラウザで直接開いて動作確認します。
- 作業項目は `docs/ISSUE_LIST.md` で管理します。
- `docs/ISSUE_LIST.md` の内容は GitHub Issue にも登録済みです。

## GitHub Pages

- 公開ブランチは `gh-pages` です。
- 公開元は `/(root)` で、`index.html` をそのまま配信します。
- `index.html` を正本として編集します。
- 公開URL: https://igapyon.github.io/svg-part-editor/

## 命名

- プロジェクト名は `svg-part-editor` です。
- 旧名の `svg-group-editor` は使用しないでください。

## 編集対象外の境界 (壊さないための最小分解)

- 最小編集単位は「最も近い `<g>`」。 `<g>` が無い場合のみ要素単体。
- `<g>` 内部の分解は行わない。
- `<defs>` 配下全て (symbol/clipPath/mask/pattern/marker 等) は編集対象外。
- `<use>` 自体は編集対象外。必要なら参照先を選択対象にする。
