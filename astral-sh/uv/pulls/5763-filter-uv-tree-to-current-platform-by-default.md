```yaml
number: 5763
title: "Filter `uv tree` to current platform by default"
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
  - cli
  - preview
assignees: []
merged: true
base: main
head: charlie/platform
created_at: 2024-08-04T18:57:29Z
updated_at: 2024-08-05T21:51:55Z
url: https://github.com/astral-sh/uv/pull/5763
synced_at: 2026-01-10T13:31:54Z
```

# Filter `uv tree` to current platform by default

---

_Pull request opened by @charliermarsh on 2024-08-04 18:57_

## Summary

`uv tree` will now filter to the current platform by default. You can pass `--universal` to show the entire tree.

Closes https://github.com/astral-sh/uv/issues/5760.


---

_Review requested from @ibraheemdev by @charliermarsh on 2024-08-04 18:57_

---

_Review comment by @charliermarsh on `crates/uv-cli/src/lib.rs`:1714 on 2024-08-04 18:58_

I had to remove this from `uv tree`. It actually doesn't work right now because we don't write the specifiers to the lockfile. We only write the resolved versions. (So `--show-version-specifiers` was just showing `==` for every annotation.)

---

_@charliermarsh reviewed on 2024-08-04 18:58_

---

_Label `enhancement` added by @charliermarsh on 2024-08-04 19:24_

---

_Label `cli` added by @charliermarsh on 2024-08-04 19:24_

---

_Label `preview` added by @charliermarsh on 2024-08-04 19:24_

---

_Comment by @T-256 on 2024-08-04 21:53_

> ##
> Open to feedback on the name, but I want to make sure that it's a name that will still make sense when I add `--python-platform` and `--python-version` to the `uv tree` API (so you can look at the resolved dependencies for any platform)

two other single-word naming: `precise`, `focused`

> so, e.g., `--filter-current-platform` wouldn't make sense.

`filter` usually comes with object word, `filter-current-python` is more correct to me.

---

_@ibraheemdev approved on 2024-08-05 03:29_

Unsure about the name, `--filter` seems fine with https://github.com/astral-sh/uv/pull/5764.

---

_Renamed from "Add `--filter` to filter uv tree to current platform" to "Filter uv tree to current platform by default" by @charliermarsh on 2024-08-05 18:42_

---

_Renamed from "Filter uv tree to current platform by default" to "Filter `uv tree` to current platform by default" by @charliermarsh on 2024-08-05 18:42_

---

_Merged by @charliermarsh on 2024-08-05 18:51_

---

_Closed by @charliermarsh on 2024-08-05 18:51_

---

_Branch deleted on 2024-08-05 18:51_

---

_Comment by @T-256 on 2024-08-05 21:51_

> `uv tree` will now filter to the current platform by default. You can pass `--universal` to show the entire tree.

This was smart! I like it.

---
