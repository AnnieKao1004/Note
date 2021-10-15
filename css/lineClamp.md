# line-clamp

Truncates text at a specific number of lines. (Introduce the ellipsis not on the first line but somewhere else)

```css
.line-clamp {
  display: -webkit-box;
  -webkit-line-clamp: 3; // ellipsis at line 3
  -webkit-box-orient: vertical;  
  overflow: hidden;
}
```
