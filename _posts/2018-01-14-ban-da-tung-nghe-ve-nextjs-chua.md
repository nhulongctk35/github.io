---
layout: post
title: "Bạn đã từng nghe về NEXT.JS chưa?"
date: 2017-10-24 16:10:13
categories: Web Development
meta: "Bạn đã từng nghe về NEXT.JS chưa?"
---

![](https://viblo.asia/uploads/27a230f9-de61-4674-873e-58732a4e6639.png)

Trong bài viết này tôi sẽ hướng dẫn mọi người  xây dựng một **UNIVERSAL JAVASCRIPT APP VỚI NEXT.JS**, Tôi cũng mới tìm hiểu về Next.js trong thời gian gần đây, nên tôi chỉ giới thiệu những cần căn bản của Next.js mà tôi biết được. Còn những phần Advanced thì các bạn có thể tìm hiểu thêm ở dưới đây.
* https://learnnextjs.com/basics/getting-started
* https://github.com/zeit/next.js/
* https://www.codementor.io/tgreco/5-of-the-many-things-to-love-about-zeit-s-next-js-bpszu99g1
## 1. UNIVERSAL JAVASCRIPT Là Gì ?
Nếu bạn chưa biết về Universal Javascript App thì nó là ứng dụng web được viết bằng javascript, chạy được cả server và client side (Browser) chung một mã nguồn. Tất nhiên có một số chỉ cần chạy phía browser như các hiệu ứng chuyển trang, animations các thứ, hoặc có sử dụng local storage của browser, còn một số mã nguồn liên quan đến lớp xử lý (logic) hoặc các hoạt động đặc thù của server như phiên làm việc (session), xác thực (authentication). Bạn có thể đọc thêm ví dụ ở [đây](https://scotch.io/tutorials/react-on-the-server-for-beginners-build-a-universal-react-and-node-app)  

## 2. NEXT.JS Là Gì ?
Next.js là một framework  được xây dựng từ React, Babel và Wepack, nó được tạo ra để giúp các devs tạo **React App** có tính năng **SSR (Server Side Render)**,** code spliting, Simple client-side routing** và nhiều tính năng cools khác. Khi chưa biết về sự tồn tại của NextJS tôi thường dùng [create-react-app](https://github.com/facebook/create-react-app) của Facebook để init source code,  sau khi init ứng dụng bằng create-react-app thì tôi phải config thêm **SSR** nếu tôi muốn apply **SSR**. Nói chung create-react-app rất tuyệt vời nhưng tôi lại chọn Next.js :)

**Mốt số tính năng cools của Next.js**
* *Server-rendered by default*
* *Automatic code splitting for faster page loads*
* *Simple client-side routing (page based)*
* *Webpack-based dev environment which supports Hot Module Replacement (HMR)*
* *Able to implement with Express or any other Node.js HTTP server*
* *Customizable with your own Babel and Webpack configurations*
## 3. Getting Started
2 phút quảng cáo về Next.js, bắt tay vào xây dựng nào. Tạo 1 folder để chứa source code
1. mkdir learn-next-js && cd learn-next-js
2. npm install --save next react react-dom
3. npm init (tao file package.json)
4. Add scripts sau vào file package.json
    ```
    {
      "name": "learn-next",
      "version": "1.0.0",
      "description": "",
      "main": "index.js",
      "dependencies": {
        "next": "^4.2.3",
        "react": "^16.2.0",
        "react-dom": "^16.2.0"
      },
      "devDependencies": {},
      "scripts": {
        "dev": "next",
        "build": "next build",
        "start": "next start"
      },
      "author": "",
      "license": "ISC"
    }
    ```
 5. Tạo folder pages, folder này chứa các pages tương ứng với từng routes. (noted: Phải đặt tên folder là pages)
 6. Tạo file index.js trong folder pages. Nextjs sử dụng file này làm index route
 7. npm run dev (default app sẽ được run ở port 3000). Mở Browser và navigate to: localhost:3000 để view app
     ![](https://viblo.asia/uploads/694191ca-6451-47d4-a9e1-c11a298c73a0.png)
 8. Thử navigate qua 1 page khác chưa tồn tại
     ![](https://viblo.asia/uploads/a3c2abe3-0aea-4a40-b149-699000b96b63.png)
 Hình trên để các bạn thấy răng Nextjs cũng đã support unknow routing cho chúng ta luôn rồi. Cho đến step này, chúng ta đã làm được những thứ sau.
* Automatic transpilation and bundling (with webpack and babel)
* Hot code reloading
* Server rendering and indexing of ./pages
## 4. Kết luận
Bài viết này mục đích của mình là giới thiệu cái tên Nextjs cho mọi người biết đến, nó là 1 framework được rất nhiều người sử dụng. Chi tiết hơn thì mời các bạn đọc documents ở trên.
