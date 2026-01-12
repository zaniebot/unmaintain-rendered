```yaml
number: 8476
title: Add pyupgrade UP041 to replace TimeoutError aliases
type: pull_request
state: merged
author: hugovk
labels:
  - rule
assignees: []
merged: true
base: main
head: pyupgrade-timeouterror
created_at: 2023-11-03T17:00:10Z
updated_at: 2023-11-03T17:35:06Z
url: https://github.com/astral-sh/ruff/pull/8476
synced_at: 2026-01-10T23:40:55Z
```

# Add pyupgrade UP041 to replace TimeoutError aliases

---

_Pull request opened by @hugovk on 2023-11-03 17:00_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Add UP041 to replace `TimeoutError` aliases:

* Python 3.10+: `socket.timeout`
* Python 3.11+: `asyncio.TimeoutError`

Re:

* https://github.com/asottile/pyupgrade#timeouterror-aliases
* https://docs.python.org/3/library/asyncio-exceptions.html#asyncio.TimeoutError
* https://docs.python.org/3/library/socket.html#socket.timeout

Based on `os_error_alias.rs`.

## Test Plan

<!-- How was it tested? -->

By running:

```
cargo clippy --workspace --all-targets --all-features -- -D warnings  # Rust linting
RUFF_UPDATE_SCHEMA=1 cargo test  # Rust testing and updating ruff.schema.json
pre-commit run --all-files --show-diff-on-failure  # Rust and Python formatting, Markdown and Python linting, etc.
cargo insta review
```

And also running with different `--target-version` values:

```sh
cargo run -p ruff_cli -- check crates/ruff_linter/resources/test/fixtures/pyupgrade/UP041.py --no-cache --select UP041 --target-version py37 --diff
cargo run -p ruff_cli -- check crates/ruff_linter/resources/test/fixtures/pyupgrade/UP041.py --no-cache --select UP041 --target-version py310 --diff
cargo run -p ruff_cli -- check crates/ruff_linter/resources/test/fixtures/pyupgrade/UP041.py --no-cache --select UP041 --target-version py311 --diff
```


---

_Label `rule` added by @charliermarsh on 2023-11-03 17:11_

---

_Comment by @github-actions[bot] on 2023-11-03 17:17_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @charliermarsh on 2023-11-03 17:19_

Thanks @hugovk, this looks great.

---

_Comment by @charliermarsh on 2023-11-03 17:20_

I moved the rule to [preview](https://docs.astral.sh/ruff/preview/) as per our versioning policy -- it will be upgraded to stable in our next minor release.

---

_@charliermarsh approved on 2023-11-03 17:20_

---

_Merged by @charliermarsh on 2023-11-03 17:24_

---

_Closed by @charliermarsh on 2023-11-03 17:24_

---

_Branch deleted on 2023-11-03 17:35_

---
