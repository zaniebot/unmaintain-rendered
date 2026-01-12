```yaml
number: 6436
title: "Add support for configuring `python-downloads` with `UV_PYTHON_DOWNLOADS`"
type: pull_request
state: merged
author: zanieb
labels:
  - configuration
assignees: []
merged: true
base: main
head: zb/python-download-env
created_at: 2024-08-22T14:02:07Z
updated_at: 2024-08-23T14:19:33Z
url: https://github.com/astral-sh/uv/pull/6436
synced_at: 2026-01-12T16:07:22Z
```

# Add support for configuring `python-downloads` with `UV_PYTHON_DOWNLOADS`

---

_@zanieb_

Part of https://github.com/astral-sh/uv/issues/6406

Replaces #6416 

---

_Label `configuration` added by @zanieb on 2024-08-22 14:02_

---

_Renamed from "Allow `UV_PYTHON_DOWNLOADS` to be configured via environment variable" to "Add support for configuring `python-downloads with `UV_PYTHON_DOWNLOADS`" by @zanieb on 2024-08-22 14:02_

---

_Renamed from "Add support for configuring `python-downloads with `UV_PYTHON_DOWNLOADS`" to "Add support for configuring `python-downloads` with `UV_PYTHON_DOWNLOADS`" by @zanieb on 2024-08-22 14:02_

---

_@zanieb reviewed on 2024-08-22 14:03_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:124 on 2024-08-22 14:03_

Copying Clap's format here, but the value is fixed.

---

_@zanieb reviewed on 2024-08-22 14:04_

---

_Review comment by @zanieb on `crates/uv/src/settings.rs`:122 on 2024-08-22 14:04_

Precedence goes CLI -> ENV -> CONFIG -> DEFAULT

---

_Comment by @zanieb on 2024-08-22 14:05_

cc @blueraft — sorry there's a little bit of nuance to this option.

---

_Comment by @blueraft on 2024-08-22 14:11_

All good ❤️

---

_Review requested from @charliermarsh by @zanieb on 2024-08-22 14:20_

---

_@charliermarsh approved on 2024-08-22 22:51_

---

_@charliermarsh reviewed on 2024-08-22 22:53_

---

_Review comment by @charliermarsh on `docs/reference/cli.md`:409 on 2024-08-22 22:53_

I guess this doesn't show for any other options.

---

_@zanieb reviewed on 2024-08-22 23:06_

---

_Review comment by @zanieb on `docs/reference/cli.md`:409 on 2024-08-22 23:06_

Eek, yeah. Ugh. Will figure out a solution...

---

_Merged by @zanieb on 2024-08-22 23:19_

---

_Closed by @zanieb on 2024-08-22 23:19_

---

_Branch deleted on 2024-08-22 23:19_

---
