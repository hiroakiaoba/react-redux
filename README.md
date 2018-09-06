# react-redux

```
yarn add redux react-reduxn
```

## Action
- action -> アプリ内で何が起きたかのデータ、JavaScriptのobject
src/actions/index.js

type: キー を持つ
キーはユニークでなければならない。
action(typeを持つJSオブジェクト)を返す関数をアクションクリエーターと呼ぶ。
type:の値は変数に入れてReducerでも使えるようにする。exportする！
*複数の場所で使う値は変数に入れ、exportする。

## Reducer
- actionが発生した時に、そのtypeに応じて状態をどう変化させるのかを定義。
src/reducers/index.js
src/redeucers/count.js

index.jsで全てのReducerを結合する ->
- import { combineReducers } from 'redux'; // reducerを結合させるもの
- import count from './count';
- export default combineReducers({ count, foo, bar })
(foo, bar はもしあったら書く)
まとめたobjectをexportし、storeで利用できるようにする。

<Reducer内の関数>
- 引数を二つ持つ(state = default値を設定する, action)
- 関数内で受け取ったactionのtypeに応じてstateを変更して返す。

## Store
- Reducerを元に作成する。
- storeを全てのComponentで使用できるようにする。

```
# src/index.js
import { createStore } from 'redux';
import { Provider } from 'react-redux'; // 全Componentに受け渡しする機能を持つComponent

import reducer from './reducers';

const store = createStore(reducer); // storeの作成,アプリ内で唯一のもの,アプリ内部の全てのstateがこのstoreに集約されている。

ReactDOM.render(
  // Providerでラップし、storeを渡すことで、全てのComponentから、storeが利用できるようになる。これによって、親->子->孫のようにpropsをバケツリレーで渡す必要はなくなる。
  <Provider store={store}>
    <App />
  </Provider>,
  document.getElementById('root')
);
```

## connectでstateとactionを関連づける
- Componentで使えるようにする。

```
import { connect } from 'react-redux';
import { increment, decrement } from '../actions'; // action(typeを持つ)をimport
```
- クラスComponentとconstructorは不要となる -> reducerがその役割を担う(stateの初期値設定)
- reducerで定義しているため、イベントが発火した時に渡していた関数も不要

->> render()内の処理
```
const props = this.props // インスタンスのpropsには状態やアクションを渡していくため

// stateで必要な情報を取り出して、Component内にpropsとしてマッピングする
// どういったobjectをpropsとして対応させるのかを記述
const mapStateToProps = state => ({ value: state.count.value });

// あるアクションが発生した時に、reducerにtypeに応じた状態遷移を実行させるための関数 -> dispatch
const mapDispatchToProps = dispatch => ({
  increment: () => dispatch(increment()), // importしたincrement関数をdispatchに渡す
  decrement: () => dispatch(decrement())
})

export default connect(mapStateToProps, mapDispatchToProps)(App)

```
