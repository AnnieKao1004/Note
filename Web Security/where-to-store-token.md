# Where to save token in Front-End ?
### Option 1: Web Storage (`Local Storage`, `Session Storage`, etc.)
- Prone to XSS attack (can be accessed using JavaScript `window.localStorage`)
### Option 2: Cookie
- Not vulnerable to XSS attack (cookies with `httpOnly` and `secure` cannot be accessed using JavaScript)
- Prone to CSRF attack
- CSRF attack can be mitigate by adding `sameSite` flag (specify that cookies cannot be sent with `cross-site` requests; 意即此 cookie 不應在 cross-site request 時被帶上)
### Option 3: Store access_token in memory, `refresh_token` in cookie
- Safe from XSS attack
- Safe from CSRF attack 
  - If attach by submitting form, response from `/refreshToken` api cannot be read
  - If attack by making a `fetch` or `AJAX` request, this requires the server's CORS policy to be set up correctly to prevent requests from unauthorized websites
- 可利用 `set-cookie` 的 `Path` 限制會傳送 `refresh_token` cookie 的 request url
- 實際應用發現，因為 cookie 是 browser & domain base (不同分頁存在同一批 cookie, 這些 cookie 又分別屬於各自的 domain)。因此當在同個 browser 開不同分頁登入不同使用者時，第一個使用者 (User A) 登入時利用 `set-cookie` 將 `refresh_token` 存在 cookie 中， 當另一個使用者 (User B) 使用另一個分頁登入時 ，又再次 `set-cookie` 把原先 User A 的 `refresh_token` 覆蓋掉。造成 User A 分頁在 refreshToken 時，也會拿這個新的 (User B 的) `refresh_token` 去取得 token，取得 token (其實是 User B 的) 後，身分就變成 User B 了。  
- 解法: 
  - 使用者開新分頁登入另一個帳號時，舊分頁的帳號就讓他覆蓋過去 (refreshToken 的時候要連同使用者資訊一併更新)
  - 若是不要覆蓋過去，可以把 refreshToken 存在 `session-storage`，發起 refreshToken request 時再拿出帶上，這樣才會是 session base。(但回到 Option 1 的問題，安全性較低)



References:
- [LocalStorage vs Cookies: All You Need To Know About Storing JWT Tokens Securely in The Front-End](https://dev.to/cotter/localstorage-vs-cookies-all-you-need-to-know-about-storing-jwt-tokens-securely-in-the-front-end-15id)
- [讓我們來談談 CSRF](https://blog.techbridge.cc/2017/02/25/csrf-introduction/)
