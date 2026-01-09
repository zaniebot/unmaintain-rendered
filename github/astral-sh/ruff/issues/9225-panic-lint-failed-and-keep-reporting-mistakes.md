---
number: 9225
title: "[Panic]Lint failed and keep reporting mistakes"
type: issue
state: closed
author: majiayu000
labels: []
assignees: []
created_at: 2023-12-21T09:34:24Z
updated_at: 2023-12-21T10:40:36Z
url: https://github.com/astral-sh/ruff/issues/9225
synced_at: 2026-01-07T13:12:15-06:00
---

# [Panic]Lint failed and keep reporting mistakes

---

_Issue opened by @majiayu000 on 2023-12-21 09:34_

When using jupyter notebook , I have a string just like
```
second_question = """问题:少年活动中心的某个绘画室中有3腿的凳子和4腿的椅子共40张，房间里恰好有40位小朋友坐在这40张凳子和椅子上。昊昊数了一下，凳子的腿、椅子的腿和小朋友的腿数，总数是225。那么绘画室中，凳子有​15​​张。
解答
​方法一：注意小明友总共有80条腿：假设40张全是椅子，那么应共有(4+2)×40=240条腿。比实际多240-225=15条腿，这说明有15÷(4-3)=15张凳子。
方法二：设有凳子x张，椅子(40-x)张，
则3x+(40-x)×4+80=225，
解得：x=15，
答：绘画室中共有15张凳子。"""
```

This will lead a lint failed and if dont fix the string, it will continous report.....

In my opinion, this string is right and I need to use it. But with this bug, when I type anyting,  it will pop-up waring....

<img width="485" alt="image" src="https://github.com/astral-sh/ruff/assets/19658300/c70748d1-fc5d-4467-81eb-4f13d79d1918">


---

_Comment by @MichaReiser on 2023-12-21 09:55_

Hy @majiayu000 

Thanks for the report. This seems to be the same issue as https://github.com/astral-sh/ruff/issues/9145 We plan to make a release today (next 24h) to get the fix out. 

---

_Comment by @majiayu000 on 2023-12-21 10:40_

Thanks for the reply, I'm looking forward to the new changes because I want to understand how it works！

---

_Closed by @majiayu000 on 2023-12-21 10:40_

---
