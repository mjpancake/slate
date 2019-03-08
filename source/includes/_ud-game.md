# 场况 `Game`

## 获取某家手牌

`local <hand> = <game>:gethand(<who>)`

参数 | 类型 | 描述
---- | ---- | ----
`<game>` | userdata `Game` | `Game`对象
`<who>` | userdata `Who` | 谁的手牌

返回值 | 类型 | 描述
------ | ---- | ----
`<hand>` | userdata `Hand` | 手牌

## 获取局数

`local <round> = <game>:getround()`

参数 | 类型 | 描述
---- | ---- | ----
`<game>` | userdata `Game` | `Game`对象

返回值 | 类型 | 描述
------ | ---- | ----
`<round>` | number | 局数

局数从 0 开始。0 为东一，1 为东二，以此类推。

## 获取本场数

`local <extra> = <game>:getextraround()`

参数 | 类型 | 描述
---- | ---- | ----
`<game>` | userdata `Game` | `Game`对象

返回值 | 类型 | 描述
------ | ---- | ----
`<extra>` | number | 本场数

## 获取庄家

`local <dealer> = <game>:getdealer()`

参数 | 类型 | 描述
---- | ---- | ----
`<game>` | userdata `Game` | `Game`对象

返回值 | 类型 | 描述
------ | ---- | ----
`<dealer>` | userdata `Who` | 本局庄家

## 获取某家自风

`local <sw> = <game>:getselfwind(<who>)`

参数 | 类型 | 描述
---- | ---- | ----
`<game>` | userdata `Game` | `Game`对象
`<who>` | userdata `Who` | 谁的自风

返回值 | 类型 | 描述
------ | ---- | ----
`<sw>` | number | 自风牌的数值（1～4）

## 获取场风

`local <rw> = <game>:getroundwind()`

参数 | 类型 | 描述
---- | ---- | ----
`<game>` | userdata `Game` | `Game`对象

返回值 | 类型 | 描述
------ | ---- | ----
`<rw>` | number | 场风牌的数值（1～4）

## 获取某家牌河

`local <river> = <game>:getriver(<who>)`

参数 | 类型 | 描述
---- | ---- | ----
`<game>` | userdata `Game` | `Game`对象
`<who>` | userdata `Who` | 谁的牌河

返回值 | 类型 | 描述
------ | ---- | ----
`<river>` | table（`T37`数组） | 牌河里的牌的序列

## 获取手役上下文

`local <formctx> = <game>:getformctx(<who>)`

参数 | 类型 | 描述
---- | ---- | ----
`<game>` | userdata `Game` | `Game`对象
`<who>` | userdata `Who` | 谁的手役上下文

返回值 | 类型 | 描述
------ | ---- | ----
`<formctx>` | userdata `Formctx` | 详见`Form`一节

## 获取规则

`local <rule> = <game>:getrule()`

参数 | 类型 | 描述
---- | ---- | ----
`<game>` | userdata `Game` | `Game`对象

返回值 | 类型 | 描述
------ | ---- | ----
`<rule>` | userdata `Rule` | 详见`Form`一节

## 获取焦点牌

`local <who>, <tile>, <discard> = <game>:getfocus()`

参数 | 类型 | 描述
---- | ---- | ----
`<game>` | userdata `Game` | `Game`对象

返回值 | 类型 | 描述
------ | ---- | ----
`<who>` | userdata `Who` | 焦点牌由谁产生
`<tile>` | userdata `T37` | 焦点牌
`<discard>` | boolean | 焦点是否由切牌产生

「焦点牌」指当前牌局中可能会有人食和的牌。
多数情况下，焦点牌就是最后一张弃牌。
刚配完牌，没人打牌时，没有焦点牌。
有人加杠时，焦点牌为加杠牌。

- 焦点牌为最后一张弃牌时，`<who>`为最后切牌的人，`<discard>`为`true`。
- 不存在焦点牌时，`<who>`和`<tile>`都是`nil`，`<discard>`为`false`。
- 焦点牌为加杠牌时，`<who>`为加杠者，`<discard>`为`false`。

