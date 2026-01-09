---
number: 16311
title: "release: Work around github secrets detection in job output"
type: pull_request
state: closed
author: zsol
labels: []
assignees: []
draft: true
base: main
head: zsol/jj-pkoqypqlkvys
created_at: 2025-10-15T10:56:37Z
updated_at: 2025-12-05T10:54:13Z
url: https://github.com/astral-sh/uv/pull/16311
synced_at: 2026-01-07T13:12:19-06:00
---

# release: Work around github secrets detection in job output

---

_Pull request opened by @zsol on 2025-10-15 10:56_

This PR works around GitHub's secrets detection in workflow job outputs by
* gzipping and base64 encoding the plan instead of using it as a raw json.
* Replacing direct fromJson() calls in workflow expressions with bash scripts that decode the base64-encoded, gzipped plan manifest into temporary files.


---

_Closed by @zsol on 2025-10-15 13:49_

---

_Branch deleted on 2025-12-05 10:54_

---
