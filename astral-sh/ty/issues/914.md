```yaml
number: 914
title: "Can I use environment variables in `extra-paths` in `pyproject.toml`?"
type: issue
state: open
author: Social-Mean
labels:
  - configuration
  - wish
assignees: []
created_at: 2025-07-30T01:28:51Z
updated_at: 2025-11-18T16:10:34Z
url: https://github.com/astral-sh/ty/issues/914
synced_at: 2026-01-10T01:58:59Z
```

# Can I use environment variables in `extra-paths` in `pyproject.toml`?

---

_Issue opened by @Social-Mean on 2025-07-30 01:28_

### Question

`extra-paths` in `pyproject.toml` are user-provided paths that should take first priority in the module resolution. However, the examples in the documentation are specific to the paths on a particular computer:

```toml
[tool.ty.environment]
extra-paths = ["~/shared/my-search-path"]
```

Is it possible to use environment variables in the `extra-paths` list, such as `$My_Extra_Path/path/to/extra-search-path`, to achieve the same effect across different computers using the same `pyproject.toml` configuration file?

```toml
[tool.ty.environment]
extra-paths = ["$My_Extra_Path/path/to/extra-search-path"]
```

### Version

ty 0.0.1-alpha.16 (c452e53a0 2025-07-25)

---

_Label `question` added by @Social-Mean on 2025-07-30 01:28_

---

_Comment by @MichaReiser on 2025-07-30 06:31_

No, this is currently not supported. I'll keep this issue open to track this as a feature request

---

_Label `question` removed by @MichaReiser on 2025-07-30 06:31_

---

_Label `configuration` added by @MichaReiser on 2025-07-30 06:31_

---

_Label `wish` added by @MichaReiser on 2025-07-30 06:31_

---

_Added to milestone `Z post-stable` by @MichaReiser on 2025-11-14 08:37_

---

_Removed from milestone `Z post-stable` by @carljm on 2025-11-18 16:10_

---
