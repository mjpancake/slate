# 牌山 `Mount`

## 获取残枚

`local <n> = <mount>:remainpii()`

参数 | 类型 | 描述
---- | ---- | ----
`<mount>` | userdata `Mount` | `Mount`对象

返回值 | 类型 | 描述
------ | ---- | ----
`<n>` | number | 壁牌残杖

此处「壁牌」指除王牌以外的普通山牌。

## 获取岭上牌残枚

`local <n> = <mount>:remainrinshan()`

参数 | 类型 | 描述
---- | ---- | ----
`<mount>` | userdata `Mount` | `Mount`对象

返回值 | 类型 | 描述
------ | ---- | ----
`<n>` | number | 岭上牌残杖

可间接得知目前已成立的杠的个数。

## 查看 A 区中某牌个数

`local <n> = <mount>:remaina(<t>)`

参数 | 类型 | 描述
---- | ---- | ----
`<mount>` | userdata `Mount` | `Mount`对象
`<t>` | userdata `T34` 或 `T37` | 要查看个数的牌

返回值 | 类型 | 描述
------ | ---- | ----
`<n>` | number | A 区中`<t>`的个数

当`<t>`为`T34`类型的数牌 5 时，结果为包含赤牌与黑牌的总个数；
当`<t>`为`T37`类型时，严格区分赤宝牌。

<aside class="notice">
B 区中某牌的个数是不允许查看的。
</aside>

## 查看宝牌指示牌

`local <drids> = <mount>:getdrids()`

参数 | 类型 | 描述
---- | ---- | ----
`<mount>` | userdata `Mount` | `Mount`对象

返回值 | 类型 | 描述
------ | ---- | ----
`<drids>` | table (`T37`数组) | （杠）宝牌指示牌序列

## 查看里宝牌指示牌

`local <urids> = <mount>:geturids()`

参数 | 类型 | 描述
---- | ---- | ----
`<mount>` | userdata `Mount` | `Mount`对象

返回值 | 类型 | 描述
------ | ---- | ----
`<urids>` | table (`T37`数组) | （杠）里宝牌指示牌序列

读取立直和牌后已亮出的（杠）里宝牌指示牌序列。
若「立直和牌」尚未发生，则返回空表。

## 改变 A 区中某牌存在感

`<mount>:lighta(<t>, <mk>, <rinshan>)`

参数 | 类型 | 描述
---- | ---- | ----
`<mount>` | userdata `Mount`，非只读 | `Mount`对象
`<t>` | userdata `T34` 或 `T37` | 要改变存在感的牌
`<mk>` | number | 存在感变化量
`<rinshan>` | boolean 或 nil | 选择影响岭上牌还是壁牌

改变 A 区壁牌或岭上牌出口处的下一张量子牌的存在感分布。

当`<t>`为`T34`类型的数牌 5 时，同时影响赤牌与黑牌；
当`<t>`为`T37`类型时，严格区分赤宝牌。

## 改变 B 区中某牌存在感

`<mount>:lightb(<t>, <mk>, <rinshan>)`

参数 | 类型 | 描述
---- | ---- | ----
`<mount>` | userdata `Mount`，非只读 | `Mount`对象
`<t>` | userdata `T34` 或 `T37` | 要改变存在感的牌
`<mk>` | number | 存在感变化量
`<rinshan>` | boolean 或 nil | 选择影响岭上牌还是壁牌

改变 B 区壁牌或岭上牌出口处的下一张量子牌的存在感分布。

当`<t>`为`T34`类型的数牌 5 时，同时影响赤牌与黑牌；
当`<t>`为`T37`类型时，严格区分赤宝牌。

## 改变某牌存在感（一般形式）

`<mount>:inkmk(<exit>, <pos>, <t>, <mk>, <bspace>)`

参数 | 类型 | 描述
---- | ---- | ----
`<mount>` | userdata `Mount`，非只读 | `Mount`对象
`<exit>` | string | 影响的出口（见下文说明）
`<t>` | userdata `T34` 或 `T37` | 要改变存在感的牌
`<mk>` | number | 存在感变化量
`<bspace>` | boolean | `true`则影响 B 区，`false`则影响 A 区

以`mk`增加出口`exit`之`pos`处`tile`的存在感。

`exit`需传入`"pii"`, `"rinshan"`, `"dorahyou"`, `"urahyou"`之一，
分别代表普通壁牌、岭上牌、未翻开的表宝牌指示牌、未挖出的里宝牌指示牌。

`pos`为 0 时代表出口处队首牌，1 时代表队首的下一张牌，以此类推。

当`<t>`为`T34`类型的数牌 5 时，同时影响赤牌与黑牌；
当`<t>`为`T37`类型时，严格区分赤宝牌。

## 放入 B 区

`<mount>:loadb(<t>, <num>)`

参数 | 类型 | 描述
---- | ---- | ----
`<mount>` | userdata `Mount`，非只读 | `Mount`对象
`<t>` | userdata `T37` | 要放入 B 区的牌
`<num>` | number | 放入数量

将指定数量的牌由 A 区放入 B 区。若 A 区储量不足，则能放多少放多少。

<aside class="warning">
参数麻将牌必须是<code>T37</code>类型的。
</aside>

