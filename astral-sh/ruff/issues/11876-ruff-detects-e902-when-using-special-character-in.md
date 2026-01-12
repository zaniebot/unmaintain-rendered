```yaml
number: 11876
title: ruff detects E902 when using special character in code
type: issue
state: closed
author: quebt
labels: []
assignees: []
created_at: 2024-06-14T14:08:25Z
updated_at: 2024-06-23T16:36:07Z
url: https://github.com/astral-sh/ruff/issues/11876
synced_at: 2026-01-12T15:54:51Z
```

# ruff detects E902 when using special character in code

---

_@quebt_

- Keywords: utf-8, E902
- script example. Use § or ü in a string to trigger the bug.

```
import os

test = 'abc§'
print(os.getcwd())
    print('wrong indent')
```
- ruff settings non special simply
`ruff check .\test.py`
- version 0.4.8

- Output. Here interesting: the syntax bug is not reported, instead ruff stops.
![image](https://github.com/astral-sh/ruff/assets/30908027/8af8f148-b61c-48fe-b6f9-af9b7df38150)


---

_Comment by @MichaReiser on 2024-06-14 14:11_

Can you try changing the encoding to UTF8 in the bottom-toolbar (click Windows-1252). Ruff only supports UTF8 encoded files.

Related issue: https://github.com/astral-sh/ruff/issues/6791

---

_Comment by @quebt on 2024-06-17 20:38_

ok, that did the trick. but what about other encodings? we have a system which does not support python code in utf-8...

---

_Comment by @MichaReiser on 2024-06-17 20:59_

Ruff doesn't support other encodings today. Can you tell us more about your system that doesn't support utf8? What python version are you using?

---

_Comment by @quebt on 2024-06-22 17:28_

Hi,
It is a trading system from FIS. Actually it does support utf-8 but since there are scripts from the vendor in non utf-8 we habe sometimes no choice. Currently we are on Python 3.7.9 but we are upgrading to 3.9.18.

---

_Comment by @charliermarsh on 2024-06-23 16:36_

I'm going to call this unsupported but happy to hear more about your use-case and whether we can help with it in ways other than supporting alternate encodings.

---

_Closed by @charliermarsh on 2024-06-23 16:36_

---
