---
title: 松饼人物编辑器 API 文档

language_tabs: # must be one of https://git.io/vQNgJ
  - lua

toc_footers:
  - <a href='https://mjpancake.github.io/'>松饼社区主站</a>
  - 本文档由 <a href='https://github.com/lord/slate'>Slate</a> 烹制而成

includes:
  - callbacks
  - stdlib
  - ud-t34
  - ud-t37
  - ud-who
  - ud-m37
  - ud-mount
  - ud-tilecount
  - ud-hand
  - ud-game
  - events

search: true
---

# 概览

欢迎来到松饼人物编辑器 API 文档！
通过松饼人物编辑器，你可以制作原创角色，发明原创技能，并分享到社区。

人物的技能由单个 Lua 脚本文件进行定义。
如果没有接触过 Lua，可食用
[主站](https://mjpancake.github.io/)
上的的零基础教程。

本文档目前比较简略，我们还在持续添加更多的细节说明与代码样例。
遇到任何问题，可在企鹅群提问。

下文会用到一些松饼麻雀内部特有的概念和术语，
例如「牌山 A 区」、「猴子阶段」、「毫兔值」等等。
将来本文档会对这些概念做一些详细的解释，
而目前这些概念的具体定义暂时可以在
[Libsaki 代码导读](https://mjpancake.github.io/docs/libsaki/)
中查到。那个代码导读是针对 C++ 的，但基本的设计与 Lua 相通。
大多数术语都定义在其中的「牌山系统」和「发牌姬」两个章节。

