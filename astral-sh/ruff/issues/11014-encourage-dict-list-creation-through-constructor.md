```yaml
number: 11014
title: Encourage dict/list creation through constructor 
type: issue
state: open
author: twoertwein
labels:
  - rule
assignees: []
created_at: 2024-04-18T18:43:53Z
updated_at: 2024-04-20T19:20:54Z
url: https://github.com/astral-sh/ruff/issues/11014
synced_at: 2026-01-10T11:09:53Z
```

# Encourage dict/list creation through constructor 

---

_Issue opened by @twoertwein on 2024-04-18 18:43_

```
a = {} # or a = dict()
a["a"] = 1
a["b"] = 2

b = []
b.append(1)
b.append(2)
```

should be 

```
a = {"a": 1, "b": 2}
b = [1, 2]
```


I thought there was already a rule in ruff for that but `ruff check --select ALL` didn't point them out

---

_Label `rule` added by @MichaReiser on 2024-04-19 05:57_

---

_Comment by @Skylion007 on 2024-04-20 19:20_

Also should cover `.extend` calls for lists, and `.update` calls for sets and dicts.

---
