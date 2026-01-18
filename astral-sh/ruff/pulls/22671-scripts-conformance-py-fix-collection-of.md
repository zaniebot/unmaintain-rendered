```yaml
number: 22671
title: "`scripts/conformance.py`: fix collection of diagnostics"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ci
  - ty
assignees: []
merged: true
base: main
head: alex/conformance-exclude-supporting-files
created_at: 2026-01-18T00:47:10Z
updated_at: 2026-01-18T11:52:39Z
url: https://github.com/astral-sh/ruff/pull/22671
synced_at: 2026-01-18T12:19:40Z
```

# `scripts/conformance.py`: fix collection of diagnostics

---

_@AlexWaygood_

## Summary

There were two issues with how the script was collecting diagnostics:
- It was using the pattern `path.rglob("*.py")` to find what the expected diagnostics were, but some tests in the conformance suite use `.pyi` files.
- It was running ty on the whole `conformance/tests` directory, and reporting all ty errors on that directory that weren't accompanied by `# E` comments as being false positives. But some of the files in that directory (in particular, all files starting with a leading underscore) are not actually test files -- they're just "supporting" files that are imported by the test files. Diagnostics on these files should not be considered as false positives.

## Test Plan

I commented out this section of the script on `main`:

```diff
diff --git a/scripts/conformance.py b/scripts/conformance.py
index 00cbe69181..055aa2801e 100644
--- a/scripts/conformance.py
+++ b/scripts/conformance.py
@@ -537,14 +537,14 @@ def render_summary(grouped_diagnostics: list[GroupedDiagnostics]):
 
     base_header = f"[Typing conformance results]({CONFORMANCE_DIR_WITH_README})"
 
-    if all(diag.change is Change.UNCHANGED for diag in grouped_diagnostics):
-        return dedent(
-            f"""
-            ## {base_header}
-
-            No changes detected ✅
-            """
-        )
+    # if all(diag.change is Change.UNCHANGED for diag in grouped_diagnostics):
+    #     return dedent(
+    #         f"""
+    #         ## {base_header}
+
+    #         No changes detected ✅
+    #         """
+    #     )
 
     true_pos_diff = diff_format(true_pos_change, greater_is_better=True)
     false_pos_diff = diff_format(false_pos_change, greater_is_better=False)
```

and then ran `uv run --no-project scripts/conformance.py --old-ty=./target/debug/ty --new-ty=./target/debug/ty --tests-path=../typing/conformance/tests`, with this result:

---

## [Typing conformance results](https://github.com/python/typing/blob/main/conformance/)

The percentage of diagnostics emitted that were expected errors held steady at 75.87%. The percentage of expected errors that received a diagnostic held steady at 60.50%.

### Summary

| Metric | Old | New | Diff | Outcome |
|--------|-----|-----|------|---------|
| True Positives  | 654 | 654 | +0 |  |
| False Positives | 208 | 208 | +0 |  |
| False Negatives | 427 | 427 | +0 |  |
| Total Diagnostics | 862 | 862 | +0 |  |
| Precision | 75.87% | 75.87% | +0.00% |  |
| Recall | 60.50% | 60.50% | +0.00% |  |

---

I then checked out this branch, applied the same patch above, and ran the same command, with this result:

---

## [Typing conformance results](https://github.com/python/typing/blob/main/conformance/)

The percentage of diagnostics emitted that were expected errors held steady at 77.13%. The percentage of expected errors that received a diagnostic held steady at 59.50%.

### Summary

| Metric | Old | New | Diff | Outcome |
|--------|-----|-----|------|---------|
| True Positives  | 661 | 661 | +0 |  |
| False Positives | 196 | 196 | +0 |  |
| False Negatives | 450 | 450 | +0 |  |
| Total Diagnostics | 857 | 857 | +0 |  |
| Precision | 77.13% | 77.13% | +0.00% |  |
| Recall | 59.50% | 59.50% | +0.00% |  |

---

These are results consistent with what I'd expect given that errors on files beginning with `_` are now not considered false positives, and given that `# E` comments on `.pyi` files are now collected as error assertions.

Cc. @WillDuke, if you have time to review 😃 (obviously no obligation to!)

---

_Label `ci` added by @AlexWaygood on 2026-01-18 00:47_

---

_Label `ty` added by @AlexWaygood on 2026-01-18 00:47_

---

_Comment by @astral-sh-bot[bot] on 2026-01-18 00:48_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## [Typing conformance results](https://github.com/python/typing/blob/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance/)

No changes detected ✅





---

_Comment by @astral-sh-bot[bot] on 2026-01-18 00:59_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.





---

_Review comment by @MichaReiser on `scripts/conformance.py`:300 on 2026-01-18 10:45_

I'm surprised that Python's globbing doesn't support multiple extensions, e.g. `.{py,pyi}`

---

_Review comment by @MichaReiser on `scripts/conformance.py`:303 on 2026-01-18 10:47_

Should we skip files starting with an `_`? I assume it's not strictly necessary, since they never contain any comments matching the error pattern but it feels unnecessary

---

_Review comment by @MichaReiser on `scripts/conformance.py`:355 on 2026-01-18 10:47_

Should we create a helper method that, given a path, returns whether this is a conformance test file?

---

_@MichaReiser approved on 2026-01-18 10:47_

---

_Comment by @AlexWaygood on 2026-01-18 11:47_

I've added helper functions that use the same logic as in the upstream `python/typing` repo to determine whether a file is a test file or not. I've also added a `--force-summary-table` CLI option so that it's easier to test and debug changes like this in the future (or to see where our latest conformance results are at, outside of the context of a PR that attempts to improve those results).

---

_Merged by @AlexWaygood on 2026-01-18 11:52_

---

_Closed by @AlexWaygood on 2026-01-18 11:52_

---

_Branch deleted on 2026-01-18 11:52_

---
