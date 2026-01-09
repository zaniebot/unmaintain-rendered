---
number: 5196
title: "isort: ignore line-length for imports"
type: issue
state: closed
author: Avasam
labels: []
assignees: []
created_at: 2023-06-19T23:15:07Z
updated_at: 2023-06-20T20:50:27Z
url: https://github.com/astral-sh/ruff/issues/5196
synced_at: 2026-01-07T13:12:15-06:00
---

# isort: ignore line-length for imports

---

_Issue opened by @Avasam on 2023-06-19 23:15_

I used to be able to do so by configuring a different line-length for isort.

**Why?**
1. It takes a lot more vertical space than it needs to. The least the better.
Imports are something we almost never have to even look at. They can by auto imported by the IDE, and unused imports can automatically be removed. Even when I have to manually add an import (almost always`from collections.abc` because of https://github.com/microsoft/pylance-release/issues/3318), I just type it anywhere and it automatically goes to the top and the file and sorted.

2. The "it reduces git conflicts" argument is moot because:
2.1. When there's a conflict, imports are easily autofixable by just "accepting both", then running autofixes. Duplicates are automatically merged and unused are removed.
2.2. Randomly wrapping or unwrapping imports because it just crossed the threshold does not help reduce git conflicts, which anyway are easily autofixable as mentioned previously.

3. This just looks silly
![image](https://github.com/astral-sh/ruff/assets/1350584/64d04aec-1dea-494d-9d9c-552ced2a2f1d)
![image](https://github.com/astral-sh/ruff/assets/1350584/224582c0-0b8a-42f3-96c1-2e7b7522a9f0)




---

_Comment by @charliermarsh on 2023-06-19 23:31_

Are you looking to set a higher or lower limit for imports? (I assume higher from above.) Do you _ever_ want them to be split across multiple lines?

---

_Comment by @Avasam on 2023-06-19 23:52_

I'm looking to never wrap imports / never split imports across multiple lines. To not have what happens at line 15 in the first image, or line 28 and 41 in the second one. If I unwrapped imports in my example, it would trigger both `I001 [*] Import block is un-sorted or un-formatted` and `E501 Line too long`.

But I still want line-length for non import statements. And for imports to be sorted.

---

_Comment by @charliermarsh on 2023-06-20 20:50_

Makes sense. I think this can be marked as a duplicate of #3206. The main challenge is that we don't have a way to enforce a separate E501 cap on import-containing lines.

---

_Closed by @charliermarsh on 2023-06-20 20:50_

---

_Referenced in [astral-sh/ruff#3206](../../astral-sh/ruff/issues/3206.md) on 2023-06-20 23:17_

---
