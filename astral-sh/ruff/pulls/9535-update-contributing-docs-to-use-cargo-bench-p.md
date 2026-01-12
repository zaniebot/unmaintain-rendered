```yaml
number: 9535
title: "Update contributing docs to use `cargo bench -p ruff_benchmark`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - documentation
assignees: []
merged: true
base: main
head: charlie/bench-docs
created_at: 2024-01-15T18:45:40Z
updated_at: 2024-01-15T19:57:31Z
url: https://github.com/astral-sh/ruff/pull/9535
synced_at: 2026-01-12T15:55:29Z
```

# Update contributing docs to use `cargo bench -p ruff_benchmark`

---

_@charliermarsh_

## Summary

I found that `cargo benchmark lexer` didn't work as expected:

```shell
❯ cargo benchmark lexer
    Finished bench [optimized] target(s) in 0.08s
     Running benches/formatter.rs (target/release/deps/formatter-4e1d9bf9d3ba529d)
     Running benches/linter.rs (target/release/deps/linter-e449086ddfd8ad8c)
```

Turns out that `cargo bench -p ruff_benchmark` is now recommended over `cargo benchmark`, so updating the docs to reflect that.

---

_Label `documentation` added by @charliermarsh on 2024-01-15 18:45_

---

_Review requested from @MichaReiser by @charliermarsh on 2024-01-15 18:45_

---

_@MichaReiser approved on 2024-01-15 19:55_

---

_Merged by @charliermarsh on 2024-01-15 19:57_

---

_Closed by @charliermarsh on 2024-01-15 19:57_

---

_Branch deleted on 2024-01-15 19:57_

---
