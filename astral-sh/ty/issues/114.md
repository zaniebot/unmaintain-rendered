```yaml
number: 114
title: "Calling `is_file_open` degrades current query to durability LOW"
type: issue
state: open
author: MichaReiser
labels:
  - performance
assignees: []
created_at: 2025-04-23T06:35:19Z
updated_at: 2025-08-15T07:00:08Z
url: https://github.com/astral-sh/ty/issues/114
synced_at: 2026-01-10T02:06:24Z
```

# Calling `is_file_open` degrades current query to durability LOW

---

_Issue opened by @MichaReiser on 2025-04-23 06:35_

We noticed in https://github.com/astral-sh/ruff/pull/17463#discussion_r2054587050 that calling `is_file_open` degrades the durability of otherwise MEDIUM or HIGH durability queries to LOW because the `Project::open_files_set` and `Project::files` fields have durability LOW. 

What this means is that we degrade the durability of type inference and semantic index queries to LOW if they have any type checking or semantic syntax errors. Which is unfortunate, because it requires that Salsa can't use the fast-path to mark the queries as "up-to-date" after a revision change. 

To some extend, this problem is similar to the one we experienced in the module resolver where module resolution first tries first-party files (that have a low durability) and only then tests for third party files. 

The easiest solution is to change `is_file_open` to early return for vendored paths and system paths that aren't subpaths of the project directory. 

---

_Label `performance` added by @MichaReiser on 2025-04-23 06:35_

---

_Renamed from "[red-knot] Calling `is_file_open` degrades current query to durability LOW" to "Calling `is_file_open` degrades current query to durability LOW" by @MichaReiser on 2025-05-07 15:25_

---

_Comment by @MichaReiser on 2025-06-17 05:16_

I think what we could do here now is to call `IncludeExcludeFilter::matches_file` before testing if the file is in the `open_files_set`. This should give us a very low false positive rate. 

The only thing we may need to investigate is if we can use a prefix filter for `ExcludeFilter` to avoid traversing all ancestors to determine if the file is excluded (similar to what we do in `IncludeFitler`). Instead, the method would return early if no prefix matcher matches. We still have to visit all ancestors if any prefix matcher matches because of negated patterns.

---

_Removed from milestone `Beta` by @MichaReiser on 2025-08-15 07:00_

---

_Added to milestone `GA` by @MichaReiser on 2025-08-15 07:00_

---
