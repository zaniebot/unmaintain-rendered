```yaml
number: 9032
title: "Make `check-url` available in configuration files"
type: pull_request
state: merged
author: bruckner
labels:
  - configuration
assignees: []
merged: true
base: main
head: check-url-setting
created_at: 2024-11-11T21:15:16Z
updated_at: 2024-12-03T14:19:40Z
url: https://github.com/astral-sh/uv/pull/9032
synced_at: 2026-01-10T12:00:00Z
```

# Make `check-url` available in configuration files

---

_Pull request opened by @bruckner on 2024-11-11 21:15_

## Summary

Fixes #9027

Minor enhancement on top of #8531 that makes the CLI parameter `--check-url` also available as the setting `check-url` in configuration files.

## Test Plan

Updates existing tests to take the new setting into account.

Within publish command testing I didn't see existing tests covering settings from toml files (instead of from CLI params), so I didn't add anything of that sort.

---

_Comment by @zanieb on 2024-11-11 21:44_

I'm a little hesitant to add this given we have a better path forward, it just creates "another way" to configure things. Curious to hear from @konstin though.

(Thanks for opening the pull request though!)

---

_@zanieb approved on 2024-12-02 23:30_

---

_Merged by @zanieb on 2024-12-02 23:30_

---

_Closed by @zanieb on 2024-12-02 23:30_

---

_Label `configuration` added by @zanieb on 2024-12-02 23:30_

---

_Branch deleted on 2024-12-03 14:19_

---
