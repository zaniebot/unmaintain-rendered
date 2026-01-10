```yaml
number: 20926
title: "[semantic error tests]: refactor semantic error tests to separate files"
type: pull_request
state: merged
author: 11happy
labels:
  - internal
  - testing
assignees: []
merged: true
base: main
head: refactor_tests
created_at: 2025-10-16T18:35:54Z
updated_at: 2025-10-27T21:28:26Z
url: https://github.com/astral-sh/ruff/pull/20926
synced_at: 2026-01-10T16:59:49Z
```

# [semantic error tests]: refactor semantic error tests to separate files

---

_Pull request opened by @11happy on 2025-10-16 18:35_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->
This PR refactors semantic error tests in each seperate file


## Test Plan

<!-- How was it tested? -->

## CC
- @ntBre 


---

_Converted to draft by @11happy on 2025-10-16 18:36_

---

_Label `internal` added by @ntBre on 2025-10-16 19:54_

---

_Comment by @ntBre on 2025-10-16 19:56_

Nice, thanks for working on this! Let me know when it's ready for review, or if you need any help. It looks like you're on the right track :)

---

_Marked ready for review by @11happy on 2025-10-22 15:45_

---

_Comment by @11happy on 2025-10-22 16:01_

CI is green : ) , its ready for review.

Thank you

---

_Comment by @github-actions[bot] on 2025-10-22 16:01_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @ntBre on `crates/ruff_linter/src/linter.rs`:1044 on 2025-10-22 20:57_

Do we need this filter? It looks like we use `test_contents_syntax_errors` in other tests in this file without the filter, so it would probably be okay to omit it.

---

_Review comment by @ntBre on `crates/ruff_linter/src/linter.rs`:1045 on 2025-10-22 20:58_

It looks like we're adding the Python version twice, once here and once above on line 1033.

---

_Review comment by @ntBre on `crates/ruff_linter/src/linter.rs`:1033 on 2025-10-22 20:59_

I think we could probably omit the `error_type` and just use the path instead. That seems closer to what we do for lint rules, and it will make the `test_case` entries a bit shorter.

Looking back at the blame, I'm actually not sure why we added `error_type` in the first place.

---

_Review comment by @ntBre on `crates/ruff_linter/src/linter.rs`:1020 on 2025-10-22 21:03_

It looks like most of these only have one Python version, so I'd probably just pass a single Python version, like we had before. And then we can duplicate the `test_case` like:

```rust
#[test_case(Path::new("some_file.py"), PythonVersion::PY311)]
#[test_case(Path::new("some_file.py"), PythonVersion::PY312)]
```

I think that looks a bit neater.

---

_@ntBre reviewed on 2025-10-22 21:06_

Thank you! I had a few small nits, but this looks great. I think this will make these tests a lot easier to work with.

---

_Label `testing` added by @ntBre on 2025-10-22 21:07_

---

_Review comment by @11happy on `crates/ruff_linter/src/linter.rs`:1044 on 2025-10-26 11:09_

done

---

_@11happy reviewed on 2025-10-26 11:09_

---

_@11happy reviewed on 2025-10-26 11:09_

---

_Review comment by @11happy on `crates/ruff_linter/src/linter.rs`:1045 on 2025-10-26 11:09_

done

---

_@11happy reviewed on 2025-10-26 11:09_

---

_Review comment by @11happy on `crates/ruff_linter/src/linter.rs`:1033 on 2025-10-26 11:09_

done

---

_@11happy reviewed on 2025-10-26 11:09_

---

_Review comment by @11happy on `crates/ruff_linter/src/linter.rs`:1020 on 2025-10-26 11:09_

done

---

_@11happy reviewed on 2025-10-27 08:56_

---

_Review comment by @11happy on `crates/ruff_linter/src/linter.rs`:1033 on 2025-10-27 08:56_

path is causing windows test to fail .

---

_Renamed from "[semantic error tests]: refactor semantic error tests to seperate files" to "[semantic error tests]: refactor semantic error tests to separate files" by @AlexWaygood on 2025-10-27 10:44_

---

_Review comment by @ntBre on `crates/ruff_linter/src/linter.rs`:965 on 2025-10-27 17:50_

Should this be 3.11? It looks like the old tests used 3.10 and 3.11, not 3.12.

---

_Review comment by @ntBre on `crates/ruff_linter/src/linter.rs`:982 on 2025-10-27 17:54_

This one was also 3.10 previously. I don't think it matters in this case, but I'd like to change as little as possible in the refactor.

---

_Review comment by @ntBre on `crates/ruff_linter/src/linter.rs`:978 on 2025-10-27 17:59_

Small nit, but should we call this file `single_starred_assignment` like the error variant?

---

_@ntBre approved on 2025-10-27 18:02_

Thank you! I had a handful of additional naming/version nits, but I think this is good to go otherwise.

---

_@11happy reviewed on 2025-10-27 18:40_

---

_Review comment by @11happy on `crates/ruff_linter/src/linter.rs`:965 on 2025-10-27 18:40_

sure done

---

_@11happy reviewed on 2025-10-27 18:40_

---

_Review comment by @11happy on `crates/ruff_linter/src/linter.rs`:982 on 2025-10-27 18:40_

done

---

_@11happy reviewed on 2025-10-27 18:40_

---

_Review comment by @11happy on `crates/ruff_linter/src/linter.rs`:978 on 2025-10-27 18:40_

done

---

_@ntBre approved on 2025-10-27 21:14_

---

_Merged by @ntBre on 2025-10-27 21:18_

---

_Closed by @ntBre on 2025-10-27 21:18_

---
