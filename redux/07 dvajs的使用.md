## dvajs的使用

`index.js`

```js
import dva from 'dva';
import './index.css';

// 1. Initialize
//设置初始值
- const app = dva();
+ const app = dva({
+   initialState: {
+     products: [
+       { name: 'dva', id: 1 },
+       { name: 'antd', id: 2 },
+     ],
+   },
+ });

// 2. Plugins
// app.use({});

// 3. Model  载入
app.model(require('./models/example').default);
app.model(require('./models/indexTest').default);

// 4. Router
app.router(require('./router').default);

// 5. Start
app.start('#root');
```

`router.js`

```js
import React from 'react';
import { Router, Route, Switch } from 'dva/router';
import IndexPage from './routes/IndexPage';

function RouterConfig({ history }) {
  return (
    <Router history={history}>
      <Switch>
        <Route path="/" exact component={IndexPage} />
      </Switch>
    </Router>
  );
}

export default RouterConfig;
```

`models/products.js`

```js
import * as apis from '../services/example.js';
export default {

  namespace: 'example',

  state: {
      name："lbb"
  },

  subscriptions: {
    setup({ dispatch, history }) {  // eslint-disable-line
    },
  },

  effects: {
    *fetch({ payload }, { call, put }) {  // eslint-disable-line
      let res = yield call(apis.testNode)
      // console.log(res);
      return res
    },
    *fetch({ payload }, { call, put }) {  // eslint-disable-line
      yield put({ type: 'save' });
    },  
  },

  reducers: {
    save(preState, action) {
      console.log(action);
      return { ...preState, ...action.payload };
    },
  },

};
```

`routes/Products.js`

```js
import React from 'react';
import { connect } from 'dva';
import ProductList from '../components/ProductList';

const Products = ({ dispatch, products }) => {
  function handleDelete(id) {
    dispatch({
      type: 'products/delete',
      payload: id,
    });
  }
  return (
    <div>
      <h2>List of Products</h2>
      <ProductList onDelete={handleDelete} products={products} />
    </div>
  );
};

const mapStateToprops = (state) => {
  return {
    name: state.example.name,
  };
};
// export default Products;
export default connect(mapStateToprops)(Products);
```

