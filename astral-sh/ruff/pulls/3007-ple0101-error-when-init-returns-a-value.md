```yaml
number: 3007
title: "[`PLE0101`] error when `__init__` returns a value"
type: pull_request
state: merged
author: r3m0t
labels: []
assignees: []
merged: true
base: main
head: return-in-init
created_at: 2023-02-18T12:03:22Z
updated_at: 2023-02-19T14:54:44Z
url: https://github.com/astral-sh/ruff/pull/3007
synced_at: 2026-01-12T15:55:12Z
```

# [`PLE0101`] error when `__init__` returns a value

---

_@r3m0t_

It's my first PR on any rust project 🥳 

#970

---

_Converted to draft by @r3m0t on 2023-02-18 12:03_

---

_Marked ready for review by @r3m0t on 2023-02-18 12:25_

---

_Comment by @sbrugman on 2023-02-18 12:50_

(@r3m0t Run `cargo dev generate-all` to update the schema)


---

_Review comment by @sbrugman on `crates/ruff/src/rules/pylint/rules/return_in_init.rs`:38 on 2023-02-18 12:53_

Please document the rule (you can take [pylint](https://pylint.readthedocs.io/en/latest/user_guide/messages/error/return-in-init.html) as a start)

See https://github.com/charliermarsh/ruff/issues/2646 or yield-on-init


---

_@sbrugman reviewed on 2023-02-18 12:53_

---

_Comment by @r3m0t on 2023-02-18 13:00_

Fixed, I had to change `README.md` to check out with LF.

---

_Review requested from @sbrugman by @r3m0t on 2023-02-18 13:00_

---

_Review comment by @sbrugman on `crates/ruff/resources/test/fixtures/pylint/return_in_init.py`:41 on 2023-02-18 15:05_

What about including:
```python
class MyClass:
    def __init__(self):
        def func():
            return 3
        self.value = func()
```

Any other cases?

---

_Review comment by @sbrugman on `crates/ruff/src/rules/pylint/rules/return_in_init.rs`:26 on 2023-02-18 15:06_

Add Instead do (example)

See other rules for examples

---

_Comment by @sbrugman on 2023-02-18 15:08_

Thanks for helping out: implementing pylint rules is really useful!

Couple of minor things.

Also note that the PR title is used for the changelog. The format for rules is "[`code`] description" (we should have PR templates to support this and automatically add the label)

---

_Renamed from "Add pylint rule return-in-init (#970)" to "[PLE0101] error when __init__ returns a value" by @r3m0t on 2023-02-18 17:36_

---

_Comment by @r3m0t on 2023-02-18 17:36_

All addressed I think

---

_Comment by @sbrugman on 2023-02-19 00:24_

> All addressed I think

Don't see the changes, did you push?

---

_Comment by @r3m0t on 2023-02-19 09:41_

> > All addressed I think
> 
> Don't see the changes, did you push?

Did you mean the PR title or the commit message itself? Yes, I just force pushed to update the commit message, and added the docs

---

_Comment by @sbrugman on 2023-02-19 10:03_

Ah sorry, posted two more comments but they were not submitted but still "pending"

---

_@sbrugman reviewed on 2023-02-19 10:21_

---

_Renamed from "[PLE0101] error when __init__ returns a value" to "[`PLE0101`] error when `__init__` returns a value" by @charliermarsh on 2023-02-19 14:52_

---

_Comment by @charliermarsh on 2023-02-19 14:52_

Welcome! Thanks for the contribution, and thanks to @sbrugman for the review.

---

_Merged by @charliermarsh on 2023-02-19 14:54_

---

_Closed by @charliermarsh on 2023-02-19 14:54_

---
