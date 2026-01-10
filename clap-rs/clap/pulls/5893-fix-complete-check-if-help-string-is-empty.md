---
number: 5893
title: "fix(complete): Check if help string is empty"
type: pull_request
state: merged
author: 8LWXpg
labels: []
assignees: []
merged: true
base: master
head: patch-2
created_at: 2025-01-25T13:15:17Z
updated_at: 2025-01-27T15:23:27Z
url: https://github.com/clap-rs/clap/pull/5893
synced_at: 2026-01-10T01:28:25Z
---

# fix(complete): Check if help string is empty

---

_Pull request opened by @8LWXpg on 2025-01-25 13:15_

fix #5341

Fallback to `data.to_string()` as well if help strig is empty.

---
