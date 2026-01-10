---
number: 6203
title: "`uv add` should preserve alphabetic package ordering"
type: issue
state: closed
author: konstin
labels: []
assignees: []
created_at: 2024-08-19T13:14:18Z
updated_at: 2024-08-29T14:55:05Z
url: https://github.com/astral-sh/uv/issues/6203
synced_at: 2026-01-10T01:23:57Z
---

# `uv add` should preserve alphabetic package ordering

---

_Issue opened by @konstin on 2024-08-19 13:14_

Currently, `uv add` always adds new entries last. It should instead preserve a lexicographic order if one is used.

Example: Running `uv add "pydantic >=2.7.1"` on:

```toml
[project]
dependencies = [
    "CacheControl[filecache]>=0.14,<0.15",
    "mwparserfromhell >=0.6.4,<0.7",
    "pywikibot >=9.1.2,<10",
    "sentry-sdk >=2.0.1,<3",
    "yarl >=1.9,<2",
    "pydantic>=2.7.1",
]
```

Current output:

```toml
[project]
dependencies = [
    "CacheControl[filecache]>=0.14,<0.15",
    "mwparserfromhell >=0.6.4,<0.7",
    "pywikibot >=9.1.2,<10",
    "sentry-sdk >=2.0.1,<3",
    "yarl >=1.9,<2",
    "pydantic>=2.7.1",
]
```

Desired output:

```toml
[project]
dependencies = [
    "CacheControl[filecache]>=0.14,<0.15",
    "mwparserfromhell >=0.6.4,<0.7",
    "pydantic >=2.7.1",
    "pywikibot >=9.1.2,<10",
    "sentry-sdk >=2.0.1,<3",
    "yarl >=1.9,<2",
]
```

---

_Label `preview` added by @konstin on 2024-08-19 13:14_

---

_Label `preview` removed by @zanieb on 2024-08-20 18:21_

---

_Referenced in [astral-sh/uv#6388](../../astral-sh/uv/pulls/6388.md) on 2024-08-21 23:11_

---

_Closed by @konstin on 2024-08-29 14:55_

---

_Referenced in [astral-sh/uv#10076](../../astral-sh/uv/issues/10076.md) on 2024-12-21 13:26_

---

_Referenced in [astral-sh/uv#10738](../../astral-sh/uv/issues/10738.md) on 2025-01-18 19:03_

---
