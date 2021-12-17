# Redux Persist
## Intro
- [`Redux Persist`](https://github.com/rt2zz/redux-persist) is a library that stores redux state in browser storage
- `persistReducer`
  ```javascript
  import { persistReducer } from 'redux-persist'
  import storage from 'redux-persist/lib/storage'
  import rootReducer from './reducers'

  const persistConfig = {
    key: 'root',
    storage,
  }

  const persistedReducer = persistReducer(persistConfig, rootReducer)
  ```
- `persistStore`
  ```javascript
  import { createStore } from 'redux'
  import { persistStore } from 'redux-persist'

  export default () => {
    let store = createStore(persistedReducer)
    let persistor = persistStore(store)
    return { store, persistor }
  }
  ```
- When using React, pass `store` to redux `Provider`, wrap app with `PersisGate` and pass `persistor` to `PersisGate`
  ```javascript
  import { PersistGate } from 'redux-persist/integration/react'
  import { store, persistor } from '...'

  const App = () => {
    return (
      <Provider store={store}>
        <PersistGate loading={null} persistor={persistor}>
          <RootComponent />
        </PersistGate>
      </Provider>
    )
  }
  ```
  
## How it work
Three main steps:
1. @@INIT
2. persist/PERSIST  
  For each persisted state object, add the property `_persist: { version: -1, rehydrated: false }`
4. persist/REHYDRATE  
  This step is where the persisted data stored in browser storage replace the data in Redux store. property changes to `_persist: { version: -1, rehydrated: true }`
  
Ref: [REDUX-PERSIST: HOW IT WORKS AND HOW TO CHANGE THE STRUCTURE OF YOUR PERSISTED STORE](https://blog.bam.tech/developer-news/redux-persist-how-it-works-and-how-to-change-the-structure-of-your-persisted-store)
  
  
## Issue:
  When reset redux state at logout by passing `undefined` to reducer state ([from this answer](https://stackoverflow.com/questions/35622588/how-to-reset-the-state-of-a-redux-store)), the function of `redux-persist` will be broken as the `_persist` key in state object is removed.
  ```javascript
  const rootReducer = (state, action) => {
    if (action.type === LOG_OUT) {
      return appReducer(undefined, action)
    }
    return appReducer(state, action)
  }
  ```
  Try `persistor.purge()` to clear the persisted state in browser and then `persistor.persist()` to resume the persistence  
  => not working as `rehydrated` remain `false` ?
  
  
  
