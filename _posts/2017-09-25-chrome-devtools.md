---
layout: post
title: "Một số thủ thuật khi làm việc với Chrome DevTools"
date: 2017-09-25 16:10:13
categories: Web Development
meta: "Chrome devtools"
---

## 1. CSS Coverage
Tính năng này khá hữu ích. Nó giúp chúng ta nhận biết được đoạn *css* hay *js* nào trong app mà chúng ta không dùng đến từ đó chúng ta có thể xóa block css đó đi để làm nhẹ file, 1 phần nào đó giúp tăng performance.
> Lưu ý: Đối với những file JavaScript thì chúng ta cũng làm tương tự.

![alt](https://cdn-images-1.medium.com/max/800/1*egUTvzNCSKUd58JHH3lzfg.gif)
**Các bước để thực hiện tính năng này.**
1. Nhấn phím **f12** thần thánh hoặc **Ctrl + Shift + i**.
2. Ở tab **Console**, Nhấn tổ hợp phím **Ctr + Shift + P** và tìm kiếm với cụm từ **Show Coverage**. Chọn panel show coverage.
![](https://viblo.asia/uploads/419ce417-1df7-45a4-a7fe-e2fed4d4ac6b.png)
3. Ở tab **Coverage**, click vào button **Instrument coverage** để bắt đầu phân tích xem đoạn *CSS* hay *JavaScript* nào mà ứng dụng của chúng ta không sử dụng đến.
4. Lưu ý là chúng ta nên navigation qua nhiều page khác nhau để phân tích chính xác đoạn code nào không sử dụng tới. Sau khi phân tích xong thì chúng ta click vào từng file để xem chi tiết.
![](https://viblo.asia/uploads/236240bf-a8f7-47b6-abbd-7491efb89a49.png)
Những dòng nào được bôi đỏ đồng nghĩa là đoạn css đó không được sử dụng, chúng ta có thể tự tin xóa những dòng css mà không sợ bị thằng khác cho ăn củ hành.
## 2. Screenshot webpage
Với tính năng này chúng ta có thể screenshot 1 page hay 1 element nào đó ngay ở trình duyệt mà không cần sử dụng đến *tools* hay *extensions* nào cả.
**Các bước thực hiện tính năng này.**
1. Ở page mà chúng ta muốn screenshot. Nhấn tổ hợp phím **Ctrl + Shift + P** và tìm kiếm với cụm từ **Screenshot**. Chọn **Capture full size screenshot**.
![](https://viblo.asia/uploads/38d1f6c9-6625-48f0-bcfd-4d930a1e4f07.png)
2. Kết quả là chúng ta sẽ screenshot được webpage như bên dưới.
![](https://viblo.asia/uploads/72d037d0-d9b8-46a0-9eae-dc48de363195.png)
Trường hợp bạn muốn chỉ screenshot 1 element nào đó trên page thì làm tương tự nhưng kìm kiếm với cụm từ là **node screenshot**. Lưu ý: tính năng này chỉ có trên phiên bản [chrome **Canary**](https://www.google.com/chrome/browser/canary.html)
![alt](https://umaar.com/assets/images/dev-tips/element-screenshot.gif)
## 3. Timeline History
Tính năng này giúp chúng ta phân tích, đánh giá performance của trang web. Từ đó tìm ra nguyên nhân để improving performance.
**Các bước thực hiện tính năng này.**
1. Mở trang web mà chúng ta muốn phân tích.
2. Chọn tab **Performance**.
3. Check vào options Screenshot, sau đó click vào nút **Record** hoặc nhấn tổ hợp phím **Ctrl + E** để tiến hành quá trình phân tích
![](https://viblo.asia/uploads/0484aa57-0d4e-41d2-8a60-79a6ed954054.png)
4. Kết quả sau khi phân tích.
![](https://viblo.asia/uploads/20718116-29f0-40eb-8113-ff07d619791b.png)
## 4. Kết Luận
Trên là một số thủ thuật với **Chrome Devtools** mình tìm hiểu được trong quá trình làm việc. Để tìm hiểu thêm nhiều tính năng khác các bạn có thể tham khảo tài liệu dưới đây.
https://developers.google.com/web/tools/chrome-devtools/
