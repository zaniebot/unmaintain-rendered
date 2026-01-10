```yaml
number: 3453
title: "[bug?] B018 `useless-expression` does not work"
type: issue
state: closed
author: smackesey
labels:
  - rule
assignees: []
created_at: 2023-03-11T23:18:31Z
updated_at: 2023-03-23T22:34:00Z
url: https://github.com/astral-sh/ruff/issues/3453
synced_at: 2026-01-10T11:09:46Z
```

# [bug?] B018 `useless-expression` does not work

---

_Issue opened by @smackesey on 2023-03-11 23:18_

None of the following trigger `B018` `useless-expression` for me, when I would expect them all to:

```
"foo" + "bar"  # should trigger B018

"foo"  # should trigger B018

object().__class__  # should trigger B018
```

I might be missing something about what this rule is supposed to do though.

---

_Comment by @charliermarsh on 2023-03-11 23:22_

I think this is consistent with [flake8-bugbear](https://github.com/PyCQA/flake8-bugbear/blob/37bd2a079e1b2ebc0a72c8cf1a8e1c90c6c69148/bugbear.py#L1042) although I'm not sure why their rule is limited to those cases.

---

_Comment by @charliermarsh on 2023-03-12 04:49_

I'm _guessing_ they got rid of strings to avoid false positives with docstrings (https://github.com/PyCQA/flake8-bugbear/pull/209/files).

I'm guessing they don't detect function calls since it's not really possible (with current information) to know whether a function call is effect-ful or pure.

We could definitely detect binary operations though.

---

_Comment by @charliermarsh on 2023-03-12 04:49_

In general I think we can improve this even if it deviates a bit from bugbear.

---

_Label `rule` added by @charliermarsh on 2023-03-12 04:49_

---

_Comment by @smackesey on 2023-03-12 13:44_

I was actually unable to get this rule to trigger at all-- do you have an example of something that triggers it? Like, `1 + 5` doesn't trigger it either.

>I'm guessing they don't detect function calls since it's not really possible (with current information) to know whether a function call is effect-ful or pure.

Yeah, the expected behavior is not to detect a straight function call, but when it's followed by attribute access (as in my example) then I would think it should be detected. FWIW that's pylint's behavior.

---

_Comment by @charliermarsh on 2023-03-12 14:07_

Prior to my open PR, the rule was only enforced in function and class bodies. I think this was consistent with bugbear but has since changed, so it’s reflected in the PR.

---

_Comment by @smackesey on 2023-03-12 14:33_

Ah nice, somehow my eyes glossed over the PR notification. Thx for addressing.

---

_Comment by @charliermarsh on 2023-03-12 18:02_

All good, that PR still doesn't include the `object().__class__` case which I may add.

---

_Closed by @charliermarsh on 2023-03-23 22:34_

---
