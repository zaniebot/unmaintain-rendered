---
number: 11980
title: "`!uv add` not work"
type: issue
state: closed
author: millievn
labels:
  - question
assignees: []
created_at: 2025-03-05T15:17:44Z
updated_at: 2025-03-08T13:53:32Z
url: https://github.com/astral-sh/uv/issues/11980
synced_at: 2026-01-10T01:25:13Z
---

# `!uv add` not work

---

_Issue opened by @millievn on 2025-03-05 15:17_

`!uv add` not work as expected on mac.  
the terminal always return previous command

```shell
➜  jupyter git:(main) ✗ which !
!: shell reserved word
➜  jupyter git:(main) ✗ 
```




---

_Comment by @hoechenberger on 2025-03-05 15:38_

You should run the command without the exclamation mark.

---

_Label `question` added by @charliermarsh on 2025-03-05 16:42_

---

_Comment by @millievn on 2025-03-08 13:07_

> You should run the command without the exclamation mark.


sorry but is something wrong with the docs? [using-jupyter-from-vs-code](https://docs.astral.sh/uv/guides/integration/jupyter/#using-jupyter-from-vs-code)

i saw the exclamation mark shown in the docs


---

_Comment by @hoechenberger on 2025-03-08 13:51_

You only need to preface the commands with an exclamation mark if you type them into Jupyter. If you're working on a regular terminal, you must not add the exclamation mark.

---

_Closed by @millievn on 2025-03-08 13:53_

---
