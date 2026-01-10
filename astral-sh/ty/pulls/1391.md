```yaml
number: 1391
title: Only add the actual schema in schemastore PRs
type: pull_request
state: merged
author: sharkdp
labels:
  - release
assignees: []
merged: true
base: main
head: david/schemastore-update
created_at: 2025-10-17T18:58:45Z
updated_at: 2025-10-17T19:01:55Z
url: https://github.com/astral-sh/ty/pull/1391
synced_at: 2026-01-10T02:34:10Z
```

# Only add the actual schema in schemastore PRs

---

_Pull request opened by @sharkdp on 2025-10-17 18:58_

Last time I ran this script, due to what I assume was a `npm` version mismatch, the `package-lock.json` file was updated while running `npm install` in the `schemastore`. Due to the use of `git commit -a`, it was accidentally included in the commit for the semi-automated schemastore PR. The solution here is to only add the actual file that we want to commit.

I'll open similar PRs for the respective scripts in ruff and uv once this is accepted.

---

_Label `release` added by @sharkdp on 2025-10-17 18:58_

---

_@AlexWaygood approved on 2025-10-17 19:01_

---

_Merged by @sharkdp on 2025-10-17 19:01_

---

_Closed by @sharkdp on 2025-10-17 19:01_

---

_Branch deleted on 2025-10-17 19:01_

---
