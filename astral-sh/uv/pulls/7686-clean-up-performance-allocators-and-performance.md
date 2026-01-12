```yaml
number: 7686
title: "Clean up \"performance allocators\" and \"performance flate2\" backends"
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: main
head: konsti/fewer-deps-rebase
created_at: 2024-09-25T14:29:51Z
updated_at: 2024-09-25T15:41:41Z
url: https://github.com/astral-sh/uv/pull/7686
synced_at: 2026-01-12T16:07:56Z
```

# Clean up "performance allocators" and "performance flate2" backends

---

_@konstin_

Rebased version of #7000, description copied below.

Closes #7000

---

## Summary

@charliermarsh has long suspected local builds could be made faster by disabling things like: tikv-jemalloc/mimalloc, zlibng etc.

I'm going through the cargo dep tree looking at things that can be disabled locally.

## Methodology:

`production` cargo flag enables all the production stuff (good allocators, fast compression libs, etc.)

I measure fresh `cargo check` runs, like so:

```shell
rm -rf /tmp/timings; CARGO_TARGET_DIR=/tmp/timings cargo check -F production-memory-allocator --timings
```

Varying the `-F` to enable/disable the features

## FAQ

Q: Why only check `check`?

A: The benefits will trickle down to other subcommands (including test/nextest etc.) — check/clippy are super common while iterating. We can do larger checks near the end.

Q: Why only check cold builds?

A: Warm builds depend on a lot on which part of the code is touched — I'll optimize typical interactions later on.

---

_Review requested from @BurntSushi by @konstin on 2024-09-25 14:29_

---

_@BurntSushi approved on 2024-09-25 14:33_

---

_Comment by @fasterthanlime on 2024-09-25 15:31_

Windows CI is being flaky again :(

---

_Merged by @konstin on 2024-09-25 15:41_

---

_Closed by @konstin on 2024-09-25 15:41_

---

_Branch deleted on 2024-09-25 15:41_

---
