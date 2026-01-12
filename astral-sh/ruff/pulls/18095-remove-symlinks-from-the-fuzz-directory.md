```yaml
number: 18095
title: Remove symlinks from the fuzz directory
type: pull_request
state: merged
author: dhruvmanila
labels:
  - internal
assignees: []
merged: true
base: main
head: dhruv/fuzz-symlinks
created_at: 2025-05-14T15:17:35Z
updated_at: 2025-05-14T15:35:54Z
url: https://github.com/astral-sh/ruff/pull/18095
synced_at: 2026-01-12T15:56:12Z
```

# Remove symlinks from the fuzz directory

---

_@dhruvmanila_

## Summary

This PR does the following:
1. Remove the symlinks from the `fuzz/` directory
2. Update `init-fuzzer.sh` script to create those symlinks
3. Update `fuzz/.gitignore` to ignore those corpus directories

## Test Plan

Initialize the fuzzer:

```sh
./fuzz/init-fuzzer.sh
```

And, run a fuzz target:

```sh
cargo +nightly fuzz run ruff_parse_simple -- -timeout=1 -only_ascii=1
```


---

_Label `internal` added by @dhruvmanila on 2025-05-14 15:17_

---

_Review requested from @MichaReiser by @dhruvmanila on 2025-05-14 15:18_

---

_@MichaReiser approved on 2025-05-14 15:19_

Thank you

---

_Merged by @dhruvmanila on 2025-05-14 15:35_

---

_Closed by @dhruvmanila on 2025-05-14 15:35_

---

_Branch deleted on 2025-05-14 15:35_

---
