# Lua 标准库与内建函数

为了防止世界被破坏，
松饼中只能使用一部分的 Lua 标准库与内建函数。

能用的部分：

- `table`中的所有函数
- `string`中的所有函数
- `math`中除去`math.random`和`math.randomseed`以外的所有函数
- `pairs`, `ipairs`
- `print`
- `tostring`

将来可能会放宽限制，支持更多的标准库成员。

