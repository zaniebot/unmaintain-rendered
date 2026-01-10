```yaml
number: 19265
title: Add simple integration tests for all output formats
type: pull_request
state: merged
author: ntBre
labels:
  - internal
  - testing
assignees: []
merged: true
base: main
head: brent/output-format-tests
created_at: 2025-07-10T17:07:22Z
updated_at: 2025-07-10T21:57:50Z
url: https://github.com/astral-sh/ruff/pull/19265
synced_at: 2026-01-10T18:33:12Z
```

# Add simple integration tests for all output formats

---

_Pull request opened by @ntBre on 2025-07-10 17:07_

Summary
--

I spun this off from #19133 to be sure to get an accurate baseline before modifying any of the formats. I picked the code snippet to include a lint diagnostic with a fix, one without a fix, and one syntax error. I'm happy to expand it if there are any other kinds we want to test.

I initially passed `CONTENT` on stdin, but I was a bit surprised to notice that some of our output formats include an absolute path to the file. I switched to a `TempDir` to use the `tempdir_filter`.

Test Plan
--

New CLI tests


---

_Label `testing` added by @ntBre on 2025-07-10 17:08_

---

_Label `internal` added by @ntBre on 2025-07-10 17:08_

---

_Marked ready for review by @ntBre on 2025-07-10 17:11_

---

_Review requested from @MichaReiser by @ntBre on 2025-07-10 17:12_

---

_Comment by @github-actions[bot] on 2025-07-10 17:22_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@MichaReiser approved on 2025-07-10 17:26_

---

_Comment by @ntBre on 2025-07-10 21:52_

After a lot of trial and error, and spinning up a Windows VPS, I finally just used a regex for the paths. Some of the structured output formats are escaped or double escaped or even use forward slashes on Windows, so `tempdir_filter` wasn't sufficient. Happy to revisit this if you have a better idea, but I think this works well enough.

---

_Merged by @ntBre on 2025-07-10 21:57_

---

_Closed by @ntBre on 2025-07-10 21:57_

---

_Branch deleted on 2025-07-10 21:57_

---
