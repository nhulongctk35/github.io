---
layout: post
title: "Tìm Hiểu Next.js Part 2"
date: 2018-03-21 16:10:13
categories: Web Development
meta: "Next.js, Tìm hiểu về Next.js part 2, Next.js tutorials"

---
Nối tiếp bài đầu tiên giới thiệu về Next.JS, Trong bài viết hôm nay mình sẽ implement 1 ví dụ cho 1 flow hoàn chỉnh. VD này mình áp dụng khá nhiều thứ, trong đó có [redux-saga](https://github.com/redux-saga/redux-saga),  inject **reducer va saga**. Vì không có nhiều thời gian cho bài viết nên mình chỉ show JSON trả về từ REST API. Bài viết sau mình sẽ refactor và implement phần UI. 

![](https://images.viblo.asia/a691cd01-61ed-45e3-a6cb-9e46bee46c27.png)
## User Page
Như bài trước mình giới thiệu thì **Next.JS** đã handle phần routing luôn rồi, ở đây mình tạo folder users trong folder pages, tương ứng với route */users*.
![](https://images.viblo.asia/3de92cf3-2ae9-499f-a2dd-6d0f45c37280.png)

### User Saga
Ở đây mình sử dụng public API để demo cho việc call API, nếu trong ứng dụng thực tế thì nên sử dụng các services để request REST API thay vì call trực tiếp thế này nhìn nó chuối lắm.
```
function loadDataSuccess(data) {
  return {
    type: 'LOAD_DATA_SUCCESS',
    payload: data,
  };
}

function * loadDataSaga () {
  try {
    const res = yield fetch('https://jsonplaceholder.typicode.com/users')
    const data = yield res.json()
    yield put(loadDataSuccess(data))
  } catch (err) {
    // yield put(failure(err))
  }
}

function * rootSaga () {
  yield all([
    takeLatest('LOAD_DATA', loadDataSaga)
  ])
}
```

### User Reducer
```
const initState = {
  users: [],
};

function userReducers(state = initState, { type, payload }) {
  switch(type) {
    case 'LOAD_DATA_SUCCESS': {
      return Object.assign({}, state, { users: payload });
    }
    default:
      return state;
  }
}
```

### User Container
Phần này code chưa được clean lắm, mình làm lười nên dispatch thẳng cái action ở đây thay vì sử dụng function `mapDispatchToProps`
```
class UsersContainer extends  Component {

  componentWillMount() {
    this.props.dispatch({
      type: 'LOAD_DATA'
    });
  }

  render() {
    const { users } = this.props;

    return <Users data={users} />
  }
}

const mapStateToProps = (state) => {
  return {
    users: state.users.users,
  };
};
```

### User View
Ở đây mình tách biệt giữa phần logic (UserContainer) với phần View, nên mình tạo thêm component UsersView (Users). Phần này chỉ show data được truyền xuống từ container.
```
export default class Users extends Component {
  render() {
    const { data = [] } = this.props;
    const usersView = data.map((user) => (
      <li key={user.id}>{JSON.stringify(user)}</li>
    ));

    return (
      <ul>
        {usersView}
      </ul>
    )
  }
}
```

## Kết Luận
Trong bài viết này mình apply khá nhiều thứ hay, hy vọng các bạn nhìn thấy được những cái đó. Source code mình để ở trên [github](https://github.com/nhulongctk35/react-redux-ssr) hy vọng được góp ý và trao đổi các ideal với mọi người.
