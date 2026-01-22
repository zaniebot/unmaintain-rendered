```yaml
number: 17651
title: "Add support for ruff version constraints and `exclude-newer` in `uv format`"
type: pull_request
state: open
author: zanieb
labels:
  - preview
assignees: []
draft: true
base: main
head: zb/ruff-version-fetch
created_at: 2026-01-21T23:22:32Z
updated_at: 2026-01-21T23:55:21Z
url: https://github.com/astral-sh/uv/pull/17651
synced_at: 2026-01-22T00:09:10Z
```

# Add support for ruff version constraints and `exclude-newer` in `uv format`

---

_@zanieb_

I'm picking up some pretty old work here prompted by https://github.com/astral-sh/setup-uv/pull/737 and a desire to be able to fetch newer `python-build-standalone` versions.

Previously, we only supported a static version which means we can construct a known GitHub asset URL trivially. However, to support the "latest" version or version constraints, we need a registry with metadata. The GitHub API is notoriously rate limited, so we don't want to use that. It'd be great to use PyPI (and more broadly, the resolver), but I don't want to introduce it in this code path yet. Instead, this hits https://github.com/astral-sh/versions in order to determine the available versions. We stream the NDJSON line by line to avoid downloading the whole file in order to read one version.

Loosely requires https://github.com/astral-sh/uv/pull/17648 to reach production and be ported to `ruff`, though it's not a blocker.

---

_Label `preview` added by @zanieb on 2026-01-21 23:47_

---
