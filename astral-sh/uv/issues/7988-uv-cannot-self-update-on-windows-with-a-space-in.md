---
number: 7988
title: uv cannot self update on windows with a space in the user name
type: issue
state: open
author: dancergraham
labels:
  - external
  - releases
assignees: []
created_at: 2024-10-07T19:21:13Z
updated_at: 2024-10-07T19:23:36Z
url: https://github.com/astral-sh/uv/issues/7988
synced_at: 2026-01-10T01:24:21Z
---

# uv cannot self update on windows with a space in the user name

---

_Issue opened by @dancergraham on 2024-10-07 19:21_

trying `uv self update`
Using uv-self 0.4.7 (a178051e8 2024-09-07) 
on Windows 11
With a space in my user name - home directory `C:\Users\Graham Knapp`
Error:

```
C:\Users\Graham : The term 'C:\Users\Graham' is not recognized as the name of a cmdlet, function, script file, or
operable program. Check the spelling of the name, or if a path was included, verify that the path is correct and try
again.
At line:1 char:1
+ C:\Users\Graham Knapp\AppData\Local\Temp\.tmp3XZhI9\installer.ps1
+ ~~~~~~~~~~~~~~~
    + CategoryInfo          : ObjectNotFound: (C:\Users\Graham:String) [], CommandNotFoundException
    + FullyQualifiedErrorId : CommandNotFoundException

---

_Label `releases` added by @zanieb on 2024-10-07 19:23_

---

_Comment by @zanieb on 2024-10-07 19:23_

Thanks!

---

_Label `upstream` added by @zanieb on 2024-10-07 19:23_

---
