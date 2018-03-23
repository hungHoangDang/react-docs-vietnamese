---
id: introducing-jsx
title: Introducing JSX
permalink: docs/introducing-jsx.html
prev: hello-world.html
next: rendering-elements.html
---

Consider this variable declaration:

```js
const element = <h1>Hello, world!</h1>;
```

This funny tag syntax is neither a string nor HTML.

It is called JSX, and it is a syntax extension to JavaScript. We recommend using it with React to describe what the UI should look like. JSX may remind you of a template language, but it comes with the full power of JavaScript.

JSX produces React "elements". We will explore rendering them to the DOM in the [next section](/docs/rendering-elements.html). Below, you can find the basics of JSX necessary to get you started.

### Tại sao lại dùng JSX?

React embraces the fact that rendering logic is inherently coupled with other UI logic: 
- cách các event được xử lý
- cách mà các states thay đổi theo thời gian
- cách mà dữ liệu được chuẩn bị để hiển thị sau đó.

Thay vì phân tách phần markup và logic trong những file riêng biệt, React [biệt lập các *vấn đề*](https://en.wikipedia.org/wiki/Separation_of_concerns) thông qua các đơn vị gắn kết lỏng lẻo được gọi là "component", mỗi component chứa cả phần logic lẫn markup. We will come back to components in a [further section](/docs/components-and-props.html), but if you're not yet comfortable putting markup in JS, [this talk](https://www.youtube.com/watch?v=x7cQ3mrcKaY) might convince you otherwise.

React [không bắt buộc](/docs/react-without-jsx.html) việc sử dụng JSX, nhưng hầu hết người dùng thấy sự hợp lý của nó khi làm việc với UI bên trong code JavaScript. Nó cũng cho phép React hiển thị nhiều thông báo lỗi cũng như cảnh báo chứa nội dung hữu ích.

### Nhúng khai báo trong JSX

Bạn có thể nhúng bất kỳ [khai báo JavaScript](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Expressions_and_Operators#Expressions) nào vào JSX bằng cách đặt nó vào giữa hai ngoặc nhọn.

Những đoạn khai báo hợp lệ có thể là: `2 + 2`, `user.firstName`, và `formatName(user)`:

```js{12}
function formatName(user) {
  return user.firstName + ' ' + user.lastName;
}

const user = {
  firstName: 'Harper',
  lastName: 'Perez'
};

const element = (
  <h1>
    Hello, {formatName(user)}!
  </h1>
);

ReactDOM.render(
  element,
  document.getElementById('root')
);
```

[](codepen://introducing-jsx).

Chúng ta chia JSX thành nhiều dòng để cho dễ đọc. Tất nhiên là điều này không bắt buộc, chúng tôi khuyến cáo nên đóng ngoặc đoạn JSX để tránh lỗi xay ra do [dấu chấm phẩy được tự động thêm vào](http://stackoverflow.com/q/2846283).

### Bản thân JSX cũng là một khai báo

After compilation, JSX expressions become regular JavaScript function calls and evaluate to JavaScript objects.

Điều này có nghĩa là ta có thể dùng JSX bên trong `if` statement và vòng lặp `for`, gán nó cho cho biết, sử dụng như tham số, và trả về từ hàm:

```js{3,5}
function getGreeting(user) {
  if (user) {
    return <h1>Hello, {formatName(user)}!</h1>;
  }
  return <h1>Hello, Stranger.</h1>;
}
```

### Đặt Attributes với JSX

Bạn có thể dùng ngoặc kép để chỉ định chuỗi thành thuộc tính:

```js
const element = <div tabIndex="0"></div>;
```

Bạn cũng có thể sử dụng ngoặc nhọn để nhúng một biểu thức JavaScript trong một thuộc tính:

```js
const element = <img src={user.avatarUrl}></img>;
```

Không sử dụng ngoặc kép xung quang ngoặc nhọn khi nhúng một biểu thức JavaScript trong một thuộc tính. Bạn nên hoặc chỉ sử dụng ngoặc kép (cho giá trị chuỗi) hoặc ngoặc nhọn (cho biểu thức), nhưng không sử dụng cả hai trong cùng một thuộc tính.

>**Warning:**
>
>Do JSX gần với JavaScript hơn HTML, React DOM sử dụng phương thức đặt tên cho đặc tính `camelCase` thay vì cách thức đặt tên cho thuộc tính HTML.
>
>Ví dụ, `class` trở thành [`className`](https://developer.mozilla.org/en-US/docs/Web/API/Element/className) trong JSX, và `tabindex` trở thành [`tabIndex`](https://developer.mozilla.org/en-US/docs/Web/API/HTMLElement/tabIndex).

### Specifying Children with JSX

Nếu một thẻ rỗng, bạn có thể đóng thẻ ngay lập tức bằng `/>`, như XML:

```js
const element = <img src={user.avatarUrl} />;
```

Thẻ JSX có thể có thẻ con:

```js
const element = (
  <div>
    <h1>Hello!</h1>
    <h2>Good to see you here.</h2>
  </div>
);
```

### JSX Prevents Injection Attacks

It is safe to embed user input in JSX:

```js
const title = response.potentiallyMaliciousInput;
// This is safe:
const element = <h1>{title}</h1>;
```

By default, React DOM [escapes](http://stackoverflow.com/questions/7381974/which-characters-need-to-be-escaped-on-html) any values embedded in JSX before rendering them. Thus it ensures that you can never inject anything that's not explicitly written in your application. Everything is converted to a string before being rendered. This helps prevent [XSS (cross-site-scripting)](https://en.wikipedia.org/wiki/Cross-site_scripting) attacks.

### JSX Represents Objects

Babel compiles JSX down to `React.createElement()` calls.

These two examples are identical:

```js
const element = (
  <h1 className="greeting">
    Hello, world!
  </h1>
);
```

```js
const element = React.createElement(
  'h1',
  {className: 'greeting'},
  'Hello, world!'
);
```

`React.createElement()` performs a few checks to help you write bug-free code but essentially it creates an object like this:

```js
// Note: this structure is simplified
const element = {
  type: 'h1',
  props: {
    className: 'greeting',
    children: 'Hello, world'
  }
};
```

These objects are called "React elements". You can think of them as descriptions of what you want to see on the screen. React reads these objects and uses them to construct the DOM and keep it up to date.

We will explore rendering React elements to the DOM in the next section.

>**Tip:**
>
>We recommend using the ["Babel" language definition](http://babeljs.io/docs/editors) for your editor of choice so that both ES6 and JSX code is properly highlighted. This website uses the [Oceanic Next](https://labs.voronianski.com/oceanic-next-color-scheme/) color scheme which is compatible with it.
