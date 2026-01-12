```yaml
number: 6077
title: "Remove `same-graph` merging in resolver"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - resolver
  - preview
assignees: []
merged: true
base: main
head: charlie/same-graph
created_at: 2024-08-14T01:55:37Z
updated_at: 2024-08-14T14:06:19Z
url: https://github.com/astral-sh/uv/pull/6077
synced_at: 2026-01-12T16:07:11Z
```

# Remove `same-graph` merging in resolver

---

_@charliermarsh_

## Summary

This was added in https://github.com/astral-sh/uv/pull/5405 but is now the cause of an instability in `github_wikidata_bot`. Specifically, on the initial run, we fork in `pydantic==2.8.2`, via:

```
Requires-Dist: typing-extensions>=4.12.2; python_version >= '3.13'
Requires-Dist: typing-extensions>=4.6.1; python_version < '3.13'
```

In the end, we resolve a single version of `typing-extensions` (`4.12.2`)... But we don't recognize the two resolutions as the "same graph", because we propagate the fork markers, and so the "edges" have different markers on them...

In the second run through, when we have the forks in advance, we don't split on Pydantic... We just try to solve from the root with the current forks. This is fundamentally different and I fear it will be the cause of many instabilities. But removing this graph check fixes the proximate issue.

I don't really understand why this was added since there was no test coverage in the PR.


---

_Review requested from @BurntSushi by @charliermarsh on 2024-08-14 01:55_

---

_Review requested from @ibraheemdev by @charliermarsh on 2024-08-14 01:55_

---

_Label `bug` added by @charliermarsh on 2024-08-14 01:55_

---

_Label `resolver` added by @charliermarsh on 2024-08-14 01:55_

---

_Label `preview` added by @charliermarsh on 2024-08-14 01:55_

---

_@charliermarsh reviewed on 2024-08-14 02:56_

---

_Review comment by @charliermarsh on `crates/uv/tests/pip_compile.rs`:7775 on 2024-08-14 02:56_

I think this was wrong before...? The requirements are:

```
cffi ; os_name == 'linux'
cffi >= 1.17.0rc1 ; os_name != 'linux'
cryptography
```

---

_@charliermarsh reviewed on 2024-08-14 02:58_

---

_Review comment by @charliermarsh on `crates/uv/tests/pip_compile.rs`:7775 on 2024-08-14 02:58_

Or is it just that these are redundant...? Since `os_name == 'linux' or os_name != 'linux'` already covers the full space?

---

_Review comment by @BurntSushi on `crates/uv/tests/pip_compile.rs`:7528 on 2024-08-14 13:18_

This looks like a bug fix.

---

_Review comment by @BurntSushi on `crates/uv/tests/pip_compile.rs`:7775 on 2024-08-14 13:32_

It is definitely redundant in terms of the marker expressions. But it's not immediately obvious to me why this change dropped that part of the marker.

---

_@BurntSushi approved on 2024-08-14 13:34_

I would say we should merge this. I _believe_ the original motivation was as an optimization. Because if we _can_ prune forks then we probably should. But removing this optimization results in a bug fix from what I can see, and I think is potentially confounding our instability investigation.

It does seem like this class of optimization _ought_ to be sound though. But I think we should chop it out for now and revisit it later. (i.e., Prioritize correctness.)

---

_Comment by @charliermarsh on 2024-08-14 13:59_

Ok this fixes two of the three instabilities that emerged after #6065.

---

_Merged by @charliermarsh on 2024-08-14 14:06_

---

_Closed by @charliermarsh on 2024-08-14 14:06_

---

_Branch deleted on 2024-08-14 14:06_

---
