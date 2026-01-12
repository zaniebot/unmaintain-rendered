```yaml
number: 16075
title: Option to include vcs tracked files in source distribution with uv build backend
type: issue
state: open
author: wpk-nist-gov
labels:
  - enhancement
assignees: []
created_at: 2025-09-30T13:27:15Z
updated_at: 2025-12-16T13:57:06Z
url: https://github.com/astral-sh/uv/issues/16075
synced_at: 2026-01-12T16:02:23Z
```

# Option to include vcs tracked files in source distribution with uv build backend

---

_@wpk-nist-gov_

### Summary

It would be great if there was an option to the uv build backend to include all files tracked by git/vcs in the generated source distribution.  This is what setuptools_scm, hatch, etc, do.  

Related to #14037, but that issue appears focused on package version coming from vcs tags.

Thank you so much for uv!

### Example

Instead of doing something like:

```toml
[tool.uv.build-backend]
source-include = [
    "ruff.toml",
    "docs/**",
    "tests/**",
    ...
]
source-exclude = [
   "_build",
   "*_cache",
   ...
]
```

You could use (assuming tacked files are set up correctly) something like:

```toml
[tool.uv.build-backend]
include-vcs-tracked-files = true
# source-include/source-exclude still respected
source-exclude = [ "examples/big-file-not-to-be-included-in-dist" ]
```

---

_Label `enhancement` added by @wpk-nist-gov on 2025-09-30 13:27_

---

_Comment by @konstin on 2025-12-16 13:57_

Currently, the source distributions from the uv build backend are intended to allow building the wheel from source, and to only include the files that are required for that build. While source distributions could be used for redistributors, many projects require a larger amount of data for running tests or building documentation, or don't track which globs are required for this at all. This can increase the size of the source distribution many times and increases the complexity of building a source distribution a lot, while only being relevant to a very small amount of consumers. Instead of relying on PyPI, I recommend redistributors to use e.g. the git tag and build from that.

---
