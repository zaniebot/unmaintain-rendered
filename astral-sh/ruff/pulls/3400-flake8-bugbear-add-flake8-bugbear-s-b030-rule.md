```yaml
number: 3400
title: "[`flake8-bugbear`] Add `flake8-bugbear`'s B030 rule"
type: pull_request
state: merged
author: aacunningham
labels:
  - rule
assignees: []
merged: true
base: main
head: add-b030-rule
created_at: 2023-03-08T09:19:00Z
updated_at: 2023-03-14T17:01:45Z
url: https://github.com/astral-sh/ruff/pull/3400
synced_at: 2026-01-12T04:39:44Z
```

# [`flake8-bugbear`] Add `flake8-bugbear`'s B030 rule

---

_Pull request opened by @aacunningham on 2023-03-08 09:19_

Addresses B030 in #2954 

Gave the rule name the good ol' college try, let me know if there's a better name we can think of.

This implementation is somewhat faithful to the original, with the exception (pun) that we'll handle iterables a little more strictly.

bugbear currently allows this:
```python
try:
    raise ValueError()
except (ValueError, (RuntimeError, (KeyError, TypeError))):
    pass
```
which causes a TypeError when `(RuntimeError, (KeyError, TypeError))` doesn't inherit from BaseException.

ruff will mark that as a failure, and will allow the following:
```python
try:
    raise ValueError()
except (ValueError, *(RuntimeError, *(KeyError, TypeError))): # Now with more stars
    pass
```

This seems like their intention, and I've already made a [PR to their project](https://github.com/PyCQA/flake8-bugbear/pull/364). Not sure how important it is that the rules match the base projects _exactly_, but we could wait and see how they respond to the change.

---

_Comment by @charliermarsh on 2023-03-08 20:29_

Awesome. Agree w/ the change you made (and looks like upstream agrees too).

---

_@charliermarsh reviewed on 2023-03-08 20:37_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_bugbear/rules/except_with_non_exception_classes.rs`:13 on 2023-03-08 20:37_

I think the name is 👍 

---

_@charliermarsh reviewed on 2023-03-08 20:37_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_bugbear/rules/except_with_non_exception_classes.rs`:18 on 2023-03-08 20:37_

Tweaked this to use backticks around except which differs from bugbear but it's more consistent with how we tend to format messages.

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_bugbear/rules/except_with_non_exception_classes.rs`:50 on 2023-03-08 20:37_

Changed this to use `let-else`.

---

_@charliermarsh reviewed on 2023-03-08 20:37_

---

_Comment by @charliermarsh on 2023-03-08 20:38_

Great PR! Thanks!

---

_Label `rule` added by @charliermarsh on 2023-03-08 20:38_

---

_Renamed from "Add flake8-bugbear B030 Rule" to "[`flake8-bugbear`] Add `flake8-bugbear`'s B030 rule" by @charliermarsh on 2023-03-08 20:38_

---

_Merged by @charliermarsh on 2023-03-08 20:41_

---

_Closed by @charliermarsh on 2023-03-08 20:41_

---

_Comment by @aacunningham on 2023-03-14 17:01_

Appreciate the updates to the code and the merge! 😄 

---

_Branch deleted on 2023-03-14 17:01_

---
