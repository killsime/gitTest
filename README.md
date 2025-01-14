



# 前端框架对比

[使用谷歌趋势查看搜索量](https://trends.google.com/trends/explore?cat=13&date=today%205-y&q=vue,react,angular,flutter&hl=zh-CN)

结果如下

![frontendFramHeat.png](https://s2.loli.net/2024/11/30/UP2T65fgSJEwoVF.png)

可见,react几乎是一直领先的



# 简介

## 学习目的

我们在学习GUI开发的时候一般思考两个问题

1. 如何来设计UI?
2. 如何实现前后端数据交互?

**问题一**

React引入了**tsx**语法,让我们能够在ts中自由的嵌入html,让我们能只写tsx代码,来实现UI的控制,而不是把所有的UI都放进一个html文件中,让各个部分更好维护

所react中,我们使用**组件**来表示各个小的部分

那与之而来的问题,react是如何将tsx代码变成网页的呢?

要知道,我们使用react可以不写任何html实现网页效果

答案是使用虚拟dom,即由react管理的dom,我们的tsx语法都会先映射到这个虚拟的dom上,然后react来对比改变的部分,然后render到浏览器中的dom上,这样还能减少对浏览器dom的更改,节省了性能

**问题二**

我们的数据通过**hook**来修改,根据不同需要,可以封装不同的修改

---



React（React.js或ReactJS）是由Facebook开发并维护的一款用于构建用户界面的JavaScript库。React主要用于构建单页面应用程序（Single Page Applications）中的用户界面，通过组件化开发的方式，可以更轻松地构建和维护复杂的前端应用。

React的设计目标是提高前端开发的效率和可维护性，使得开发者能够更容易地构建现代、响应式的用户界面。它广泛应用于各种Web应用开发项目，并且与其他技术（如React Native）结合，使得开发者可以用相似的方式构建Web、移动端等不同平台上的应用。



## 运行环境

**1. 创建 React 应用**

在工作目录下打开终端,输入下面命令,设置名称为my-app后全部yes

```sh
> npx create-next-app@latest
√ What is your project named? ... my-blog
√ Would you like to use TypeScript? ... No / Yes
√ Would you like to use ESLint? ... No / Yes
√ Would you like to use Tailwind CSS? ... No / Yes
√ Would you like your code inside a `src/` directory? ... No / Yes
√ Would you like to use App Router? (recommended) ... No / Yes
√ Would you like to use Turbopack for `next dev`? ... No / Yes
√ Would you like to customize the import alias (`@/*` by default)? ... No / Yes
```

建议全选

**2. 进入 create-react-app 创建的文件夹并安装 typescript**

```shell
cd my-app
```

**3.启动 React**

```shell
npm run dev
```

然后会自动在浏览器打开`localhost:3000`网页,然后就能看到官方示例了



# HOOK

格式如下,解释 : 用setTodos来设置todos,并初始值为<Todo[]>([])这个空列表

```tsx
  const [todos, setTodos] = useState<Todo[]>([])
```

然后我们可以利用这个来管理todos,如下

```tsx
  const addTodo = (text: string) => {
    const newTodo = {
      id: Date.now(),
      text,
      completed: false
    }
    setTodos([...todos, newTodo])//打开旧的,添加新的
  }
  
  const deleteTodo = (id: number) => {
    setTodos(todos.filter(todo => todo.id !== id))
    //将不需要删除的筛选出来称为新的
  }

  const toggleTodo = (id: number) => {
    setTodos(todos.map(todo => {
      if (todo.id === id) {
        todo.completed = !todo.completed
      }
      return todo
    }))
  }
```

# TSX

TSX是TypeScript代码中嵌套XML标记的语法扩展。方便描述用户界面的结构。

所以跟其他ts类型的使用方法相同,可以类比**字符串**理解

## 常见用法

**声明**

```jsx
const element = <h1>Hello, world!</h1>;
```

**格式化**

```jsx
const name = "Josh Perez";
const element = <h1>Hello, {name}</h1>;

ReactDOM.render(element, document.getElementById("root"));
```

**返回值**

```jsx
function getGreeting(user) {
  if (user) {
    return <h1>Hello, {formatName(user)}!</h1>;
  }
  return <h1>Hello, Stranger.</h1>;
}
```

**属性**

你可以通过使用引号，来将属性值指定为字符串字面量：

```jsx
const element = <div tabIndex="0"></div>;
```

也可以使用大括号，来在属性值中插入一个 JavaScript 表达式：

```jsx
const element = <img src={user.avatarUrl}></img>;
```

## 习惯

在属性中嵌入 JavaScript 表达式时，不要在大括号外面加上引号。你应该仅使用引号（对于字符串值）或大括号（对于表达式）中的一个，对于同一属性不能同时使用这两种符号。

> 因为 JSX 语法上更接近 JavaScript 而不是 HTML，所以 React DOM 使用 `camelCase`（小驼峰命名）来定义属性的名称，而不使用 HTML 属性名称的命名约定。
>
> 例如，JSX 里的 `class` 变成了 `className`，而 `tabindex` 则变为 `tabIndex`。

## 原理

babel会把 JSX 转译成一个名为 `React.createElement()` 函数调用。

以下两种示例代码完全等效：

```jsx
const element = <h1 className="greeting">Hello, world!</h1>;
```

```jsx
const element = React.createElement(
  "h1",
  { className: "greeting" },
  "Hello, world!"
);
```

`React.createElement()` 会预先执行一些检查，以帮助你编写无错代码，但实际上它创建了一个这样的对象：

```jsx
const element = {
  type: "h1",
  props: {
    className: "greeting",
    children: "Hello, world!",
  },
};
```

这些对象被称为 “React 元素”。它们描述了你希望在屏幕上看到的内容。React 通过读取这些对象，然后使用它们来构建 DOM 以及保持随时更新。

# render

## 渲染新元素

```jsx
const element = <h1>Hello, world</h1>;
ReactDOM.render(element, document.getElementById("root"));
```

## 更新元素

React 元素是不可变对象。更新 UI 唯一的方式是创建一个全新的元素，并将其传入 `ReactDOM.render()`。

考虑一个计时器的例子：

```jsx
function tick() {
  const element = (
    <div>
      <h1>Hello, world!</h1>
      <h2>It is {new Date().toLocaleTimeString()}.</h2>
    </div>
  );
  ReactDOM.render(element, document.getElementById("root"));
}

setInterval(tick, 1000);
```

这个例子会在 `setInterval()`回调函数，每秒都调用 `ReactDOM.render()`。也就是说，它每秒都渲染一个新的 React 元素，并将其传入`ReactDOM.render()`中。

# 组件

## 组件

实际上是函数,props接收任意入参.不过一般参数还是都写好了,便于我们调用

在使用时我们可以像一般html一样使用,不过参数必须要传递

```ts
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}
```

像用html一样使用,如下

```jsx
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}

const element = <Welcome name="Sara" />;
ReactDOM.render(element, document.getElementById("root"));
```

> **注意：** 组件名称必须以**大写字母开头**。
>
> React 会将以小写字母开头的组件视为原生 DOM 标签。例如，`<div />` 代表 HTML 的 div 标签
>
> 而 `<Welcome />` 则代表一个组件，并且需在作用域内使用 `Welcome`。



## 使用

如下将接收的参数的类型可以通过接口AddTodoProps来解释,然后后面就是具体用法了

AddTodo.tsx

```tsx
import { useState } from "react";

interface AddTodoProps {
    addTodo: (text: string) => void
}

function AddTodo({ addTodo }: AddTodoProps) {
    const [text, setText] = useState('')

    const handleSubmit = (e: React.FormEvent<HTMLFormElement>) => {
        e.preventDefault();
        if (text.trim().length === 0) {
            return
        }
        addTodo(text)
        setText('')
    }
    return (
        <form onSubmit={handleSubmit}>
            <input
                type="text"
                value={text}
                onChange={e => setText(e.target.value)}
            />
            <button>Add</button>
        </form>
    )
}

export default AddTodo;
```

page.tsx

像使用一般html标签一样使用,注意传递参数

```tsx
<AddTodo addTodo={addTodo}></AddTodo>
```



## antd



```sh
npm install antd
```





# 文件路由

`Link` 组件是用于在应用内实现路由跳转的核心工具。它依赖于 Next.js 的文件系统路由机制，自动根据项目目录中的文件结构匹配目标页面。以下是 `Link` 组件寻找页面的详细过程

## 路由机制

Next.js 使用文件系统作为路由的基础：

- 项目中的 `app` 或 `pages` 目录（取决于使用的路由系统）对应于应用的路由。
- 文件和文件夹的名称决定了页面的路径。

文件结构示例

```
app/
├── page.tsx         // 对应路径: /
├── blog/
│   ├── page.tsx     // 对应路径: /blog
│   ├── [id]/
│   │   ├── page.tsx // 对应路径: /blog/:id (动态路由)
│   └── new/
│       ├── page.tsx // 对应路径: /blog/new
├── about/
│   ├── page.tsx     // 对应路径: /about
```

------

## 示例

`Link` 组件接受一个 `href` 属性，用于指定目标路径：

```tsx
import Link from 'next/link';

export default function Home() {
  return (
    <div>
      <Link href="/blog">跳转到博客</Link>
      <Link href="/about">跳转到关于页面</Link>
      <Link href="/blog/123">查看博客 123</Link>
    </div>
  );
}
```

如果 `href` 是 `/about`，Next.js 会查找 `app/about/page.tsx` 或 `pages/about.tsx`

如果没有查找到,就会显示404



