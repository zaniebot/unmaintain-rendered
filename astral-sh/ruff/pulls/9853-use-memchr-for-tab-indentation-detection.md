```yaml
number: 9853
title: "Use `memchr` for tab-indentation detection"
type: pull_request
state: merged
author: charliermarsh
labels:
  - performance
assignees: []
merged: true
base: main
head: charlie/tabs
created_at: 2024-02-06T03:25:44Z
updated_at: 2024-02-06T14:44:57Z
url: https://github.com/astral-sh/ruff/pull/9853
synced_at: 2026-01-10T22:57:09Z
```

# Use `memchr` for tab-indentation detection

---

_Pull request opened by @charliermarsh on 2024-02-06 03:25_

## Summary

The benchmarks show a pretty consistent 1% speedup here for all-rules, though not enough to trigger our threshold of course:

![Screenshot 2024-02-05 at 11 55 59 PM](https://github.com/astral-sh/ruff/assets/1309177/317dca3f-f25f-46f5-8ea8-894a1747d006)


---

_Comment by @github-actions[bot] on 2024-02-06 03:39_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **encountered linter errors**. (no lint changes; 1 project error)

<details><summary><a href="https://github.com/sphinx-doc/sphinx">sphinx-doc/sphinx</a> (error)</summary>
<p>

```
ruff failed
  Cause: Selection of unstable rules without the `--preview` flag is not allowed. Enable preview or remove selection of:
	- FURB113
	- FURB131
	- FURB132
```

</p>
</details>

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Renamed from "Try fast-path for tab detection" to "Use `memchr` for tab-indentation detection" by @charliermarsh on 2024-02-06 04:55_

---

_Review requested from @BurntSushi by @charliermarsh on 2024-02-06 04:56_

---

_Marked ready for review by @charliermarsh on 2024-02-06 04:56_

---

_Label `performance` added by @charliermarsh on 2024-02-06 04:56_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pycodestyle/rules/tab_indentation.rs`:89 on 2024-02-06 12:17_

Nit: I believe the rule will now have a false positive for

```python
a = "b\
		test"
```

---

_@MichaReiser approved on 2024-02-06 12:27_

---

_@charliermarsh reviewed on 2024-02-06 14:44_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pycodestyle/rules/tab_indentation.rs`:89 on 2024-02-06 14:44_

I swear I tested this and it wasn't valid but you're totally right. Regardless, we need to solve this for the trailing whitespace rule too.

---

_Merged by @charliermarsh on 2024-02-06 14:44_

---

_Closed by @charliermarsh on 2024-02-06 14:44_

---

_Branch deleted on 2024-02-06 14:44_

---
