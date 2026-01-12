```yaml
number: 5323
title: "Allow symlinks with `--find-links`"
type: pull_request
state: merged
author: zackelia
labels:
  - compatibility
assignees: []
merged: true
base: main
head: find-links-symlinks
created_at: 2024-07-23T01:11:53Z
updated_at: 2024-07-23T08:34:16Z
url: https://github.com/astral-sh/uv/pull/5323
synced_at: 2026-01-12T16:06:45Z
```

# Allow symlinks with `--find-links`

---

_@zackelia_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

In my setup, I have a directory of wheels symlinked from different directories. I can point `--find-links` at it with `pip` and it works but not `uv`.

Currently, `uv` checks if a candidate file `is_file` which is for regular files. By also checking `is_symlink` I was able to install a symlinked wheel. I'm not *exactly* sure where, but some other place is eventually resolving the absolute path of the wheel. (`uv`? The OS?)

## Test Plan

<!-- How was it tested? -->
Manually tested - I didn't see any tests for `FlatIndexClient` in the `uv-client` crate.

```
mkdir /tmp/a /tmp/b                               # Create a directory of wheels (/tmp/a) and a directory of symlinked wheels (/tmp/b)
cp test-0.0.1-py3-none-any.whl /tmp/a             # Add a wheel to the directory of wheels
ln -s /tmp/a/test-0.0.1-py3-none-any.whl /tmp/b/  # Create a symlink to that wheel
uv pip install test --find-links /tmp/b           # Install pointing at the symlinked wheels directory
```

---

_@charliermarsh approved on 2024-07-23 02:44_

Thanks!

---

_@charliermarsh reviewed on 2024-07-23 02:45_

---

_Review comment by @charliermarsh on `crates/uv-client/src/flat_index.rs`:247 on 2024-07-23 02:45_

I tweaked this such that we still skip symlinked _directories_.

---

_Label `compatibility` added by @charliermarsh on 2024-07-23 02:45_

---

_Merged by @charliermarsh on 2024-07-23 02:53_

---

_Closed by @charliermarsh on 2024-07-23 02:53_

---

_Branch deleted on 2024-07-23 08:34_

---
