```yaml
number: 6429
title: Export development dependencies to requirements.txt
type: issue
state: closed
author: Winand
labels:
  - enhancement
assignees: []
created_at: 2024-08-22T13:06:28Z
updated_at: 2024-08-22T13:16:46Z
url: https://github.com/astral-sh/uv/issues/6429
synced_at: 2026-01-12T15:59:04Z
```

# Export development dependencies to requirements.txt

---

_@Winand_

In Poetry I export all the dependencies including the dev ones using the following command:
```bash
poetry export --with=dev --format=requirements.txt --without-hashes --output=requirements-dev.txt
```

I've tried to do the same thing with uv but it doesn't export dev dependencies:
```
uv pip compile pyproject.toml --universal --no-annotate --no-header -o requirements-dev.txt
```

Is it somehow possible?

---

_Comment by @charliermarsh on 2024-08-22 13:16_

It doesn't exist yet, but we'll likely add it: https://github.com/astral-sh/uv/issues/6007

---

_Comment by @charliermarsh on 2024-08-22 13:16_

(I'm gonna combine with that issue.)

---

_Closed by @charliermarsh on 2024-08-22 13:16_

---

_Label `enhancement` added by @charliermarsh on 2024-08-22 13:16_

---
