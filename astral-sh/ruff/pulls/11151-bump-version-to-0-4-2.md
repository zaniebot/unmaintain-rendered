```yaml
number: 11151
title: Bump version to 0.4.2
type: pull_request
state: merged
author: snowsignal
labels:
  - release
assignees: []
merged: true
base: main
head: release/042
created_at: 2024-04-25T17:09:49Z
updated_at: 2024-04-25T17:42:22Z
url: https://github.com/astral-sh/ruff/pull/11151
synced_at: 2026-01-10T22:37:01Z
```

# Bump version to 0.4.2

---

_Pull request opened by @snowsignal on 2024-04-25 17:09_

_No description provided._

---

_@charliermarsh reviewed on 2024-04-25 17:10_

---

_Review comment by @charliermarsh on `CHANGELOG.md`:38 on 2024-04-25 17:10_

Mind moving this to "Bug fixes"?

---

_Review comment by @charliermarsh on `CHANGELOG.md`:27 on 2024-04-25 17:10_

Lets remove this.

---

_@charliermarsh reviewed on 2024-04-25 17:10_

---

_@charliermarsh reviewed on 2024-04-25 17:11_

---

_Review comment by @charliermarsh on `CHANGELOG.md`:28 on 2024-04-25 17:11_

Lets remove this.

---

_@charliermarsh reviewed on 2024-04-25 17:11_

---

_Review comment by @charliermarsh on `CHANGELOG.md`:31 on 2024-04-25 17:11_

Missing closing backtick after isort.

---

_Label `release` added by @snowsignal on 2024-04-25 17:11_

---

_@charliermarsh reviewed on 2024-04-25 17:11_

---

_Review comment by @charliermarsh on `CHANGELOG.md`:36 on 2024-04-25 17:11_

Lets move this under a `### Performance` section.

---

_@charliermarsh reviewed on 2024-04-25 17:11_

---

_Review comment by @charliermarsh on `CHANGELOG.md`:36 on 2024-04-25 17:11_

We can move this to "Bug fixes" and add "to enable macOS 11 compatibility" to the end of the title.

---

_Review requested from @charliermarsh by @snowsignal on 2024-04-25 17:14_

---

_Comment by @codspeed-hq[bot] on 2024-04-25 17:21_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/release/042)

### Merging #11151 will **degrade performances by 6.27%**

<sub>Comparing <code>release/042</code> (846068f) with <code>main</code> (1c9f5e3)</sub>



### Summary

`❌ 2` regressions
`✅ 28` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/release/042)._

### Benchmarks breakdown

|     | Benchmark | `main` | `release/042` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `parser[numpy/ctypeslib.py]` | 5.3 ms | 5.6 ms | -6.27% |
| ❌ | `parser[pydantic/types.py]` | 11.4 ms | 11.9 ms | -4.56% |


---

_@charliermarsh reviewed on 2024-04-25 17:22_

---

_Review comment by @charliermarsh on `CHANGELOG.md`:1487 on 2024-04-25 17:22_

I think this should be changed back though I know it's annoying.

---

_@charliermarsh approved on 2024-04-25 17:22_

---

_Merged by @snowsignal on 2024-04-25 17:31_

---

_Closed by @snowsignal on 2024-04-25 17:31_

---

_Branch deleted on 2024-04-25 17:31_

---

_Comment by @github-actions[bot] on 2024-04-25 17:42_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---
