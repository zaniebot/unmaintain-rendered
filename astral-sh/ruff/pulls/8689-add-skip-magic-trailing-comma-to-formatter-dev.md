```yaml
number: 8689
title: "Add `--skip-magic-trailing-comma` to formatter dev comment"
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: main
head: skip_magic_trailing_comma-in-dev-command
created_at: 2023-11-15T09:19:01Z
updated_at: 2023-11-16T15:00:11Z
url: https://github.com/astral-sh/ruff/pull/8689
synced_at: 2026-01-10T23:40:55Z
```

# Add `--skip-magic-trailing-comma` to formatter dev comment

---

_Pull request opened by @konstin on 2023-11-15 09:19_

Testing the compatibility with the future stable black style, i realized the `ruff_python_formatter` dev main was lacking the `--skip-magic-trailing-comma` option. This does not affect `ruff format`.

Usage:
```shell
cargo run --bin ruff_python_formatter -p ruff_python_formatter -- --skip-magic-trailing-comma --emit stdout scratch.py
```

---

_Marked ready for review by @konstin on 2023-11-15 09:19_

---

_Merged by @konstin on 2023-11-15 09:23_

---

_Closed by @konstin on 2023-11-15 09:23_

---

_Branch deleted on 2023-11-15 09:23_

---

_Comment by @github-actions[bot] on 2023-11-15 09:29_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Comment by @Apreche on 2023-11-15 16:21_

Right now whenever I run `ruff format` with version 0.1.5, it's behaving as if `skip_magic_trailing_comma = true` even though the default is supposed to be false. It's really annoying to put in a bunch of newlines and then have them blown away to make one long line.

Does this fix that issue?

---

_Comment by @charliermarsh on 2023-11-15 16:22_

@Apreche - This is just an internal development command for Ruff so it shouldn't affect your production setup. Can you give an example of code that's being reformatted?

---

_Comment by @Apreche on 2023-11-16 15:00_

I investigated, and it was my mistake. No issues. Sorry about that.

---
