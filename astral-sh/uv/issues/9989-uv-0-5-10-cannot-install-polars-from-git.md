```yaml
number: 9989
title: uv 0.5.10 cannot install Polars from git
type: issue
state: closed
author: ritchie46
labels: []
assignees: []
created_at: 2024-12-18T08:52:04Z
updated_at: 2024-12-18T09:28:08Z
url: https://github.com/astral-sh/uv/issues/9989
synced_at: 2026-01-10T04:36:21Z
```

# uv 0.5.10 cannot install Polars from git

---

_Issue opened by @ritchie46 on 2024-12-18 08:52_

`$ uv pip install git+https://github.com/pola-rs/polars.git@00837b8ab9197e44fab9c398699e61e8d611c924#subdirectory=py-polars`

succeeds on `0.5.9`, but fails on `0.5.10`.

```
  × Failed to download and build `polars @
  │ git+https://github.com/pola-rs/polars.git@00837b8ab9197e44fab9c398699e61e8d611c924#subdirectory=py-polars`
  ├─▶ Failed to parse: `/home/ritchie46/.cache/uv/git-v0/checkouts/e54880a276d63f63/00837b8ab/py-polars/pyproject.toml`
  ╰─▶ TOML parse error at line 5, column 1
        |
      5 | [project]
        | ^^^^^^^^^
      `pyproject.toml` is using the `[project]` table, but the required `project.version` field is neither set nor present in the
      `project.dynamic` list

```

We don't have a version set,  as that is handled by maturin during build.

---

_Comment by @ritchie46 on 2024-12-18 09:28_

Ah, I see https://github.com/pola-rs/polars/pull/20345, nvm!

---

_Closed by @ritchie46 on 2024-12-18 09:28_

---
