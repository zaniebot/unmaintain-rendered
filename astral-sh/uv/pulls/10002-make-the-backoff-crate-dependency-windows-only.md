```yaml
number: 10002
title: Make the backoff crate dependency Windows-only
type: pull_request
state: merged
author: musicinmybrain
labels:
  - internal
assignees: []
merged: true
base: main
head: backoff-windows-only
created_at: 2024-12-18T14:52:11Z
updated_at: 2024-12-18T15:05:07Z
url: https://github.com/astral-sh/uv/pull/10002
synced_at: 2026-01-12T16:09:04Z
```

# Make the backoff crate dependency Windows-only

---

_@musicinmybrain_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Since the `backoff` dependency is only *used* on Windows in practice, this PR would ensure that it is only *compiled* on Windows, too. This is helpful because it appears to be unmaintained upstream, https://github.com/astral-sh/uv/issues/10001, and it would be nice to be able to [drop it from Fedora](https://bugzilla.redhat.com/show_bug.cgi?id=2329729).
<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

<!-- How was it tested? -->
```
$ cargo run python install
$ cargo test
```

---

_Review requested from @charliermarsh by @konstin on 2024-12-18 15:02_

---

_@charliermarsh approved on 2024-12-18 15:04_

---

_Merged by @charliermarsh on 2024-12-18 15:04_

---

_Closed by @charliermarsh on 2024-12-18 15:04_

---

_Label `internal` added by @charliermarsh on 2024-12-18 15:05_

---

_Comment by @charliermarsh on 2024-12-18 15:05_

Thanks, this makes sense.

---
