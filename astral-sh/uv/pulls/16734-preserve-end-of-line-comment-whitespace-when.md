```yaml
number: 16734
title: "Preserve end-of-line comment whitespace when editing `pyproject.toml`"
type: pull_request
state: merged
author: terror
labels:
  - bug
assignees: []
merged: true
base: main
head: comment-formatting
created_at: 2025-11-14T08:47:57Z
updated_at: 2025-11-20T19:15:27Z
url: https://github.com/astral-sh/uv/pull/16734
synced_at: 2026-01-12T16:12:24Z
```

# Preserve end-of-line comment whitespace when editing `pyproject.toml`

---

_@terror_

Resolves https://github.com/astral-sh/uv/issues/16719

`uv add` collapses multiple spaces before inline comments in `[project.dependencies]`, causing unrelated diffs and moving comments onto the wrong columns.  This diff captures the exact whitespace padding that preceded each end-of-line comment when parsing the array and reuses it when formatting.

---

_Comment by @codspeed-hq[bot] on 2025-11-14 09:30_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/uv/branches/terror%3Acomment-formatting?utm_source=github&utm_medium=comment&utm_content=header)

### Merging #16734 will **not alter performance**

<sub>Comparing <code>terror:comment-formatting</code> (65818fe) with <code>main</code> (7d8634b)</sub>



### Summary

`✅ 6` untouched  





---

_Comment by @terror on 2025-11-15 03:05_

> ## [CodSpeed Performance Report](https://codspeed.io/astral-sh/uv/branches/terror%3Acomment-formatting?utm_source=github&utm_medium=comment&utm_content=header)
> ### Merging #16734 will **not alter performance**
> Comparing `terror:comment-formatting` ([e868edb](https://github.com/astral-sh/uv/commit/e868edbd7f186039ce8cd6c165b6e86204b34766)) with `main` ([f5ce5b4](https://github.com/astral-sh/uv/commit/f5ce5b47c816b371f68c91f4518474401157dd53))
> 
> ### Summary
> `✅ 6` untouched

This is cool 👀 

---

_Review comment by @konstin on `crates/uv-workspace/src/pyproject_mut.rs`:1702 on 2025-11-19 11:15_

Does this mean we're inserting a whitespace when there was non before?

If we want to store padding specifically for end-of-line comments, we can store the data in the `CommentType::EndOfLine` variant and avoid the `Option<T>`.

---

_@konstin reviewed on 2025-11-19 11:15_

---

_@terror reviewed on 2025-11-19 17:15_

---

_Review comment by @terror on `crates/uv-workspace/src/pyproject_mut.rs`:1702 on 2025-11-19 17:15_

Ah good catch, I've refactored this to carry padding on the variant instead (and thus, no longer default if there's no padding).

---

_Review comment by @konstin on `crates/uv-workspace/src/pyproject_mut.rs`:1797 on 2025-11-20 16:17_

For these it doesn't matter, but anything more complex I recommend snapshots, they are easier to maintain if we ever want to change the style

---

_Review comment by @konstin on `crates/uv-workspace/src/pyproject_mut.rs`:1632 on 2025-11-20 17:24_

Sorry for coming up with this only now, but can we make those safer by using e.g. `split_once`? Indexing can panic, and it's easy to get logic such as this wrong, so we try to use non-indexing versions where they aren't too inconvenient or too slow.

---

_@konstin reviewed on 2025-11-20 17:25_

---

_@konstin approved on 2025-11-20 19:14_

Thanks!

---

_Label `bug` added by @konstin on 2025-11-20 19:14_

---

_Renamed from "Preserve inline comment spacing when editing dependency arrays" to "Preserve end-of-line comment spacing when editing dependency arrays" by @konstin on 2025-11-20 19:14_

---

_Renamed from "Preserve end-of-line comment spacing when editing dependency arrays" to "Preserve end-of-line comment whitespace when editing `pyproject.toml`" by @konstin on 2025-11-20 19:15_

---

_Merged by @konstin on 2025-11-20 19:15_

---

_Closed by @konstin on 2025-11-20 19:15_

---
