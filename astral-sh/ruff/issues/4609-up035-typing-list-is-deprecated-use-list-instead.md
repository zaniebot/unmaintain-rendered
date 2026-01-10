```yaml
number: 4609
title: "UP035 `typing.List` is deprecated, use `list` instead"
type: issue
state: closed
author: jackdesert
labels: []
assignees: []
created_at: 2023-05-23T17:02:27Z
updated_at: 2023-05-23T20:46:30Z
url: https://github.com/astral-sh/ruff/issues/4609
synced_at: 2026-01-10T11:09:47Z
```

# UP035 `typing.List` is deprecated, use `list` instead

---

_Issue opened by @jackdesert on 2023-05-23 17:02_


Ruff is raising UP035 (typing.List is deprecated, use list instead), but when I run `pyupgrade` on the same file, it says the file is fine. I actually prefer using `typing.List` over using `list` for type annotations. But I didn't want to simply disable UP035 since there may be other outdated import styles that also fall under rule UP035. I'm not sure if this is a bug or not. The docs for pyupgrade do not appear to mention that it alters `from typing import List`. 

Code Snippet:
    
    from typing import List

    def hello(names: List[str]):
        for name in names:
            print(name)



Ruff version: ruff 0.0.269


Command invoked:

    ruff --isolated --select=UP /tmp/sample.py

Output from ruff:

    /tmp/sample.py:1:1: UP035 `typing.List` is deprecated, use `list` instead
    /tmp/sample.py:3:18: UP006 [*] Use `list` instead of `List` for type annotation

---

_Comment by @charliermarsh on 2023-05-23 18:35_

Hmm, I'm not sure why pyupgrade doesn't consider those deprecated, but I feel like this correct / intended, in that it would be _incorrect_ not to flag `typing.List` here given that it is marked as deprecated via [PEP 585](https://peps.python.org/pep-0585/).

---

_Comment by @bersbersbers on 2023-05-23 18:35_

I would say `ruff` is right and maybe `pyupgrade` is wrong: `typing.List` *is* deprecated, see https://docs.python.org/3/library/typing.html#typing.List.

> I actually prefer using typing.List over using list for type annotations

I am not sure this is relevant, but may I ask why that is?

---

_Comment by @charliermarsh on 2023-05-23 18:36_

I'm going to err on the side of closing because I consider this to be reasonable behavior.

---

_Closed by @charliermarsh on 2023-05-23 18:36_

---

_Comment by @jackdesert on 2023-05-23 20:46_

This makes sense now. At first I preferred `List[str]` because I did not realize that `list[str]` was an option. I'll start using `list[str]` and avoid the extra import.

---
