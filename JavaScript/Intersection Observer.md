# Intersection Observer (Web API)

### Intro
- To **asynchronously** observe changes in the intersection of a **target element** & **root element (ancestor element or viewport)** (用於檢測兩元素相交情況的變化)
- Use cases:
  - lazy loading (loading images or content as page is scroll)
  - infinite scrolling
  - deciding whether or not to perform tasks/animation based on whether or note the user can see the result
  - reporting of visibility of advertisements
  - provide a signal when a `position: sticky` element becomes fixed (e.g., apply shadow to header when it's fixed)

### Usage
1. Creating an intersection observer
  ```javaScript
  let callback = (entries, observer) => {
  entries.forEach(entry => {
    // Each entry describes an intersection change for one observed
    // target element:
    //   entry.boundingClientRect
    //   entry.intersectionRatio (target 有多少比例出現在 root 中)
    //   entry.intersectionRect
    //   entry.isIntersecting
    //   entry.rootBounds
    //   entry.target
    //   entry.time
  });
};
  
  let options = {
    root: null // document.querySelector('#scrollArea')
    rootMargin: "0px",
    threshold: 0 // the observer's callback will be executed at this percentage of target visibility (can be either a single number or array of numbers), default is 0 (means the cb will be run as soon as one pixel is visible)
  }
  
  let observer = new IntersectionObserver(callback, options)
  ```
2.  Targeting an element to be observed
  ```javaScript
  let target = document.querySelector('#target');
  observer.observe(target);
  ```

### Examples
- [Image lazyload](https://codepen.io/AnnieYC/pen/WNEKzao?editors=1011)
- [Infinite Scrolling](https://codepen.io/AnnieYC/pen/XWaPdGj?editors=1111)

### Reference
- https://developer.mozilla.org/en-US/docs/Web/API/Intersection_Observer_API
- https://www.letswrite.tw/intersection-oserver-basic/
- https://www.letswrite.tw/intersection-oserver-demo/
