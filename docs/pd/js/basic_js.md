!> 读 现代JavaScript教程 所做笔记

## 简介

### 手册

- **MDN（Mozilla）JavaScript 索引** 是一个带有用例和其他信息的手册。它是一个获取关于个别语言函数、方法等深入信息的很好的来源。
  
  你可以在 https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference 阅读它。
  
  虽然，利用互联网搜索通常是最好的选择。只需在查询时输入“MDN [关键字]”，例如 https://google.com/search?q=MDN+parseInt 搜索 parseInt 函数。

## JavaScript 基础知识

嵌入js

?> 一般来说，只有最简单的脚本才嵌入到 HTML 中。更复杂的脚本存放在单独的文件中。
使用独立文件的好处是浏览器会下载它，然后将它保存到浏览器的 缓存 中。
之后，其他页面想要相同的脚本就会从缓存中获取，而不是下载它。所以文件实际上只会下载一次。
这可以节省流量，并使得页面（加载）更快。

### 类型转换

```javascript
"4px"-2 = NaN

obj === null  // 检测为null  (一般用!判断null和其他一起)
```

> 使用+ 将非数字转为数字,等同于Number(...)

```javascript
 +apples + +oranges  //两个数字字符串相加

 alert( 4 ** (1/2) ); // 2 (1 / 2 幂相当于开方，这是数学常识)
 alert( 8 ** (1/3) ); // 2 (1 / 3 幂相当于开三次方)
```

### 代码风格 推荐 stardardjs风格

!> vscode 使用： 
```bash
npm i standard -g
npm i eslint -g
npm i eslint-plugin-vue@next -g
npm i eslint-config-standard eslint-plugin-standard eslint-plugin-promise eslint-plugin-import 
eslint-plugin-node -g
```
- vscode插件：`prittier-standard ` 
- vscode设置： 
  
```json
"[javascript]": {
        "editor.defaultFormatter": "numso.prettier-standard-vscode"
    },
```
!> 限制

- 除需要转义的情况外，**字符串统一使用单引号**。

- **始终使用** `===` 替代 `==`。

- **不要丢掉**异常处理中`err`参数。

- **使用浏览器全局变量时加上** `window.` 前缀。  
  
  `document`、`console` 和 `navigator` 除外。

- **不允许有连续多行空行**。

- **条件语句中赋值语句**使用括号包起来。这样使得代码更加清晰可读，而不会认为是将条件判断语句的全等号（`===`）错写成了等号（`=`）。

```js
// ✓ ok
while ((m = text.match(expr))) {
  // ...
}

// ✗ avoid
while (m = text.match(expr)) {
  // ...
}
```

- **单行代码块两边加空格**。

```js
  function foo () {return true}    // ✗ avoid
  function foo () { return true }  // ✓ ok
```

- **对于变量和函数名统一使用驼峰命名法**。

```js
  function myFunction () { }     // ✓ ok
  function my_function () { }    // ✗ avoid

  var myVar = 'hello'            // ✓ ok
  var my_var = 'hello'           // ✗ avoid
```
- **不允许有多余的行末逗号**。
- 

> 使用ESlint 做代码自动检测 备选jshlint prettier好像不错 webstorm自带

> 使用mocha进行自动化测试

> 使用babel新规范


### 文件头加上 'use strict' 进入严格模式

### 数据类型

> 数字类型

```javascript
// 这些是相同的写法
let billion = 1000000000
let billion = 1e9

let ms = 0.0000012
let ms = 1.2e-6

// 数字类型转换
let num = 255;

alert( num.toString(16) );  // ff
alert( num.toString(2) );   // 11111111
alert( 123456..toString(36) ); // 2n9c

// Mach.trunc 舍弃小数点后的  和ceil类似 ceil负数会加  不支持ie
//  toFixed(n) 将点数后的数字四舍五入到 n 个数字并返回结果的字符串表示。
//  (推荐第二种)
let num = 12.34;
alert( num.toFixed(5) ); // "12.34000", added zeroes to make exactly 5 digits

// 浮点数运算一定要转为整数 再除, 同样适用于 php java c perl ruby
```

>  字符串

```javascript
//只要记住：if (~str.indexOf(...)) 读作 “if found”。

if (~str.indexOf(...)) === if ( str.indexOf(...) != -1)

// includes, startsWith, endsWith  
str.includes(substr, pos)  // 和上面一样,更现代的写法
str.startWith(substr)

// 截取字符串总是使用
str.slice(start, end)

// 比较两个字符串

// 调用 str.localeCompare(str2)：
// 根据语言规则，如果 str 大于 str2 返回 1。
// 如果if str 小于 str2 返回 -1。
// 如果相等，返回 0。
```

> 数组

```javascript
// 清空数组最好的方法就是：
arr.length = 0;
```

遍历数组的元素：

- `for (let i=0; i<arr.length; i++)` — 运行的最快, 可兼容旧版本浏览器。
- `for (let item of arr)` — 现代语法，只能访问 items。
- `for (let i in arr)` — 永远不会使用。

数组删除或添加元素

`array.splice(start[, deleteCount[, item1[, item2[, *...]]]]*)`

如果我们想检查是否包含需要的元素，并且不想知道确切的索引，那么 `arr.includes` 是首选。

```javascript
let users = [
  {id: 1, name: "John"},
  {id: 2, name: "Pete"},
  {id: 3, name: "Mary"}
];

let user = users.find(item => item.id == 1);

alert(user.name); // John
```

数组经常被使用，以至于有一种特殊的方法用于判断：[Array.isArray(value)](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/Array/isArray)。如果 `value` 是一个数组，则返回 `true`；否则返回 `false`。

数组方法备忘录：

- 添加/删除元素：
  
  - `push(...items)` — 从结尾添加元素，
  - `pop()` — 从结尾提取元素，
  - `shift()` — 从开头提取元素，
  - `unshift(...items)` — 从开头添加元素，
  - `splice(pos, deleteCount, ...items)` — 从 `index` 开始：删除 `deleteCount` 元素并在当前位置插入元素。
  - `slice(start, end)` — 它从所有元素的开始索引 `"start"` 复制到 `"end"` (不包括 `"end"`) 返回一个新的数组。
  - `concat(...items)` — 返回一个新数组：复制当前数组的所有成员并向其中添加 `items`。如果有任何`items` 是一个数组，那么就取其元素。

- 查询元素：
  
  - `indexOf/lastIndexOf(item, pos)` — 从 `pos` 找到 `item`，则返回索引否则返回 `-1`。
  - `includes(value)` — 如果数组有 `value`，则返回 `true`，否则返回 `false`。
  - `find/filter(func)` — 通过函数过滤元素，返回 `true` 条件的符合 find 函数的第一个值或符合 filter 函数的全部值。
  - `findIndex` 和 `find` 类似，但返回索引而不是值。

- 转换数组：
  
  - `map(func)` — 从每个元素调用 `func` 的结果创建一个新数组。
  - `sort(func)` — 将数组倒序排列，然后返回。
  - `reverse()` — 在原地颠倒数组，然后返回它。
  - `split/join` — 将字符串转换为数组并返回。
  - `reduce(func, initial)` — 通过为每个元素调用 `func` 计算数组上的单个值并在调用之间传递中间结果。

- 迭代元素：
  
  - `forEach(func)` — 为每个元素调用 `func`，不返回任何东西。

- 其他： – `Array.isArray(arr)` 检查 `arr` 是否是一个数组。

请注意，`sort`，`reverse` 和 `splice` 方法修改数组本身。

这些方法是最常用的方法，它们覆盖 99％ 的用例。但是还有其他几个：

- [arr.some(fn)](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/Array/some)/[arr.every(fn)](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/Array/every) 检查数组。
  
  在类似于 `map` 的数组的每个元素上调用函数 `fn`。如果任何/所有结果为 `true`，则返回 `true`，否则返回 `false`。

- [arr.fill(value, start, end)](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/Array/fill) — 从 `start` 到 `end` 用 `value` 重复填充数组。

- [arr.copyWithin(target, start, end)](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/Array/copyWithin) — copies its elements from position `start` till position `end` into *itself*, at position `target` (overwrites existing).将其元素从 `start` 到 `end` 在 `target` 位置复制到 **本身**（覆盖现有）。

- ``Array.from(obj[, mapFn, thisArg])` 将可迭代对象或类数组对象 `obj` 转化为真正的 `Array` 数组，然后我们就可以对它应用数组的方法。可选参数 `mapFn` 和 `thisArg` 允许我们对每个元素都应用一个函数。

> **Map**

[Map](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/Map) 是一个键值对的集合，很像 `Object`。但主要的区别是，`Map` 允许所有数据类型作为键。

### DOM

| Method                   | Searches by... | Can call on an element? | Live? |
| ------------------------ | -------------- | ----------------------- | ----- |
| `getElementById`         | `id`           | -                       | -     |
| `getElementsByName`      | `name`         | -                       | ✔     |
| `getElementsByTagName`   | tag or `'*'`   | ✔                       | ✔     |
| `getElementsByClassName` | class          | ✔                       | ✔     |
| `querySelector`          | CSS-selector   | ✔                       | -     |
| `querySelectorAll`       | CSS-selector   | ✔                       | -     |

> attributes 的一些方法：

- `elem.hasAttribute(name)` —— 检查是否存在这个特性
- `elem.getAttribute(name)` —— 获取这个特性
- `elem.setAttribute(name, value)` —— 把这个特性设置为 name 值
- `elem.removeAttribute(name)` —— 移除这个特性
- `elem.attributes` —— 所有特性的集合

> `classList` 方法：

- `elem.classList.add/remove("class")` —— 添加/移除类。
- `elem.classList.toggle("class")` —— 如果类存在就移除，否则添加。
- `elem.classList.contains("class")` —— 返回 `true/false`，检查给定类。

> 在管理 class 时，有两个 DOM 属性：

- `className` —— 字符串值可以很好地管理整个类集合。
- `classList` —— 拥有 `add/remove/toggle/contains` 方法的对象可以很好地支持单独的类。

> 改变样式：

- `style` 属性是一个带有 camelCased 样式的对象。对它的读取和修改 `"style"` 属性中的单个属性等价。要留意如果应用 `important` 和其他稀有内容 —— 在 [MDN](https://developer.mozilla.org/zh/docs/Web/API/CSSStyleDeclaration) 上有一个方法列表。

- `style.cssText` 属性对应于整个“样式”属性，即完整的样式字符串。
  
  获取已经解析的样式（对应于所有类，在应用所有 CSS 并计算最终值后）：

- `getComputedStyle(elem[, pseudo])` 返回与 `style` 对象类似且包含了所有类的对象，是只读的。

> 滚动：

- 读取当前的滚动：`window.pageYOffset/pageXOffset`
- 改变当前的滚动：
  - `window.scrollTo(pageX,pageY)` — 绝对定位
  - `window.scrollBy(x,y)` — 相对当前位置的滚动
  - `elem.scrollIntoView(top)` — 滚动到正好`elem`可视的位置（`elem` 与窗口的顶部/底部对齐）

### 事件

这里有一张最有用的 DOM 事件列表，请看：

**鼠标事件：**

- `click` —— 当鼠标点击一个元素时（触摸屏设备在 tap 时生成）。
- `contextmenu` —— 当鼠标右击一个元素时。
- `mouseover` / `mouseout` —— 当鼠标光标移入或移出一个元素时。
- `mousedown` / `mouseup` —— 当鼠标按下/释放一个元素时。
- `mousemove` —— 当鼠标移出时。

**表单元素事件**：

- `submit` —— 当访问者提交了一个 `<form>` 时。
- `focus` —— 当访问者聚焦一个元素时，例如 `<input>`。

**键盘事件**：

- `keydown` and `keyup` —— 当访问者按下然后松开按钮时。

**Document 事件**：

- `DOMContentLoaded` —— 当加载和处理 HTML 时，DOM 将会被完整地构建。

**CSS 事件**：

- `transitionend` —— 当 CSS 动画完成时。

> 有 3 种方法可以分发事件处理器：

1. HTML 属性：`onclick="..."`。
2. DOM 属性 `elem.onclick = function`。
3. 方法：添加 `elem.addEventListener(event, handler[, phase])`，移除 `removeEventListener`。

> [**insertAdjacentHTML**](https://developer.mozilla.org/zh-CN/docs/Web/API/Element/insertAdjacentHTML) 用来将结果节点插入到dom树的指定位置, 比innerHTML好

```html
element.insertAdjacentHTML(position, text);
```

position是相对于元素的位置，并且必须是以下字符串之一：

- 'beforebegin';
  
  元素自身的前面。

- 'afterbegin';
  
  `插入元素内部的第一个子节点之前。`

- 'beforeend';
  
  插入元素内部的最后一个子节点之后。

- 'afterend';
  
  元素自身的后面。

text是要被解析为HTML或XML,并插入到DOM树中的字符串。

### 正则 regex

> 在 JavaScript 中，有 5 个修饰符：

- `i`
  
  使用此修饰符后，搜索时不区分大小写: `A` 和 `a` 没有区别（具体看下面的例子）。

- `g`
  
  使用此修饰符后，搜索时会查找所有的匹配项，而不只是第一个（在下一章会讲到）。

- `m`
  
  多行模式（详见章节 文章 "regexp-multiline" 未找到）。

- `u`
  
  开启完整的 unicode 支持。该修饰符能够修正对于代理对的处理。更详细的内容见章节 [unicode 标记](https://zh.javascript.info/regexp-unicode)。

- 只查找第一个匹配项的：
  
  找到第一个匹配项的位置 — `str.search(reg)`。

- 找到完全匹配 — `str.match(reg)`。

- 检查是否有符合条件的匹配 — `regexp.test(str)`。

- 从指定位置查找匹配 — `regexp.exec(str)`，设置 `regexp.lastIndex` 位置。

- 查找全匹配：
  
  由匹配项组成的数组 — `str.match(reg)`, 使用 `g` 修饰符。

- 获取所有匹配项的完整信息 — `regexp.exec(str)`，在循环中使用 `g` 修饰符。

- 搜索然后替换：
  
  替换成另一个字符串或者函数返回的结果 — `str.replace(reg, str|func)`

- 拆分字符串：
  
  `str.split(str|reg)`

`\d`（“d” 来源于 “digit”）

一个数字：`0` 到 `9` 的一个字符。

`\s`（“s” 来源于 “space”）

一个空格符：包括空格，制表符和换行符。

`\w`（“w” 来源于 “word”）

一个单字字符：英语字母表中的一个字母或者一个数字或一个下划线。非英语字母（像西里尔字母或者印地语）不包含在 `\w` 里面。

- **\d** —— 表示的意思和 `[0-9]` 一样，
- **\w** —— 表示的意思和 `[a-zA-Z0-9_]` 一样，
- **\s** —— 表示的意思和 `[\t\n\v\f\r ]` 加上其它一些 unicode 编码的空格一样。

> 单词边界：\b

单词边界 `\b` —— 匹配单个单词, 不适用于中文。

量词有两种工作模式：

- 贪婪模式
  
  默认情况下，正则表达式引擎会尝试尽可能多地重复量词。例如，`\d+` 检测所有可能的字符。当不可能检测更多（没有更多的字符或到达字符串末尾）时，然后它再匹配模式的剩余部分。如果没有匹配，则减少重复的次数（回溯），并再次尝试。

- 懒惰模式
  
  通过在量词后添加问号 `?` 来启用。在每次重复量词之前，引擎会尝试去匹配模式的剩余部分。

Javascript 中，一个点（.）表示除换行符之外的任意字符。所以这是无法匹配多行的。

我们可以用 `[\s\S]`，而不是用点（.）来匹配“任何东西”

- `?:x` 匹配'x'但是不记住
- `x(?=y)` 先行断言: 匹配x仅当x后面跟着y
- `(?<=y)x` 后行断言: 匹配x仅当x前面是y
- `x(?!y)` 正向否定查找: 匹配x仅当x后面不跟着y

> 使用正则的方法

| 方法        | 描述                                                  |
| --------- | --------------------------------------------------- |
| `exec`    | 一个在字符串中执行查找匹配的RegExp方法，它返回一个数组（未匹配到则返回null）。        |
| `test`    | 一个在字符串中测试是否匹配的RegExp方法，它返回true或false。               |
| `match`   | 一个在字符串中执行查找匹配的String方法，它返回一个数组或者在未匹配到时返回null。       |
| `search`  | 一个在字符串中测试匹配的String方法，它返回匹配到的位置索引，或者在失败时返回-1。        |
| `replace` | 一个在字符串中执行查找匹配的String方法，并且使用替换字符串替换掉匹配到的子字符串。        |
| `split`   | 一个使用正则表达式或者一个固定字符串分隔一个字符串，并将分隔后的子字符串存储到数组中的String方法 |

### 异步

函数前的 `async` 关键字有两个作用：

1. 总是返回 promise。
2. 允许在其中使用 `await`。

在 promise 之前的 `await` 关键字，使 JavaScript 等待 promise 被处理，然后：

1. 如果有 error，就会产生异常，就像在那个地方调用了 `throw error` 一样。
2. 否则，就会返回值，我们可以给它分配一个值。

它们一起为编写易于读写的异步代码提供了一个很好的框架。

对于 `async/await`，我们很少需要编写 `promise.then/catch`，但我们不应该忘记它们是基于 promise 的。因为有时（例如，在最外面的范围）我们不得不使用这些方法。`Promise.all` 也是一个很好的东西，它能够同时等待很多任务。
