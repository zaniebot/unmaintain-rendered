```yaml
number: 5015
title: "Fix `CONTRIBUTING.md` instructions to install multiple Python versions"
type: pull_request
state: merged
author: silvanocerza
labels:
  - documentation
assignees: []
merged: true
base: main
head: fix-contributing-instructions
created_at: 2024-07-12T15:48:40Z
updated_at: 2024-07-13T08:47:15Z
url: https://github.com/astral-sh/uv/pull/5015
synced_at: 2026-01-10T13:42:52Z
```

# Fix `CONTRIBUTING.md` instructions to install multiple Python versions

---

_Pull request opened by @silvanocerza on 2024-07-12 15:48_

## Summary

I noticed the command to install multiple Python versions was wrong as it was failing cause `toolchain` is not a known command. 

I looked in the `ci.yml` workflow to see which command is used there and updated the instructions accordingly.

## Test Plan

I just ran the command locally. :)

---

_@zanieb approved on 2024-07-12 16:00_

Thank you! This was on my todo list :D

---

_Label `documentation` added by @zanieb on 2024-07-12 16:00_

---

_Merged by @zanieb on 2024-07-12 16:00_

---

_Closed by @zanieb on 2024-07-12 16:00_

---

_Branch deleted on 2024-07-13 08:47_

---
