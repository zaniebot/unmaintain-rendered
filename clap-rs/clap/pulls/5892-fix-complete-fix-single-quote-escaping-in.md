```yaml
number: 5892
title: "fix(complete): Fix single quote escaping in PowerShell"
type: pull_request
state: merged
author: 8LWXpg
labels: []
assignees: []
merged: true
base: master
head: patch-1
created_at: 2025-01-25T10:20:42Z
updated_at: 2025-01-27T15:22:06Z
url: https://github.com/clap-rs/clap/pull/5892
synced_at: 2026-01-12T16:14:17Z
```

# fix(complete): Fix single quote escaping in PowerShell

---

_@8LWXpg_

fix #5820

PowerShell treats `’` as regular single quote, so it needs to be escaped with single quote as well.

---
