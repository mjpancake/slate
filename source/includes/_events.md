# Game Event 表结构

`ongameevent`中使用的`event`变量类型为 table，
包含`type`和`args`两个字段。
`type`字段为 string 类型，表示事件的类别；
`args`字段为 table 类型，根据`type`的不同含有不同的字段。

(文档准备中。目前可通过自行`pairs`遍历查看`args`内容。)

