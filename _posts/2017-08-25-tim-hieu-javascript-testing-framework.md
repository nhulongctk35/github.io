---
layout: post
title: "Tìm hiểu AVA: JavaScript Testing Framework"
date: 2017-07-31 16:10:13
categories: Web Development
meta: "AVAJS JavaScript Testing Framework"
---
Gần đây tôi đã dành thời gian rảnh rỗi của mình để tìm hiểu về [avajs](https://github.com/avajs/ava) và apply nó cho dự án React/Redux mà tôi đang tham gia. Tôi từng tham gia nhiều dự án **React**, **Angular** và **EmberJS** nhưng hầu như chỉ có một vài dự án là bắt buộc phải viết **Unit Test**. Trước đây tôi dùng **[Jest](https://facebook.github.io/jest/docs/en/getting-started.html)** nhưng ở dự án này Client muốn dử dụng ava nên tôi theo (Đặc thù của công ty outsourcing). Bài viết này tôi xin chia sẽ lại những gì tôi đã tìm hiểu được và tổng hợp lại một số phần quan trọng của  avajs và **[Enzyme](airbnb.io/enzyme/docs/guides.html)**.

## 1. What is AVAJS?

Nếu bạn đã từng sử dụng **Jest**, **Jasmine** hay **Karma** thì Avajs cũng tương tự như vậy, nó là một JavaScipt Testing Frameworks. Với một số tính năng nỗi trội như. Syntax đơn giản, dễ sử dụng và dễ cài đặt chỉ một vài dòng config là bạn có thể run tests.
> 1. Runs tests song song.
> 1. Supports ES6 syntax và một số tính năng nổi bật như Promise và Asynchronous code
> 1. Lúc run time thì mỗi file sẽ tạo ra 1 enviroment riêng biệt, không impact tới những files test khác và không tạo môi trường global.
> 1. Supports Snapshot  testing, đây là cách để test UI Component được cung cấp bởi [jest-snapshot](https://facebook.github.io/jest/blog/2016/07/27/jest-14.html)
>  ...
## 2. Setting and Using AVA

Cài đặt ava sử dụng **npm** hoặc **yarn**
```javascript
// using npm
npm install --save-dev ava
// using yarn
yarn add --dev ava
// config package.json to run ava
"scripts": {
    "test": "ava src/**/*.test.js"
 }
```
Việc setting khá đơn giản, chỉ cần một vài dòng lệnh trên là bạn đã setup xong ava, ở config `ava src/**/*.test.js ` tôi đang chỉ định cho ava là chỉ run những file trong folder **src** và có đuôi là **.test.js**

## 3. Writing Test
Để dễ hiểu, tôi sẽ đưa ra một ví dụ đơn giản trước để các bạn làm quen rồi sau đó chúng ta sẽ tìm hiểu những phần phức tạp hơn.
### 1. Simple Example
Tôi assumption là file chung ta cần viết Test như sau. Tôi đặt tên file này là `sum.js`
```javascript
function sum(a, b) {
  return a + b;
}

export default sum;
```

File test của chúng ta sẽ nằm chung folder với file mà chúng ta cần viết Test, và như config ở trên thì tôi sẽ đặt tên file này là `sum.test.js`
```javascript
import test from 'ava';
import sum from './';

test('adds 1 + 2 to equal 3', (t) => {
  t.is(sum(1, 2), 3);
});
```
**Kết quả:**
![](https://viblo.asia/uploads/6ab69f7c-85bf-448e-9e6e-fb3f95144e20.png)

Nếu bạn đã từng sử dụng thằng Jest hay Jasmine. Thì chắc bạn đã nghe qua **test suite**. Nhưng trong ava tôi không thấy có test suite mà thay vào đó là từng `test case` . Mỗi test case là một function và bạn sử dụng assertion để kiểm tra tính đúng đắn của đoạn code mà bạn đang muốn test.
Giải thích qua một chút về đoạn test trên.
**test** là một function và nó có 2 tham số như sau. `test([title], implementation)`
> 1. **title**: là optional vậy nên bạn có thể truyền vào hoặc không, Nhưng trong trường hợp bạn có nhiều hơn 1 function test thì tốt hơn hết là nên có title cho mỗi func.
> 1. **implementation**: là assertion, ở trên tôi đang test func sum và tôi expect là khi execute func này sẽ trả về kết quả là 3 khi tôi truyền vào 1, 2.

### 2. Complex Examples
Ví dụ tôi có  component là **Button.jsx** như bên dưới và file **Button.test.js** là file test.
`Button.jsx`
```javascript
import React from 'react';
import PropTypes from 'prop-types';
import cx from 'classnames';

function Button({ type, color, className, children, onClick }) {
  const classes = cx(Button.name, {
    [`-${color}`]: color,
  }, className);

  return (
    <button className={classes} type={type} onClick={onClick}>
      {children}
    </button>
  );
}

Button.propTypes = {
  type: PropTypes.string,
  color: PropTypes.string,
  className: PropTypes.string,
  children: PropTypes.node.isRequired,
  onClick: PropTypes.func,
};

Button.defaultProps = {
  type: 'button',
  color: '',
  className: '',
  onClick: null,
};

export default Button;

```
`Button.test.js`
```javascript
import React from 'react';
import test from 'ava';
import sinon from 'sinon';
import { shallow } from 'enzyme';
import Button from '.';

const children = 'Test';
const render = (props = {}) => shallow((
  <Button {...props}>
    {children}
  </Button>
));

test('#props.type', (t) => {
  const defaultButton = render();
  const submitButton = render({ type: 'submit' });

  t.is(defaultButton.prop('type'), 'button');
  t.is(submitButton.prop('type'), 'submit');
});

test('#props.color', (t) => {
  const button = render({ color: 'blue' });

  t.true(button.hasClass('-blue'));
});

test('#props.children', (t) => {
  const button = render();

  t.true(button.contains(children));
});

test('#props.onClick', (t) => {
  const onClick = sinon.spy();
  const button = render({ onClick });

  button.simulate('click');

  t.true(onClick.calledOnce);
});

```

**Kết quả:**

![](https://viblo.asia/uploads/2efdf571-7a4f-46eb-8039-13d70171d704.png)
Chi tiết về setting, configuration cũng như những ví dụ trên tôi đặt trong link repo bên dưới.
> https://github.com/nhulongctk35/react-atom-architecture
## 4. Conclusion
Trên là một vài ví dụ tôi viết ra để giải thích về ava. Ngoài avajs thì trong ví dụ tôi còn sử dụng thêm một số test utils như **Spies** và **Enzyme**. Các bạn có thể tìm hiểu thêm thông qua link bên dưới.
1. [Sinon](http://sinonjs.org/)
2. [Enzyme](http://airbnb.io/enzyme/docs/api/)
