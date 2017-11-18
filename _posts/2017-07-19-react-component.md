---
layout: post
title: "Tìm hiểu Presentational với Container Component trong React"
date: 2017-07-19 12:19:58
categories: Web Development
meta: "react presentational vs container components"
---
Trong quá trình làm việc với những dự án React, Tôi nhận thấy có 1[ **pattern**](https://en.wikipedia.org/wiki/Software_design_pattern) đơn giản nhưng lại rất hữu ích trong **React Application**. Bài biết hôm nay tôi sẽ nói về pattern đó.
Nếu bạn từng làm việc với React, có thể bạn đã thấy pattern này. Ở bài viết này, tôi chỉ nói lại pattern đó và chia sẽ thêm một số lý tưởng đồng thời tôi sẽ giải thích cho bạn hiểu rõ hơn về Container & Presentational Component.

## 1. Tại sao lại tách biệt Container vs Presentational Component.
Tôi lấy ví dụ bạn đang viết 1 Component để hiển thị danh sách các comments. Nếu như bạn chưa từng nghe về khái niệm Container Component, thì có thể bạn sẽ code như thế này.
```javascript
class CommentList extends React.Component {
 this.state = { comments: [] };

 componentDidMount() {
   fetchSomeComments(comments =>
     this.setState({ comments: comments }));
 }
 render() {
   return (
     <ul>
       {this.state.comments.map(c => (
         <li>{c.body}—{c.author}</li>
       ))}
     </ul>
   );
 }
}
```

Đoạn code trên nhìn có vẻ rất dễ hiểu. Nhưng nếu bạn từng đọc qua cuốn [Clean Code](https://www.amazon.com/Clean-Code-Handbook-Software-Craftsmanship/dp/0132350882) thì tôi chắc chắn rằng bạn đã nghe đến principle **Single responsibility principle**
Hiện tại component CommentList đang làm 2 thứ.
1. **Fetching data**
1. **Presenting data**

Làm như thế thì không có gì sai cả, code vẫn chạy bình thường và component sẽ render ra 1 list comments mà nó fetch từ API.
Khi chúng ta design 1 component như thế thì chúng ta đã bỏ lỡ 1 số benefits của React như tính tái sử dụng (**Reusability**) component. Component CommentList hầu như không thể tái sử dụng được.
Vậy câu hỏi đặt ra là làm thế nào để tách 1 component thành container component? Để tách đúng và hiểu rõ nó, dưới đây là 1 số key để nhận biết khi nào thì 1
component nên là Container Component và khi nào nên là Presentational Component.

## 2. Container vs Presentational Components
### 2.1 Container component
> - 1 container Component thường sẽ làm nhiệm vụ fetching data (call API) và truyền data vừa fetch cho Presentational Component render ra.
> - Container component chỉ tập trung xử lý logic (How things work) chứ ko xử lý việc view (How thing look like).

### 2.2 Presentational components:
> - 1 Presenting component đảm nhiệm việc show data (view)
> - Thường nhận data từ container component và render
> - Chỉ tập trung vào việc (How things look like) chứ ko xử lý bất cứ logic nào bên trong.

Lý thuyết là thế, bây giờ hãy cùng tôi tách biết Container Component vs Presentational cho CommentList component.
```javascript
class CommentListContainer extends React.Component {
 state = { comments: [] };
 componentDidMount() {
   fetchSomeComments(comments =>
     this.setState({ comments: comments }));
 }
 render() {
   return <CommentList comments={this.state.comments} />;
 }
}

const CommentList = props =>
 <ul>
   {props.comments.map(c => (
     <li>{c.body}—{c.author}</li>
   ))}
 </ul>
 ```

## 3. Những lợi ích khi apply pattern này
- App của chúng ta trông clean hơn, và dễ maintain hơn.
- Tách biệt giữa phần xử lý logic và phần view
- Tái sử dụng tốt hơn, chúng ta có thể sử dụng lại presentational component với những data source khác nhau.
