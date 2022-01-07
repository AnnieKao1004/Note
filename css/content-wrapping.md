# Content Wrapping

## `white-space` CSS property

Specify how white-space is handled.

- **normal** (default)  
  Text will wrap when necessary.
- **nowrap**  
  Sequences of whitespace will collapse into a single whitespace. Text will _never_ wrap to the next line until `<br>` is met.

## `overflow-wrap` CSS property (alias `word-wrap`, more browser support)

Specify whether the browser can break lines with long words, if overflow.

- **normal** (default)  
  Default line breaking behavior. (break when there is soft wrap opportunity, e.g., white space)
- **anywhere (break-word)**
  Long words will be break if overflow. (會先找 soft wrap point 換行，如果沒有或換行後還是 overflow 才直接斷單詞)

## `word-break` CSS property

- **normal** (default)  
  Default line breaking behavior.
- **break-all**  
  Word may be broken at any character (最暴力，不管是否有 soft wrap point 可以換行，直接切斷單詞)
- **keep-all**
  Same behavior as normal for Non-CJK (Chinese/Japanese/Korean) text. 不允許 CJK 中單詞換行
