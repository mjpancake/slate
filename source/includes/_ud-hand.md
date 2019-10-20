# 手牌 `Hand`

本文档中「手牌」一词指包含纯手牌、自摸牌、副露、暗杠的广义手牌。

通过`game:gethand(<who>)`获取的手牌代表牌桌上的真实手牌，
具有「只读」属性，
只能由松饼系统修改，技能代码无法干涉（否则就是作弊了）。
如果需要修改手牌（为了计算手牌改变后的向听数等目的），
可以通过下文中的「复制构造」方法，
创建一份不带只读属性的假想手牌，从而调用要求`Hand`非只读的方法。

但修改不是乱改，否则是要谢罪的。
`Hand`应始终处于「14 - 3k 张」或「13 - 3k 张」的状态（k 代表 bark 数，范围是 0 ~ 4）。
如果你尝试让`Hand`脱离这两种状态，代码会报错并中止运行。

<aside class="warning">
<code>Hand</code>的许多方法的参数都是<code>T37</code>类型的，
不要误传入<code>T34</code>。
</aside>

## 复制构造

`local <hand2> = Hand.new(<hand>)`

参数 | 类型 | 描述
---- | ---- | ----
`<hand>` | userdata `Hand` | 被复制的手牌

返回值 | 类型 | 描述
------ | ---- | ----
`<hand2>`  | userdata `Hand` | 复制出的`Hand`对象


## 获取纯手牌

`local <closed> = <hand>:closed()`

参数 | 类型 | 描述
---- | ---- | ----
`<hand>` | userdata `Hand` | `Hand`对象

返回值 | 类型 | 描述
------ | ---- | ----
`<closed>` | userdata `Tilecount` | 纯手牌

获取手牌中除自摸牌、副露、暗杠以处的纯手牌部分。

## 获取 barks

`local <barks> = <hand>:barks()`

参数 | 类型 | 描述
---- | ---- | ----
`<hand>` | userdata `Hand` | `Hand`对象

返回值 | 类型 | 描述
------ | ---- | ----
`<barks>` | table （`M37`数组） | barks 序列

获取表示副露与暗杠的 barks 部分。

## 计数

`local <n> = <hand>:ct(<t>)`

参数 | 类型 | 描述
---- | ---- | ----
`<hand>` | userdata `Hand` | `Hand`对象
`<t>` | userdata `T34` | 要计数的牌

返回值 | 类型 | 描述
------ | ---- | ----
`<n>` | number | 手牌中`<t>`的总数量

## 赤 5 计数

`local <n> = <hand>:ctaka5()`

参数 | 类型 | 描述
---- | ---- | ----
`<hand>` | userdata `Hand` | `Hand`对象

返回值 | 类型 | 描述
------ | ---- | ----
`<n>` | number | 手牌中赤 5 总数量

## 听牌判断

`local <ok> = <hand>:ready()`

参数 | 类型 | 描述
---- | ---- | ----
`<hand>` | userdata `Hand` | `Hand`对象

返回值 | 类型 | 描述
------ | ---- | ----
`<ok>` | number | 是否已形式听牌（不含纯空听）

## 计算向听数

`local <step> = <hand>:step()`

参数 | 类型 | 描述
---- | ---- | ----
`<hand>` | userdata `Hand` | `Hand`对象

返回值 | 类型 | 描述
------ | ---- | ----
`<step>` | number | 向听数

## 计算四面子向听数

`local <step> = <hand>:step4()`

参数 | 类型 | 描述
---- | ---- | ----
`<hand>` | userdata `Hand` | `Hand`对象

返回值 | 类型 | 描述
------ | ---- | ----
`<step>` | number | 四面子向听数

## 计算七对子向听数

`local <step> = <hand>:step7()`

参数 | 类型 | 描述
---- | ---- | ----
`<hand>` | userdata `Hand` | `Hand`对象

返回值 | 类型 | 描述
------ | ---- | ----
`<step>` | number | （日麻）七对子向听数

## 计算国标七对子向听数

`local <step> = <hand>:step7gb()`

参数 | 类型 | 描述
---- | ---- | ----
`<hand>` | userdata `Hand` | `Hand`对象

返回值 | 类型 | 描述
------ | ---- | ----
`<step>` | number | 国标七对子向听数

## 计算国士向听数

`local <step> = <hand>:step13()`

参数 | 类型 | 描述
---- | ---- | ----
`<hand>` | userdata `Hand` | `Hand`对象

返回值 | 类型 | 描述
------ | ---- | ----
`<step>` | number | 国士向听数

## 计算一类有效牌

`local <effas> = <hand>:effa()`

参数 | 类型 | 描述
---- | ---- | ----
`<hand>` | userdata `Hand` | `Hand`对象

返回值 | 类型 | 描述
------ | ---- | ----
`<effas>` | table（`T34`数组） | 一类有效牌序列

## 计算四面子一类有效牌

`local <effa4s> = <hand>:effa4()`

参数 | 类型 | 描述
---- | ---- | ----
`<hand>` | userdata `Hand` | `Hand`对象

返回值 | 类型 | 描述
------ | ---- | ----
`<effa4s>` | table（`T34`数组） | 四面子一类有效牌序列

## 门前清判断

`local <ok> = <hand>:ismenzen()`

参数 | 类型 | 描述
---- | ---- | ----
`<hand>` | userdata `Hand` | `Hand`对象

返回值 | 类型 | 描述
------ | ---- | ----
`<ok>` | boolean | 手牌是否为门前清

## 宝牌计数

`local <n> = <indicator> % <hand>`

参数 | 类型 | 描述
---- | ---- | ----
`<indicator>` | userdata `T34`，`T37` 或其数组  | 宝牌指示牌
`<hand>` | userdata `Hand` | `Hand`对象

返回值 | 类型 | 描述
------ | ---- | ----
`<n>` | number | 被指示出的宝牌数

## 摸牌

`<hand>:draw(<t37>)`

参数 | 类型 | 描述
---- | ---- | ----
`<hand>` | userdata `Hand`，非只读 | 摸牌之前的手牌
`<t37>` | userdata `T37` | 要摸入的牌

为`<hand>`以摸牌的方式添加一张牌。
若`<hand>`并非处于可摸牌的状态（比如已经摸牌，或正在鸣牌），则报错并中止程序。

## 手切

`<hand>:swapout(<t37>)`

参数 | 类型 | 描述
---- | ---- | ----
`<hand>` | userdata `Hand`，非只读  | 切牌之前的手牌
`<t37>` | userdata `T37` | 要手切的牌

从`<hand>`中，以手切的方式去掉一张牌。

若`<hand>`并非处于可手切的状态，则报错并中止程序。
若`<hand>`的纯手牌部分不含`<t37>`，则报错并中止程序。

<aside class="warning">
松饼严格区分手切、自摸切、鸣切。
</aside>

## 自摸切

`<hand>:spinout()`

参数 | 类型 | 描述
---- | ---- | ----
`<hand>` | userdata `Hand`，非只读  | 切牌之前的手牌

从`<hand>`中，以自摸切的方式去掉一张牌。

若`<hand>`并非处于可自摸切的状态，则报错并中止程序。

<aside class="warning">
松饼严格区分手切、自摸切、鸣切。
</aside>
