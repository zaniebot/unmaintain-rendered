---
number: 12989
title: "Deprecate the uv-proprietary `.python-versions` (plural)"
type: issue
state: open
author: edmorley
labels: []
assignees: []
created_at: 2025-04-20T14:44:37Z
updated_at: 2025-04-20T15:53:37Z
url: https://github.com/astral-sh/uv/issues/12989
synced_at: 2026-01-07T13:12:18-06:00
---

# Deprecate the uv-proprietary `.python-versions` (plural)

---

_Issue opened by @edmorley on 2025-04-20 14:44_

uv currently supports specifying the Python version via either a `.python-version` or `.python-versions` file.

The plural filename form is not supported by pyenv and other tooling, and appears to be a uv creation?

The plural filename form is also not necessary since pyenv has supported multiple versions within the `.python-version` file since 2012:
https://github.com/pyenv/pyenv/commit/8187bc84e3f42d41ebd70537b55f1a1b753b5619#diff-5aa90772bd0a391bd523f92a07cdb2d64e414e9e6ee7b2cfd0e606b310726270

My concern is that this uv-specific filename:
1. Goes against the spirit of `There should be one-- and preferably only one --obvious way to do it.`
2. Means all tooling downstream of uv has to either (a) add support for a redundant filename, (b) has to add a specific error case for it (since users will get confused why our tooling isn't using the Python version they requested - and even if we warn that they "don't have a `.python-version` file", I guarantee many won't notice the "s" difference, given from our metrics we already see a lot of file naming related [confusion](https://github.com/heroku/buildpacks-python/blob/3183e18c99a06f576b70f2041cbd9ae9599e1eed/src/detect.rs#L27-L33) for things even less subtle than that)

Would it be possible to deprecate the uv-specific `.python-versions` filename?

eg: Emit a warning saying its deprecated and to use `.python-version` instead.


---

_Comment by @zanieb on 2025-04-20 15:53_

I'm fine with deprecating it. Would you be willing to open a pull request?


---

_Assigned to @zanieb by @zanieb on 2025-04-20 15:53_

---
