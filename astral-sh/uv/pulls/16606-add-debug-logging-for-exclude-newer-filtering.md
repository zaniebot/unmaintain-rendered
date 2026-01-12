```yaml
number: 16606
title: Add debug logging for exclude-newer filtering
type: pull_request
state: open
author: marcvernet31
labels: []
assignees: []
base: main
head: main
created_at: 2025-11-05T21:13:46Z
updated_at: 2025-11-06T17:43:47Z
url: https://github.com/astral-sh/uv/pull/16606
synced_at: 2026-01-12T16:12:21Z
```

# Add debug logging for exclude-newer filtering

---

_@marcvernet31_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

When `exclude-newer` filters out all candidates for a package, the debug output doesn't indicate that the date filter was applied. This makes it difficult to diagnose why no compatible versions are found. Issue described here: https://github.com/astral-sh/uv/issues/16485


**Example scenario:**
- User pins `mlx==0.29.3` (published on 2025-10-18)
- User sets `exclude-newer = "2025-10-11T00:00:00Z"`
- Resolution fails silently as there is no matching version found for mlx.


**New output:**
```
DEBUG Searching for a compatible version of mlx (>=0.29.3, <0.29.3+)
DEBUG Excluding candidates for mlx published after 2025-10-11T00:00:00Z
TRACE Exhausted all candidates for package mlx with range >=0.29.3, <0.29.3+ after 1 steps
DEBUG No compatible version found for: mlx
```

## Test Plan

<!-- How was it tested? -->


Tested with:
```bash
# pyproject.toml
[project]
dependencies = ["mlx==0.29.3"]

[tool.uv]
exclude-newer = "2025-10-11T00:00:00Z"  
```

Running `uv lock -vvv` now shows the exclude-newer debug message.


---

_@zanieb reviewed on 2025-11-06 17:42_

---

_Review comment by @zanieb on `crates/uv-resolver/src/candidate_selector.rs`:535 on 2025-11-06 17:42_

I don't understand why we need to pass it through here if it's not used here?

---

_@zanieb reviewed on 2025-11-06 17:43_

---

_Review comment by @zanieb on `crates/uv-resolver/src/resolver/mod.rs`:1290 on 2025-11-06 17:43_

Is this being logged if exclude newer is set and we visit this package? That seems misleading? Shouldn't we only log it if we actually filtered a distribution due to exclude newer?

Why should we have this in a debug log rather than a hint on failed resolution?

---
