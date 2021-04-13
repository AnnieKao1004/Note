# Properties for flex item

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
````

### flex-grow (default 0, do not expand)
剩餘空間的分配

if container width = 1000px, each box = 100px => **remaining width 1000 - (100 * 3) = 700px**

- senario 1
```css
.box1 {
  flex-grow: 1;
}
```
box1會佔據所有remaining width, width 變為 100px (original width)+ 700px (remaining width) = 800px

- senario 2
```css
.box1, .box2 {
  flex-grow: 1;
}
```
box1 & box1 會平分所有remaining width, width 變為 100px (original width)+ 700 (remaining width) / 2 px  = 450px

### flex-shrink (default 1, 每個item壓縮相同比例)
空間不夠時的壓縮比例

if container width = 1000px, box1 width = 300px; box2 width = 400px; box3 width = 500px; => **width 超過 (300 + 400 + 500) - 1000 = 200px**

- senario 1
```css
.box1, .box2, .box3 {
  flex-shrink: 1;
}
```
根據**width比例**及**flex-shrink權重**去壓縮:

box1 壓縮 => 200 (超過的 width) * 1 (flex-shrink) * 300 (width) / 1200 (total width) = 50 px

box2 壓縮 => 200 * 1 * 400 / 1200 = 67 px

box3 壓縮 => 200 * 1 * 500 / 1200 = 83 px


### flex-basis (default auto, means look at width or height)
類似width, 但優先程度較高 (可設為0 / ?px / ?%, ?rem, etc)

Ref: [Day24 Flex 空間分配 flex-grow / flex-shrink / flex-basis](https://ithelp.ithome.com.tw/articles/10208741) 
