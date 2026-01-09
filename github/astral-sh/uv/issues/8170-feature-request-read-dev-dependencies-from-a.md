---
number: 8170
title: "Feature request: Read dev dependencies from a packaging extra"
type: issue
state: closed
author: czechnology
labels:
  - enhancement
assignees: []
created_at: 2024-10-14T09:04:20Z
updated_at: 2024-10-16T06:38:21Z
url: https://github.com/astral-sh/uv/issues/8170
synced_at: 2026-01-07T13:12:17-06:00
---

# Feature request: Read dev dependencies from a packaging extra

---

_Issue opened by @czechnology on 2024-10-14 09:04_

Hi, first of all, thanks for this great tool which has significantly changed how I develop and work with Python!

I would like to request **an extra configuration option to read the dev dependencies from a specific packaging extra** (i.e. a key under optional dependencies).

### Example

`uv` allows developers to configure [dev dependencies](https://docs.astral.sh/uv/reference/settings/#dev-dependencies) as part of the `uv.tool` table in pyproject.toml.
```toml
[tool.uv]
dev-dependencies = ["ruff==0.5.0"]
```

Many teams have been using an optional `dev` extra to specify the optional dependencies needed for development, testing etc.
```toml
[project.optional-dependencies]
dev = ["ruff==0.5.0"]
```

Using `tool.uv.dev-dependencies` limits the use of dev dependencies only to `uv`. It would be very helpful if we could alternatively tell `uv` to use a specific extra, something like this:
```toml
[tool.uv]
dev-dependencies.extra = "dev"
```

This would allow using dev dependencies also by other tools, such as pure `pip` (`pip install .[dev]`) and additionally help us migrate to `uv` more seamlessly.

Thanks!

---

(Side note on the suggested `pyproject.toml` notation: It reminds me of how [`setuptools` configures packages](https://setuptools.pypa.io/en/latest/userguide/package_discovery.html) -- either set the packages as list in `tool.setuptools.packages` or configure custom discovery settings using options such as `tool.setuptools.packages.find`.)

---

_Comment by @charliermarsh on 2024-10-14 13:47_

Good timing -- I think the newly-accepted PEP 735 standard (#8090) will cover this, and we've already started implementing this. Feel free to track that issue!

---

_Closed by @charliermarsh on 2024-10-14 13:47_

---

_Label `enhancement` added by @charliermarsh on 2024-10-14 13:47_

---

_Comment by @czechnology on 2024-10-14 13:55_

Thanks, Charlie, looks promising, depends on how `uv` will approach the dev dependencies on top of that PEP, I will follow the progress there.

---

_Comment by @czechnology on 2024-10-16 06:38_

I'll just reference also #5632, which is even closer to what I was looking for.

---
