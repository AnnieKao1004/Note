# Properties for flex

shorthand:

```css
flex: [flex-grow] [flex-shrink] [flex-basis];
/* default => flex: 0 1 auto */
```

example html:

```html
<div class="container">
  <div class="box1"></div>
  <div class="box2"></div>
  <div class="box3"></div>
</div>
```

## flex-grow (default 0, do not expand)

剩餘空間的分配

if container width = 1000px, each box = 100px => **remaining width 1000 - (100 \* 3) = 700px**

- senario 1

```css
.box1 {
  flex-grow: 1;
}
```

box1 會佔據所有 remaining width, width 變為 100px (original width)+ 700px (remaining width) = 800px

- senario 2

```css
.box1,
.box2 {
  flex-grow: 1;
}
```

box1 & box2 會平分所有 remaining width, width 變為 100px (original width)+ 700 (remaining width) / 2 px = 450px
**note:**

## flex-shrink (default 1, 每個 item 壓縮相同比例)

空間不夠時的壓縮比例

if container width = 1000px, box1 width = 300px; box2 width = 400px; box3 width = 500px; => **width 超過 (300 + 400 + 500) - 1000 = 200px**

- senario 1

```css
.box1,
.box2,
.box3 {
  flex-shrink: 1;
}
```

根據**width 比例**及**flex-shrink 權重**去壓縮:

box1 壓縮 => 200 (超過的 width) _1 (flex-shrink)_ 300 (width) / 1200 (total width) = 50 px

box2 壓縮 => 200 _1_ 400 / 1200 = 67 px

box3 壓縮 => 200 _1_ 500 / 1200 = 83 px

## flex-basis (default auto, means look at width or height)

類似 width, 但優先程度較高 (可設為 0 / ?px / ?%, ?rem, etc)

Ref: [Day24 Flex 空間分配 flex-grow / flex-shrink / flex-basis](https://ithelp.ithome.com.tw/articles/10208741)

## Use Cases

> 需求: 在一 container 中，Product Logo 需垂直置中，Company Logo 放在最底

**Use pseudo-elements:**

- 在要置中的 Product Logo 前插入一個內容空白的 pseudo-element
- 將 Product Logo 上面的 pseudo-element 及下面的 Company Logo 分別設定 `flex: 1` (means `flex: 1 1 0`, pseudo-element 和 Company Logo 會平分剩餘的空間，所以中間的 Product Logo 就會致中)
- `flex: 1` 和 `flex-grow: 1`不同在於前者 `flex-basis: 0`，後者 `flex-basis: auto`。當 `flex-basis: auto`，總空間會扣掉該 flex item 佔的空間後，剩餘空間再依 `flex-grow` 比例分配。當 flex item 的 `flex-basis: 0`， 該 flex item 佔的空間會被忽略，總空間會扣掉其他 item 佔的空間，剩餘空間再依 `flex-grow` 比例分配。

  ```css
  .container {
    background-color: #e6f7ff;
    display: flex;
    flex-direction: column;
    algin-items: center;
    justify-content: center;
    text-align: center;

    &::before {
      content: "";
      flex: 1;
    }

    .companyLogo {
      flex: 1;
      display: flex;
      flex-direction: column;
      justify-content: flex-end;
      align-items: center;
    }
  }
  ```

- References
  - [Flexbox: Align between bottom and center?](https://stackoverflow.com/questions/33944163/flexbox-align-between-bottom-and-center)
  - [The difference between flex:1 and flex-grow:1](https://stackoverflow.com/questions/45767405/the-difference-between-flex1-and-flex-grow1)
