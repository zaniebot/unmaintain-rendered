```yaml
number: 6129
title: "Add debug representation for `MarkerTree`"
type: pull_request
state: closed
author: ibraheemdev
labels: []
assignees: []
draft: true
base: main
head: ibraheem/marker-tree-debug
created_at: 2024-08-15T21:19:10Z
updated_at: 2024-09-06T20:47:52Z
url: https://github.com/astral-sh/uv/pull/6129
synced_at: 2026-01-12T16:07:14Z
```

# Add debug representation for `MarkerTree`

---

_@ibraheemdev_

## Summary

Adds a simple `Debug` representation for `MarkerTree` based on the underlying decision diagram. For example, `python_version == '3.7' and os_name == 'Linux') or os_name == 'Windows'` is displayed as:
```
python_version<3.7 | >3.7 -> 
  os_name<Windows | >Windows -> false
  os_name==Windows -> true
python_version==3.7 -> 
  os_name<Linux | >Linux, <Windows | >Windows -> false
  os_name==Linux | ==Windows -> true
```
 
I'm not sure we want to use this form for our debug logs because it could be confusing to users but we've also run into issues where markers looked equal when simplified despite being represented slightly differently..

---

_Label `preview` added by @ibraheemdev on 2024-08-15 21:19_

---

_Comment by @ibraheemdev on 2024-08-15 21:29_

We should probably add a separate method `display_maybe_empty` for user facing output.

---

_Converted to draft by @ibraheemdev on 2024-08-15 21:29_

---

_Comment by @BurntSushi on 2024-08-16 14:14_

> but we've also run into issues where markers looked equal when simplified despite being represented slightly differently..

Can you show an example? I really like how our `Debug` implementation works today, especially since it fits on one line. (Although that does break down for larger markers.)

---

_Label `preview` removed by @zanieb on 2024-08-20 19:05_

---

_Closed by @BurntSushi on 2024-09-06 20:47_

---
