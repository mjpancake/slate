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

