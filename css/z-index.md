
# Z-index
- Sets the z-order of a **positioned element** (one with `position` other than `static`)
- A **stacking context** is formed by element with:
1.  `position` `absolute` or `relative` and `z-index` other than `auto`
1.  `position` `fixed` or `sticky`
1.  child of a `flex` container, with `z-index` other than `auto`
1. `transform` properties other than `none`  
[See all](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Positioning/Understanding_z_index/The_stacking_context) 

-Checking steps if z-index is not working:
1. Check if the element is **positioned**
1. 如果1成立，判斷是否欲覆蓋層的z-index比被覆蓋層的z-index數值大
1. 如果2成立，判斷是否欲覆蓋層的父級元素比被覆蓋層的z-index數值大
1. 如果3成立，判斷是否欲覆蓋層的父級元素比被覆蓋層的父級層的z-index數值大
