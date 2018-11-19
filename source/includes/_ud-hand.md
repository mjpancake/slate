# 手牌 `Hand`

本文档中「手牌」一词指包含纯手牌、自摸牌、副露、暗杠的广义手牌。

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
`<indicator>` | userdata ~T34`，`T37` 或其数组  | 宝牌指示牌
`<hand>` | userdata `Hand` | `Hand`对象

返回值 | 类型 | 描述
------ | ---- | ----
`<n>` | number | 被指示出的宝牌数

