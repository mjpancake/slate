# 手役 `Form`

`Form`用于计算日麻和了形的手役。
一个`Form`对象代表一副和牌在当前规则下的的最终解释方式。

对于多解的手牌，松饼优先采用「高点法」。
点数相同时，采用「高翻法」。翻数相同时，无作为地选取任意一种解法。

役种的计算会在构建`Form`对象时完成，
调用`fu`，`han`等方法只是查询已算好的结果，不会消耗大量运算时间。
相对地，构建`Form`会消耗大量的运算时间（毫秒级），
因此构建`Form`的次数则应该尽可能减少。
构建一两次`Form`，所消耗的时间只有几毫秒，界面不会出现卡顿；
但在短时间内几十次、上百次地构建`Form`，所消耗的时间就是零点几秒，
会出现明显的卡顿。
因此，能不用`Form`，就尽量别用`Form`
—— 尤其是不要在大量循环的计算中使用`Form`。

<aside class="warning">
不要在<code>checkinit</code>等要求快速计算的函数中使用<code>Form</code>。
</aside>

<aside class="warning">
不要用「暴力枚举所有和了形，
再求<code>Form</code>平均值」的方法计算一向听或以下的手牌的得点。
</aside>

`Form`是用来「绝对精确」地计算得点的。
然而在技能的实现中，我们往往并不需要绝对精确，
或者因条件不足无法做到绝对精确。
这时如果使用`Form`就是在浪费计算性能了。
如果只是需要粗略地估算得点，建议自已另行设计一套算法。
反正可以通过`Hand`读取手牌的完整信息，
理论上任何基于牌形的得点预估算法都是能自己做出来的。

<aside class="notice">
一个典型的估点算法就是「数宝牌」。
当然这有些太粗略了。
可以在此基础之上，再加一些断么、役牌等常见役种的判断 ——
这些都不难实现。
</aside>

## 从自摸构建

`local <form> = Form.new(<hand>, <formctx>, <rule>, <drids>, <urids>)`

参数 | 类型 | 描述
---- | ---- | ----
`<hand>` | userdata `Hand` | 自摸和了形
`<formctx>` | userdata `Formctx` | 役种上下文
`<rule>` | userdata `Rule` | 麻将规则配置
`<drids>` | table (`T37`数组) | 表宝牌指示牌序列，可省略
`<urids>` | table (`T37`数组) | 里宝牌指示牌序列，可省略

返回值 | 类型 | 描述
------ | ---- | ----
`<form>`  | userdata `Form` | 创建出的`Form`对象

通过一组已通过自摸达到「形式和了」的手牌构建`Form`。
若`<hand>`并未达到形式和了，则报错并中止运行。

## 从食和构建

`local <form> = Form.new(<hand>, <final>, <formctx>, <rule>, <drids>, <urids>)`

参数 | 类型 | 描述
---- | ---- | ----
`<hand>` | userdata `Hand` | 食和听牌形
`<final>` | userdata `T37` | 铳牌
`<formctx>` | userdata `Formctx` | 役种上下文
`<rule>` | userdata `Rule` | 麻将规则配置
`<drids>` | table (`T37`数组) | 表宝牌指示牌序列，可省略
`<urids>` | table (`T37`数组) | 里宝牌指示牌序列，可省略

返回值 | 类型 | 描述
------ | ---- | ----
`<form>` | userdata `Form` | 创建出的`Form`对象

通过一组已通过食和达到「形式和了」的手牌构建`Form`。
若`<hand>`与`<final>`并未构成形式和了，则报错并中止运行。


## 关于`Formctx`和`Rule`

> 例：计算当前听牌在两立直情况下的食和得点

```lua
function ondraw()
  if who ~= self then
    return
  end

  local hand = game:gethand(self)
  if not hand:ready() then
    return
  end

  local formctx = game:getformctx(self)
  formctx.riichi = 2
  for _, t in ipairs(hand:effa()) do
    local form = Form.new(hand, T37.new(t:id34()), formctx, game:getrule())
    print(form:gain())
  end
end
```

`Formctx`和`Rule`都是 userdata，但用法类似于 table，
可以读写里面的各种属性。

`Formctx`用于描述一副手牌和牌时的上下文。
在日麻（及其它多数麻将规则）中，一副和了形的得点不仅仅是由牌形决定的，
还和和牌发生时的场况，和之前的一系列黑历史有关系。
`Formctx`就是用来记录这些信息的。
具体属性见下表。

属性 | 类型 | 描述
---- | ---- | ----
`ippatsu`    | boolean | 一发
`bless`      | boolean | 天和或地和
`duringkan`  | boolean | 岭上或枪杠
`emptymount` | boolean | 海底或河底
`riichi`     | number | 0=未立直，1=立直，2=两立直
`roundwind`  | number | 场风数值
`selfwind`   | number | 自风数值
`extraround` | number | 本场数

`Rule`用于表示用户配置的规则细项，具体属性见下表。

属性 | 类型 | 描述
---- | ---- | ----
`fly`           | boolean | 击飞
`headjump`      | boolean | 头跳
`nagashimangan` | boolean | 流满
`ippatsu`       | boolean | 一发
`uradora`       | boolean | 里宝
`kandora`       | boolean | 杠宝
`daiminkanpao`  | boolean | 大明杠包牌
`akadora`       | number | 赤牌数，只能是 0, 3, 4
`hill`          | number | 丘（头名赏）
`returnlevel`   | number | 返点
`roundlimit`    | number | 局数（4=东风，8=半庄）

`game:getformctx`与`game:getrule`返回的对象都是副本。
修改其返回傎并不会影响到当前牌桌上的上下文和规则。

## 例牌役满判断

`local <ok> = <form>:isprototypalyakuman()`

参数 | 类型 | 描述
---- | ---- | ----
`<form>` | userdata `Form` | 手役

返回值 | 类型 | 描述
------ | ---- | ----
`<ok>` | boolean | 是否为例牌役满

判断手牌是否为大三元、四暗刻等例牌役满。
是则返回`true`；返回`false`则代表是个累计役满，或者根本不是役满。

## 符数

`local <fu> = <form>:fu()`

参数 | 类型 | 描述
---- | ---- | ----
`<form>` | userdata `Form` | 手役

返回值 | 类型 | 描述
------ | ---- | ----
`<fu>` | number | 符数

## 翻数

`local <han> = <form>:han()`

参数 | 类型 | 描述
---- | ---- | ----
`<form>` | userdata `Form` | 手役

返回值 | 类型 | 描述
------ | ---- | ----
`<han>` | number | 翻数

## 基本点

`local <base> = <form>:base()`

参数 | 类型 | 描述
---- | ---- | ----
`<form>` | userdata `Form` | 手役

返回值 | 类型 | 描述
------ | ---- | ----
`<base>` | number | 翻数

基本点就是「符 ^ (翻 + 2)」那个玩意，
满贯 2000，跳满 3000 什么的。

## 表宝牌数

`local <dora> = <form>:dora()`

参数 | 类型 | 描述
---- | ---- | ----
`<form>` | userdata `Form` | 手役

返回值 | 类型 | 描述
------ | ---- | ----
`<dora>` | number | （杠）表宝牌数

当存在多枚相同的指示牌时，同一张宝牌会被计算对应次。

## 里宝牌数

`local <uradora> = <form>:uradora()`

参数 | 类型 | 描述
---- | ---- | ----
`<form>` | userdata `Form` | 手役

返回值 | 类型 | 描述
------ | ---- | ----
`<uradora>` | number | （杠）里宝牌数

当存在多枚相同的指示牌时，同一张里宝牌会被计算对应次。

## 赤宝牌数

`local <akadora> = <form>:akadora()`

参数 | 类型 | 描述
---- | ---- | ----
`<form>` | userdata `Form` | 手役

返回值 | 类型 | 描述
------ | ---- | ----
`<akadora>` | number | 赤宝牌数

## 役种集合

`local <yakus> = <form>:yakus()`

参数 | 类型 | 描述
---- | ---- | ----
`<form>` | userdata `Form` | 手役

返回值 | 类型 | 描述
------ | ---- | ----
`<yakus>` | table（string 集合）| 役种集合

此方法用于查看该和了形中成立的具体役种。

`<yakus>`是一个 Lua 表，键类型为 string，代表役种；键若存在，则对应的值为`true`。
各种 string 代表的役种见下表。

字符串  | 代表役种
------- | ----
`"rci"` | 立直
`"ipt"` | 一发
`"tmo"` | 门前清自摸和
`"tny"` | 断么九
`"pnf"` | 平和
`"y1y"`, `"y2y"`, `"y3y"` | 役牌白，役牌发，役牌中
`"jk1"`, `"jk2"`, `"jk3"`, `"jk4"` | 自风东，南，西，北
`"bk1"`, `"bk2"`, `"bk3"`, `"bk4"` | 场风东，南，西，北
`"ipk"` | 一杯口
`"rns"`, `"hai"`, `"hou"`, `"ckn"` | 岭上开花，海底捞月，河底摸鱼，枪杠
`"ss1"`, `"it1"`, `"ct1"` | 食下三色同顺，一气通贯，混全带么九
`"wri"` | 两立直
`"ss2"`, `"it2"`, `"ct2"` | 门前清三色同顺，一气通贯，混全带么九
`"toi"`, `"ctt"` | 对对和，七对子
`"sak"`, `"skt"` | 三暗刻，三杠子
`"stk"`, `"hrt"`, `"s3g"` | 三色同刻，混老头，小三元
`"h1t"`, `"jc2"` | 食下混一色，纯全带么九
`"mnh"`, `"jc3"` | 门前清混一色，纯全带么九
`"rpk"` | 两杯口
`"c1t"` | 食下清一色
`"mnc"` | 门前清清一色
`"x13"`, `"xd3"`, `"x4a"`, `"xt1"` | 国、元、暗、字
`"xs4"`, `"xd4"`, `"xcr"`, `"xr1"` | 小、大、清、绿
`"xth"`, `"xch"`, `"x4k"`, `"x9r"` | 天、地、杠、九
`"w13"`, `"w4a"`, `"w9r"` | 面、单、纯

## 有役判断

`local <ok> = <form>:hasyaku()`

参数 | 类型 | 描述
---- | ---- | ----
`<form>` | userdata `Form` | 手役

返回值 | 类型 | 描述
------ | ---- | ----
`<ok>` | boolean | 是否有役

## 纯手牌失点

`local <loss> = <form>:netloss(<explode>)`

参数 | 类型 | 描述
---- | ---- | ----
`<form>` | userdata `Form` | 手役
`<explode>` | boolean | 是否为自摸炸庄

返回值 | 类型 | 描述
------ | ---- | ----
`<loss>` | number | 一家失点，不计供托场棒

如果`<form>`为自摸，
`<explode>`传`false`则计算闲家失点，传`true`则计算庄家失点。
`<form>`为食和时，`<explode>`传`true`或`false`均可，不影响结果。

## 纯手牌得点

`local <gain> = <form>:netgain()`

参数 | 类型 | 描述
---- | ---- | ----
`<form>` | userdata `Form` | 手役

返回值 | 类型 | 描述
------ | ---- | ----
`<gain>` | number | 和牌者得点，不计供托场棒

## 失点

`local <loss> = <form>:loss(<explode>)`

参数 | 类型 | 描述
---- | ---- | ----
`<form>` | userdata `Form` | 手役
`<explode>` | boolean | 是否为自摸炸庄

返回值 | 类型 | 描述
------ | ---- | ----
`<loss>` | number | 一家失点，计场棒，不计供托

如果`<form>`为自摸，
`<explode>`传`false`则计算闲家失点，传`true`则计算庄家失点。
`<form>`为食和时，`<explode>`传`true`或`false`均可，不影响结果。

供托需根据座位（多家和的情况）另行计算。

## 得点

`local <gain> = <form>:gain()`

参数 | 类型 | 描述
---- | ---- | ----
`<form>` | userdata `Form` | 手役

返回值 | 类型 | 描述
------ | ---- | ----
`<gain>` | number | 和牌者得点，计场棒，不计供托

供托需根据座位（多家和的情况）另行计算。

## 迷之咒语

`local <spell> = <form>:spell()`

参数 | 类型 | 描述
---- | ---- | ----
`<form>` | userdata `Form` | 手役

返回值 | 类型 | 描述
------ | ---- | ----
`<spell>` | string | 迷之咒语

以鸟语字符串表示所有役种，主要用于调试。

## 迷之报点

`local <charge> = <form>:charge()`

参数 | 类型 | 描述
---- | ---- | ----
`<form>` | userdata `Form` | 手役

返回值 | 类型 | 描述
------ | ---- | ----
`<charge>` | string | 迷之报点

以鸟语字符串表示报点文字，主要用于调试。

