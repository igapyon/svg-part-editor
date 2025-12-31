# ISSUE_LIST.md

## 🧱 Phase 0: プロジェクト基盤

### Issue 01: 静的HTML1枚で起動できるプロジェクトを作る
**Done条件**
- `src/svg-part-editor.html` を直接開いて動く  
- 画面が表示される（空でもOK）
**ステータス**
- 完了

### Issue 02: CDN依存を固定バージョンで読み込む
**Done条件**
- SVG.js / Interact.js / svg-pan-zoom を読み込み  
- `@latest` を使っていない  
- console errorなし
**ステータス**
- 完了

### Issue 03: 基本レイアウトを作る（Toolbar / Stage / Properties）
**Done条件**
- ツールバー、キャンバス、属性パネル領域がある  
- 最低限のCSSで崩れない  
**ステータス**
- 完了

---

## 📥 Phase 1: 入出力（A 分解）

### Issue 04: SVGファイル入力で文字列を取得できる
**Done条件**
- `<input type="file" accept=".svg">` から `file.text()` で読める  
- 文字列取得に失敗しない  
**ステータス**
- 完了

### Issue 05: SVG文字列をStageに挿入して表示できる
**Done条件**
- SVGがDOMとして挿入され表示される  
- ルート `<svg>` が取得できる  
**ステータス**
- 完了

### Issue 06: 初期表示をfit/centerできる
**Done条件**
- 読み込み直後に見える状態になる（極端に小さい/大きいを吸収）  
- 「表示が空っぽ」にならない  
**ステータス**
- 完了

### Issue 07: SVGを文字列化できる（serializer）
**Done条件**
- 現在の `<svg>` を文字列として取得できる  
- 文字列が空にならない  

### Issue 08: SVGをダウンロード（Blob）保存できる
**Done条件**
- ボタンで `.svg` をダウンロードできる  

### Issue 09: 保存→再読み込みで編集結果が保持される
**Done条件**
- 保存したSVGを読み込んで、変更が反映されている  
**ステータス**
- 完了

---

## 👀 Phase 2: 表示操作

### Issue 10: ズーム（wheel等）を有効化する
**Done条件**
- ズームできる  
- 変なジャンプがない  
**ステータス**
- 完了

### Issue 11: パン（ドラッグ等）を有効化する
**Done条件**
- 背景操作でパンできる  
**ステータス**
- 完了

### Issue 12: 表示リセット（fit/center）ボタンを追加する
**Done条件**
- ワンクリックで初期表示に戻せる  
**ステータス**
- 完了

---

## 🖱 Phase 3: 選択（C 分解）

### Issue 13: クリック対象要素を取得できる（event → target）
**Done条件**
- クリックした要素が取れる  
- ルート `<svg>` クリックは無視できる  
**ステータス**
- 完了

### Issue 14: closest `<g>` 探索で「パーツ」を決定できる
**Done条件**
- クリック要素から親を辿り、最も近い `<g>` を返す  
- `g` が無い場合は要素自体を返す  
**ステータス**
- 完了

### Issue 15: 選択状態を管理できる（select / deselect）
**Done条件**
- 1つだけ選択状態を保持できる  
- 背景クリックで解除できる  
**ステータス**
- 完了

### Issue 16: 選択状態を視覚的に表示する（ハイライト or 枠）
**Done条件**
- 選択中が分かる（CSSクラスでもOK）  
**ステータス**
- 完了

---

## ✋ Phase 4: 変形（移動）（D 分解）

### Issue 17: ドラッグ移動のイベント骨格を実装する
**Done条件**
- pointerdown / move / up（または Interact.js）でドラッグ検出できる  
**ステータス**
- 完了

### Issue 18: パーツにtranslateを適用して移動できる（SVG.js側で反映）
**Done条件**
- ドラッグで要素が移動する  
- 更新がCSS transformではなく、SVG要素側（SVG.js）で保持される  
**ステータス**
- 完了

### Issue 19: 移動結果が保存後も維持される（見た目だけにならない）
**Done条件**
- 保存→再読み込みでも位置が変わったまま  
**ステータス**
- 完了

### Issue 20: パン操作と要素移動が衝突しない（背景=pan / 要素=move）
**Done条件**
- 背景ドラッグはパン  
- パーツドラッグは移動  
- 誤爆が目立たない  
**ステータス**
- 完了

---

## 🎨 Phase 5: 属性編集

### Issue 21: 属性パネルUI（fill / stroke / stroke-width）を配置する
**Done条件**
- 入力UIが存在（color / number など）  
**ステータス**
- 完了

### Issue 22: fillを編集して反映できる
**Done条件**
- 選択パーツのfillが変わる  
**ステータス**
- 完了

### Issue 23: strokeを編集して反映できる
**Done条件**
- 選択パーツのstrokeが変わる  
**ステータス**
- 完了

### Issue 24: stroke-widthを編集して反映できる
**Done条件**
- 選択パーツのstroke-widthが変わる  
**ステータス**
- 完了

### Issue 25: 選択変更時に属性パネルが追従する
**Done条件**
- 選択中パーツの現在値がUIに反映される  
**ステータス**
- 完了

### Issue 25.1: Propertiesの表示/編集可能項目の設計を整理する（GitHub #53）
**Done条件**
- Propertiesで表示する項目一覧が決まっている  
- 各項目が「編集可能 / 表示のみ / 対象外」のいずれかに分類されている  
- 判断理由（例: `<g>` 最小単位、参照構造維持、安全性）が簡潔に記録されている  
**メモ**
- タブ分けで構成する  
- 表示のみ（初期）: id / class / tagName / transform（生文字列）/ bbox（x/y/width/height）  
- 選択要素のソース表示は参照を解決せず、そのまま表示する前提  

---

## ↩️ Phase 6: Undo / Redo（G 分解）

### Issue 26: 履歴スタックのデータ構造を用意する
**Done条件**
- undoStack / redoStack（または index 方式）がある  
- 最大件数（例：20〜50）を決めている  
**ステータス**
- 完了

### Issue 27: スナップショットをpushできる（編集のたびに保存）
**Done条件**
- 移動 / 属性変更のタイミングでSVG状態が保存される  
- 余計にpushしすぎない（ドラッグ中は間引く等、MVPなら終了時pushでもOK）  
**ステータス**
- 完了

### Issue 28: Undo（Ctrl / Cmd + Z）を実装する
**Done条件**
- 直前状態に戻れる  
- 最低20手戻れる  
**ステータス**
- 完了

### Issue 29: Redo（Ctrl / Cmd + Shift + Z / Ctrl + Y）を実装する
**Done条件**
- Undoした操作をやり直せる  
**ステータス**
- 完了

---

## 🧯 Phase 7: 安定性

### Issue 30: viewBoxが無いSVGでも破綻しにくくする
**Done条件**
- 読み込み後に見える（fit / centerで吸収 or 軽い補正）  
- 操作不能にならない  
**ステータス**
- 完了

### Issue 31: 保存時にCSS transformが残らないようにする
**Done条件**
- transformはSVG要素側に反映されている  
- 保存後の見た目が一致する  
**ステータス**
- 完了

---

## 🧭 Phase 7.5: UIモード

### Issue 31.1: Select / Move / Pan アイコンにモード機能を割り当てる（GitHub #54）
**Done条件**
- 右側アイコンで Select / Move / Pan のモードが切り替えられる  
- モードに応じてクリック/ドラッグ挙動が変わる  
- 現在のモードが視覚的に分かる  
**ステータス**
- 完了
**仕様メモ（確定）**
- モードは Select / Move / Pan の3つに限定する
- Select: クリック選択/解除のみ、ドラッグ移動は無効
- Move: クリック選択 + ドラッグ移動を許可
- Pan: 背景ドラッグでビュー移動のみ、選択はしない

### Issue 31.2: 追加モード候補（Rotate/Scale/Duplicate/Measure等）を検討する（GitHub #55）
**Done条件**
- 追加するモード候補の一覧が整理されている  
- 各候補の目的と優先度が簡潔に記録されている  
- 非採用の理由（必要なら）を記録している  
**ステータス**
- 完了
**仕様メモ（確定）**
- 次の1-2スプリント候補に絞る  
- 右側アイコンは5-6個を上限にし、超える分は "More" メニュー併用  
**優先候補（次の1-2スプリント）**
- Duplicate: 選択パーツの複製と簡易配置  
- Delete: 選択パーツの削除  
- Nudge: キー操作による微小移動  
- Rotate: 角度指定の回転（UIは簡易でOK）  
- Scale: 拡縮（ハンドル or 数値指定、簡易でOK）  
**保留候補（次段階以降）**
- Measure / Inspect: 表示系モード  
- Align / Distribute / Snap: 幾何計算とガイド  
- Lasso / Marquee Select: 複数選択UI  
- Group / Ungroup / Layer / Order: 構造変更  
- Draw: 新規作成（Issue 31.3 と連動）  
**候補（洗い出し）**
- 基本操作: Rotate / Scale / Duplicate / Delete / Nudge  
- 整列・配置: Align / Distribute / Snap  
- 計測・確認: Measure / Inspect  
- 表示・選択: Lasso・Marquee Select / Lock・Unlock / Hide・Show  
- 構造: Group・Ungroup / Layer・Order  
- 作成: Draw（図形作成モード、Issue 31.3 と関連）  
**難易度（補足）**
- 高: Lasso・Marquee Select（複数選択UI/判定）, Snap/Align/Distribute（幾何計算/ガイド）, Group・Ungroup/Layer・Order（構造変更の整合）, Measure（座標系とUI）, Draw（新規作成フロー）  
- 中: Rotate/Scale（中心/数値入力/UI設計）, Lock・Unlock/Hide・Show（状態管理/UI）  
- 低: Duplicate（cloneと配置）, Delete（削除）, Nudge（キー移動）, Inspect（表示のみなら簡易）  

---

## 🧩 Phase 7.6: Part作成

### Issue 31.3: 新規 Part 作成機能の検討（GitHub #56）
**Done条件**
- 作成対象のPart種別（例: rect/circle/path など）が整理されている  
- UI導線（ツール/メニュー/ショートカット等）の案が決まっている  
- 参照構造や `<g>` 最小単位との整合性が検討されている  
**ステータス**
- 完了
**メモ**
- 初期バージョン対象: rect / circle / ellipse / line  
- 初期バージョン対象外: path / polygon / polyline / text / image / g  
- 推奨順: 基本図形作成（rect/circle/ellipse/line）→ 複数選択 → グループ化（g）  
**仕様メモ（確定）**
- 作成はクリック&ドラッグでサイズ決定  
- 新規作成は `<g>` で包む  
- 初期属性は固定値  
  - rect/circle/ellipse: `fill="#e2e8f0"`, `stroke="#111827"`, `stroke-width="1"`  
  - line: `fill="none"`, `stroke="#111827"`, `stroke-width="1"`  

---

## 🧩 Phase 7.7: Part作成 UI 導線

### Issue 31.4: Part作成 UI 導線の具体化（GitHub #64）
**Done条件**
- 作成モードの導線（ツール配置・ショートカット・切り替え手順）が決まっている  
- 作成時の操作フロー（開始/ドラッグ/確定/キャンセル）が明文化されている  
- 右側アイコンの上限（5-6個）超過時のUI方針が決まっている  

---

## ✅ Phase 8: MVP完了

### Issue 32: MVP動作確認シナリオを通す
**Done条件**
- 読み込み → 選択 → 移動 → 色変更 → 保存 → 再読み込み が通る  
**チェックリスト**
- [x] SVGを読み込む
- [x] パーツを選択する
- [x] パーツをドラッグ移動する
- [x] fill / stroke / stroke-width を変更する
- [x] SVGとして保存する
- [x] 保存したSVGを再読み込みし、変更が保持されている
**ステータス**
- 完了

### Issue 33: READMEにMVP仕様（`<g>` 最小単位など）を明文化する
**Done条件**
- 「パーツ最小単位は `<g>`」などの設計方針がREADMEにある  
- MVPの含む / 含まないが書かれている  
**ステータス**
- 完了

### Issue 34: リリース用の最小手順をREADMEに追記する
**Done条件**
- 起動方法（ローカル / 静的ホスト）が1分で分かる  
- 既知の制約（例：パス編集なし）が書かれている  
**ステータス**
- 完了

---

## 📝 運用メモ（おすすめ）
- **Milestone案**：Phase 0〜Phase 8  
- **最初の“縦切り”最短ルート**：  
  `04 → 05 → 13〜16 → 17〜19 → 22 → 08 → 09 → 32`  

---
