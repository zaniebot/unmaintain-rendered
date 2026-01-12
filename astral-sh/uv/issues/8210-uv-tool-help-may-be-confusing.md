```yaml
number: 8210
title: uv tool help may be confusing
type: issue
state: open
author: callegar
labels:
  - documentation
assignees: []
created_at: 2024-10-15T10:51:06Z
updated_at: 2024-10-15T15:24:30Z
url: https://github.com/astral-sh/uv/issues/8210
synced_at: 2026-01-12T15:59:22Z
```

# uv tool help may be confusing

---

_@callegar_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

`uv tool install --help` and `uv tool update --help` mention *pinning* that is not described in the same help lines.

---

_Comment by @zanieb on 2024-10-15 14:07_

Can you share an example?

---

_Comment by @callegar on 2024-10-15 15:14_

Sure:
`uv tool install --help`  results in:
```
...
  -U, --upgrade                            Allow package upgrades, ignoring pinned versions ...
  -P, --upgrade-package <UPGRADE_PACKAGE>  Allow upgrades for a specific package, ignoring pinned versions ...
...
```
However, these are the only mentions to pinning in this help text. There is no mention on how to pin something related to tools or how something related to tools might get pinned. If pinning happens with another subcommand where the help text describes it, probably there should be a 'see also'...

---

_Comment by @callegar on 2024-10-15 15:15_

For what concerns `--refresh`, it was my mistake, sorry for the noise...

---

_Comment by @zanieb on 2024-10-15 15:24_

Thanks!

---

_Assigned to @zanieb by @zanieb on 2024-10-15 15:24_

---

_Label `documentation` added by @zanieb on 2024-10-15 15:24_

---
