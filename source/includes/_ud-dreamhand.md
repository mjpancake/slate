# 梦中手牌 `Dreamhand`

`Dreamhand`是`Hand`的扩展牌本，是可以修改的手牌。

`Hand`代表牌桌上的真实手牌，只能由正常的打牌操作修改，外挂技能无法干涉
（否则就是作弊了）。
`Dreamhand`与`Hand`不同，是一个脑中假想的手牌，因此可以随意修改。

但修改不是乱改，否则是要谢罪的。`Dreamhand`与`Hand`相同，
应始终处于「14 - 3k 张」或「13 - 3k 张」的状态（k 代表 bark 数，范围是 0 ~ 4）。
如果你尝试让`Dreamhand`脱离这两种状态，代码会报错并中止运行。

`Dreamhand`是一个`Hand`。
所有适用于`Hand`的方法（如`step`，`effa`等）也适用于`Dreamhand`。
所有能接受`Hand`的地方也都能接受`Dreamhand`。

<aside class="warning">
<code>Dreamhand</code>的方法的参数多为<code>T37</code>类型，
不要误传入<code>T34</code>。
</aside>

## 从`Hand`复制

`local <dream> = Dreamhand.new(<hand>)`

参数 | 类型 | 描述
---- | ---- | ----
`<hand>` | userdata `Hand` | 被复制的手牌

返回值 | 类型 | 描述
------ | ---- | ----
`<dream>`  | userdata `Dreamhand` | 创建出的`Dreamhand`对象

<aside class="notice">
别忘了<code>Dreamhand</code>是一个<code>Hand</code>，
能够从<code>Hand</code>复制，
就意味着也能从<code>Dreamhand</code>复制。
</aside>

## 摸牌

`<dreamhand>:draw(<t37>)`

参数 | 类型 | 描述
---- | ---- | ----
`<dreamhand>` | userdata `Dreamhand`  | 摸牌之前的手牌
`<t37>` | userdata `T37` | 要摸入的牌

为`<dreamhand>`以摸牌的方式添加一张牌。
若`<dreamhand>`并非处于可摸牌的状态（比如已经摸牌，或正在鸣牌），则报错并中止程序。

## 手切

`<dreamhand>:swapout(<t37>)`

参数 | 类型 | 描述
---- | ---- | ----
`<dreamhand>` | userdata `Dreamhand`  | 切牌之前的手牌
`<t37>` | userdata `T37` | 要手切的牌

从`<dreamhand>`中，以手切的方式去掉一张牌。

若`<dreamhand>`并非处于可手切的状态，则报错并中止程序。
若`<dreamhand>`的纯手牌部分不含`<t37>`，则报错并中止程序。

<aside class="warning">
松饼严格区分手切、自摸切、鸣切。
</aside>

## 自摸切

`<dreamhand>:spinout()`

参数 | 类型 | 描述
---- | ---- | ----
`<dreamhand>` | userdata `Dreamhand`  | 切牌之前的手牌

从`<dreamhand>`中，以自摸切的方式去掉一张牌。

若`<dreamhand>`并非处于可自摸切的状态，则报错并中止程序。

<aside class="warning">
松饼严格区分手切、自摸切、鸣切。
</aside>
