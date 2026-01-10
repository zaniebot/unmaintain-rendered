```yaml
number: 9387
title: Add cell indexes to all diagnostics
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/row
created_at: 2024-01-04T02:44:01Z
updated_at: 2024-01-04T14:13:03Z
url: https://github.com/astral-sh/ruff/pull/9387
synced_at: 2026-01-10T23:07:18Z
```

# Add cell indexes to all diagnostics

---

_Pull request opened by @charliermarsh on 2024-01-04 02:44_

## Summary

Ensures that any lint rules that include line locations render them as relative to the cell (and include the cell number) when inside a Jupyter notebook.

Closes https://github.com/astral-sh/ruff/issues/6672.

## Test Plan

`cargo test`


---

_Label `bug` added by @charliermarsh on 2024-01-04 02:44_

---

_Review requested from @zanieb by @charliermarsh on 2024-01-04 02:45_

---

_Review requested from @konstin by @charliermarsh on 2024-01-04 02:45_

---

_Marked ready for review by @charliermarsh on 2024-01-04 02:45_

---

_Comment by @github-actions[bot] on 2024-01-04 03:02_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **encountered linter errors**. (no lint changes; 1 project error)

<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (error)</summary>
<p>

```
Failed to clone ibis-project/ibis: warning: Could not find remote branch master to clone.
fatal: Remote branch master not found in upstream origin
```

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **encountered linter errors**. (no lint changes; 1 project error)

<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

```
Failed to clone ibis-project/ibis: warning: Could not find remote branch master to clone.
fatal: Remote branch master not found in upstream origin
```

</p>
</details>

### Formatter (stable)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (error)</summary>
<p>

```
Failed to clone ibis-project/ibis: warning: Could not find remote branch master to clone.
fatal: Remote branch master not found in upstream origin
```

</p>
</details>

### Formatter (preview)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (error)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

```
Failed to clone ibis-project/ibis: warning: Could not find remote branch master to clone.
fatal: Remote branch master not found in upstream origin
```

</p>
</details>




---

_Review comment by @konstin on `crates/ruff_source_file/src/lib.rs`:260 on 2024-01-04 11:22_

nit: I'd give those explicit names given that the description mentions first row then cell while the data is first cell then row.

---

_@konstin approved on 2024-01-04 11:22_

---

_Merged by @charliermarsh on 2024-01-04 14:02_

---

_Closed by @charliermarsh on 2024-01-04 14:02_

---

_Branch deleted on 2024-01-04 14:02_

---
