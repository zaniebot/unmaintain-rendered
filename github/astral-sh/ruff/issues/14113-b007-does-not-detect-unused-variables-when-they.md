---
number: 14113
title: B007 does not detect unused variables when they are shadowed or appear in inner scope
type: issue
state: open
author: LordAro
labels:
  - bug
assignees: []
created_at: 2024-11-05T17:23:59Z
updated_at: 2024-11-05T22:22:45Z
url: https://github.com/astral-sh/ruff/issues/14113
synced_at: 2026-01-07T13:12:16-06:00
---

# B007 does not detect unused variables when they are shadowed or appear in inner scope

---

_Issue opened by @LordAro on 2024-11-05 17:23_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* List of keywords you searched for before creating this issue. Write them down here so that others can find this issue more easily and help provide feedback.
  e.g. "RUF001", "unused variable", "Jupyter notebook"
* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

B007 (unused-loop-control-variable) does not trigger when variables are shadowed.

See this extremely contrived example:

```python3
my_dict = my_other_dict = {}
my_list = []
for k, (v1, v2) in my_dict.items():
    for v1 in my_other_dict.values():
        print(v1)
    v2 = "flibble"
    print([k for k in my_list if k > 0])
```

https://play.ruff.rs/92ded63b-80d4-4d7d-989a-3460865af51b

(Example is somewhat nested loop focused as that's what I thought the problem was initially)

Interestingly, PERF102 *does* trigger with this example, but I think only because it doesn't process comprehensions at all? If it's expanded out the warning goes away.

The only thing of note from ALL is PLW2901, which detects the variable shadowing.

Related issues: #8786 #9454 #10723

ruff 0.7.2


---

_Renamed from "B007 does not detect shadowed variables" to "B007 does not detect unused variables when they are shadowed" by @LordAro on 2024-11-05 17:24_

---

_Comment by @dylwil3 on 2024-11-05 19:39_

Excellent, thanks for this! It looks like the same problem occurs even in this smaller example [Playground link](https://play.ruff.rs/44a26685-7932-4a77-85c9-6c7978ee0260):

```python
for k in ls:
    print([k for k in otherls])
```


---

_Label `bug` added by @dylwil3 on 2024-11-05 19:39_

---

_Renamed from "B007 does not detect unused variables when they are shadowed" to "B007 does not detect unused variables when they are shadowed or appear in inner scope" by @dylwil3 on 2024-11-05 21:59_

---

_Comment by @dylwil3 on 2024-11-05 22:12_

I guess technically my previous example wasn't "shadowing", but an issue of scope. But shadowing _also_ doesn't work (as you showed well in your original example):

```python
for k in ls:
    k = 2
```

But, as you also point out, this is caught by [PLW2901](https://docs.astral.sh/ruff/rules/redefined-loop-name/).

---

_Assigned to @dylwil3 by @dylwil3 on 2024-11-05 22:22_

---

_Referenced in [astral-sh/ruff#14122](../../astral-sh/ruff/pulls/14122.md) on 2024-11-06 04:09_

---
