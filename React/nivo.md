# [nivo](https://github.com/plouc/nivo)
**nivo** is a npm package of react components for dataviz

- **Issue:**

  Radar Chart does not re-render when data is updated in production environment (work normally in local developement)
  - Browser console shows error message `TypeError: r.willAdvance is not a function`
  - Search results show that the issue relates to `react-spring` (dependency of `nivo`) 

- **Cause**
  - The issue was that `sideEffects: false` was being used in `package.json` when it shouldn't be, causing function calls to be `tree-shake`
  - Only work in production mode
  - `react-spring` [Issue #1078](https://github.com/pmndrs/react-spring/issues/1078)

- **Solution**
  1. Manually remove `"sideEffects": false` from the `package.json` of every `node_modules/@react-spring/*` directory, then use `patch-package` to auto-apply those changes when node_modules is re-installed
  2. When using `create-react-app`, you can modify webpack config without ejecting by using `@craco/craco`
      ```javascript
      // craco.config.js
      module.exports = {
        webpack: {
          configure: {
            module: {
              rules: [
                {
                  test: /react-spring/,
                  sideEffects: true
                }
              ]
            }
          }
        }
      };
      ```

- **Ref**  
  What is `tree-shake` & `sideEffects` in `webpack` ([webpack](https://webpack.js.org/guides/tree-shaking/#mark-the-file-as-side-effect-free))

  > - `Tree shaking` is a term commonly used in the JavaScript context for *dead-code elimination*.  
  > (意即當 `webpack` 打包時，會檢查哪些程式碼是沒有用到的，沒有用到的程式碼不會被打包)  

  > - An example of `sideEffects` is when a function modifies something or performs special behavior outside of its own scope (程式碼雖然沒有被 `export` 但還是會被執行ㄝ, e.g., `console.log()`, make api calls, manipulate DOM)  
  > `"sideEffects": false` in `webpack config` / `package.json` is a hint to webpack's compiler that a specific package is *free of side effect* and *the unused exports are safe to be eliminated*  
  > - 標示哪些 `module` 有 `side effects`:
  >   ```javascript
  >   "sideEffects": ["./index.js", "./configure.js"]
  >   ```
  >  - (若 `module`被標記為 no side effect, 且 `export` 未被使用，打包時會被排除)
