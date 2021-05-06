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

