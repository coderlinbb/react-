## redux的基本使用（Redux Toolkit）

```bash
# NPM
npm install @reduxjs/toolkit

# Yarn
yarn add @reduxjs/toolkit

#创建一个 React Redux 应用
#官方推荐的创建 React Redux 新应用的方式是使用 官方 Redux+JS 模版，它基于 Create React App，它利#用了 Redux Toolkit 和 Redux 与 React 组件的集成.
npx create-react-app my-app --template redux
```

> store.js

```js
import { configureStore } from '@reduxjs/toolkit'
import counterSlice from './count_reducer.js'

const store = configureStore({
  reducer: counterSlice.reducer
})
```

>  count_action.js

```js

```

>  count_reducer.js

```js
import { createSlice } from '@reduxjs/toolkit'

const counterSlice = createSlice({
  name: 'counter',
  initialState: {
    value: 0
  },
  reducers: {
    incremented: state => {
      // Redux Toolkit allows us to write "mutating" logic in reducers. It
      // doesn't actually mutate the state because it uses the Immer library,
      // which detects changes to a "draft state" and produces a brand new
      // immutable state based off those changes
      state.value += 1
    },
    decremented: state => {
      state.value -= 1
    }
  }
})

export const { incremented, decremented } = counterSlice.actions
export default counterSlice
```

>  在组件中使用

```jsx
import {incremented,decremented} from './count_reducer.js'

// Still pass action objects to `dispatch`, but they're created for us
store.dispatch(incremented())
// {value: 1}
store.dispatch(incremented())
// {value: 2}
store.dispatch(decremented())
// {value: 1}
```

>  index.js 

```js
//需要在这里更新响应
store.subscribe(()=>{
	ReactDOM.render(<App/>,document.getElementById('root'))
})
```



