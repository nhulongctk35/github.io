---
layout: post
title: "Tìm hiểu về Async/Await trong JavaScript ES 2017"
date: 2017-10-24 16:10:13
categories: Web Development
meta: "Tìm hiểu về Async/Await trong JavaScript ES 2017"
---

![](https://images.viblo.asia/36b5268b-ac36-47c3-b36c-cf0c659993f4.png)

ES 2017 vừa giới thiệu thêm một tính năng Async/Await trong việc xử lý bất đồng bộ trong JavaScript. Các hàm Async về bản chất là một cách viết khác rõ ràng, dễ nhìn hơn so với cách xử lý bất đồng bộ. Khi chưa có Async/Await thì chúng ta phải xử lý bằng Promise or callback, những cách xử lý thường dẫn đến Promise hell or callback hell khi ta có nhiều thao tác bất đồng bộ phải chờ nhau thực hiện, callback hell làm cho code của chúng ta trong rất rối từ đó dẫn đến khó bảo trì.

Với phiên bản ES6, Promise đã giải quyết vấn đề callback hell. Với Promise thì code của chúng ta trông clean hơn so với callback nhưng vẫn bị dính Promise hell như tôi mention ở trên. Để hiểu rõ hơn thì các bạn hãy xem đoạn code dưới đây.

```
function waitFiveSecond() {
  return new Promise(resolve => setTimeout(resolve, 5000));
}
```

 ```
waitFiveSecond().then(() => {
    return waitFiveSecond()
  }).then(() => {
    return waitFiveSecond()
  }).then(() => {
    console.log('Done!');
  });
```

Để giải quyết vấn đề đó thì chúng ta cùng tìm hiểu ES7.

## Syntax
Xử lý một async function trong JS khá đơn giản. Bạn chỉ cần thêm keyword **async** trước function.
### Function bình thường

```
// func bình thường
function add(x,y){
  return x + y;
}
```

```
// Async function
async function add(x,y){
  return x + y;
}
```

### Đối với arrow function
```
let arr = ['JS', 'node.js'].map(async val => {
   return await new Google().search(val)
})
```

### Đối với class
```
class Google {
   constructor() {
     this.apiKey = '...'
   }

   async search(keyword) {
     return await this.searchApi(keyword)
   }
}
```

Đoạn code trên được viết lại ngắn gọn như sau.

```
  function waitFiveSecond() {
    return new Promise(resolve => setTimeout(resolve, 5000));
  }

  async function handleAsyncFunction() {
    await this.waitFiveSecond();
    await this.waitFiveSecond();
    await this.waitFiveSecond();
    console.log('Done!');
  }
```

Thực ra bản chất của hàm async chính là Promise cho nến nếu bạn chưa hiểu rõ lắm or chưa nghe đến Promise thì bạn có thể tìm đọc thêm
[tại đây](https://developer.mozilla.org/vi/docs/Web/JavaScript/Reference/Global_Objects/Promise).
## Xử lý catch trong theo cách Async/Await
Xử lý catch đối với hàm async cực kì đơn giản, giống như chúng ta xử lý try catch hàng ngày.
```
  async function handleAsyncFunction() {
    try {
      await this.waitFiveSecond();
      await this.waitFiveSecond();
      await this.waitFiveSecond();
      console.log('Done!');
    } catch (e) {
      console.log('Không ổn l'ắm);
    }
  }
```
## Kết luận
Cho tới thời điểm hiện tại thì các trình duyệt chưa hỗ trợ async/await, điều đó có nghĩa là chúng ta phải config để trình duyệt có thể hiểu được cú pháp async/await. Vấn đề này chungs ta có thể giải quyết bằng cách sử dụng babel.  Tôi đã từng gặp dự án khách hàng muốn upgrade es6 lên es7 cho nên tôi nghĩ ngay từ bây giờ chúng ta cố gắng sử dụng async/await để giảm thiểu việc bảo trì sau này.
