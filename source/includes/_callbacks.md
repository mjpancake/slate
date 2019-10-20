# 关于「只读」属性

松饼为 Lua 的 userdata 添加了「只读」的概念。
以下文档中，如果一个 userdata 变量注有「只读」字样，
那么就无法通过该变量调用标有「非只读」的方法。

例如`game:getmount():lighta(T34.new("1m"), 100)`就无法执行，
因为`game:getmount()`的返回值是只读的，
无法调用其非只读方法`lighta`。

# 回调函数

在 Lua 脚本文件中，以特定的名称定义函数，即可在特定的时机插入自己的逻辑。
这些函数都不需要参数，
必要的数据可从特定的全局变量中获取。

有一个特殊的全局变量`self`，在 Lua 文件的任何位置都可用。
该变量为 userdata `Who` 类型，代表当前 Lua 文件对应的自身角色。
其它的全局变量（例如`game`, `who`, `mount`等）只在特定的回调函数中可用，
下面的文档中有详细说明。

> 请勿通过任何绕弯的手段调用不属于当前回调函数的全局变量。
例如：

```lua
-- 错误示范

function ondice()
  rand_copy = rand
end

function ondraw()
  local num = rand_copy:gen(10)
end
```

> `ondraw`的可用全局变量里没有`rand`，
但以上代码通过复制引用，间接地在`ondraw`中使用了`rand`。
该代码无法正常执行，会在运行时报错。

<aside class="warning">
请勿通过任何手段调用不属于当前回调函数的全局变量。（见代码样例）
</aside>

## 转骰子前搞事情

> 例：生成一个 0 ~ 9 的随机数，保存到全局变量`doge`

```lua
doge = 0

function ondice()
  doge = rand:gen(10)
end
```

### 函数名

`ondice`

### 可用全局变量

变量名 | 类型 | 描述
------ | ---- | ----
`game` | userdata `Game` | 当前牌桌数据
`rand` | userdata `Rand` | 随机数生成器

### 具体用法

定义`ondice`函数，即可在每局开始时转骰子之前搞事情。
通常我们会在这个阶段初始化一些状态，或生成随机数。

## 猴子阶段前搞事情

> 例：增加自己的配牌中摸到 1m 的概率：

```lua
function onmonkey()
  local exist = exists[self:index()]
  exist:incmk(T34.new("1m"), 400)
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

定义`onmonkey`函数，即可在猴子配牌阶段搞事情。
通常我们会在这个阶段通过操作`exists`来影响配牌中各种牌出现的概率。

`exists`是一个数组，里面有四个`Exist`类型的 userdata 对象，
分别控制四个人的配牌。该数组需与`Who`类型的`index`函数配合使用 ——
例如，`exists[self:index()]`即为自己的`Exist`，
`exists[self:right():index()]`即为下家的`Exist`。

<aside class="warning">
请勿直接使用 number 字面值索引<code>exists</code>，例如<code>exists[1]</code>。
当前角色的索引不一定是某个特定数字。
</aside>

`Exist`类型只有一个`incmk`方法，用于改变某种牌的存在感。
其基本用法为：

`<exist>:incmk(<t>, <mk>)`

其中`<t>`为`T34`或`T37`类型的麻将牌，`<mk>`为毫兔单位的存在感变化量。
当`<t>`为`T37`类型时，该变化严格区分赤宝牌。

## 猴子阶段配牌检查

> 例：要求自己的配牌向听数小于等于 1：

```lua
function checkinit()
  -- 他家配牌，无条件满意
  -- 最多坚持 100 次生成
  if who ~= self or iter > 100 then
    return true
  end

  -- 自家配牌，向听数小于 1
  return init:step() <= 1
end
```

> 例：要求他家配牌不含西风：

```lua
function checkinit()
  -- 自家配牌，无条件满意
  -- 最多坚持 100 次生成
  if who == self or iter > 100 then
    return true
  end

  -- 自家配牌，西风数为 0
  return init:ct(T34.new("3f")) == 0
end
```

### 函数名

`checkinit`

### 可用全局变量

变量名 | 类型 | 描述
------ | ---- | ----
`game`| userdata `Game` | 当前牌桌数据
`init`| userdata `Hand`，只读 | 待测配牌，无论亲子都是 13 张
`who` | userdata `Who`  | 待测配牌所有者
`iter`| number | 检测轮数，从0 开始递增，最大 999

### 具体用法

猴子阶段中，发牌姬会不断地为一家生成配牌，
直到四家都对生成的配牌表示满意。
这个用来表示是否满意的地方，就是`checkinit`的返回值。
若对配牌满意，`checkinit`则应当返回`true`；不满意则应当返回`false`。

<aside class="notice">
<code>checkinit</code>必须返回一个 boolean 类型的值。
</aside>

`init`为发牌姬在第`iter`轮猴子配牌中为`who`生成的配牌草案。
我们可以对`init`做各种分析，看看它是否满足我们想要的条件。

`checkinit`中的逻辑应该是小于 1 毫秒的轻量级的计算。
计算向听数、有效牌、或查看手牌组成都是没问题的。
比较容易出问题的是相对精确的和了率分析与打点期望预估。

<aside class="warning">
不要在<code>checkinit</code>中做重型计算。
如须更复杂的配牌条件，考虑使用非猴子阶段的发牌姬接口。
</aside>

> 例：通过`onmonkey`和`checkinit`的配合，实现万子清一色配牌：

```lua
function onmonkey()
  for i = 1, 9 do
    exists[self:index()]:incmk(T34.new(i .. "m"), 200)
  end
end

function checkinit()
  if who ~= self or iter > 100 then
    return true
  end
  
  return init:closed():ct("m") == 13
end
```

此外，`checkinit`只适合筛选高概率出现的配牌 ——
例如一向听、五向听、无字等等。
如果需要筛选出现概率本不高的牌（例如清一色），
则需要配合`onmonkey`或非猴子阶段的处理，先使得这种配牌出现的概率变高，
然后再通过`checkinit`进行筛选。

<aside class="success">
<code>checkinit</code>只是一个最后把关的质检员，
而复杂配牌的创造还需靠<code>onmonkey</code>和非猴子阶段 ——
除非你想要的配牌本身就很常见。
</aside>

## 摸牌前搞事情

> 例：增加自己摸到白板的概率：

```lua
function ondraw()
  if who ~= self or rinshan then
    return
  end

  mount:lighta(T34.new("1y"), 200)
end
```

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

摸牌前被调用，可干涉牌山。

## 发生事件后做处理

### 函数名

`ongameevent`

### 可用全局变量

变量名 | 类型 | 描述
------ | ---- | ----
`game` | userdata `Game` | 当前牌桌数据
`event`| table | 发生的事件

### 具体用法

`ongameevent`会在发生某些事件前调用。

`ongameevent`内不可直接干涉牌山，
一舰用于更新一些当前角色的内部状态，或记录一些信息。

`event`参数是一个 table，里面记录了与本次事件相关的各种信息。
[Game Event 表结构](#game-event) 一章中有对该 table 的详细说明。

