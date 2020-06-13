## HTML 语言与扩展

### HTML 定义

* HTML 与 XML、SGML 关系

* SGML 使用 DTD 进行定义，HTML 使用 namespace 方式来定义

  参考：https://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd

* ```javascript
  <!ENTITY % HTMLlat1 PUBLIC
     "-//W3C//ENTITIES Latin 1 for XHTML//EN"
     "xhtml-lat1.ent">
  %HTMLlat1;
  
  <!ENTITY % HTMLsymbol PUBLIC
     "-//W3C//ENTITIES Symbols for XHTML//EN"
     "xhtml-symbol.ent">
  %HTMLsymbol;
  
  <!ENTITY % HTMLspecial PUBLIC
     "-//W3C//ENTITIES Special for XHTML//EN"
     "xhtml-special.ent">
  %HTMLspecial;
  
  ```

  > ent 文件可以使用 https://www.w3.org/TR/xhtml1/DTD/ + ent 文件名的方式来获取对应文件。
  >
  > 字符打印方式： lambda 使用 "&lambda" / "U+03BB" 方式来进行访问

* https://www.w3.org/1999/xhtml/

### HTML 标签语义

* aside 并不是 HTML 语义上的侧栏，不要理解为 sidebar
* 如何理解标签的语义，首先需要明白标签所代表的的含义，以及它在上下文中表达的关系。例如：br 是否需要放在 h1 和 h2 标签中间，要根据 h1 和 h2 标签之间的关系来确定是否需要，如果是补充说明的关系，就不需要

* 语义和元素外在表现的样式是不相关的，不要为了呈现某种表现样式而使用不相关的语义标签。大多数情况下，不知道具体语义的话，用 div 和 span
* 其他一些常用的标签： dfn、address、time
* 并非所有的标签都需要闭合的。p 标签可以不闭合。
* 了解 html 中一些标签的作用，可以参考： https://html.spec.whatwg.org/multipage/parsing.html

### HTML 合法元素

* `Element <tagname>...</tagname>`
* `Text:text`
* `Comment:<!-- comments -->`
* `DocumentType: <!Doctype html>`
* `ProcessingInstruction: <?a 1?>`

* `CDATA <![CDATA]>`

### 字符引用

直接一个 about:blank 打开空的标签页，再使用 `document.body.innerHTML="&quot"` 可以直接查看

* `&#161;`
* `&amp;`

* `&lt;`
* `&quot;`

## DOM

### Node

#### 导航类操作

* parentNode
* childNodes
* firstChild
* lastChild
* nextSibling
* perviousSibling

#### 修改操作

* appendChild
* insertBefore
* removeChild
* replaceChild

> 面试考点：
>
> 1. DOM 树上有 2 个位 A，B。当你把一个 DOM 节点 C 成功插入到 A 中，此时再把 C 插入 B 位置，之前的 A 位置的结点会被 remove 掉。
>
>    **元素二次插入会将元素从原有位置挪走，互不相关的 DOM 树也是这样**
>
> 2. removeChild / appendChild 执行完毕后，childNodes 也会跟着实时变化，即使把 childNodes 存放在一个变量中，这个变量也是会变化的。

#### 高级操作

* compareDocumentPosition:：比较两个节点关系

* contains：检查一个节点是否包含另一个节点

* isEqualNode: 检查两个节点是否完全相同

* isSameNode: 检查两个节点是否是同一个节点，在 JavaScript 中可以使用 `===` 来进行比较

* cloneNode：复制一个节点，如果传入参数为 true，则会连同子元素做深拷贝

* ```javascript
  <div id="x">
      <div>1</div>
      <div>2</div>
      <div>3</div>
      <div>4</div>
  </div>
  
  <div id="b"></div>
  
  <script>
      var x = document.getElementById("x")
      var b = document.getElementById("b")
  
      // 错误示例
      /**
        * 当 i = 0 时，b append 结点值为 1 的结点，由于 x.children 是实时变化的，x.children.length 值减 1，为 3， i++ 为 1
        * 当 i = 1 时，b append 结点值为 3 的结点(下标为 1 的结点)， x.children.length 值减 1，为 2，i++ 为 2
        * i 不满足条件，循环退出
      */
      // for (var i = 0; i < x.children.length; i++) {
      //     console.log(i, x.children[i])
      //     b.appendChild(x.children[i])
      // }
  
      // 正确示例
      while(x.children.length) {
          b.appendChild(x.children[0])
      }
  </script>
  ```

### Event

* addEventListener 可以直接添加 handleEvent

```javascript
document.body.addEventListener("click", { handleEvent: function () { console.log("I am clicked!") } });
```

