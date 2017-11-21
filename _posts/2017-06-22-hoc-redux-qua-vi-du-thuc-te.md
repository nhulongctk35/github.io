---
layout: post
title: Học Redux qua ví dụ thực tế
date: 2017-06-22
---

Trong quá trình tìm hiểu về [Redux](http://redux.js.org/), tôi nhận ra rằng mình đã hiểu sai về [Flux](https://facebook.github.io/flux/docs/in-depth-overview.html#content) thông qua những bài viết mà tôi đã đọc. Tôi không có ý là những bài viết đó  không tốt mà là do tôi hiểu sai về những những khái niệm ( **actions / actions creators, store, dispatcher, etc** ).
Cho đến khi tôi bắt đầu sử dụng Redux. Tôi nhận ra rằng Flux đơn giản hơn nhiều so với những gì tôi đã hiểu. Tất cả đều nhờ Redux được thiết kế rất tốt, theo quan điểm của cá nhân tôi thì cách tốt nhất để hiểu Flux là tìm hiểu Redux trước. Đó là lý do tại sao tôi muốn chia sẽ những gì mà tôi hiểu về Redux với các bạn.
Có thể bạn đã thấy sơ đồ này, sơ đồ này mô tả về khái niệm [unidirectional data flow](https://medium.com/@AdamRNeary/unidirectional-data-flow-yes-flux-i-am-not-so-sure-b4acf988196c) của Flux.
![](https://viblo.asia/uploads/07a97662-64eb-4757-8923-bc8426099687.png)

Trong bài viết này, Tôi sẽ dần dần giới thiệu cho các bạn về những khái niệm trong sơ đồ trên nhưng thay vì cố gắng giải thích tổng thể về diagram trên. Tôi sẽ chia nhỏ nó ra từng phần và gố gắng giải thích tại sao nó lại tồn tại và vai trò của nó trong sơ đồ này là gì. Cuối cùng bạn sẽ hiểu được toàn bộ sơ đồ trên khi đã hiểu được từng phần. Tôi chắc chắn rằng  sau khi đọc bài viết này bạn sẽ hiểu hơn về Redux và dễ dàng tham gia vào những dự án thực tế của công ty.
 ## 1. Action creator
Những dòng code dưới đây sẽ nói lên tất cả action creator là gì.
```javascript
// Action creator chẵng qua là 1 function ...
var actionCreator = function() {
  // ... mà tạo ra 1 action (thực sự cái tên actionCreator đã nói lên tất cả) và nó return
   return {
     type: 'AN_ACTION'
  }
}
```

> Note: `Action trong Redux là một object mà bắt buộc phải có ít nhất 1 property là "type". Mục đích của prop Type này là để Reducer nhận biết được là Action nào`.
Action ngoài prop type nó còn có thể bao gồm nhiều props khác để truyền data qua Reducer.

```javascript
// Gọi action creator func
console.log(actionCreator())
// Output: { type: 'AN_ACTION' }
```

## 2. CreateStore
Những actions mà chúng ta xử lý trong ứng dụng không chỉ thông báo cho chúng ta biết là something happened mà
còn thông báo cho chúng ta biết dữ liệu (data) nào cần được update.

Chúng ta khởi tạo redux như sau.
```javascript
import { createStore } from 'redux';
var store = createStore();
```

Nhưng nếu bạn run đoạn code trên, bạn sẽ nhận được thông báo lỗi như sau.
`// Error: Invariant Violation: Expected the reducer to be a function.`
Thông báo lỗi ở trên là vì hàm createStore cần truyền vào một func để hold toàn bộ state của ứng dụng.

OK, tôi sẽ sữa lại như sau.
```javascript
import { createStore } from 'redux';

var store = createStore(() => {});
```

## 3. Reducer
Khi khởi tạo store (createStore) chúng ta cần truyền vào một reducer func và Redux sẽ call func này mỗi khi
có một action nào đó xảy ra.
```javascript
var reducer = function (state, action) {
  console.log('Reducer này đã được gọi cùng với state', state, ' va action ', action);
};

var store = createStore(reducer);
// Output: Reducer này đã được gọi cùng với state undefined va action { type: '@@redux/INIT' }
```
Huh? Lần đầu tôi học Redux thực sự là tôi rất confuse vì output ở trên, vấn đề là tôi chưa dispatch
ra bất kỳ action nào mà reducer func vẫn log ra đoạn text trên. Nhìn kĩ hơn thì tôi thấy đoạn code trên có action type là
**{ type: '@@redux/INIT' }**. Hỏi thánh Google thì tôi biết đó là khi khởi tạo Redux, Redux sẽ bắn ra (dispatch) một action có type là: **{ type: '@@redux/INIT' }**.

Huh?, tôi nghĩ ở trên là ví dụ đơn giản nhất về Reducer, Nhưng trong dự án thực tế thì 1 Reducer sẽ trông như
thế nào? Hãy cùng tôi phân tích đoạn code dưới đây.
```javascript
const loginReducer = function (state = { isAuthenticated: false }, action) {
    console.log('loginReducer đã được gọi với state', state, 'và action', action);

    switch (action.type) {
        case 'LOGIN_SUCCESS':
          return {
            isAuthenticated: true,
            message: action.message
          }
        default:
            return state;
    }
};

var store_2 = createStore(loginReducer);

// Output: loginReducer đã được gọi với state {} và action { type: '@@redux/INIT' }
```
Chúng ta nhận được Output này là hoàn toàn make sense đúng ko? Hãy thử log ra state xem nào.
```javascript
console.log('loginReducer state sau khi khởi tạo:', loginReducer.getState());
// Output: loginReducer state sau khi khởi tạo: { isAuthenticated: false }
```

Trong thực tế thì chúng ta không chỉ có 1 Reducer mà có rất nhiều Reducers, mỗi Reducer lại
keep state riêng của nó. Vậy làm thế nào để chúng ta có thể gom toàn bộ state từ nhiều reducers lại với nhau?
Tôi ví dụ ngoài **loginReducer** ở trên, tôi có thêm những Reducers khác như sau.
```javascript
var userReducer = function (state = {}, action) {
    console.log('userReducer đã được gọi với state', state, 'và action', action)

    switch (action.type) {
        default:
            return state;
    }
}

var itemsReducer = function (state = [], action) {
    console.log('itemsReducer đã được gọi với state', state, 'và action', action)

    switch (action.type) {
        // etc.
        default:
            return state;
    }
}
```

Để combine những reducer lại với nhau, Redux đã cung cấp cho chúng ta 1 func helper là **combineReducers**.
Hãy nhìn đoạn code sau.
```javascript
import { createStore, combineReducers } from 'redux';

var rootReducer = combineReducers({
    user: userReducer,
    items: itemsReducer
});

var store = createStore(reducer);
// Output:
// userReducer đã được gọi với state {} và action { type: '@@redux/INIT' }
// itemsReducer đã được gọi với state [] và action { type: '@@redux/INIT' }
```

Huh? Với Output ở trên thì chúng ta thấy rằng mỗi Reducer sẽ được gọi mỗi khi có 1 action nào đó xảy ra, trong
trường hợp này là **action INIT** mặc định của Redux.

Hãy thử console log ra rootReducer xem có gì?
```javascript
console.log(store.getState());
// Output:
// { user: {}, items: [] }
```
Wow, bây giờ thì các bạn cũng đã hiểu tại sao người ta lại đặt nó với cái tên là Reducer rồi, Như Output ở trên thì chúng
ta thấy rằng, mỗi reducer sẽ hold 1 state riêng, như vậy là nó đang reduce (chia nhỏ) state của ứng dụng và
rồi được combine lại nhờ **combineReducers** helper của Redux.

## 4. Dispatch action
Ở trên tôi có nhắc đến **dispatch** nhưng chưa demo cho các bạn thấy thực chất dispatch là gì. OK, cùng tôi phân tích ví dụ
dưới đây.
 ```javascript
const loginReducer = function (state = { isAuthenticated: false }, action) {
    console.log('loginReducer đã đươc gọi với', state, 'and action',  action);

    switch (action.type) {
        case 'LOGIN_SUCCESS':
          return {
            isAuthenticated: true,
            message: action.message
          }

        default:
            return state;
    }
};


var userReducer = function (state = {}, action) {
    console.log('userReducer đã được gọi với', state, 'and action', action)

    switch (action.type) {
        case 'SET_NAME':
            return {
                ...state,
                name: action.name
            }
        default:
            return state;
    }
};

import { createStore, combineReducers } from 'redux';

var rootReducer = combineReducers({
    user: userReducer,
    login: loginReducer,
});
var store = createStore(rootReducer);
console.log('store state sau khi khoi tao:', store.getState());
// store state sau khi khoi tao: { login: { isAuthenticated: false }, user: {} }

// Để dispatch 1 action, chúng ta đơn giản dùng
store.dispatch({
    type: 'SET_NAME',
    name: 'Long Tran'
});

console.log('store state sau khi dispatch action SET_NAME:', store.getState());
// Output: store state sau khi dispatch action SET_NAME: { login: { isAuthenticated: false }, user: { name: 'Long Tran' } }
```

Cho đến hiện tại, các bạn đã tìm hiểu được những thứ mà tôi nghĩ chúng ta cần phải biết khi làm việc
với Redux.
> // ActionCreator -> Action -> dispatcher -> reducer

Những ví dụ mà tôi chia sẽ ở trên có thể phần nào giúp bạn hiểu rõ hơn về Redux, nhưng trong dự án thực tế
thì còn nhiều khái niệm liên quan như **async action**, **middleware**, ... mà bạn cần phải biết nữa. Trong khuôn khổ 1 bài viết
tôi không thể cover toàn bộ hết được, hy vọng những bài viết sau tôi sẽ cover tiếp phần đang còn thiếu này.
Tôi có 1 repo mà tôi code lúc tôi mới học Redux, hy vọng sẽ giúp ích cho các bạn.
> https://github.com/nhulongctk35/react-app-redux

`Những bài viết, tại liệu hay mà tôi nghĩ các bạn nên đọc qua để có thể pro khi code React`
> https://medium.com/@learnreact/container-components-c0e67432e005
> https://facebook.github.io/react/docs/thinking-in-react.html
> https://ifelse.io/2016/10/20/introducing-react-components/
> https://medium.com/@nimelrian/thinking-in-react-a-paradox-statement-33c19e2eb9e2
