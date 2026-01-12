```yaml
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
synced_at: 2026-01-12T16:12:13Z
```

# release: Work around github secrets detection in job output

---

_@zsol_

This PR works around GitHub's secrets detection in workflow job outputs by
* gzipping and base64 encoding the plan instead of using it as a raw json.
* Replacing direct fromJson() calls in workflow expressions with bash scripts that decode the base64-encoded, gzipped plan manifest into temporary files.


---

_Closed by @zsol on 2025-10-15 13:49_

---

_Branch deleted on 2025-12-05 10:54_

---
