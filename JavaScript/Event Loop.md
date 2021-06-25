# Event Loop
- Javascript runtime (執行環境): browser or node.js
- 以 chrome 來說，V8 javascript engine 本身並沒有提供像 `setTimeout`, `DOM`操作 或 發 `HTTP request` 這些 `api`，這些是由 browser 另外提供的 `web APIs` (在 node.js 裡是 C++ api)。除此之外還有，`callback queue` & `event loop`
- `blocking` = `synchronous`: 程式會卡在那一行，在結束前無法繼續往下執行
- `non-blocking` = `asynchronous`: 程式會繼續往下執行，並將回傳值傳給 `callback function`處理
- 如果 `request` 是 `synchronous` 的 (影片作者用 `while loop` 來模擬同步的情況)，在 `request` 結束前，browser 無法做其他事 (like render or run any other code)
- To not block the stack => using `asynchronous callback` (There is almost no blocking function in browser & `node`)
- How `asynchronous` work ? => `Event Loop` !
  - JavaScript can only do one thing at a time (single thread) ?  => *JavaScript runtime* can only do one thing at a time, but browser is more than just *JavaScript runtime*, it has other effectively threads, you can just make calls to
  - 考慮以下 code 的執行順序
    ```javascript
    console.log('start');
    
    setTimeout(() => console.log('cb'), 5000);
    
    console.log('end')
    ```
    Event Loop (check `call stack` and pick up `callback function` from `task queue`)
    1. 執行主程式 main() 
    1. 執行 `console.log('start')` (on `call stack`)
    1. 執行 `setTimeout` (on `call stack`), web api 開始計時 wait for 5s to put the callback function `() => console.log('cb')` to `task queue`
    1. 執行 `console.log('end')` (on `call stack`) 
    1. 5 秒後 callback function `() => console.log('cb')` 被放到 `task queue` 中
    1. 確認 call stack 已清空，task queue 中 的 callback function `() => console.log('cb')` 被放到 `call stack` 去執行  
     
     
    發送 `asynchronous request` 就類似 `setTimeout` 的行為，是由 web api 執行發 `request`，等到 request complete 後把 callback function 推到 `task queue`，再由 event loop 把 callback function 推到 `call stack`
    
#### Take notes of the following resource:
- [What the heck is the event loop anyway](https://2014.jsconf.eu/speakers/philip-roberts-what-the-heck-is-the-event-loop-anyway.html)
- [JavaScript 中的同步與非同步（上）](https://blog.techbridge.cc/2019/10/05/javascript-async-sync-and-callback/)
