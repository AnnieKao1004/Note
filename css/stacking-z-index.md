# Stacking (z-index)

## Stacking w/o z-index (default)

From bottom to top:

1. **non-positioned** elements, in order of appearance in HTML
2. **positioned elements**, in order of appearance in HTML

## Stacking w/ z-index

- Sets the z-order on a **positioned element** (one with `position` other than `static`)
- A **stacking context** is formed by element with:

  1. root element (`<html>`)
  1. `position` `absolute` or `relative` and `z-index` other than `auto`
  1. `position` `fixed` or `sticky`
  1. child of a `flex` container, with `z-index` other than `auto`
  1. `transform` properties other than `none`  
     [See all](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Positioning/Understanding_z_index/The_stacking_context)

- 每個 stacking context 都是獨立的，只有形成 stacking context 該元素下面的子元素 (代表這些子元素在同一個 stacking context 裡) 會互相影響
- Checking steps if z-index is not working:

  1. Check if the element is **positioned**
  1. 如果 1 成立，比較兩元素的 z-index property
  1. 如果 2 成立，代表兩元素可能不在同一個 stacking context 裡，往上檢查父級元素的 z-index
