```yaml
number: 17202
title: Added config file settings validation for preview features
type: pull_request
state: open
author: j-helland
labels: []
assignees: []
base: main
head: jwh/config-file-validation
created_at: 2025-12-21T02:02:38Z
updated_at: 2025-12-23T21:32:20Z
url: https://github.com/astral-sh/uv/pull/17202
synced_at: 2026-01-10T05:49:14Z
```

# Added config file settings validation for preview features

---

_Pull request opened by @j-helland on 2025-12-21 02:02_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

Stacked on #16452

## Summary

> [!NOTE]
> **For reviewers:** see the last commit

Per discussion https://github.com/astral-sh/uv/pull/16452#issuecomment-3677363094, it was determined that we need to have additional config validation for the `preview` and `preview-features` fields.

This is a sketch of how we might approach that via a `Validator` trait implemented for `Options` and `GlobalOptions`.

## Test Plan

New and existing automated tests.

## Example

```shell
# Setup
> mkdir example && cd example
> cargo run -- init
> echo '\n[tool.uv]\npreview = false\npreview-features = ["pylock"]' >> pyproject.toml
> tail -n3 pyproject.toml
[tool.uv]
preview = false
preview-features = ["pylock"]

# New behavior
> cargo run -- format
error: Failed to parse: `pyproject.toml`. Cannot specify both `preview` and `preview-features`.
```


---

_Review comment by @j-helland on `crates/uv-settings/src/validation.rs`:9 on 2025-12-21 02:05_

This trait might be overkill. If we think that there will be lots of additional validation rules in the future, something like this makes sense. If not, simplifying to something like [validate_uv_toml](https://github.com/j-helland/uv/blob/main/crates/uv-settings/src/lib.rs#L205) would be better.

YAGNI?

---

_Review comment by @j-helland on `crates/uv-settings/src/lib.rs`:53 on 2025-12-21 02:05_

I didn't consolidate `validate_uv_toml` into the validator trait impl because it actually doesn't get called in the `pyproject.toml` branch and tests would fail if it was called there.

---

_@j-helland reviewed on 2025-12-21 02:05_

---

_Marked ready for review by @j-helland on 2025-12-21 02:09_

---
