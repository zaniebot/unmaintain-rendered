```yaml
number: 9605
title: "Do not show empty version specifier in `uv tool list`"
type: pull_request
state: merged
author: j178
labels:
  - bug
assignees: []
merged: true
base: main
head: main
created_at: 2024-12-03T12:19:28Z
updated_at: 2024-12-03T13:56:05Z
url: https://github.com/astral-sh/uv/pull/9605
synced_at: 2026-01-12T16:08:53Z
```

# Do not show empty version specifier in `uv tool list`

---

_@j178_

## Summary

Do not show empty version specifier in `uv tool list --show-version-specifier`.

Before:

```console
$ uv tool list --show-version-specifiers
ipython v8.29.0 [required: ]
- ipython
- ipython3
maturin v1.7.4 [required: ]
- maturin
pre-commit v4.0.1 [required: ]
- pre-commit
rooster-blue v0.0.8 [required: ]
- rooster
ruff v0.8.0 [required: ]
- ruff
yt-dlp v2024.11.18 [required: ]
- yt-dlp
```

After:

```console
$ cargo run -- tool list --show-version-specifiers
ipython v8.29.0
- ipython
- ipython3
maturin v1.7.4
- maturin
pre-commit v4.0.1
- pre-commit
rooster-blue v0.0.8
- rooster
ruff v0.8.0
- ruff
yt-dlp v2024.11.18
- yt-dlp
```


---

_@charliermarsh approved on 2024-12-03 13:55_

---

_Merged by @charliermarsh on 2024-12-03 13:55_

---

_Closed by @charliermarsh on 2024-12-03 13:55_

---

_Label `bug` added by @charliermarsh on 2024-12-03 13:55_

---

_Comment by @charliermarsh on 2024-12-03 13:56_

Thanks! This was bothering me recently hah.

---
