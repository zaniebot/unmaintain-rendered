```yaml
number: 13115
title: Add pylock.toml mentions where relevant
type: pull_request
state: merged
author: ReinforcedKnowledge
labels: []
assignees: []
merged: true
base: main
head: main
created_at: 2025-04-26T03:58:51Z
updated_at: 2025-04-26T14:26:30Z
url: https://github.com/astral-sh/uv/pull/13115
synced_at: 2026-01-10T11:10:40Z
```

# Add pylock.toml mentions where relevant

---

_Pull request opened by @ReinforcedKnowledge on 2025-04-26 03:58_

Just a small PR to add mentions to `pylock.toml` in the CLI manual where appropriate.

I tried to say "PEP-751 compatible lock files" when appropriate to also include the case `r"^pylock\.([^.]+)\.toml$"`. Feel free to change that if you think it's cluttery.

I also tried to include the "single-use" wording when it made sense.

I also have almost never used the `uv pip` interface, so maybe there are some other minor things to add here and there about the usage of `pylock.toml` that I missed.

---

_Assigned to @charliermarsh by @charliermarsh on 2025-04-26 14:09_

---

_@charliermarsh approved on 2025-04-26 14:18_

---

_Merged by @charliermarsh on 2025-04-26 14:26_

---

_Closed by @charliermarsh on 2025-04-26 14:26_

---
