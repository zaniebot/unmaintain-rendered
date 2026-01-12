```yaml
number: 478
title: "Clarify `ty check` output format default in CLI doc"
type: pull_request
state: closed
author: brainwane
labels: []
assignees: []
base: main
head: cli-guidance
created_at: 2025-05-21T20:27:21Z
updated_at: 2025-05-22T12:09:55Z
url: https://github.com/astral-sh/ty/pull/478
synced_at: 2026-01-12T15:54:27Z
```

# Clarify `ty check` output format default in CLI doc

---

_@brainwane_

## Summary

Add a "[default]" note in the CLI documentation to indicate that, by default, `ty check` prints "full" diagnostic output, not "concise" output.

## Test Plan

I pushed the branch, and read the preview of the Markdown file on GitHub in my browser.

---

_Comment by @AlexWaygood on 2025-05-21 20:31_

Thanks! Unfortunately this particular file is generated from a MarkDown file found in the Ruff repo (https://github.com/astral-sh/ruff/blob/main/crates/ty/docs/cli.md) -- you'd have to edit that file to make changes to these docs. Sorry about this, I know it's a pretty unusual setup :-(

---

_Closed by @AlexWaygood on 2025-05-21 20:31_

---

_Branch deleted on 2025-05-22 12:09_

---
