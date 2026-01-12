```yaml
number: 2732
title: "[`flake8-pytest-style`] autofix for composite-assertion (PT018)"
type: pull_request
state: merged
author: sbrugman
labels: []
assignees: []
merged: true
base: main
head: autofix-PT018
created_at: 2023-02-10T19:05:47Z
updated_at: 2023-02-16T00:36:08Z
url: https://github.com/astral-sh/ruff/pull/2732
synced_at: 2026-01-12T15:55:10Z
```

# [`flake8-pytest-style`] autofix for composite-assertion (PT018)

---

_@sbrugman_

Autofix for composite assertion/PT018

Made some choices to keep it simple, see the test file for details.

---

_Comment by @spaceone on 2023-02-10 21:16_

C417 doesn't detect all cases anymore.
It fixes:

```diff
-        assert all(map(lambda v: isinstance(v, dict), iterator))
+        assert all((isinstance(v, dict) for v in iterator))
```

better would be:
```diff
-        assert all(map(lambda v: isinstance(v, dict), iterator))
+        assert all(isinstance(v, dict) for v in iterator)
```

---

_Comment by @spaceone on 2023-02-10 21:19_

btw - replacing `filter()` in the same manner would also be helpful.

---

_Comment by @spaceone on 2023-02-10 21:31_

with the MR the following is not detected anymore as C417: `bitstring = ''.join(map(lambda x: x in logontimes and '1' or '0', range(168)))`

---

_Comment by @sbrugman on 2023-02-10 21:59_

Thanks, will look at them (in https://github.com/charliermarsh/ruff/pull/2693, this PR was intended for PT018)

---

_Comment by @spaceone on 2023-02-10 22:02_

I found out why I have so many differences in the detection of that code:

Prior each finding emitted two violations:
```
C417 Unnecessary `map` usage (rewrite using a `list` comprehension)
C417 Unnecessary `map` usage (rewrite using a generator expression)
```

Now only one is detected:
```
C417 Unnecessary `map` usage (rewrite using a `list` comprehension)
```

→ I think this is ok.

---

_Marked ready for review by @sbrugman on 2023-02-10 22:12_

---

_Renamed from "Autofix composite-assertion" to "[`flake8-pytest-style`] autofix for composite-assertion (PT018)" by @sbrugman on 2023-02-11 15:47_

---

_@sbrugman reviewed on 2023-02-15 15:37_

---

_Review comment by @sbrugman on `crates/ruff/resources/test/fixtures/flake8_pytest_style/PT018.py`:26 on 2023-02-15 15:37_

Alternative: repeat message for each subcondition

---

_Review comment by @sbrugman on `docs/rules/composite-assertion.md`:1 on 2023-02-15 15:38_

Needs to be excluded after refactor

---

_@sbrugman reviewed on 2023-02-15 15:38_

---

_@charliermarsh reviewed on 2023-02-16 00:08_

---

_Review comment by @charliermarsh on `crates/ruff/resources/test/fixtures/flake8_pytest_style/PT018.py`:26 on 2023-02-16 00:08_

I think this is the right call (not to fix these cases).

---

_Merged by @charliermarsh on 2023-02-16 00:36_

---

_Closed by @charliermarsh on 2023-02-16 00:36_

---
