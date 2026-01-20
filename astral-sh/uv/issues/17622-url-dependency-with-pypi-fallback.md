```yaml
number: 17622
title: URL dependency with PyPI fallback
type: issue
state: open
author: evakkuri
labels:
  - question
assignees: []
created_at: 2026-01-20T12:31:06Z
updated_at: 2026-01-20T12:31:06Z
url: https://github.com/astral-sh/uv/issues/17622
synced_at: 2026-01-20T12:35:56Z
```

# URL dependency with PyPI fallback

---

_@evakkuri_

### Question

## Context
I'm running ML workloads on [Anyscale](https://www.anyscale.com/). They have a proprietary version of Ray which is only available when running on their platform, and they have provided me with a URL to a wheel to use the proprietary version.

However, I would also like to run my code locally, and so would need the `ray` dependency to resolve when the direct URL is not usable. Locally I would use the [open-source version](https://pypi.org/project/ray/) which is available on public PyPI. 

I'm having a hard time configuring this - 

## What I have tried
- `uv add <direct url to wheel>` -> works on Anyscale but not locally
- Add two sources to `[tool.uv.sources]` for package `ray` -> does not work as requires markers for each source, and I don't know of any reliable markers that I could use here
- Somehow using a different source for dev and another for prod, but couldn't find a way that works

### Platform

macOS 26.2 arm64 for dev, some Linux amd64 for Anyscale

### Version

uv 0.9.17

---

_Label `question` added by @evakkuri on 2026-01-20 12:31_

---
