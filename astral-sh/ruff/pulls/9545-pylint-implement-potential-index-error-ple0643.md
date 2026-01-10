```yaml
number: 9545
title: "[`pylint`] Implement `potential-index-error` (`PLE0643`)"
type: pull_request
state: merged
author: diceroll123
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: add-PLE0643
created_at: 2024-01-16T07:56:38Z
updated_at: 2024-01-21T04:07:04Z
url: https://github.com/astral-sh/ruff/pull/9545
synced_at: 2026-01-10T22:57:09Z
```

# [`pylint`] Implement `potential-index-error` (`PLE0643`)

---

_Pull request opened by @diceroll123 on 2024-01-16 07:56_

## Summary

add `potential-index-error` rule (`PLE0643`)

See: #970 

## Test Plan

`cargo test`

---

_Comment by @github-actions[bot] on 2024-01-16 08:09_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **encountered linter errors**. (no lint changes; 1 project error)

<details><summary><a href="https://github.com/sphinx-doc/sphinx">sphinx-doc/sphinx</a> (error)</summary>
<p>

```
ruff failed
  Cause: Required version `0.1.13` does not match the running version `0.1.14`
```

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **encountered linter errors**. (no lint changes; 1 project error)

<details><summary><a href="https://github.com/sphinx-doc/sphinx">sphinx-doc/sphinx</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

```
ruff failed
  Cause: Required version `0.1.13` does not match the running version `0.1.14`
```

</p>
</details>




---

_Comment by @Skylion007 on 2024-01-18 15:36_

It's quite common to deal with int64 array indices in machine learning and big data applications so I would recommend allow 64bit indices. Seems like the speed difference wouldn't be worth improperly handling these edge cases?

---

_Comment by @diceroll123 on 2024-01-19 04:54_

I'd love to see a way for arbitrary integer comparisons here, but I don't think we'll be able to do that without adding dependencies. I'd love to be proven wrong, but I'd also argue that 64 bits is beyond plenty in this scenario.

---

_@charliermarsh approved on 2024-01-21 03:54_

Thanks!

---

_Label `rule` added by @charliermarsh on 2024-01-21 03:54_

---

_Label `preview` added by @charliermarsh on 2024-01-21 03:54_

---

_Renamed from "[`pylint`] - add `potential-index-error` rule (`PLE0643`)" to "[`pylint`] Implement `potential-index-error` (`PLE0643`)" by @charliermarsh on 2024-01-21 03:54_

---

_Comment by @diceroll123 on 2024-01-21 03:56_

Great tweaks!

---

_Merged by @charliermarsh on 2024-01-21 03:59_

---

_Closed by @charliermarsh on 2024-01-21 03:59_

---
