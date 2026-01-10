```yaml
number: 702
title: Improve release instructions
type: pull_request
state: merged
author: AlexWaygood
labels:
  - release
assignees: []
merged: true
base: main
head: alex/release-instructions
created_at: 2025-06-25T11:37:10Z
updated_at: 2025-06-25T12:51:05Z
url: https://github.com/astral-sh/ty/pull/702
synced_at: 2026-01-10T02:34:10Z
```

# Improve release instructions

---

_Pull request opened by @AlexWaygood on 2025-06-25 11:37_

## Summary

The release script initially failed for me when I ran it locally for https://github.com/astral-sh/ty/pull/700, with some somewhat opaque error messages. It seems like I had two problems:

1. Despite my `main` branch locally being up to date with the upstream ty `main` branch, the `ruff` submodule on my `main` branch was not up to date with the `ruff` submodule on the upstream ty `main` branch
2. Similarly, the latest tag I had locally was 0.0.1-alpha.5, which caused an assertion in the release script to fail, as it expected me to have 0.0.1-alpha.11 locally in order to create the 0.0.1-alpha.12 release

This PR updates the release instructions so I won't have to figure this out again 😄

---

_Label `release` added by @AlexWaygood on 2025-06-25 11:37_

---

_@MichaReiser approved on 2025-06-25 12:38_

---

_Merged by @AlexWaygood on 2025-06-25 12:51_

---

_Closed by @AlexWaygood on 2025-06-25 12:51_

---

_Branch deleted on 2025-06-25 12:51_

---
