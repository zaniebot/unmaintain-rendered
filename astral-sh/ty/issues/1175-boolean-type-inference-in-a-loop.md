```yaml
number: 1175
title: Boolean type inference in a loop
type: issue
state: closed
author: benmosher
labels: []
assignees: []
created_at: 2025-09-12T18:18:44Z
updated_at: 2025-09-12T19:29:47Z
url: https://github.com/astral-sh/ty/issues/1175
synced_at: 2026-01-10T02:06:25Z
```

# Boolean type inference in a loop

---

_Issue opened by @benmosher on 2025-09-12 18:18_

### Summary

Hello, just wanted to provide a quick note / repro for a type inference issue I'm seeing for a boolean in a loop wrongly set as `Literal[True]` instead of either `bool` or `Literal[True, False]`.

Code:

```
first = True
for line in []:
    x = first
    if first:
        first = False
    y = first
```

screenshot of the inline types in VSCode:

<img width="339" height="132" alt="Image" src="https://github.com/user-attachments/assets/7e311689-7efb-402e-b8df-3ca93afe62ae" />

very cool and correct that `y` is typed `Literal[False]` as `first` is always `False` _there_, but it may be `True` or `False` (mostly `False`) on line 3.

Love `uv` and `ruff`. thanks for everything y'all do. hope this helps. (python version is 3.11 btw)

### Version

0.0.1-alpha.20 (f41f00af1 2025-09-03)

---

_Comment by @carljm on 2025-09-12 19:29_

Thanks for the report! Yes, this is a known issue, it's #232 -- we originally planned to do it a while ago but it's surprising how little it comes up, so we keep prioritizing other things before it. We'll get to it.

---

_Closed by @carljm on 2025-09-12 19:29_

---
