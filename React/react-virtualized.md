# [react-virtualized](https://github.com/bvaughn/react-virtualized)
- Used to render long table/list  (因 api repsponse 沒有做分頁而出現的需求)
- Works by only rendering part of a large data set, just enough to fill the viewport
- Similar to `react-window` (a complete rewrite of `react-virtualized`)
- Choose to use `react-virtualized` because of `WindowScroller` component which allow table/list to be scrolled based on the `window`'s scroll positions, like Facebook (according to the [author](https://github.com/bvaughn/react-window/issues/30), `WindowScroller` is one of the painful part of `react-virtualized`, so it's not inlcuded in `react-window`)
- [Docs](https://github.com/bvaughn/react-virtualized/tree/master/docs)

### `Table` & `Column`
Basic Table: Define table width/height, row height/count and column props
```javascript
// columns is an array with objects which describes the properties of each column

<Table
  width={width}
  height={height}
  headerHeight={40}
  rowHeight={100}
  rowCount={data.length}
  rowGetter={({ index }) => data[index]}
>
  {columns.map((col) => (
    <Column
      key={col.label}
      width={col.width}
      label={col.label}
      dataKey={col.dataKey}
    />
  ))}
</Table>
````

### `AutoSizer`
Wrap Table with `AutoSizer` to automatically adjust the available width/height for `Table` (when you just want a component to grow to fill all of the available space)
```javascript
<AutoSizer>
  ({width, height}) => {
  return (
    <Table
      width={width}
      height={height}
      headerHeight={40}
      rowHeight={100}
      rowCount={data.length}
      rowGetter={({ index }) => data[index]}
    >
     {columns}
    </Table>
  )}
</AutoSizer>
````

### `WindowScroller`
Wrap component with `WindowScroller` to make it scroll based on the window's scroll positions
- Add `disableHeight` prop to `AutoSizer`
- Add `autoHeight`, `isScrolling`, `onScroll`, `scrollTop` props to `Table`
- Use `height` render props from `WindowScroller` in `Table` instead
```javascript
<WindowScroller>
  ({ height, isScrolling, onChildScroll, scrollTop, registerChild }) => {
    return (
      <div ref={registerChild}>
        <AutoSizer disableHeight>
          ({width}) => {
          return (
            <Table
              autoHeight
              width={width} // pass by AutoSizer
              height={height} // pass by WindowScroller
              headerHeight={40}
              rowHeight={100}
              rowCount={data.length}
              rowGetter={({ index }) => data[index]}
              isScrolling={isScrolling}
              onScroll={onChildScroll}
              scrollTop={scrollTop}
            >
             {columns}
            </Table>
          )}
        </AutoSizer>
      </div>
    )
  }
 
<WindowScroller>
````

### `CellMeasurer`
Allow dynamic row height based on cell's content instead of fixed row height
- Wrap cell content with `CellMeasurer`
- Create `CellMeasurerCache` where stores the measurements
- Use `cache.clearAll()` when render `Table`/`List` which needs to reflow content due to a resizing event for a responsive layout (e.g., window resize could affect cell's height)

```javascript
// fixed width, dynamic height
const cache = new CellMeasurerCache({
  fixedWidth: true,
  defaultHeight: 60,
  minHeight: 60,
});

const cellRenderer = ({cellData, rowData, key, parent, rowIndex, style, columnIndex}) => (
  <CellMeasurer
    cache={cache}
    columnIndex={columnIndex}
    key={key}
    parent={parent}
    rowIndex={rowIndex}
  >
    {({ registerChild }) => (
      <div style={style} ref={registerChild}>
        {cellData}
      </div>
    )}
  </CellMeasurer>
)

<Table
  {...otherProps}
  cellRenderer={cellRenderer}
>
  {columns}
</Table>
````

## Issue
Expected behavior: 從 detil page 按下返回按鈕回到 `Table` 的畫面時，希望畫面自動下滑至原先的位置高度

Actual behavior: `window.scrollTo(x, y)` is not working in `useEffect` when using `Table` with `WinodwScrolle`
- 嘗試另外設計一個 `button` 在頁面render完後，以手動 trigger `window.scrollTo(x, y)` 發現下滑功能正常。
- `console.log` 查看 `document.body.scrollHeight` 發現在 component `render` 完成後， `useEffect` 中讀到的值跟 `window.innerHeight` 相同 (意即是不可滑動的狀態，`Table`高度還沒長出來 ?)
- 嘗試用 `setTimeout` delay 100ms 後再執行`window.scrollTo(x, y)`發現就可以正常下滑了 (此時的 `document.body.scrollHeight` 也已經被展開，大於 `window.innerHeight`)
- 即使把 `setTimeout` delay 時間改為 0，也同樣可以運作 (`setTimeout` 時間設為 0 不代表馬上執行 `callback`，而是代表 `callback` 會被堆到 Event Queue，等 main thread 清空之後就會被執行)
- 原因是當 page rendering 和 呼叫 `window.scrollTo(x, y)` 的時候，會產生競爭，In effect, **running JavaScript blocks the updating of the DOM**，因為 `DOM` 還來不及 update，所以 `window.scrollTo(x, y)` 也無法發揮作用 (因此時 `document.body.scrollHeight` === `window.innerHeight`)
- 簡單來說，解決方法是先讓 page render 完成，再執行其他的 js function
- 猜測只發生在用 `react-virtualized` 的情況下也許是因為還需要花資源計算 `Table` 高度
- 發現有個東西叫 `window.requestAnimationFrame`，此函數接受一個 `callback function`，一般用在優化動畫。機制是 browser 會在下次進行 repaint 之前，根據本身的效率，計算出算好的時機來執行這個 `callback function`。功能類似`setTimeout` / `setInterval`，但不須自己定義頻率

