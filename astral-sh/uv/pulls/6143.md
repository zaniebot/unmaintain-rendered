```yaml
number: 6143
title: "fork every time we see a non-trivial marker (\"aggressive\" forking)"
type: pull_request
state: closed
author: BurntSushi
labels: []
assignees: []
draft: true
base: main
head: ag/aggressive-forking
created_at: 2024-08-16T12:14:01Z
updated_at: 2025-07-22T11:34:08Z
url: https://github.com/astral-sh/uv/pull/6143
synced_at: 2026-01-10T06:53:01Z
```

# fork every time we see a non-trivial marker ("aggressive" forking)

---

_Pull request opened by @BurntSushi on 2024-08-16 12:14_

This PR is a revival of #5733 on top of current `main`, as many things
have changed since #5733.

It is suspected that, at least at this time, we probably don't
want to switch to aggressive forking, but we may in the future. In
particular, aggressive forking does fix #4668 and #6127.
Moreover, it fixes some issues that aren't filed but for which we only
have hand-crafted artificial examples (see the `non-local-*` packse
scenarios).

There are two main downsides to aggressive forking:

1. It, well, creates a lot more forks. This leads to slower resolutions
overall, sometimes dramatically so.
2. It changes the resolutions/markers in ways we don't yet fully
understand. Part of the point of this PR is to be able to look at the
snapshot diffs to see the kinds of effects it has. In some cases, the
changes are quite large and can be difficult to comprehend and reason
about.

We haven't spent a ton of time on (1), but it is suspected that in the
aggressive strategy, there should still be a way to prune superfluous
forks.

---

_Converted to draft by @BurntSushi on 2024-08-16 12:14_

---

_Comment by @codspeed-hq[bot] on 2024-08-16 12:27_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/uv/branches/ag/aggressive-forking)

### Merging #6143 will **degrade performances by 83.07%**

<sub>Comparing <code>ag/aggressive-forking</code> (d4fd44a) with <code>main</code> (89efe24)</sub>



### Summary

`❌ 1` regressions
`✅ 13` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/uv/branches/ag/aggressive-forking)._

### Benchmarks breakdown

|     | Benchmark | `main` | `ag/aggressive-forking` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `resolve_warm_jupyter_universal` | 321.6 ms | 1,899.6 ms | -83.07% |


---

_Comment by @zanieb on 2025-07-22 11:34_

I'm going to close this as it's quite stale now.

---

_Closed by @zanieb on 2025-07-22 11:34_

---
