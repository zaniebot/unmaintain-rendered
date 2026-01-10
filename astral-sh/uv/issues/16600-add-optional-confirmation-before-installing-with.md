---
number: 16600
title: "Add (optional) confirmation before installing with `uvx`"
type: issue
state: open
author: Saucy9607
labels:
  - enhancement
assignees: []
created_at: 2025-11-05T14:16:04Z
updated_at: 2025-11-05T15:22:36Z
url: https://github.com/astral-sh/uv/issues/16600
synced_at: 2026-01-10T01:26:07Z
---

# Add (optional) confirmation before installing with `uvx`

---

_Issue opened by @Saucy9607 on 2025-11-05 14:16_

### Summary

It would be helpful if `uvx` could (optionally) prompt before downloading software for the first time.

For prior art, see `npx` and the `--yes`/`--no` flags.




### Example

The purpose of this feature is to mitigate package typosquatting.

For example, if a developer's workflow normally consists of running `uvx mytool`, but one day they mis-type `uvx mytook` after some malicious actor sneaked a package by that name onto pypi, uvx will download and run it without further interaction. However, if the dev knows the tool should already be available locally, prompting a second look could prevent a security incident.


---

_Label `enhancement` added by @Saucy9607 on 2025-11-05 14:16_

---

_Comment by @DhavalGojiya on 2025-11-05 14:29_

➕1 for this.

---

_Referenced in [astral-sh/uv#16743](../../astral-sh/uv/pulls/16743.md) on 2025-11-14 20:11_

---
