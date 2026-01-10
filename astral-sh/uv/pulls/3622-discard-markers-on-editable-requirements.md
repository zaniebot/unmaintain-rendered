```yaml
number: 3622
title: Discard markers on editable requirements
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - compatibility
assignees: []
merged: true
base: main
head: charlie/extras
created_at: 2024-05-15T19:48:17Z
updated_at: 2024-05-16T20:53:53Z
url: https://github.com/astral-sh/uv/pull/3622
synced_at: 2026-01-10T14:32:20Z
```

# Discard markers on editable requirements

---

_Pull request opened by @charliermarsh on 2024-05-15 19:48_

## Summary

If a user includes markers after an editable, we now ignore them (rather than including them in the parsed URL). This matches pip's behavior. In the future, we could further improve by respecting them, but that _would_ be a deviation from pip.

For example, given:

```
-e ./scripts/packages/black_editable ; python_version >= "3.9" and python_ver
```

We now split at the first whitespace (just before the `;`), parse everything before, and throw out everything after.

This logic also extends to extras. So given:

```
-e ./scripts/packages/black_editable[dev, colorama]
```

We'll now parse this as the URL `./scripts/packages/black_editable[dev,`, and throw out ` colorama]`. Instead, you need to do:

```
-e ./scripts/packages/black_editable[dev,colorama]
```

(I.e., remove the space.)

This _also_ matches pip's behavior. I could "fix" this but I'm unsure if I should -- it means requirements files will be parseable by uv that won't work with pip. Open to input. My gut reaction is that we _should_ properly support `-e ./scripts/packages/black_editable[dev, colorama]` even if pip would reject it, but `requirements.txt` is implementation-defined so it'd be a "deviation".

Closes https://github.com/astral-sh/uv/issues/3604.


---

_Review requested from @zanieb by @charliermarsh on 2024-05-15 19:48_

---

_Review requested from @konstin by @charliermarsh on 2024-05-15 19:48_

---

_Label `compatibility` added by @charliermarsh on 2024-05-15 19:48_

---

_Marked ready for review by @charliermarsh on 2024-05-15 19:49_

---

_Label `bug` added by @charliermarsh on 2024-05-15 19:49_

---

_@konstin reviewed on 2024-05-16 07:47_

---

_Review comment by @konstin on `crates/requirements-txt/test-data/requirements-txt/editable.txt`:1 on 2024-05-16 07:47_

This file contained some regression tests for whitespace behavior in the parser

---

_Comment by @konstin on 2024-05-16 07:47_

To me, https://github.com/pypa/pip/issues/8581#issuecomment-658794436 makes it look like this is a bug in pip, could we check back with the pip devs on this?

---

_@charliermarsh reviewed on 2024-05-16 11:12_

---

_Review comment by @charliermarsh on `crates/requirements-txt/test-data/requirements-txt/editable.txt`:1 on 2024-05-16 11:12_

I’m pretty sure those exist in a separate file. This was just a duplicate of another test file, no?

---

_Comment by @charliermarsh on 2024-05-16 12:10_

I don’t think it’s a bug. I think it’s an intentional limitation right now because it’s not covered by any standard. For that reason I’m also somewhat hesitant to support it… It also means that files that work with uv won’t work with pip.

---

_@konstin approved on 2024-05-16 13:14_

---

_Comment by @charliermarsh on 2024-05-16 20:09_

Ok, I decided to change the behavior to continue to allow spaces between extras in editables (i.e., we continue to support `-e /editable[d, dev]`).

---

_Merged by @charliermarsh on 2024-05-16 20:53_

---

_Closed by @charliermarsh on 2024-05-16 20:53_

---

_Branch deleted on 2024-05-16 20:53_

---
