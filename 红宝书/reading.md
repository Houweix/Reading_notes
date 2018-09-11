## 一、基础

- ECMAScript 中的基本数据类型包括 Unde-fined、Null、Boolean、Number 和 String。

## 三、函数

- ECMAScript 中的函数使用 `function` 关键字来声明，后跟一组参数以及函数体。

- return 语句也可以不带有任何返回值。在这种情况下，函数在停止执行后将返回 unde-fined 值。

- 编译器不会在意传递参数的数量，原因是 ECMAScript 中的 `参数在内部是用一个数组` 来表示的。在函数体内可以通过 `arguments` 对象来访问这个参数数组。

- `arguments对象` 只是与数组类似（它并不是 Array 的实例），因为可以 `使用方括号语法访问它的每一个元素`，使用 `length属性` 来确定传递进来多少个参数。

```js
function howManyArgs() {
  alert(arguments.length);
}

function howManyArgs() {
  alert(arguments.length);
}
```

- 那就是 `argument` 的值永远与 `对应命名参数的值` 保持同步。

- 函数没有重载，但是可以通过对参数的检查和处理模拟重载。

## 四、变量、作用域和内存问题

- ECMAScript 变量可能包含两种不同数据类型的值：`基本类型值` 和 `引用类型值`。基本类型值指的是简单的数据段，而引用类型值指那些可能由多个值构成的对象。

- 对于 `引用类型的值`，我们可以为其 `添加属性` 和 `方法`，也可以改变和删除其属性和方法。

- 当从一个变量向另一个变量复制引用类型的值时，同样也会将存储在变量对象中的值复制一份放到为新变量分配的空间中。不同的是，`这个值的副本实际上是一个指针`，而这个指针指向存储在堆中的一个对象。复制操作结束后，`两个变量实际上将引用同一个对象`。因此，改变其中一个变量，就会影响另一个变量。

- ECMAScript 中所有函数的参数都是按值传递的。

- 基本类型值的传递如同基本类型变量的复制一样，而引用类型值的传递，则如同引用类型变量的复制一样 `(复制指针)`。

- 即使在函数内部修改了参数的值，但原始的引用仍然保持未变。

```js
function setName(obj) {
  obj.name = "Nicholas";
  //  '复制' 的指针重新指向了一块新的地方，因此对这块内存的修改不会影响原始引用。
  obj = new Object();
  obj.name = "Greg";
}

var person = new Object();
setName(person); //"Nicholas"
alert(person.name);
```

- 可以把ECMAScript函数的参数想象成局部变量。

- 检测类型：`typeof` 操作符是确定一个变量是字符串、数值、布尔值，还是undefined的最佳工具。如果变量的值是一个 `对象` 或 `null`，则typeof操作符会像下面例子中所示的那样返回`"object"`