```yaml
number: 4440
title: "implement `--depth`, `--prune` for `pip tree`"
type: pull_request
state: merged
author: ChannyClaus
labels:
  - cli
assignees: []
merged: true
base: main
head: pip-tree-depth
created_at: 2024-06-21T20:52:26Z
updated_at: 2024-06-27T00:34:38Z
url: https://github.com/astral-sh/uv/pull/4440
synced_at: 2026-01-12T16:06:14Z
```

# implement `--depth`, `--prune` for `pip tree`

---

_@ChannyClaus_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary
Resolves https://github.com/astral-sh/uv/issues/4439 partially.

Implements for `uv pip tree`:
- `--depth` flag, similar to `cargo tree --depth` .
- `--prune` flag, similar to `cargo tree --prune` .

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan
The existing tests pass, added a test for:
- `--depth` varying the value of the `--depth` from `0` to `2`.
- `--prune` pruning a bunch of different packages in small / large nested dependency trees.
<!-- How was it tested? -->


---

_Renamed from "implement --depth for `pip tree`" to "implement `--depth` for `pip tree`" by @ChannyClaus on 2024-06-21 20:53_

---

_Renamed from "implement `--depth` for `pip tree`" to "implement `--depth`, `--prune` for `pip tree`" by @ChannyClaus on 2024-06-21 21:23_

---

_@zanieb reviewed on 2024-06-22 05:31_

---

_Review comment by @zanieb on `crates/uv/src/commands/pip/tree.rs`:166 on 2024-06-22 05:31_

I wonder if there's a significant cost for this cast here.

It seems like it'll happen a bunch of times and maybe you should just cast it once up-front?

---

_Review requested from @zanieb by @ChannyClaus on 2024-06-22 16:09_

---

_Review requested from @ibraheemdev by @zanieb on 2024-06-22 16:25_

---

_@zanieb approved on 2024-06-27 00:34_

---

_Merged by @zanieb on 2024-06-27 00:34_

---

_Closed by @zanieb on 2024-06-27 00:34_

---

_Label `cli` added by @zanieb on 2024-06-27 00:34_

---
