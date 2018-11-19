# 身价标识 `Who`

## 相等判断

`local <ok> = <who1> == <who2>`

参数 | 类型 | 描述
---- | ---- | ----
`<who1>` | userdata `Who` | 一个`Who`对象
`<who2>` | userdata `Who` | 另一个`Who`对象

返回值 | 类型 | 描述
------ | ---- | ----
`<ok>` | boolean | `<who1>`与`<who2>`是否指代同一人

## 获取下家

`local <who2> = <who1>:right()`

参数 | 类型 | 描述
---- | ---- | ----
`<who1>` | userdata `Who` | `Who`对象

返回值 | 类型 | 描述
------ | ---- | ----
`<who2>` | userdata `Who` | `<who1>`的下家

## 获取对家

`local <who2> = <who1>:cross()`

参数 | 类型 | 描述
---- | ---- | ----
`<who1>` | userdata `Who` | `Who`对象

返回值 | 类型 | 描述
------ | ---- | ----
`<who2>` | userdata `Who` | `<who1>`的对家

## 获取上家

`local <who2> = <who1>:left()`

参数 | 类型 | 描述
---- | ---- | ----
`<who1>` | userdata `Who` | `Who`对象

返回值 | 类型 | 描述
------ | ---- | ----
`<who2>` | userdata `Who` | `<who1>`的上家

## 转骰选人

`local <who2> = <who1>:bydice(<dice>)`

参数 | 类型 | 描述
---- | ---- | ----
`<who1>` | userdata `Who` | 转骰子的人
`<dice>` | number | 骰子点数

返回值 | 类型 | 描述
------ | ---- | ----
`<who2>` | userdata `Who` | 被骰子指向的人

## 摸牌次数选人

`local <who2> = <who1>:byturn(<turn>)`

参数 | 类型 | 描述
---- | ---- | ----
`<who1>` | userdata `Who` | `Who`对象
`<turn>` | number | 骰子点数

返回值 | 类型 | 描述
------ | ---- | ----
`<who2>` | userdata `Who` | `<who1>`之后第`<turn>`个摸牌的人

## 数字索引

`local <index> = <who>:index()`

参数 | 类型 | 描述
---- | ---- | ----
`<who>` | userdata `Who` | `Who`对象

返回值 | 类型 | 描述
------ | ---- | ----
`<index>` | number | 一个数字，常用于表键

## 相对距离计算

`local <dist> = <who1>:turnfrom(<who2>)`

参数 | 类型 | 描述
---- | ---- | ----
`<who1>` | userdata `Who` | 等待方
`<who2>` | userdata `Who` | 被等待方

返回值 | 类型 | 描述
------ | ---- | ----
`<dist>` | number | 双方距离

在没有鸣牌的假设下，
计算从`<who2>`摸牌到`<who1>`摸牌之间最少需要的回合跳转数。

即：如果`<who2> == <who1>`，返回 0，
`<who2> == <who1>:left()`，返回 1，以此类推。

