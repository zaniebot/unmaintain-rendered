```yaml
number: 8073
title: "`uv publish` interactive like twine?"
type: issue
state: closed
author: hauntsaninja
labels:
  - enhancement
  - good first issue
  - cli
assignees: []
created_at: 2024-10-10T07:24:46Z
updated_at: 2024-10-15T08:00:44Z
url: https://github.com/astral-sh/uv/issues/8073
synced_at: 2026-01-12T15:59:19Z
```

# `uv publish` interactive like twine?

---

_@hauntsaninja_

I've been doing `uvx twine upload dist/*` instead of `uv publish` because twine gives you a nice prompt for token and you don't have to worry about leaking a credential into shell history or something. Maybe this is something `uv publish` could also do?

---

_Assigned to @charliermarsh by @charliermarsh on 2024-10-10 08:39_

---

_Assigned to @konstin by @charliermarsh on 2024-10-10 08:39_

---

_Unassigned @charliermarsh by @charliermarsh on 2024-10-10 08:39_

---

_Label `enhancement` added by @charliermarsh on 2024-10-11 14:46_

---

_Label `cli` added by @charliermarsh on 2024-10-11 14:46_

---

_Label `good first issue` added by @konstin on 2024-10-13 08:18_

---

_Comment by @konstin on 2024-10-13 08:22_

Mentoring instructions: When there are no token, no trusted-publishing and no keyring, we should ask for the username (if not given) and the password. For this, the `console` crate should be used, so we don't add another heavy depedency, like in https://github.com/astral-sh/uv/blob/71d5661bd8d5d6c60dbb02c4285c22fd06894c17/crates/uv-console/src/lib.rs. We should provide an interactive hint that tokens can be used as username/password with username `__token__`.

---

_Unassigned @konstin by @konstin on 2024-10-13 08:22_

---

_Closed by @konstin on 2024-10-15 08:00_

---
