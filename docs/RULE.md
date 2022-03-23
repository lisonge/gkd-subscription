# 规则语法说明

本规则语法扩展于 CSS 匹配器部分语法

一条规则由 节点匹配单元 和 连接匹配单元 交叉构成, 相同类型匹配单元不可相邻

两个单元之间必须相隔 一个或以上 的 空格 (\x20)

匹配到第一个元素后立刻停止匹配

## 节点匹配单元

一个节点匹配单元由 类名匹配器 和它的多个 属性匹配器 构成

```txt
TextView[text='hello'][childCount>6][clickable=true]
// 匹配一个 TextView , 它的 text 属性是 hello 并且 childCount > 6 并且 clickable=true
```

### 属性匹配器

属性匹配器由一个 [] 包裹, 从左至右依次是 属性名, 比较运算符, 比较值

属性名: 由 26 个字母大小写 以及 .\_1234567890 构成

目前一个节点有如下属性可放在属性匹配器中

- index:int
- childCount:int
- depth:int
- id:string?
- text:string?
- text.length:int?
- hintText:string?
- hintText.length:int?
- contentDescription:string?
- contentDescription.length:int?
- isPassword:boolean?
- isChecked:boolean?

<!--
- content-desc:string?
- hintText:string?
- checkable:boolean
- checked:boolean
- clickable:boolean
- enabled:boolean
- focusable:boolean
- focused:boolean
- scrollable:boolean
- long-clickable:boolean
- password:boolean
- selected:boolean
-->

比较值目前有 4 种 int, string, boolean, null

int: 10 进制整数, 取值范围与 java int 一致

string: 由反引号 ` 包裹, 内部的单引号需要转义表示为 ``, 其他转义与 java string 一致

boolean: 有两种 false, true

另外它们有个共同值 null

对于 int, 有比较运算 =, !=, <, ,>, >=, <=

对于 string, 有比较运算 =, !=, ^=, $=, \*=

对于 boolean, 有比较运算 =, !=

## 连接匹配单元

连接匹配单元表示 `节点匹配单元` 之间的关系

- 直接祖先匹配 `View >1 View` 泛匹配 `View >n View`
- 直接子代匹配 `View <1 View` 泛匹配 `View <n View`
- 前置兄弟匹配 `View +1 View` 泛匹配 `View +n View`
- 后置兄弟匹配 `View -1 View` 泛匹配 `View -n View`

考虑有 `View [-+><]f(n) View` 泛匹配, 其中 `fn(n)` 是关于 n 的整数多项式

<!-- 示例: View[x1=1][x2^='2'][x3^='3'][left>30vw](3px, 4px) -->
<!-- 示例: View[x1=1][x2^='2'][x3^='3'][left>30vw](50%, 50%) -->
<!-- int/boolean/string/null/length -->
<!-- length:1dp,1px,1vw,1vh,1% -->
