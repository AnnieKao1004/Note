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



References:
- [LocalStorage vs Cookies: All You Need To Know About Storing JWT Tokens Securely in The Front-End](https://dev.to/cotter/localstorage-vs-cookies-all-you-need-to-know-about-storing-jwt-tokens-securely-in-the-front-end-15id)
- [讓我們來談談 CSRF](https://blog.techbridge.cc/2017/02/25/csrf-introduction/)
