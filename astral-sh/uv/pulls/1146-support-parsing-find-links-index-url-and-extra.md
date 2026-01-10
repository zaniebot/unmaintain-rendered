```yaml
number: 1146
title: "Support parsing `--find-links`, `--index-url`, and `--extra-index-url` in `requirements.txt`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
assignees: []
merged: true
base: main
head: charlie/index-url
created_at: 2024-01-27T03:33:43Z
updated_at: 2024-01-29T15:06:41Z
url: https://github.com/astral-sh/uv/pull/1146
synced_at: 2026-01-10T15:39:03Z
```

# Support parsing `--find-links`, `--index-url`, and `--extra-index-url` in `requirements.txt`

---

_Pull request opened by @charliermarsh on 2024-01-27 03:33_

## Summary

This PR adds support for `--find-links`, `--index-url`, and `--extra-index-url` arguments when specified in a `requirements.txt`.

It's a mostly-straightforward change. The only uncertain piece is what to do when multiple files include these flags, and/or when we include them on the CLI and in other files.

In general:

- If _anything_ specifies `--no-index`, we respect it.
- We combine all `--extra-index-url` and `--find-links` across all sources, since those are just vectors.
- If we see multiple `--index-url` in requirements files, we error.
- We respect the `--index-url` from the command line over any provided in a requirements file.

(`pip-compile` seems to just pick one semi-arbitrarily when multiple are provided.)

Closes https://github.com/astral-sh/puffin/issues/1143.


---

_Marked ready for review by @charliermarsh on 2024-01-27 03:35_

---

_Review requested from @zanieb by @charliermarsh on 2024-01-28 00:22_

---

_Review requested from @konstin by @charliermarsh on 2024-01-28 00:22_

---

_Label `enhancement` added by @charliermarsh on 2024-01-28 00:22_

---

_Review comment by @konstin on `crates/requirements-txt/src/lib.rs`:97 on 2024-01-29 08:05_

I think this needs the same windows compat as https://github.com/astral-sh/puffin/blob/5219d372509f157a98a34aec02c3a3d93e4ab2ec/crates/pep508-rs/src/lib.rs#L747-L771. Should we put this in `puffin-fs`?

---

_@konstin approved on 2024-01-29 08:06_

---

_@charliermarsh reviewed on 2024-01-29 12:51_

---

_Review comment by @charliermarsh on `crates/requirements-txt/src/lib.rs`:97 on 2024-01-29 12:51_

Yeah, good call, will do.

---

_Merged by @charliermarsh on 2024-01-29 15:06_

---

_Closed by @charliermarsh on 2024-01-29 15:06_

---

_Branch deleted on 2024-01-29 15:06_

---
