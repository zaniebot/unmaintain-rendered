```yaml
number: 869
title: Commit compiled requirement test files
type: pull_request
state: merged
author: bojanserafimov
labels: []
assignees: []
merged: true
base: main
head: bojan/compiled-reqs
created_at: 2024-01-10T14:18:13Z
updated_at: 2024-01-10T14:24:18Z
url: https://github.com/astral-sh/uv/pull/869
synced_at: 2026-01-12T16:04:15Z
```

# Commit compiled requirement test files

---

_@bojanserafimov_

These are useful for benchmarking `pip-sync`. Committing them in the repo makes results stable over time, and makes it easier to talk about perf experiments (you can say "I got this result with input compiled/all-kinds.txt", etc. and people will know what you mean)

Some of them failed compilation (maybe they're tests that are supposed to fail) and some failed install (maybe I'm missing system dependencies). If I was here for longer I'd investigate but for now just testing the ones that work

---

_Review requested from @konstin by @bojanserafimov on 2024-01-10 14:18_

---

_@konstin approved on 2024-01-10 14:18_

---

_Merged by @bojanserafimov on 2024-01-10 14:24_

---

_Closed by @bojanserafimov on 2024-01-10 14:24_

---

_Branch deleted on 2024-01-10 14:24_

---
