---
number: 21014
title: "`loop-iteration-mutation` (B909) cannot be in-line disabled"
type: issue
state: closed
author: Jtachan
labels:
  - question
assignees: []
created_at: 2025-10-21T11:04:15Z
updated_at: 2025-10-21T12:03:15Z
url: https://github.com/astral-sh/ruff/issues/21014
synced_at: 2026-01-07T13:12:16-06:00
---

# `loop-iteration-mutation` (B909) cannot be in-line disabled

---

_Issue opened by @Jtachan on 2025-10-21 11:04_

### Summary

I cannot find the way to disable one line where the linter complains about the violation of rule B909.

I know the better approach for my code is to actually fix this issue, but this code is currently in a state where:
- The issue will be fixed later on.
- It is desired to ignore just this line of code.
- It is desired to keep all other ruff checks through the file.

Here is an example of a different code where `ruff` warns about B909:
```python
1. my_list = list(range(10))
2. 
3. for idx, _ in enumerate(my_list):
4.     if idx % 2 == 0:
5.         my_list.pop(idx)
```

I have tested these solutions do not work:

- Add `# noqa: B909` at the end of line 3
- Add `# noqa` at the end of line 3

I have tested these solutions work:

- Within `ruff.toml` extend `ignore = ["B909"]`
- Within the python file, disable the linter by writing `# ruff: noqa` at the top of the file

> PS: I've also tried to reproduce the issue in the playground, but there the linter does not complain at all. And I mean that I am not even trying to ignore the rule. I have checked the rule `"B"` is within the linter select configuration.

### Version

ruff v0.14.1

---

_Comment by @MichaReiser on 2025-10-21 11:28_

The noqa must be added at the end of the line mentioned in the diagnostic. In this case, it's line 5. So this should work


```py
my_list = list(range(10))

for idx, _ in enumerate(my_list):
    if idx % 2 == 0:
        my_list.pop(idx) # noqa: B909
```

https://play.ruff.rs/9583b75a-dcff-4951-973e-1f5cf230abe4

---

_Label `question` added by @MichaReiser on 2025-10-21 11:28_

---

_Comment by @Jtachan on 2025-10-21 12:03_

You are right. I did not correctly check the line ruff was pointing to.
I will close this issue as everything seems to work then.

---

_Closed by @Jtachan on 2025-10-21 12:03_

---
