```yaml
number: 3226
title: "Gave a better name to the `--color` placeholder"
type: pull_request
state: merged
author: michaelritsema
labels:
  - cli
assignees: []
merged: true
base: main
head: cli-color-argname
created_at: 2024-04-23T21:46:14Z
updated_at: 2024-04-23T22:13:17Z
url: https://github.com/astral-sh/uv/pull/3226
synced_at: 2026-01-12T16:05:30Z
```

# Gave a better name to the `--color` placeholder

---

_@michaelritsema_

## Summary

The cli gives <COLOR> as value placeholder which is misleading. I changed this to <COLOR_CHOICE> to make it obvious you don't supply a color but a ColorChoice value.

## Test Plan

Compiled and verified --help text was correct


---

_@charliermarsh approved on 2024-04-23 22:13_

Makes sense, thanks!

---

_Label `cli` added by @charliermarsh on 2024-04-23 22:13_

---

_Renamed from "Gave a better name to the --color example value" to "Gave a better name to the `--color` placeholder" by @charliermarsh on 2024-04-23 22:13_

---

_Merged by @charliermarsh on 2024-04-23 22:13_

---

_Closed by @charliermarsh on 2024-04-23 22:13_

---
