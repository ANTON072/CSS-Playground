# Modal

### `dialog` 要素とモーダル実装

- **`showModal()`**: dialog をモーダル表示（背景に薄い背景が追加される）
- **`data-active` 属性**: CSSアニメーションの制御
- **`::backdrop` 疑似要素**: モーダルの背景層をスタイリング
- **`backdrop-filter`**: 背景にブラー効果を適用

### 8. メディアクエリ `any-hover`

- `any-hover: hover` → ホバー可能なデバイス（PC）
- `any-hover: none` → ホバー不可なデバイス（スマートフォン）
- タッチデバイスで不自然な `:hover` の挙動を防止
