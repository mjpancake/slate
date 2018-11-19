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

