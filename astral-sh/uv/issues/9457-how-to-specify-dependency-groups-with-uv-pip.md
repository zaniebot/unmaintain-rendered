---
number: 9457
title: "How to specify dependency groups with `uv pip install` and `uv pip compile`?"
type: issue
state: closed
author: cthoyt
labels:
  - duplicate
assignees: []
created_at: 2024-11-26T23:31:10Z
updated_at: 2024-11-26T23:40:37Z
url: https://github.com/astral-sh/uv/issues/9457
synced_at: 2026-01-10T01:24:41Z
---

# How to specify dependency groups with `uv pip install` and `uv pip compile`?

---

_Issue opened by @cthoyt on 2024-11-26 23:31_

I'm on uv 0.5.4 (Homebrew 2024-11-20) and it doesn't appear there's a way to install dependency groups with `uv pip install` nor include it in compilation with `uv pip compile`.

Based on the PEP (cf. https://peps.python.org/pep-0735/#installing-dependency-groups), pip should eventually be able to do this with something like:

```console
$ pip install --dependency-groups=tests .
```

I wasn't able to find any evidence that an implementation has made it into pip's recent October v24.3.1 release nor mention on its changelog (https://pip.pypa.io/en/stable/news/). 

Q: Is uv waiting for pip to establish an interface, then it will adopt it? I can see how implementing something that is different from pip might be problematic for maintaining compatibility.

For context, `uv sync` already does implement dependency groups with `--group` and related flags.

---

_Comment by @zanieb on 2024-11-26 23:36_

Please see 

- https://github.com/astral-sh/uv/issues/8969
- https://github.com/astral-sh/uv/issues/8590

We're waiting for an upstream implementation. I'm engaging a lot on the design.

---

_Label `duplicate` added by @zanieb on 2024-11-26 23:36_

---

_Comment by @cthoyt on 2024-11-26 23:40_

sorry about that, I did in fact search the issue tracker, but didn't find those. Maybe you could also add the text "dependency group" / "dependency groups" without dashes to make it more findable!

---

_Closed by @cthoyt on 2024-11-26 23:40_

---
