---
number: 6180
title: "feat: allow capturing closures as arguments to Command::defer"
type: pull_request
state: closed
author: calebkiage
labels: []
assignees: []
draft: true
base: master
head: u/calebkiage/defer-closures
created_at: 2025-11-08T19:11:21Z
updated_at: 2025-11-16T16:00:29Z
url: https://github.com/clap-rs/clap/pull/6180
synced_at: 2026-01-10T01:28:29Z
---

# feat: allow capturing closures as arguments to Command::defer

---

_Pull request opened by @calebkiage on 2025-11-08 19:11_

When using the defer feature, it's possible to want capturing closures as arguments to the function.

Change the behavior of defer when using multiple calls to chain instead of replacing the closure.

closes #6179

---
