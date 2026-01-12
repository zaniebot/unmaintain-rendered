```yaml
number: 3956
title: "Respect resolved Git SHAs in `uv lock`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
  - preview
assignees: []
merged: true
base: main
head: charlie/git-lock
created_at: 2024-06-01T12:21:53Z
updated_at: 2024-06-01T12:40:13Z
url: https://github.com/astral-sh/uv/pull/3956
synced_at: 2026-01-12T16:05:56Z
```

# Respect resolved Git SHAs in `uv lock`

---

_@charliermarsh_

## Summary

This PR ensures that if a lockfile already contains a resolved reference (e.g., you locked with `main` previously, and it locked to a specific commit), and you run `uv lock`, we use the same SHA, even if it's not the latest SHA for that tag. This avoids upgrading Git dependencies without `--upgrade`.

Closes #3920.


---

_Label `enhancement` added by @charliermarsh on 2024-06-01 12:22_

---

_Label `preview` added by @charliermarsh on 2024-06-01 12:22_

---

_Marked ready for review by @charliermarsh on 2024-06-01 12:22_

---

_Comment by @codspeed-hq[bot] on 2024-06-01 12:39_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/uv/branches/charlie/git-lock)

### Merging #3956 will **improve performances by 6.27%**

<sub>Comparing <code>charlie/git-lock</code> (bf529c1) with <code>main</code> (b7d77c0)</sub>



### Summary

`⚡ 1` improvements
`✅ 12` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `charlie/git-lock` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `resolve_warm_jupyter` | 92.8 ms | 87.3 ms | +6.27% |


---

_Merged by @charliermarsh on 2024-06-01 12:40_

---

_Closed by @charliermarsh on 2024-06-01 12:40_

---

_Branch deleted on 2024-06-01 12:40_

---
