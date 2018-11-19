# 回调函数

在 Lua 脚本文件中，以特定的名称定义函数，即可在特定的时机插入自己的逻辑。
这些函数都不需要参数，
必要的数据可从特定的全局变量中获取。

(TODO global var 'self')

## 猴子阶段前搞事情

> 例：增加自己的配牌中摸到 1m 的概率：

```lua
function onmonkey()
  local exist = exists[self:index()]
  exist.inkmk(T34.new("1m"), 400)
end
```

### 函数名

`onmonkey`

### 可用全局变量

变量名 | 类型 | 描述
------ | ---- | ----
`game` | userdata `Game` | 当前牌桌数据
`exists`| table（`Exist`数组） | 4 个人的存在感分布

### 具体用法

(TODO)

## 检查配牌

> 例：要求配牌向听数小于等于 1：

```lua
function checkinit()
  if who ~= self then
    return true
  end

  return init:step() <= 1
end
```

### 函数名

`checkinit`

### 可用全局变量

变量名 | 类型 | 描述
------ | ---- | ----
`game`| userdata `Game` | 当前牌桌数据
`init`| userdata `Hand` | 待测配牌，无论亲子都是 13 张
`who` | userdata `Who`  | 待测配牌所有者
`iter`| number | 检测轮数，从0 开始递增，最大 999

### 具体用法

检查配牌，当且仅当满意配牌时返回`true`
(TODO)

<aside class="notice">
<code>checkinit</code>必须返回一个 boolean 类型的值
</aside>

## 摸牌前搞事情

### 函数名

`ondraw`

### 可用全局变量

变量名 | 类型 | 描述
------ | ---- | ----
`game` | userdata `Game` | 当前牌桌数据
`mount`| userdata `Mount` | 牌山
`who`  | userdata `Who`  | 将要摸牌的人
`rinshan`| boolean | 下一张摸牌是否为岭上牌

### 具体用法

摸牌前被调用，可干涉牌山
(TODO)

## 发生事件后做处理

### 函数名

`ongameevent`

### 可用全局变量

变量名 | 类型 | 描述
------ | ---- | ----
`game` | userdata `Game` | 当前牌桌数据
`event`| table | 发生的事件

### 具体用法

发生某些事件前调用，不可直接干涉牌山
(TODO)
