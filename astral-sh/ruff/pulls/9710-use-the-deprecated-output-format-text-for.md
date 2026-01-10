```yaml
number: 9710
title: "Use the deprecated output format `text` for ecosystem checks in release/0.2.0"
type: pull_request
state: closed
author: snowsignal
labels: []
assignees: []
base: release/0.2.0
head: jane/ci/ecosystem-fix
created_at: 2024-01-30T17:31:56Z
updated_at: 2024-01-30T18:01:28Z
url: https://github.com/astral-sh/ruff/pull/9710
synced_at: 2026-01-10T22:57:09Z
```

# Use the deprecated output format `text` for ecosystem checks in release/0.2.0

---

_Pull request opened by @snowsignal on 2024-01-30 17:31_

This prevents us from running into the following error during an ecosystem check against `main`:
```
error: invalid value 'concise' for '--output-format <OUTPUT_FORMAT>'
  [possible values: text, json, json-lines, junit, grouped, github, gitlab, pylint, azure, sarif]
```

This error appears because `main` doesn't support `concise` as an output format yet. The deprecated output format that concise/full replaced will be used until then (`text` falls back to `concise` on the release branch).

---

_@zanieb approved on 2024-01-30 17:40_

Let's make sure the ecosystem checks display nice before merging

---

_Comment by @snowsignal on 2024-01-30 18:01_

Closing since preview mode will still treat `text` as `full`, and the ecosystem check does both preview and non-preview checks.

---

_Closed by @snowsignal on 2024-01-30 18:01_

---
