```yaml
number: 1469
title: "Add `--force-exclude` option to CLI"
type: issue
state: closed
author: MichaReiser
labels:
  - cli
assignees: []
created_at: 2025-11-03T12:54:58Z
updated_at: 2025-12-20T09:03:42Z
url: https://github.com/astral-sh/ty/issues/1469
synced_at: 2026-01-12T15:54:25Z
```

# Add `--force-exclude` option to CLI

---

_@MichaReiser_

ty checks every path specified on the CLI even if they match an exclude pattern. This is 
because explicitly passed paths are considered explicit than an exclusion pattern. 

This behavior generally matches expectation but it's problematic when integrating ty with other tools that aren't aware of the exclusions. For example, pre-commit (also see https://github.com/astral-sh/ty/issues/269), passes all changed files to ty, including paths excluded in ty.

We should add an option similar to Ruff's [`--force-exclude`](https://github.com/astral-sh/ty/issues/269) that gives exclude patterns higher precedence over the paths passed on the CLI, enforcing that the exclusion rules are respected even for files passed on the CLI



---

_Label `cli` added by @MichaReiser on 2025-11-03 12:55_

---

_Added to milestone `Stable` by @MichaReiser on 2025-11-14 08:23_

---

_Comment by @jimmyt857 on 2025-12-18 16:35_

Bazel is another example of a tool that typically passes files as individual paths, and therefore would benefit from this feature.

Thank you for all the incredible work!

---

_Comment by @MichaReiser on 2025-12-18 17:24_

Good to know. This should be very easy to implement.

---

_Closed by @MichaReiser on 2025-12-20 09:03_

---
