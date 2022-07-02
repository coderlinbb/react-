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
import { configureStore } from '@reduxjs/toolkit';
import counterReducer from '../features/counter/counterSlice';

export const store = configureStore({
  reducer: {
    counter: counterReducer,
  },
});
```

>  counterSlice.js

```js
import { createAsyncThunk, createSlice } from '@reduxjs/toolkit';
import { fetchCount } from './counterAPI';

//下面的函数被称为一个thunk，它允许我们执行异步逻辑。   
//它可以像常规动作一样调度:' dispatch(incrementAsync(10)) '。   
//这将以' dispatch '函数作为第一个参数调用thunk。   
//异步代码可以被执行，其他动作可以被分派。
//Thunks通常用于异步请求。 

export const incrementAsync = createAsyncThunk(
  'counter/fetchCount',
  async (amount) => {
    const response = await fetchCount(amount);//异步请求
    // The value we return becomes the `fulfilled` action payload
    return response.data;
  }
);

export const counterSlice = createSlice({
  name: 'counter',
  initialState: {
    value: 0,
    status: 'idle',
  },
  // “reducers”字段允许我们定义reducers并生成相关的操作 
  reducers: {
    increment: (state) => {
      state.value += 1;
    },
    decrement: (state) => {
      state.value -= 1;
    },
    incrementByAmount: (state, action) => {
      state.value += action.payload;
    },
  },
// 'extraReducers'字段允许slice处理在其他地方定义的动作，  
//包括由createAsyncThunk或其他片中生成的动作。 
  extraReducers: (builder) => {
    builder
      .addCase(incrementAsync.pending, (state) => {
        state.status = 'loading';
      })
      .addCase(incrementAsync.fulfilled, (state, action) => {
        state.status = 'idle';
        state.value += action.payload;
      });
  },
});

export const { increment, decrement, incrementByAmount } = counterSlice.actions;


//下面的函数叫做selector，它允许我们从中选择一个值  状态。
//选择器也可以在使用它们的地方内联定义，而不是切片文件。 
//例如:' usselelector ((state: RootState) => state.counter.value) ' 
export const selectCount = (state) => state.counter.value;

//我们也可以手工编写thunks，它可以包含同步和异步逻辑。  
//下面是一个基于当前状态的有条件调度操作的示例。 
export const incrementIfOdd = (amount) => (dispatch, getState) => {
  const currentValue = selectCount(getState());
  if (currentValue % 2 === 1) {
    dispatch(incrementByAmount(amount));
  }
};

export default counterSlice.reducer;
```

>  在组件中使用

```jsx
import React, { useState } from 'react';
import { useSelector, useDispatch } from 'react-redux';
import {
  decrement,
  increment,
  incrementByAmount,
  incrementAsync,
  incrementIfOdd,
  selectCount,
} from './counterSlice';

const count = useSelector(selectCount) // 获取全局数据
const dispatch = useDispatch()  // 获取dispatch方法

dispatch(decrement())
dispatch(increment())
dispatch(incrementByAmount(2))
```

>  index.js 

```js
//需要在这里更新响应
import { Provider } from 'react-redux';

import React from 'react';
import { createRoot } from 'react-dom/client';
import { Provider } from 'react-redux';
import { store } from './app/store';
import App from './App';

const container = document.getElementById('root');
const root = createRoot(container);

root.render(
  <React.StrictMode>
    <Provider store={store}>
      <App />
    </Provider>
  </React.StrictMode>
);


```



