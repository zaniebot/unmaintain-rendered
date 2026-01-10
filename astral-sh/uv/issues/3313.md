```yaml
number: 3313
title: "Add environment variable for `--link-mode`"
type: issue
state: closed
author: davidfritzsche
labels:
  - good first issue
  - configuration
assignees: []
created_at: 2024-04-29T16:03:08Z
updated_at: 2024-04-29T17:29:38Z
url: https://github.com/astral-sh/uv/issues/3313
synced_at: 2026-01-10T05:31:37Z
```

# Add environment variable for `--link-mode`

---

_Issue opened by @davidfritzsche on 2024-04-29 16:03_

Hello,

thanks for you work on uv!

The outout of `uv pip install --help` (uv version 0.1.139) does not offer an environment variable for `--link-mode`. As I'm commonly editing installed Python packages (to debug something), it would be of great help if I could set the default link mode to `clone` instead of `hardlink` (I'm mostly working on Linux).

In general, from the documentation it is not really clear why the OS-specific defaults were chosen (probably performance?) and how uv protects itself against stupid old-school developers like me when they accidentially modify cached files installed with link mode `hardlink`.

PS: At $WORK my Windows-using colleagues are very much looking forward to use `uv` to speed up the creation of tox venvs to not take several minutes anymore. 🥇 for the astral team and all the contributors!

---

_Label `configuration` added by @zanieb on 2024-04-29 16:07_

---

_Comment by @zanieb on 2024-04-29 16:07_

Thanks for the kind words!

This seems reasonable to add.

---

_Label `good first issue` added by @zanieb on 2024-04-29 16:07_

---

_Closed by @zanieb on 2024-04-29 17:29_

---
