```yaml
number: 3050
title: Add negation flags to the CLI
type: pull_request
state: merged
author: charliermarsh
labels:
  - configuration
assignees: []
merged: true
base: main
head: charlie/workspace-vi
created_at: 2024-04-16T00:52:01Z
updated_at: 2024-04-17T18:16:11Z
url: https://github.com/astral-sh/uv/pull/3050
synced_at: 2026-01-10T14:43:31Z
```

# Add negation flags to the CLI

---

_Pull request opened by @charliermarsh on 2024-04-16 00:52_

## Summary

Now that we can pick up configuration values from persistent files, we need to enable users to _disable_ those values from the CLI. For example, if a user has `emit_index_url = true` in the configuration file, they should be able to do `--no-emit-index-url` on the command-line. This PR adds support for such negations, following the same patterns we use in Ruff.


---

_Label `configuration` added by @charliermarsh on 2024-04-16 00:52_

---

_@charliermarsh reviewed on 2024-04-16 00:52_

---

_Review comment by @charliermarsh on `crates/uv/src/cli.rs`:317 on 2024-04-16 00:52_

These ones are a bit weird, because the hidden value is `--header` and the visible value (and value in the configuration file) is `--no-header` / `no-header = true`. Like, in theory it should be... `--no-no-header`?


---

_@zanieb reviewed on 2024-04-16 02:43_

---

_Review comment by @zanieb on `crates/uv/src/cli.rs`:317 on 2024-04-16 02:43_

I think it's okay as-is.

---

_Marked ready for review by @charliermarsh on 2024-04-16 03:59_

---

_@konstin approved on 2024-04-16 09:36_

---

_Comment by @charliermarsh on 2024-04-17 02:29_

Added negation flags for all commands...

---

_Merged by @charliermarsh on 2024-04-17 18:16_

---

_Closed by @charliermarsh on 2024-04-17 18:16_

---

_Branch deleted on 2024-04-17 18:16_

---
