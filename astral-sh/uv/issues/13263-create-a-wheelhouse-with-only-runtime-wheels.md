```yaml
number: 13263
title: Create a wheelhouse with only runtime wheels
type: issue
state: closed
author: gsemet
labels:
  - enhancement
assignees: []
created_at: 2025-05-02T10:34:41Z
updated_at: 2025-05-02T10:49:53Z
url: https://github.com/astral-sh/uv/issues/13263
synced_at: 2026-01-12T16:01:23Z
```

# Create a wheelhouse with only runtime wheels

---

_@gsemet_

### Summary

I need to use shiv to create zipapp. Using poetry, i was using `poetry export` to create `requirements.txt` with only runtime dependencies (no need to package pytest or sphinx in the zipapp package).

Then i create wheelhouse (a folder with all the right wheel copied in the same location)

and then `shiv` to create a single auto-executable app.

How to do that with uv?

My current solution is:

```
$ uv export --no-dev --no-hashes > dist/prod-requirements.txt
$ uv run pip wheel --extra-index=https://my/private/repoisitory --find-links distshiv -r dist/prod-requirements.txt -w distshiv/
$ shiv ...
```

Notes:
- `uv run pip` does not pass the current environment private repository settings (defined in pyproject.toml)
- `i need to execute `uv run pip` that is pretty slow
- and this does not use wheel from `uv`, it redownloads everything again

### Example

Expected command:
```bash
$ uv export-wheel --only-dev -t wheelhouse/
```

---

_Label `enhancement` added by @gsemet on 2025-05-02 10:34_

---

_Comment by @konstin on 2025-05-02 10:44_

We're tracking this in https://github.com/astral-sh/uv/issues/3163, a solution to which should handle your case, too.

---

_Closed by @konstin on 2025-05-02 10:44_

---
