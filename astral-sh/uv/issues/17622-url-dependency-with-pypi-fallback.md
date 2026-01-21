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
updated_at: 2026-01-21T13:14:50Z
url: https://github.com/astral-sh/uv/issues/17622
synced_at: 2026-01-21T14:07:26Z
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

_Comment by @zanieb on 2026-01-20 14:52_

I think you want to just use `--no-sources-package ray` locally and have a single source for the proprietary version.

---

_Comment by @zanieb on 2026-01-20 14:52_

Otherwise the idea you're looking for is basically https://github.com/astral-sh/uv/issues/7945

---

_Comment by @evakkuri on 2026-01-21 13:14_

Thanks for the comments!

> I think you want to just use `--no-sources-package ray` locally and have a single source for the proprietary version.

This seems to break UV workspace dependencies, maybe similar to https://github.com/astral-sh/uv/issues/7572.

I get following error starts occurring if I add the URL source and try `--no-sources-package`. Neither of the packages have dependencies on ray.

```
❯ uv sync --locked --no-sources-package ray
error: Failed to generate package metadata for `input-data==0.1.0 @ editable+packages/input_data`
  Caused by: Failed to parse entry: `offer-gen`
  Caused by: `offer-gen` references a workspace in `tool.uv.sources` (e.g., `offer-gen = { workspace = true }`), but is not a workspace member
```

> Otherwise the idea you're looking for is basically [#7945](https://github.com/astral-sh/uv/issues/7945)

Do I understand correctly that there were no resolutions yet in the issue? Or I did not at least catch anything that I could immediately implement on my side. 🤔 

---
