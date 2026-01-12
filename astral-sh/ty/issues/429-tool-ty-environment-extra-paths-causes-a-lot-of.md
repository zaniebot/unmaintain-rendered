```yaml
number: 429
title: "[tool.ty.environment] extra-paths causes a lot of failures and in general works differently from `ty check --extra-search-path`"
type: issue
state: closed
author: Guitaricet
labels:
  - configuration
  - needs-mre
  - imports
assignees: []
created_at: 2025-05-16T19:19:01Z
updated_at: 2025-07-10T16:28:55Z
url: https://github.com/astral-sh/ty/issues/429
synced_at: 2026-01-12T15:54:23Z
```

# [tool.ty.environment] extra-paths causes a lot of failures and in general works differently from `ty check --extra-search-path`

---

_@Guitaricet_

### Summary

Hi, I have built a pipeline to run ty on an internal codebase and originally I had to provide a lot of `--extra-search-path` arguments to make it work properly.

Now, when configuration files are documented, I wanted to move these from `ty check` command to my `pyproject.toml`. However, it seems like they work differently, because when I run

```
ty check --project <my_project> --extra-search-path /opt/ros/humble/lib/python3.10/site-packages
```
I get ~100 diagnostics, but when I add either of:

```
[tool.ty.environment]
extra-paths = [
    "/opt/ros/humble/lib/python3.10/site-packages",
]
```

I get ~3000 diagnostics (more than 10x). I tried a couple of things to explore what this is related to:
```
[tool.ty.environment]
extra-paths = []
```

```
[tool.ty.environment]
extra-paths = ["stubs"]  # this directory exists, contains a few custom stubs
```
Every time I get slightly different, but very large number of diagnostics. I can dive deeper into it, but for now I'll just keep using a lot of `--extra-search-path`.

This happens on both 0.0.1 alpha 3 and alpha 2

### Version

0.0.1-alpha.3

---

_Comment by @carljm on 2025-05-20 19:01_

Thanks for the report!

Are the extra diagnostics mostly failed imports? If so, that might suggest the version with "lots of extra diagnostics" is failing to set up your import paths correctly. Or are they lots of other diagnostics? If the latter, it might suggest that we are actually finding many diagnostics, and the "few diagnostics" version is not finding your first-party code correctly, and thus not even checking it.

It would be helpful to know what your overall directory structure is, where your `pyproject.toml` is, and from what directory you are running ty.

A minimal example that we can reproduce ourselves would be ideal.

---

_Label `configuration` added by @carljm on 2025-05-20 19:01_

---

_Label `imports` added by @carljm on 2025-05-20 19:01_

---

_Label `needs-mre` added by @MichaReiser on 2025-07-10 16:28_

---

_Closed by @MichaReiser on 2025-07-10 16:28_

---

_Comment by @MichaReiser on 2025-07-10 16:28_

I'll close this, given we don't have an MRE

---
