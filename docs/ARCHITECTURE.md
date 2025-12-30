# Tech Stack 詳細

**SVGの構造・操作・視点を、意図的に3レイヤーに分離している**

[ 構造 ] SVG.js        ← SVGをどう持つか
[ 操作 ] Interact.js   ← ユーザーがどう触るか
[ 視点 ] svg-pan-zoom  ← どう見ているか

この分離ができてるのが最大の価値。

---

## ① SVG.js —「SVGを“状態”として扱うため」

### 何を担当させているか
- SVG要素の取得・生成  
- 属性（fill / stroke / transform）の更新  
- `<g>` 単位の操作  
- SVGを正しいSVGとして保存する責務  

### なぜ生DOM操作じゃダメ？

生DOMでも動くけど、すぐこうなる👇
- transform 文字列のパース地獄  
- rotate / scale / translate の合成ミス  
- 数値誤差でじわじわ壊れる  
- 保存時に「見た目と実体がズレる」  

SVG.jsはここを肩代わりしてくれる。

```js
group.translate(dx, dy)
group.rotate(15)
group.fill('#ff0000')
````

👉 **「操作の意図」をコードで書ける**
👉 **状態管理が壊れにくい**

---

## ② Interact.js —「“触る”ことだけを任せる」

### 何を担当させているか

* ドラッグ
* ポインタイベント（マウス / タッチ / ペン）
* スナップや制限（将来）

### なぜ自前でpointerイベントを書かない？

書ける。でも確実にこうなる👇

* タッチ対応が後から地獄
* pointer capture / cancel の罠
* ドラッグ中の状態管理が肥大化
* UXの微調整でコードが崩れる

Interact.jsは **「dx / dy を安定して出す」** ことに特化している。

```js
interact(node).draggable({
  listeners: {
    move (e) {
      group.translate(e.dx, e.dy)
    }
  }
})
```

👉 Interact.jsは **SVGを知らなくていい**
👉 SVG.jsは **入力デバイスを知らなくていい**

これが分離の美しさ。

---

## ③ svg-pan-zoom —「視点を壊さないため」

### 何を担当させているか

* SVG全体のズーム
* パン（平行移動）
* viewBoxの管理

### なぜここを自前でやらない？

SVGのズーム・パンは **想像以上に厄介**。

* viewBoxの計算
* 中心点維持
* マウス位置基準ズーム
* スクロールとの競合

しかもこれは **編集とは別次元の問題**。

```
編集: パーツがどこにあるか  
表示: それをどう見ているか  
```

svg-pan-zoomは **viewBox専業**。
中のSVG構造を一切触らない。

👉 編集結果がズームに引きずられない
👉 Undo/Redoと独立できる

---

## 3つを混ぜないのが設計の肝

### やってはいけない例

* Interact.jsでCSS transformを当てる
* ズーム時に各要素をscaleする
* SVG.jsでpointerイベントを処理する

### 正しい責務分離

| レイヤー         | 触っていいもの          |
| ------------ | ---------------- |
| SVG.js       | SVG属性・構造         |
| Interact.js  | dx / dy / 操作開始終了 |
| svg-pan-zoom | viewBox          |

---

## なぜ「ブラウザ単独」でも成立する？

* 全部 純粋なクライアントJS
* 状態はSVGそのもの
* 保存＝SVG文字列

👉 サーバ不要
👉 オフライン可
👉 デバッグしやすい

---

## 将来拡張との相性がいい理由

### 例：リサイズを足す

* Interact.js：resizeイベント
* SVG.js：scale反映
* svg-pan-zoom：無関係

### 例：パス編集

* SVG.js：path操作
* Interact.js：ハンドルドラッグ
* svg-pan-zoom：無関係

👉 **波及が最小**

---

## まとめ（設計の核心）

**svg-group-editor の本体は
「この3つをどう組み合わせたか」そのもの**

* SVG.js = 状態
* Interact.js = 入力
* svg-pan-zoom = 視点

この分離がある限り、

* MVPは壊れない
* 仕様が増えても破綻しない
* READMEの思想がコードに一致する
