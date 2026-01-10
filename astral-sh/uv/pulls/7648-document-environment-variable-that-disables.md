```yaml
number: 7648
title: Document environment variable that disables printing of virtual environment name in prompt
type: pull_request
state: merged
author: namurphy
labels:
  - documentation
assignees: []
merged: true
base: main
head: docs-virtual_env_disable_prompt
created_at: 2024-09-23T19:32:52Z
updated_at: 2024-09-23T20:06:21Z
url: https://github.com/astral-sh/uv/pull/7648
synced_at: 2026-01-10T12:53:52Z
```

# Document environment variable that disables printing of virtual environment name in prompt

---

_Pull request opened by @namurphy on 2024-09-23 19:32_

This PR adds a line to `docs/configuration/environment.md` that documents `VIRTUAL_ENV_DISABLE_PROMPT`.  If set to `1` when the virtual environment is activated, then the virtual environment name will not be prepended to a terminal prompt.

So far I've tested this in bash, but from the various activation scripts, it looks like it is respected for a variety of shells.

Maintainers should please feel free to edit this PR directly. Thank you!

---

_Label `documentation` added by @zanieb on 2024-09-23 19:48_

---

_@zanieb approved on 2024-09-23 19:48_

Thanks!

---

_Marked ready for review by @zanieb on 2024-09-23 19:48_

---

_Merged by @zanieb on 2024-09-23 19:48_

---

_Closed by @zanieb on 2024-09-23 19:48_

---

_Branch deleted on 2024-09-23 20:06_

---
