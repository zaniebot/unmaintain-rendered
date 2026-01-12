```yaml
number: 16346
title: Only add actual schema in schemastore PRs
type: pull_request
state: merged
author: sharkdp
labels:
  - internal
  - releases
assignees: []
merged: true
base: main
head: david/schemastore-precise-update
created_at: 2025-10-17T19:24:14Z
updated_at: 2025-10-17T19:35:54Z
url: https://github.com/astral-sh/uv/pull/16346
synced_at: 2026-01-12T16:12:13Z
```

# Only add actual schema in schemastore PRs

---

_@sharkdp_

## Summary

Last time I ran the `update_schemastore.py` script in ty, due to what I assume was a `npm` version mismatch, the `package-lock.json` file was updated while running `npm install` in the `schemastore`. Due to the use of `git commit -a`, it was accidentally included in the commit for the semi-automated schemastore PR. The solution here is to only add the actual file that we want to commit.

Same as https://github.com/astral-sh/ty/pull/1391

## Test Plan

I did a dry-run of this script (by commenting out the final `push`) and verified that the commit did include the schema, but not the updated `package-lock.json` file.

---

_Label `internal` added by @sharkdp on 2025-10-17 19:24_

---

_Label `releases` added by @sharkdp on 2025-10-17 19:24_

---

_@zanieb approved on 2025-10-17 19:33_

Thank you!

---

_Merged by @zanieb on 2025-10-17 19:35_

---

_Closed by @zanieb on 2025-10-17 19:35_

---

_Branch deleted on 2025-10-17 19:35_

---
