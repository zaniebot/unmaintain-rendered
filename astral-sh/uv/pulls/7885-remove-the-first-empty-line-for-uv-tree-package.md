```yaml
number: 7885
title: "Remove the first empty line for `uv tree --package foo`"
type: pull_request
state: merged
author: j178
labels: []
assignees: []
merged: true
base: main
head: empty-line
created_at: 2024-10-03T06:17:32Z
updated_at: 2024-10-03T13:14:45Z
url: https://github.com/astral-sh/uv/pull/7885
synced_at: 2026-01-10T12:53:58Z
```

# Remove the first empty line for `uv tree --package foo`

---

_Pull request opened by @j178 on 2024-10-03 06:17_

## Summary

When using `uv tree --package foo`, an extra empty line appears at the beginning, which seems unnecessary since `uv tree` without the package option doesn’t have this. It’s possible that the intention was to add separation between packages, i.e. the correct implementation shoule be:

```rust
if !std::mem::take(&mut first) {
    lines.push(String::new());
}
```

Even if corrected, this extra spacing might be redundant as `uv tree` doesn’t include these empty lines between packages by default.

```console
$ uv init project
$ cd project
$ uv init foo
$ uv tree
Using CPython 3.12.5
Resolved 2 packages in 1ms
foo v0.1.0
project v0.1.0

$ uv tree --package project
Using CPython 3.12.5
Resolved 2 packages in 1ms

project v0.1.0
```


---

_Merged by @charliermarsh on 2024-10-03 12:04_

---

_Closed by @charliermarsh on 2024-10-03 12:04_

---

_Comment by @charliermarsh on 2024-10-03 12:04_

Good catch. Seems fine to just remove it.

---

_Branch deleted on 2024-10-03 13:14_

---
