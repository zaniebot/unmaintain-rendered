```yaml
number: 3921
title: Use lockfile versions as resolution preferences
type: pull_request
state: merged
author: charliermarsh
labels:
  - preview
assignees: []
merged: true
base: main
head: charlie/preferences
created_at: 2024-05-30T01:46:32Z
updated_at: 2024-05-30T17:59:54Z
url: https://github.com/astral-sh/uv/pull/3921
synced_at: 2026-01-12T16:05:55Z
```

# Use lockfile versions as resolution preferences

---

_@charliermarsh_

## Summary

Ensures that we avoid upgrading packages unless `--upgrade` or similar is passed.

For now, the resolver only respects these for registry distributions.

Closes https://github.com/astral-sh/uv/issues/3918.


---

_Review requested from @BurntSushi by @charliermarsh on 2024-05-30 01:46_

---

_Marked ready for review by @charliermarsh on 2024-05-30 01:46_

---

_Label `preview` added by @charliermarsh on 2024-05-30 01:46_

---

_@charliermarsh reviewed on 2024-05-30 01:47_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/lock.rs`:56 on 2024-05-30 01:47_

@BurntSushi - Let me know if you have a preferred way to do this (other than what's here). I expose `Distribution`, but none of the fields or methods are `pub`.

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/lock.rs`:56 on 2024-05-30 13:49_

I think this looks fine.

The thing I care about _most_ is not exposing representation. I don't mean to say we shouldn't do it, but that's where the biggest benefit is. If we don't expose representation, then it makes it much easier to change that representation in the future. (It's all a trade off.)

---

_@BurntSushi approved on 2024-05-30 14:39_

This is awesome!

---

_Comment by @codspeed-hq[bot] on 2024-05-30 17:56_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/uv/branches/charlie/preferences)

### Merging #3921 will **degrade performances by 6.17%**

<sub>Comparing <code>charlie/preferences</code> (d7f803b) with <code>main</code> (502e042)</sub>



### Summary

`⚡ 1` improvements
`❌ 1` regressions
`✅ 11` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/uv/branches/charlie/preferences)._

### Benchmarks breakdown

|     | Benchmark | `main` | `charlie/preferences` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `resolve_warm_airflow` | 1.5 s | 1.4 s | +7.48% |
| ❌ | `resolve_warm_jupyter` | 80.2 ms | 85.5 ms | -6.17% |


---

_Merged by @charliermarsh on 2024-05-30 17:59_

---

_Closed by @charliermarsh on 2024-05-30 17:59_

---

_Branch deleted on 2024-05-30 17:59_

---
