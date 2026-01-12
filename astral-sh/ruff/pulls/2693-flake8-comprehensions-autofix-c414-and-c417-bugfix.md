```yaml
number: 2693
title: "[`flake8-comprehensions`] autofix C414 and C417 + bugfix"
type: pull_request
state: merged
author: sbrugman
labels:
  - fixes
assignees: []
merged: true
base: main
head: c414-c417-autofix
created_at: 2023-02-09T19:09:06Z
updated_at: 2023-02-12T05:20:43Z
url: https://github.com/astral-sh/ruff/pull/2693
synced_at: 2026-01-12T04:52:00Z
```

# [`flake8-comprehensions`] autofix C414 and C417 + bugfix

---

_Pull request opened by @sbrugman on 2023-02-09 19:09_

Closes https://github.com/charliermarsh/ruff/issues/2262 and closes https://github.com/charliermarsh/ruff/issues/2423

Fixes bug where some cases generated duplicated violations (see https://github.com/charliermarsh/ruff/pull/2732#issuecomment-1426397842)

---

_Review comment by @sbrugman on `src/rules/flake8_comprehensions/fixes.rs`:934 on 2023-02-09 19:27_

Resolved, remove comment

---

_@sbrugman reviewed on 2023-02-09 19:27_

---

_@sbrugman reviewed on 2023-02-09 19:27_

---

_Review comment by @sbrugman on `src/rules/flake8_comprehensions/rules.rs`:696 on 2023-02-09 19:27_

Done, remove comment

---

_Label `autofix` added by @charliermarsh on 2023-02-10 04:13_

---

_Comment by @sbrugman on 2023-02-10 21:19_

Comment by @spaceone 
> C417 doesn't detect all cases anymore. It fixes:
> 
> ```diff
> -        assert all(map(lambda v: isinstance(v, dict), iterator))
> +        assert all((isinstance(v, dict) for v in iterator))
> ```
> 
> better would be:
> 
> ```diff
> -        assert all(map(lambda v: isinstance(v, dict), iterator))
> +        assert all(isinstance(v, dict) for v in iterator)
> ```


---

_Renamed from "feat: C414 and C417 autofix and bugfix" to "C414 and C417 autofix and bugfix" by @sbrugman on 2023-02-10 21:58_

---

_Comment by @spaceone on 2023-02-10 22:09_

with the MR the following is not detected anymore as `C417`:
```python
bitstring = ''.join(map(lambda x: x in logontimes and '1' or '0', range(168)))
```

---

_Comment by @sbrugman on 2023-02-10 22:20_

Fixed the false negatives. Improving the fixing by taking into account wether the parent call takes a generator would be a nice addition (not essential for merging in my view). Is there already another rule detecting this case above?

---

_Review comment by @spaceone on `crates/ruff/src/rules/flake8_comprehensions/rules/unnecessary_map.rs`:27 on 2023-02-10 23:41_

```suggestion
    /// Rewrite `dict(map(lambda v: (v, v ** 2), values))` to `{v: v ** 2 for v in values}`
```

---

_@spaceone reviewed on 2023-02-10 23:41_

---

_Marked ready for review by @sbrugman on 2023-02-11 00:13_

---

_Renamed from "C414 and C417 autofix and bugfix" to "[`flake8-comprehensions`] autofix C414 and C417 + bugfix" by @sbrugman on 2023-02-11 15:47_

---

_Merged by @charliermarsh on 2023-02-12 05:20_

---

_Closed by @charliermarsh on 2023-02-12 05:20_

---
