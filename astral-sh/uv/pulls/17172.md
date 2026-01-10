```yaml
number: 17172
title: Fix --color flag not working with --help
type: pull_request
state: open
author: okhsunrog
labels:
  - bug
assignees: []
base: main
head: fix-color-help
created_at: 2025-12-18T11:46:53Z
updated_at: 2025-12-18T13:03:16Z
url: https://github.com/astral-sh/uv/pull/17172
synced_at: 2026-01-10T05:49:14Z
```

# Fix --color flag not working with --help

---

_Pull request opened by @okhsunrog on 2025-12-18 11:46_

The` --color=always` flag was not being honored when used with `--help` because clap exits immediately on` --help` before the global color choice was set via `anstream::ColorChoice::write_global()`.

## Summary

This adds a `preparse_color_from_args()` function that scans CLI args for `--color=VALUE`, `--color VALUE`, and `--no-color` before clap parsing, setting the global color choice early so it takes effect for help output.

## Tests


  | Test                  | Result                     |
  |-----------------------|----------------------------|
  | --color=always \| pipe |  Colors preserved         |
  | --color=never in TTY  | Colors stripped          |
  | --no-color            | Color stripped|



---

_Review requested from @zanieb by @konstin on 2025-12-18 12:29_

---

_Label `bug` added by @konstin on 2025-12-18 12:30_

---

_Comment by @zanieb on 2025-12-18 13:03_

related

- https://github.com/astral-sh/uv/pull/7223

---
