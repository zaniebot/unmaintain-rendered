```yaml
number: 9798
title: Slight speed-up for lowercase and uppercase identifier checks
type: pull_request
state: merged
author: charliermarsh
labels:
  - performance
assignees: []
merged: true
base: main
head: charlie/str
created_at: 2024-02-02T20:26:27Z
updated_at: 2024-02-03T14:47:20Z
url: https://github.com/astral-sh/ruff/pull/9798
synced_at: 2026-01-10T22:57:09Z
```

# Slight speed-up for lowercase and uppercase identifier checks

---

_Pull request opened by @charliermarsh on 2024-02-02 20:26_

It turns out that for ASCII identifiers, this is nearly 2x faster:

```
Parser/before     time:   [15.388 ns 15.395 ns 15.406 ns]
Parser/after      time:   [8.3786 ns 8.5821 ns 8.7715 ns]
```


---

_Review requested from @BurntSushi by @charliermarsh on 2024-02-02 20:26_

---

_Label `performance` added by @charliermarsh on 2024-02-02 20:26_

---

_Comment by @github-actions[bot] on 2024-02-02 20:39_

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

_Marked ready for review by @charliermarsh on 2024-02-02 20:43_

---

_@MichaReiser approved on 2024-02-02 21:03_

Now you're micro micro optimising. I'm not sure if this is worth the extra complexity

---

_Comment by @charliermarsh on 2024-02-02 21:30_

The counterargument is that we run this on every single identifier declared within every single function.

---

_@BurntSushi approved on 2024-02-02 21:57_

LGTM. I don't mind the extra complexity so much in routines like this personally, because it's isolated and doesn't spread.

---

_Comment by @charliermarsh on 2024-02-02 22:47_

Need to add some non-ASCII tests (which we should have anyway).

---

_Merged by @charliermarsh on 2024-02-03 14:40_

---

_Closed by @charliermarsh on 2024-02-03 14:40_

---

_Branch deleted on 2024-02-03 14:40_

---
