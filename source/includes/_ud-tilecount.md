# 麻将牌多集 `Tilecount`

## 计数

`local <n> = <tc>:ct(<t>)`

参数 | 类型 | 描述
---- | ---- | ----
`<tc>` | userdata `Tilecount` | `Tilecount`对象
`<t>` | userdata `T34` 或 `T37` | 要计数的牌

返回值 | 类型 | 描述
------ | ---- | ----
`<n>` | number | `<t>`的数量

对于数牌 5，参数`<t>`为`T37`类型时区分赤宝牌，`T34`类型时返回包含赤、黑的总数量。

## 花色计数

`local <n> = <tc>:ct(<suit>)`

参数 | 类型 | 描述
---- | ---- | ----
`<tc>` | userdata `Tilecount` | `Tilecount`对象
`<suit>` | string | 要计数的花色

返回值 | 类型 | 描述
------ | ---- | ----
`<n>` | number | `<t>`的数量

参数`<suit>`须为`"m"`, `"p"`, `"s"`, `"f"`, `"y"`之一。

