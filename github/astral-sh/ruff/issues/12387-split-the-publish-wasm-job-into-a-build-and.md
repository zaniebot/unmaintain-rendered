---
number: 12387
title: "Split the `publish_wasm` job into a build and publish step"
type: issue
state: open
author: MichaReiser
labels:
  - help wanted
  - ci
assignees: []
created_at: 2024-07-18T17:10:48Z
updated_at: 2024-07-18T17:10:48Z
url: https://github.com/astral-sh/ruff/issues/12387
synced_at: 2026-01-07T13:12:15-06:00
---

# Split the `publish_wasm` job into a build and publish step

---

_Issue opened by @MichaReiser on 2024-07-18 17:10_

https://github.com/astral-sh/ruff/pull/12317 added a publishing pipeline for our WASM crate to NPM. We should split that pipeline into two workflows:

1. Builds the binaries
2. Publish step to NPM



---

_Label `help wanted` added by @MichaReiser on 2024-07-18 17:10_

---

_Label `ci` added by @MichaReiser on 2024-07-18 17:10_

---
