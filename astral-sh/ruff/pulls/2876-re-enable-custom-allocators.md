```yaml
number: 2876
title: Re-enable custom allocators
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/allocator
created_at: 2023-02-14T00:20:42Z
updated_at: 2023-02-15T13:13:41Z
url: https://github.com/astral-sh/ruff/pull/2876
synced_at: 2026-01-12T15:55:11Z
```

# Re-enable custom allocators

---

_@charliermarsh_

This PR un-reverts #2768 with extra guards to ensure that (e.g.) `i686` builds work as expected.

---

_Merged by @charliermarsh on 2023-02-14 02:37_

---

_Closed by @charliermarsh on 2023-02-14 02:37_

---

_Branch deleted on 2023-02-14 02:37_

---

_Comment by @MichaReiser on 2023-02-15 11:00_

Hi @charliermarsh, what was your reasoning behind re-enabling this PR after rolling it back? Did you only roll it back? Was the main reason for rolling it back that it didn't build correctly on i686?

---

_Comment by @charliermarsh on 2023-02-15 13:13_

@MichaReiser - Yeah the motivation for the revert was to figure out how to get builds passing on those other architectures, so it felt appropriate to re-enable once I'd added these extra conditionals. I didn't do any further investigation into memory usage etc. but I'll admit I was buoyed by some of your other analyses :)

---
