```yaml
number: 10240
title: "Add `ls` alias to `uv {tool, python, pip} list`"
type: pull_request
state: merged
author: unexge
labels:
  - enhancement
assignees: []
merged: true
base: main
head: ls-alias
created_at: 2024-12-30T14:08:16Z
updated_at: 2025-01-08T20:29:44Z
url: https://github.com/astral-sh/uv/pull/10240
synced_at: 2026-01-12T16:09:11Z
```

# Add `ls` alias to `uv {tool, python, pip} list`

---

_@unexge_

## Summary

This PR adds `ls` alias to `uv {tool, python, pip} list` for convenience. 

Not sure if folks previously discussed this or have any opinion on having aliases – but I have a muscle memory for `ls` for listing things in commands I'm using (like `docker images ls`, `zellij ls`, `helm ls` etc.) and thought having `ls` alias for `list` command would be useful. 

## Test Plan

I simply compiled `uv` and manually checked `./target/release/uv {tool, python, pip} ls`.

---

_@Gankra approved on 2025-01-08 19:08_

Nice, really appreciate the citations, thanks!

---

_Merged by @Gankra on 2025-01-08 19:09_

---

_Closed by @Gankra on 2025-01-08 19:09_

---

_Label `enhancement` added by @Gankra on 2025-01-08 19:11_

---

_Branch deleted on 2025-01-08 20:29_

---
