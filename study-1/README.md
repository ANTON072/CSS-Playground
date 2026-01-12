# Grid Auto Fill

参照: https://codepen.io/tak-dcxi/pen/gbrKLMV

## レスポンシブグリッドレイアウト

```css
grid-template-columns: repeat(auto-fill, minmax(min(20rem, 100%), 1fr));
```

### 構造の解説

#### `repeat(auto-fill, ...)`

- カラム数を自動で決定
- コンテナの幅に収まるだけカラムを作成
- 空のカラムも生成される

#### `minmax(min(20rem, 100%), 1fr)`

- **最小値**: `min(20rem, 100%)` - 20rem と 100% の小さい方
- **最大値**: `1fr` - 利用可能な空間を均等に分配

#### `min(20rem, 100%)`

- **狭い画面**: 100% を選択 → カラムが1つだけ、横幅いっぱい
- **広い画面**: 20rem を選択 → 複数カラムが並ぶ、各カラムは最低 20rem

### 動作例

- **コンテナ幅 300px** (約 18.75rem): 1カラム (100%)
- **コンテナ幅 800px** (50rem): 2〜3カラム (各 20rem〜)
- **コンテナ幅 1200px**: 5〜6カラム (各 20rem〜)

### メリット

- メディアクエリ不要でレスポンシブ対応
- カラム数が自動調整される
- 小さい画面では横スクロールしない

---

## カード要素のレイアウト

### `._item` の仕組み

各カード要素 (`._item`) は、複雑な内部グリッドを持つように設計されています。

```css
._item {
  display: grid;
  grid-template-columns:
    [--full-start] var(--_gutter)
    [--content-start] 1fr [--content-end] var(--_gutter) [--full-end];
  grid-template-rows: subgrid;
  gap: 1lh 0;
}
```

### 内部グリッド構造

#### カラム構成 (3カラム)

```
[--full-start]  |  余白(1.5rem)  |  [--content-start]  |  コンテンツ(1fr)  |  [--content-end]  |  余白(1.5rem)  |  [--full-end]
```

- **左余白**: 1.5rem
- **コンテンツエリア**: 1fr (残りの空間)
- **右余白**: 1.5rem

#### 短縮記法（ペアの自動範囲選択）

CSS Grid の `[prefix-start]` と `[prefix-end]` のペアは、`prefix` だけで自動参照できます：

```css
& > * {
  grid-column: --content; /* --content-start / --content-end と同じ */
}

& > ._imageLayout {
  grid-column: --full; /* --full-start / --full-end と同じ */
}
```

### Subgrid による行の統一

```css
grid-template-rows: subgrid;
```

**重要な仕組み:**

- 親 (`._layout`): `grid-row: span 4` で各カードに4行分を割り当て
- 子 (`._item`): `subgrid` で親の4行構造を**継承**
- 結果: すべてのカードが同じ4行で構成される

**効果:**
横に並んだカード間で、各行の高さが自動的に揃う（例: 全カードの画像が同じ行、タイトルが同じ行）

### 行間のギャップ

```css
gap: 1lh 0;
```

- `1lh` = 1 line-height (行の高さ)
- `0` = 列間なし
- テキストの行間に合わせた自然な間隔

### ビジュアルスタイル

```css
padding-block-end: var(--_gutter); /* 下側の余白 */
border-radius: 4px; /* 角丸 */
box-shadow: 0 20px 40px -14px oklch(...); /* 影 */
```

- `padding-block-end`: 論理プロパティ（書字方向に対応）
- `oklch(from black l c h / 25%)`: 色相保持で黒色の透明版

### レイアウト例

```
┌─────────────────────────────────┐
│        画像 (--full)            │  ← 全幅
├─────────────────────────────────┤
│ [余白] タイトル [余白]           │  ← コンテンツ領域
│ [余白] 説明文 [余白]            │  ← コンテンツ領域
│ [余白] (余白) [余白]            │  ← コンテンツ領域
└─────────────────────────────────┘
```

- 画像は全幅（余白含む）で表示
- テキスト類は左右の余白が付いた領域に配置
- 全て 4 行のグリッド構造で揃う
