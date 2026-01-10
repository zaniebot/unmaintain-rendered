```yaml
number: 647
title: Document file exclusion
type: pull_request
state: merged
author: MichaReiser
labels:
  - documentation
assignees: []
merged: true
base: main
head: micha/include-excluede
created_at: 2025-06-13T13:22:22Z
updated_at: 2025-06-18T09:10:32Z
url: https://github.com/astral-sh/ty/pull/647
synced_at: 2026-01-10T02:34:10Z
```

# Document file exclusion

---

_Pull request opened by @MichaReiser on 2025-06-13 13:22_

## Summary

Document the new `src.include` and `src.exclude` fields (not yet released)

Note: The `include` field is currently missing in the generated docs. It will exist once my overrides PR is merged (it's when I noticed that it's missing). This is hopefully before the next release.


---

_Label `documentation` added by @MichaReiser on 2025-06-13 13:22_

---

_Marked ready for review by @MichaReiser on 2025-06-13 13:34_

---

_Review comment by @sharkdp on `docs/README.md`:199 on 2025-06-14 07:28_

Maybe
```suggestion
Paths that are passed as positional arguments to `ty check` are included even if they would otherwise be ignored through `exclude` filters or an ignore-file.
```

---

_Review comment by @sharkdp on `docs/README.md`:205 on 2025-06-14 07:30_

Should this maybe describe whether or not the directory contents would be affected?

---

_Review comment by @sharkdp on `docs/README.md`:207 on 2025-06-14 07:30_

This seems like a duplication?

---

_Review comment by @sharkdp on `docs/README.md`:216 on 2025-06-14 07:32_

Maybe
```suggestion
Include patterns are anchored: The pattern `src` only includes `<project_root>/src` but not something like `<project_root>/test/src`. To include any directory named `src`, use the `**/src` prefix match. However, note that these can notably slow down the Python file discovery.
```

---

_Review comment by @sharkdp on `docs/README.md`:218 on 2025-06-14 07:35_

> unless they contain a `/`

This is interesting. I think I would expect something like `src/generated` to apply at arbitrary depths, and only something like `/src/generated` with a leading `/` to be anchored. Maybe it's worth clarifying?

I think the examples here might not be needed?
```suggestion
Exclude patterns are *not* anchored, unless they contain a `/`: `venv` excludes all directories named `venv`, at arbitrary depths.
```

---

_@sharkdp approved on 2025-06-14 07:37_

Thank you!

---

_@dhruvmanila reviewed on 2025-06-17 06:14_

---

_Review comment by @dhruvmanila on `docs/README.md`:210 on 2025-06-17 06:14_

I'm very confused by this examples (`**a`, `b**`), is there some kind of context that's required here? Would it be useful to include a directory tree as an example where this example is relevant?

---

_@dhruvmanila reviewed on 2025-06-17 06:15_

---

_Review comment by @dhruvmanila on `docs/README.md`:218 on 2025-06-17 06:15_

This is going to change with https://github.com/astral-sh/ruff/pull/18685, right?

---

_@MichaReiser reviewed on 2025-06-17 06:21_

---

_Review comment by @MichaReiser on `docs/README.md`:218 on 2025-06-17 06:21_

Yes

---

_@MichaReiser reviewed on 2025-06-18 09:04_

---

_Review comment by @MichaReiser on `docs/README.md`:210 on 2025-06-18 09:04_

I don't think a directory tree would help here. These patterns are not valid syntax. `**` can only be used like `/**/a` but not inside a path component

---

_Merged by @MichaReiser on 2025-06-18 09:10_

---

_Closed by @MichaReiser on 2025-06-18 09:10_

---

_Branch deleted on 2025-06-18 09:10_

---
