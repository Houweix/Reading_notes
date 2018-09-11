# 《vue.js 实战》读书笔记
## 内容简介：

以 vue.js2.0 为基础，渐进式的学习 vue。
2017 年 10 月第一版

## 第一章 初识 Vue.js

**MVVM 模式：**
由 MVC 模式衍生，当 view 变化，会自更新到 ViewModel 中，反之亦然。View 和 ViewModel 之间通过双向绑定建立。

传统的前端开发模式：
jquery+requireJS+artTemplate+Gulp

但后来出现了更加复杂的业务如 SPA，组件解耦等。

**Vue.JS 的开发模式**
使用 cdn 获取最新的 VueJS

引入框架后，在 body 的底部使用 new Vue（）方式创建一个实例。
对于复杂的业务逻辑，可以使用 vue 单文件形式配合 webpack 使用，必要时配合 vuex 管理状态，vue-router 管理路由。

**模板语法**
Vue.js 使用了基于 HTML 的模板语法，允许开发者声明式地将 DOM 绑定至底层 Vue 实例的数据。所有 Vue.js 的模板都是合法的 HTML ，所以能被遵循规范的浏览器和 HTML 解析器解析。

在底层的实现上，Vue 将模板编译成虚拟 DOM 渲染函数。结合响应系统，Vue 能够智能地计算出最少需要重新渲染多少组件，并把 DOM 操作次数减到最少。

## 第二章 数据绑定第一个 Vue 应用

**Vue 实例与数据绑定**
vue 实例选项：
el：指定页面中已经存在的 DOM 元素来挂载 Vue 实例。CSS 选择器或者是 HTMLElement。

vue 实例属性和方法都以$开头。

```javascript
var app = new Vue({
    el:’#app’,
    data:{
        a: 2
    }
})

//可以通过vue实例访问数据
app.a   // 2
```

也可以指向一个已经存在的变量。

**生命周期**

每个 Vue 实例在被创建时都要经过一系列的初始化过程——例如，需要设置数据监听、编译模板、将实例挂载到 DOM 并在数据变化时更新 DOM 等。同时在这个过程中也会运行一些叫做生命周期钩子的函数，这给了用户在不同阶段添加自己的代码的机会。

![生命周期](https://cn.vuejs.org/images/lifecycle.png)

> 注：_不要在选项属性或回调上使用箭头函数_，因为箭头函数是和父级上下文绑定在一起的，this 不会是如你所预期的 Vue 实例

常用的钩子函数：

- created 实例创建完成后调用，完成了数据观测，但尚未挂载，$el 不可用。
- mounted el 挂载到实例上后调用。
- beforeDestroy。实例销毁前调用。

**插值与表达式**
使用{{ }}显示在 data 中的数据属性。

**注意**直接使用 v-html 输出内容有可能导致 XSS 攻击，所以一般要在服务端对用户提交的内容进行处理，<>转义。

另外

1.  使用 v-pre 可以跳过元素的编译。
2.  可以在{ } 使用 js 表达式进行简单的运算。
3.  另外只支持单个表达式，不能使用用户自定义的全局变量。

_使用 JavaScript 表达式_

```javascript
{
  {
    number + 1;
  }
}

{
  {
    ok ? "YES" : "NO";
  }
}

{
  {
    message
      .split("")
      .reverse()
      .join("");
  }
}

<div v-bind:id="'list-' + id" />;
```

### 过滤器

Vue 支持在{}插值的尾部添加管道符“（ | ）”对数据进行过滤，经常用于格式化文本，如字母全部大写等。过滤的规则是自定义的，通过给 Vue 实例添加选项 filters 来设置。

注：

1.  在新版本可以用于 v-bind 表达式。
2.  过滤器函数总是接受表达式之前的值。
3.  过滤器也可以串联（参数为上一个过滤器的结果），也可以接受参数（接受的参数作为第二，第三。。参数，第一参数为上一个的结构）
4.  过滤器应该用于简单的文本处理，复杂的应该使用**计算属性**。

### 指令与事件

标志：前缀 **v-**
职责是当表达式的值改变时，执行相应的行为。

~数据驱动 DOM 时 vue 的核心，不 到万不得已不要主动操作 DOM~

v-on：click/dblclick/keyup/mousemove

事件的处理可以放在 method 中，也可以内联样式书写。

### 语法糖 🍬

语法糖是指不影响功能的前提下实现同样的效果，方便程序开发。

如：

- v-bind ：
- v-on @

## 第三章 计算属性

当模版内的表达式逻辑复杂使用计算属性。
注意 ⚠️ 计算属性是定义一个新的变量，当这个变量变化（它包含的变量也包括）时就会触发函数，如果不需要新增变量使用 watch。

每个计算属性默认包含一个 getter 和一个 setter。当手动修改计算属性的值的时候，就会触发 setter 函数。

**技巧：**

1.  计算属性可以依赖其他计算属性
2.  计算属性不仅可以依赖当前 vue 实例的数据，还可以依赖其他实例的数据。

**一些区别**

1.  计算属性 vs 方法
    **区别**是**计算属性是基于它们的依赖进行缓存的**。也就是说，当相关依赖未发生变化，不再执行函数，直接利用缓存返回结果。

2.  计算属性 vs 侦听属性（watch）
    使用 watch 可能会造成代码重复。

## 第四章 v-bind 以及 style 的绑定

DOM 经常会动态的绑定 class 以及 style 类名，通常使用 v-bind 绑定。

### 绑定 class 的几种方式：

1.  对象语法
    1.  当逻辑复杂时可以绑定一个计算属性。
2.  数组语法
    当需要应用多个类时，可以使用数组语法。
3.  也可以使用三元表达式切换 class。同时可以在数组语法中嵌套对象语法。

### 绑定内联样式

使用：style 可以给元素直接绑定内联 样式（注意拼接 px）

**CSS 使用驼峰或短横分隔命名**
绑定一个样式对象通常更好，多个对象可以使用数组。

## 第五章 内置指令

### 基本指令

v-cloak（不需要表达式）
配合 display：none 来隐藏未编译完成的元素。直到编译结束才会显示。

```
[v-cloak] {
    dispaly:none;
}
<div v-cloak>
    {{ message }}
</div>
```

v-once（不需要表达式）
被定义的元素只渲染一次，用于优化性能。

### 条件渲染指令

#### v-if v—else-if v-else

与 if else 语句类似，条件指令可以根据**表达式**的值在 DOM 中渲染或销毁元素/组件。

**注意**
在使用一些组件的时候，如果加载组件需要一些数据进行初始化，则首先应该让组件 v-if = "false",等数据加载完成再 v-if = "true"。

#### v-show

用法基本与 v-if 一致，但 v-show 改变了元素的 display 属性。

#### v-show 与 v-if 对比

当组件需要频繁的切换时应使用**v-show**。

### 列表渲染 v-for

遍历数组：

```
<ul>
    <li v-for="(book,index) in books">{{ book.name }}</li>
</ul>
```

除了数组外，**对象**也可以遍历。

```
<li v-for="(value, key, index) in user">...</li>
```

v-for 还可以迭代整数：

```
<span v-for="n in 10">{{n}}</span>
```

### 数组更新

Vue 检测到数组变化时，并不是直接重新渲染整个列表，相同项不会被重新渲染。

如：

- push()
- pop()
- shift()
- unshift()
- splice()
- sort()
- reverse()
  **使用以上方法会改变这些方法调用的原始数组。**

但有些方法不会改变数组：(总是返回新数组)

- filter()
- concat()
- slice()

但有两个特殊情况，Vue 是不能检测到的：

1.  通过索引直接设置项。
2.  修改数组长度

解决第一个问题可以使用 Vue.set 解决,在 webpack 组件化方式中使用 this.$set.

### 过滤与排序

当你不想改变数组，想通过一个数组的副本来做过滤或排序的显示时，可以使用计算属性来返回过滤或排序后的数组：

```javascript
computed: {
    //计算属性filterBooks依赖books，但不会修改books
    filterBooks: function () {
        return this.books.filter(function(){
            return book.name.match(/javascript/);
        });
    }
}

computed: {
    //新增一个排序的计算属性
    sortedBooks: function () {
        return this.books.sort(function(a,b){
            return a - b;
        });
    }
}
```

### 方法与事件

#### 基本用法

> @click 是 v-on：click 的语法糖.

@click 的表达式可以直接使用 js 语句，也可以是一个在 vue 实例中 methods 中的函数名。

> 需要注意的是，@click 调用的方法名后可以不加（），如果该方法有参数，

> 两种方法详见印象笔记。

_特别的，vue 提供了一个特殊的变量$event，用于访问原生 DOM 事件。_

### 修饰符

Vue.js 为 v-on 提供了事件修饰符。之前提过，修饰符是由点开头的指令后缀来表示的。

- .stop
- .prevent
- .capture
- .self
- .once
- .passive

```html
<!-- 阻止单击事件继续传播 -->
<a v-on:click.stop="doThis"></a>

<!-- 提交事件不再重载页面 -->
<form v-on:submit.prevent="onSubmit"></form>

<!-- 修饰符可以串联 -->
<a v-on:click.stop.prevent="doThat"></a>

<!-- 只有修饰符 -->
<form v-on:submit.prevent></form>

<!-- 添加事件监听器时使用事件捕获模式 -->
<!-- 即元素自身触发的事件先在此处处理，然后才交由内部元素进行处理 -->
<div v-on:click.capture="doThis">...</div>

<!-- 只当在 event.target 是当前元素自身时触发处理函数 -->
<!-- 即事件不是从内部元素触发的 -->
<div v-on:click.self="doThat">...</div>
```

> 使用修饰符时，顺序很重要；相应的代码会以同样的顺序产生。因此，用 v-on:click.prevent.self 会阻止所有的点击，而 v-on:click.self.prevent 只会阻止对元素自身的点击

Vue 还对应 addEventListener 中的 passive 选项提供了 .passive 修饰符。

```html
<!-- 滚动事件的默认行为 (即滚动行为) 将会立即触发 -->
<!-- 而不会等待 `onScroll` 完成  -->
<!-- 这其中包含 `event.preventDefault()` 的情况 -->
<div v-on:scroll.passive="onScroll">...</div>
```

这个 .passive 修饰符尤其能够提升移动端的性能。

### 按键修饰符

全部的按键别名：

- .enter
- .tab
- .delete (捕获“删除”和“退格”键)
- .esc
- .space
- .up
- .down
- .left
- .right

```html
<!-- 同上 -->
<input v-on:keyup.enter="submit">

<!-- 缩写语法 -->
<input @keyup.enter="submit">
```

### 系统修饰键

- .ctrl
- .alt
- .shift
- .meta

> 在 Mac 系统键盘上，meta 对应 command 键 (⌘)。在 Windows 系统键盘 meta 对应 Windows 徽标键 (⊞)。

```html
<!-- Alt + C -->
<input @keyup.alt.67="clear">

<!-- Ctrl + Click -->
<div @click.ctrl="doSomething">Do something</div>
```

> 请注意修饰键与常规按键不同，在和 keyup 事件一起用时，事件触发时修饰键必须处于按下状态。换句话说，只有在按住 ctrl 的情况下释放其它按键，才能触发 keyup.ctrl。而单单释放 ctrl 也不会触发事件。如果你想要这样的行为，请为 ctrl 换用 keyCode：keyup.17。

### 鼠标按钮修饰符

- .left
- .right
- .middle

### 为什么在 HTML 中监听事件?

你可能注意到这种事件监听的方式违背了关注点分离 (separation of concern) 这个长期以来的优良传统。但不必担心，因为所有的 Vue.js 事件处理方法和表达式都严格绑定在当前视图的 ViewModel 上，它不会导致任何维护上的困难。实际上，使用 v-on 有几个好处：

- 扫一眼 HTML 模板便能轻松定位在 JavaScript 代码里对应的方法。
- 因为你无须在 JavaScript 里手动绑定事件，你的 ViewModel 代码可以是非常纯粹的逻辑，和 DOM 完全解耦，更易于测试。
- 当一个 ViewModel 被销毁时，所有的事件处理器都会自动被删除。你无须担心如何自己清理它们。

## 表单输入绑定

你可以用 v-model 指令在表单 `<input>` 及 `<textarea>` 元素上创建双向数据绑定。它会根据控件类型自动选取正确的方法来更新元素。尽管有些神奇，但 v-model 本质上不过是语法糖。它负责监听用户的输入事件以更新数据，并对一些极端场景进行一些特殊处理。

> v-model 会忽略所有表单元素的 value、checked、selected 特性的初始值而总是将 Vue 实例的数据作为数据来源。你应该通过 JavaScript 在组件的 data 选项中声明初始值。

- 普通 input 文本、多行文本（textarea）、单个复选框(checkbox)、单选按钮(radio)、选择框单选时（select 下的 option）使用单一字符串变量绑定即可（复选框也可以使用布尔值）。
- 多个复选框、选择框的多选等要绑定在同一个数组上。

> select 的 v-model 要绑定在 select 上而不是 option 上。

> 对于 select，如果 v-model 表达式的初始值未能匹配任何选项，`<select>` 元素将被渲染为“未选中”状态。在 iOS 中，这会使用户无法选择第一个选项。因为这样的情况下，iOS 不会触发 change 事件。因此，更推荐像上面这样提供一个值为空的禁用选项。

```html
<div id="example-5">
  <select v-model="selected">
    <option disabled value="">请选择</option>
    <option>A</option>
    <option>B</option>
    <option>C</option>
  </select>
  <span>Selected: {{ selected }}</span>
</div>
```

```javascript
new Vue({
  el: "...",
  data: {
    selected: ""
  }
});
```

### 修饰符

- .lazy

在默认情况下，v-model 在每次 input 事件触发后将输入框的值与数据进行同步 (除了上述输入法组合文字时)。你可以添加 lazy 修饰符，从而转变为使用 change 事件进行同步：

```html
<!-- 在“change”时而非“input”时更新 -->
<input v-model.lazy="msg" >
```

- .number

如果想自动将用户的输入值转为数值类型，可以给 v-model 添加 number 修饰符,这通常很有用，因为即使在 type="number" 时，HTML 输入元素的值也总会返回字符串。

- .trim

如果要自动过滤用户输入的首尾空白字符，可以给 v-model 添加 trim 修饰符。

### 组件基础

一个组件的实例：

```javascript
// 定义一个名为 button-counter 的新组件
Vue.component("button-counter", {
  data: function() {
    return {
      count: 0
    };
  },
  template:
    '<button v-on:click="count++">You clicked me {{ count }} times.</button>'
});
```

```html
<div id="components-demo">
  <button-counter></button-counter>
</div>
```

- 可以将组件任意次数的复用。每个组件都会独立的维护变量。
- data 必须是一个函数。

#### 组件的组织

通常一个应用会以一棵嵌套的组件树的形式来组织：

![](https://cn.vuejs.org/images/components.png)

例如，你可能会有页头、侧边栏、内容区等组件，每个组件又包含了其它的像导航链接、博文之类的组件。

为了能在模板中使用，这些组件必须先注册以便 Vue 能够识别。这里有两种组件的注册类型：全局注册和局部注册。至此，我们的组件都只是通过 Vue.component 全局注册的：

```javascript
Vue.component("my-component-name", {
  // ... options ...
});
```

#### 通过 prop 向*子组件*传递数据

_Prop 是你可以在组件上注册的一些自定义特性。当一个值传递给一个 prop 特性的时候，它就变成了那个组件实例的一个属性。_

> ↑ 这句话是理解 prop 的重点。

我们可以用一个 props 选项将其包含在该组件可接受的 prop 列表中：

```javascript
Vue.component("blog-post", {
  //这里声明了一个属性‘title’，可以在其他使用组件的地方给这个属性赋值就把值传递给了该组件
  props: ["title"],
  template: "<h3>{{ title }}</h3>"
});
```

一个组件默认可以拥有任意数量的 prop，任何值都可以传递给任何 prop。在上述模板中，你会发现我们能够在组件实例中访问这个值，就像访问 data 中的值一样。

一个 prop 被注册之后，你就可以像这样把数据作为一个自定义特性传递进来：

```html
<blog-post title="My journey with Vue"></blog-post>
<blog-post title="Blogging with Vue"></blog-post>
<blog-post title="Why Vue is so fun"></blog-post>
```

#### 单个根元素

每个组件的模板只能有一个根元素，需要将模板中的内容包含在一个根元素中。

> 当组件变得很复杂的时候，我们可以合并多个 prop 为一个单独的 prop。

### 通过事件向父级组件发送消息

使用$emit 方法传入事件的名字，来向父级组件触发一个事件。

```html
<button v-on:click="$emit('enlarge-text')">
  Enlarge text
</button>
```

## Prop

### prop 的大小写

HTML 中的特性名是大小写不敏感的，所以浏览器会把所有大写字符解释为小写字符。这意味着当你使用 DOM 中的模板时，camelCase (驼峰命名法) 的 prop 名需要使用其等价的 kebab-case (短横线分隔命名) 命名：

```html
Vue.component('blog-post', {
  // 在 JavaScript 中是 camelCase 的
  props: ['postTitle'],
  template: '<h3>{{ postTitle }}</h3>'
})
```

```javascript
<!-- 在 HTML 中是 kebab-case 的 -->
 <blog-post post-title="hello!"></blog-post>
```

> 如果你使用字符串模板，那么这个限制就不存在了。

### prop 的类型

通常你希望每个 prop 都有指定的值类型。这时，你可以以对象形式列出 prop，这些属性的名称和值分别是 prop 各自的名称和类型：

```javascript
props: {
  title: String,
  likes: Number,
  isPublished: Boolean,
  commentIds: Array,
  author: Object
```

这不仅为你的组件提供了文档，还会在它们遇到错误的类型时从浏览器的 JavaScript 控制台提示用户。你会在这个页面接下来的部分看到类型检查和其它 prop 验证。

### 传递静态或动态 Prop

#### 传入一个布尔值

```html
<!-- 包含该 prop 没有值的情况在内，都意味着 `true`。-->
<blog-post is-published></blog-post>

<!-- 即便 `false` 是静态的，我们仍然需要 `v-bind` 来告诉 Vue -->
<!-- 这是一个 JavaScript 表达式而不是一个字符串。-->
<blog-post v-bind:is-published="false"></blog-post>

<!-- 用一个变量进行动态赋值。-->
<blog-post v-bind:is-published="post.isPublished"></blog-post>
```

其余的传入对象，传入数字、数组写法类似。

#### 传入一个对象的所有属性

如果你想要将一个对象的所有属性都作为 prop 传入，你可以使用不带参数的 v-bind (取代 v-bind:prop-name)。例如，对于一个给定的对象 post：

```
post: {
  id: 1,
  title: 'My Journey with Vue'
}
```

下面的模板：

```html
<blog-post v-bind="post"></blog-post>
```

等价于：

```
<blog-post
  v-bind:id="post.id"
  v-bind:title="post.title"
></blog-post>
```

### 单项数据流

所有的 prop 都使得其父子 prop 之间形成了一个单向下行绑定：父级 prop 的更新会向下流动到子组件中，但是反过来则不行。这样会防止从子组件意外改变父级组件的状态，从而导致你的应用的数据流向难以理解。

额外的，每次父级组件发生更新时，子组件中所有的 prop 都将会刷新为最新的值。这意味着你*不应该在一个子组件内部改变 prop*。如果你这样做了，Vue 会在浏览器的控制台中发出警告。

这里有两种常见的试图改变一个 prop 的情形：

1.  这个 prop 用来传递一个初始值；这个子组件接下来希望将其作为一个本地的 prop 数据来使用。在这种情况下，最好定义一个本地的 data 属性并将这个 prop 用作其初始值：

```javascript
props: ['initialCounter'],
data: function () {
  return {
    counter: this.initialCounter
  }
}
```

2.  这个 prop 以一种原始的值传入且需要进行转换。在这种情况下，最好使用这个 prop 的值来定义一个计算属性：

```javascript
props: ['size'],
computed: {
  normalizedSize: function () {
    return this.size.trim().toLowerCase()
  }
}
```

> 注意在 JavaScript 中对象和数组是通过引用传入的，所以对于一个数组或对象类型的 prop 来说，在子组件中改变这个对象或数组本身将会影响到父组件的状态。

### Prop 验证

我们可以为组件的 prop 指定验证要求，例如你知道的这些类型。如果有一个需求没有被满足，则 Vue 会在浏览器控制台中警告你。这在开发一个会被别人用到的组件时尤其有帮助。

为了定制 prop 的验证方式，你可以为 props 中的值提供一个带有验证需求的对象，而不是一个字符串数组。例如：

```javascript
Vue.component("my-component", {
  props: {
    // 基础的类型检查 (`null` 匹配任何类型)
    propA: Number,
    // 多个可能的类型
    propB: [String, Number],
    // 必填的字符串
    propC: {
      type: String,
      required: true
    },
    // 带有默认值的数字
    propD: {
      type: Number,
      default: 100
    },
    // 带有默认值的对象
    propE: {
      type: Object,
      // 对象或数组且一定会从一个工厂函数返回默认值
      default: function() {
        return { message: "hello" };
      }
    },
    // 自定义验证函数
    propF: {
      validator: function(value) {
        // 这个值必须匹配下列字符串中的一个
        return ["success", "warning", "danger"].indexOf(value) !== -1;
      }
    }
  }
});
```

当 prop 验证失败的时候，(开发环境构建版本的) Vue 将会产生一个控制台的警告。

> 注意那些 prop 会在一个组件实例创建之前进行验证，所以实例的属性 (如 data、computed 等) 在 default 或 validator 函数中是不可用的。

### 类型检查

type 可以是下列原生构造函数中的一个：

- String
- Number
- Boolean
- Array
- Object
- Date
- Function
- Symbol

额外的，type 还可以是一个自定义的构造函数，并且通过 instanceof 来进行检查确认。例如，给定下列现成的构造函数：

```javascript
function Person(firstName, lastName) {
  this.firstName = firstName;
  this.lastName = lastName;
}
```

你可以使用：

```javascript
Vue.component("blog-post", {
  props: {
    author: Person
  }
});
```

来验证*author* prop 值是否是通过 new Person 创建的。

### 非 Prop 的特性

一个非 prop 特性是指传向一个组件，但是该组件并没有相应 prop 定义的特性。

因为显式定义的 prop 适用于向一个子组件传入信息，然而组件库的作者并不总能预见组件会被用于怎样的场景。这也是为什么组件可以接受任意的特性，而这些特性会被添加到这个组件的根元素上。

例如，想象一下你通过一个 Bootstrap 插件使用了一个第三方的 `<bootstrap-data-input>` 组件，这个插件需要在其 `<input>` 上用到一个 data-date-picker 特性。我们可以将这个特性添加到你的组件实例上：

```
<bootstrap-date-input data-date-picker="activated"></bootstrap-date-input>
```

然后这个 data-date-picker="activated" 特性就会自动添加到 <bootstrap-date-input> 的根元素上。

## 自定义事件

### 事件名

跟组件和 prop 不同，事件名不存在任何自动化的大小写转换。而是触发的事件名需要*完全匹配*监听这个事件所用的名称。举个例子，如果触发一个 camelCase 名字的事件：

跟组件和 prop 不同，事件名不会被用作一个 JavaScript 变量名或属性名，所以就没有理由使用 camelCase 或 PascalCase 了。并且 v-on 事件监听器在 DOM 模板中会被自动转换为全小写 (因为 HTML 是大小写不敏感的)，所以 v-on:myEvent 将会变成 v-on:myevent——导致 myEvent 不可能被监听到。

因此，我们推荐你*始终使用 kebab-case 的事件名*。

### 自定义组件的 v-model

一个组件上的 v-model 默认会利用名为 value 的 prop 和名为 input 的事件，但是像单选框、复选框等类型的输入控件可能会将 value 特性用于不同的目的。model 选项可以用来避免这样的冲突：

```javascript
Vue.component("base-checkbox", {
  model: {
    prop: "checked",
    event: "change"
  },
  props: {
    checked: Boolean
  },
  template: `
    <input
      type="checkbox"
      v-bind:checked="checked"
      v-on:change="$emit('change', $event.target.checked)"
    >
  `
});
```

现在在这个组件上使用 v-model 的时候：

```html
<base-checkbox v-model="lovingVue"></base-checkbox>
```

这里的 `lovingVue` 的值将会传入这个名为 `checked` 的 `prop`。同时当 <base-checkbox> 触发一个 change 事件并附带一个新的值的时候，这个 lovingVue 的属性将会被更新。

> 注意你仍然需要在组件的 `props` 选项里声明 `checked` 这个 prop。

### 将原生事件绑定到组件

你可能有很多次想要在一个组件的根元素上直接监听一个原生事件。这时，你可以使用 v-on 的 .native 修饰符：

```html
<base-input v-on:focus.native="onFocus"></base-input>
```

在有的时候这是很有用的，不过在你尝试监听一个类似 `<input>` 的非常特定的元素时，这并不是个好主意。比如上述 `<base-input>` 组件可能做了如下重构，所以根元素实际上是一个 `<label>` 元素：

...未完待续

## 插槽

### 插槽内容

Vue 实现了一套内容分发的 API，这套 API 基于当前的 Web Components 规范草案，将 `<slot>` 元素作为承载分发内容的出口。

它允许你像这样合成组件：

```html
<navigation-link url="/profile">
  Your Profile
</navigation-link>
```

然后你再 `<navigation-link>` 的模板中可能会写为：

```html
<a
  v-bind:href="url"
  class="nav-link"
>
  <slot></slot>
</a>
```

_当组件渲染时，这个 <slot> 元素将会被替换为“Your Profile”_
插槽内可以包含任何模板代码，包括 HTML：

```html
<navigation-link url="/profile">
  <!-- 添加一个 Font Awesome 图标 -->
  <span class="fa fa-user"></span>
  Your Profile
</navigation-link>
```

> 如果 <navigation-link> 没有包含一个 <slot> 元素，则任何传入它的内容都会被抛弃。

### 具名插槽

有些时候我们需要多个插槽。例如，一个假设的 <base-layout> 组件多模板如下：

```html
<div class="container">
  <header>
    <!-- 我们希望把页头放这里 -->
  </header>
  <main>
    <!-- 我们希望把主要内容放这里 -->
  </main>
  <footer>
    <!-- 我们希望把页脚放这里 -->
  </footer>
</div>
```

_对于这样的情况，<slot> 元素有一个特殊的特性：name。这个特性可以用来定义额外的插槽：_

```html
<div class="container">
  <header>
    <slot name="header"></slot>
  </header>
  <main>
    <slot></slot>
  </main>
  <footer>
    <slot name="footer"></slot>
  </footer>
</div>
```

在向具名插槽提供内容的时候，我们可以在一个父组件的 <template> 元素上使用 `slot` 特性：

```html
<base-layout>
  <template slot="header">
    <h1>Here might be a page title</h1>
  </template>

  <p>A paragraph for the main content.</p>
  <p>And another one.</p>

  <template slot="footer">
    <p>Here's some contact info</p>
  </template>
</base-layout>
```

另一种 slot 特性的用法是直接用在一个普通的元素上：

```html
<base-layout>
  <h1 slot="header">Here might be a page title</h1>

  <p>A paragraph for the main content.</p>
  <p>And another one.</p>

  <p slot="footer">Here's some contact info</p>
</base-layout>
```

我们还是可以保留一个未命名插槽，这个插槽是默认插槽，也就是说它会作为所有未匹配到插槽的内容的统一出口。上述两个示例渲染出来的 HTML 都将会是：

```html
<div class="container">
  <header>
    <h1>Here might be a page title</h1>
  </header>
  <main>
    <p>A paragraph for the main content.</p>
    <p>And another one.</p>
  </main>
  <footer>
    <p>Here's some contact info</p>
  </footer>
</div>
```

### 插槽的默认内容

有的时候为插槽提供默认的内容是很有用的。例如，一个 <submit-button> 组件可能希望这个按钮的默认内容是“Submit”，但是同时允许用户覆写为“Save”、“Upload”或别的内容。

你可以在 <slot> 标签内部指定默认的内容来做到这一点。

```
<button type="submit">
  <slot>Submit</slot>
</button>
```

如果父组件为这个插槽提供了内容，则默认的内容会被替换掉。

### 编译作用域

```
<navigation-link url="/profile">
  Logged in as {{ user.name }}
</navigation-link>
```

_父组件模板的所有东西都会在父级作用域内编译；子组件模板的所有东西都会在子级作用域内编译。_

### 作用域插槽

...

## 动态组件 & 异步组件

### 在动态组件上使用 keep-alive

我们之前曾经在一个多标签的界面中使用 is 特性来切换不同的组件：

```
<component v-bind:is="currentTabComponent"></component>
```

重新创建动态组件的行为通常是非常有用的，但是在这个案例中，我们更希望那些标签的组件实例能够被在它们第一次被创建的时候缓存下来。为了解决这个问题，我们可以用一个 `<keep-alive>` 元素将其动态组件包裹起来。

```
<!-- 失活的组件将会被缓存！-->
<keep-alive>
  <component v-bind:is="currentTabComponent"></component>
</keep-alive>
```

> 注意这个 `<keep-alive>` 要求被切换到的组件都有自己的名字，不论是通过组件的 name 选项还是局部/全局注册。

### 异步组件


在大型应用中，我们可能需要将应用分割成小一些的代码块，并且只在需要的时候才从服务器加载一个模块。为了简化，Vue 允许你以一个工厂函数的方式定义你的组件，这个工厂函数会异步解析你的组件定义。Vue 只有在这个组件需要被渲染的时候才会触发该工厂函数，且会把结果缓存起来供未来重渲染。例如：

```
Vue.component('async-example', function (resolve, reject) {
  setTimeout(function () {
    // 向 `resolve` 回调传递组件定义
    resolve({
      template: '<div>I am async!</div>'
    })
  }, 1000)
})
```

## 处理边界情况

### 访问父级组件实例

和 `$root` 类似，`$parent` 属性可以用来从一个子组件访问父组件的实例。它提供了一种机会，可以在后期随时触达父级组件，以替代将数据以 prop 的方式传入子组件的方式。

## 过渡 & 动画

### 概述

Vue 在插入、更新或者移除 DOM 时，提供多种不同方式的应用过渡效果。
包括以下工具：

- 在 CSS 过渡和动画中自动应用 class
- 可以配合使用第三方 CSS 动画库，如 Animate.css
- 在过渡钩子函数中使用 JavaScript 直接操作 DOM
- 可以配合使用第三方 JavaScript 动画库，如 Velocity.js
  在这里，我们只会讲到进入、离开和列表的过渡，你也可以看下一节的 管理过渡状态。

### 单元素/组件的过渡

Vue 提供了 transition 的封装组件，在下列情形中，可以给任何元素和组件添加进入/离开过渡

- 条件渲染 (使用 v-if)
- 条件展示 (使用 v-show)
- 动态组件
- 组件根节点
