```yaml
number: 4066
title: Simplify marker expressions in lockfile
type: pull_request
state: merged
author: ibraheemdev
labels: []
assignees: []
merged: true
base: main
head: simplify-markers
created_at: 2024-06-05T19:43:47Z
updated_at: 2024-06-07T20:14:24Z
url: https://github.com/astral-sh/uv/pull/4066
synced_at: 2026-01-12T16:06:01Z
```

# Simplify marker expressions in lockfile

---

_@ibraheemdev_

## Summary

Simplify and normalize marker expressions in the lockfile. Right now this does a simple analysis by only looking at related operators at the same level of precedence. I think anything more complex would be out of scope.

Resolves https://github.com/astral-sh/uv/issues/4002.

---

_Review requested from @BurntSushi by @ibraheemdev on 2024-06-05 19:44_

---

_Review requested from @charliermarsh by @ibraheemdev on 2024-06-05 19:44_

---

_@konstin reviewed on 2024-06-06 10:04_

---

_Review comment by @konstin on `crates/pep440-rs/src/version_specifier.rs`:422 on 2024-06-06 10:04_

This is perfect, i'm stealing it for https://github.com/astral-sh/uv/pull/4041

---

_@charliermarsh approved on 2024-06-07 00:29_

This looks reasonable to me.

---

_@BurntSushi approved on 2024-06-07 18:39_

Nice work!

---

_Merged by @ibraheemdev on 2024-06-07 20:14_

---

_Closed by @ibraheemdev on 2024-06-07 20:14_

---
