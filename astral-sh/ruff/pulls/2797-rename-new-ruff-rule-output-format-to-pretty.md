```yaml
number: 2797
title: "Rename new `ruff rule` output format to \"pretty\""
type: pull_request
state: merged
author: not-my-profile
labels: []
assignees: []
merged: true
base: main
head: pretty-format
created_at: 2023-02-12T04:09:26Z
updated_at: 2023-02-12T04:23:37Z
url: https://github.com/astral-sh/ruff/pull/2797
synced_at: 2026-01-12T15:55:11Z
```

# Rename new `ruff rule` output format to "pretty"

---

_@not-my-profile_

The new `ruff rule` output format introduced in 551b810aebb86f396e4c45a8b6b4406a8cf8cf71 doesn't print Markdown but rather some rich text with escape sequences for colors and links, it's actually the "text" format that prints Markdown, so naming the new format "markdown" is very confusing. This commit therefore renames it to "pretty".

This isn't a breaking change since there hasn't been a release yet.

---

_Comment by @charliermarsh on 2023-02-12 04:17_

This is better. I struggled with this a bit.

---

_Merged by @charliermarsh on 2023-02-12 04:23_

---

_Closed by @charliermarsh on 2023-02-12 04:23_

---
