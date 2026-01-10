```yaml
number: 21087
title: "[`flynt`] Fix static-join-to-f-string when mixing raw and non-raw strings (`FLY002`)"
type: pull_request
state: open
author: danparizher
labels:
  - bug
  - fixes
assignees: []
base: main
head: fix-21082
created_at: 2025-10-27T00:56:21Z
updated_at: 2025-10-29T01:25:06Z
url: https://github.com/astral-sh/ruff/pull/21087
synced_at: 2026-01-10T16:59:49Z
```

# [`flynt`] Fix static-join-to-f-string when mixing raw and non-raw strings (`FLY002`)

---

_Pull request opened by @danparizher on 2025-10-27 00:56_

## Summary

Fix FLY002 to bail on transformations when mixing raw and non-raw strings. Mixing them can cause syntax errors, null bytes, or incorrect carriage-return behavior.

Closes #21082

## Problem Analysis

When the first string in a join is raw (`r""`) and later strings are not:
1. The code applies the raw flag to the entire result
2. Non-raw string escape sequences are not processed
3. This leads to:
   - Syntax errors (e.g., `"".join((r"", '"'))` → `r"""`)
   - Null bytes (e.g., `"".join((r"", "\0"))`)
   - Behavior changes (e.g., carriage returns or unexpected characters)

## Approach

Added a check in `build_fstring` to verify all strings have the same raw status. If not, the transformation is skipped:

1. Collect raw status for all string literals in the join
2. Return `None` early if raw statuses differ
3. Otherwise, proceed with the transformation

## Testing

Added tests covering:
- `nok14-nok18`: Mixed raw/non-raw (not transformed)
- `ok7`, `ok8`: All raw (transformed)
- `ok9`, `ok10`: All non-raw (transformed)

Ran with problematic examples from the issue; transformations are skipped as expected.


---

_Comment by @github-actions[bot] on 2025-10-27 01:07_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @dscorbett on 2025-10-27 16:05_

Joining raw and non-raw strings is not the problem. The fix for FLY002 works fine for some combinations:
```console
$ cat >fly002.py <<'# EOF'
print("".join(('"', r"@")))
print("".join((r"@", "!")))
# EOF

$ python fly002.py
"@
@!

$ ruff check --isolated fly002.py --select FLY002 --unsafe-fixes --fix
Found 2 errors (2 fixed, 0 remaining).

$ cat fly002.py
print('"')
print(r"!")

$ python fly002.py
"@
@!
```
Removing support for such combinations would need to be done in preview, but it would be more helpful to instead keep the support and fix the bugs. In particular, that means that if the fix’s output string cannot be represented as a raw string, it should be represented as a non-raw string, even if the first input string is raw.

---

_Review comment by @dscorbett on `crates/ruff_linter/resources/test/fixtures/flynt/FLY002.py`:39 on 2025-10-27 18:36_

`"\\r"` does not have a carriage return escape: it has a backslash escape followed by a literal ⟨r⟩. FLY002 doesn’t change its behavior.

---

_@dscorbett reviewed on 2025-10-27 18:36_

---

_Review comment by @dscorbett on `crates/ruff_linter/src/rules/flynt/rules/static_join_to_fstring.rs`:112 on 2025-10-28 17:09_

A line feed (`'\n'`) _can_ safely be represented in a raw string. The section after “If the result is a raw string and contains a newline, use triple quotes” handles that case.

---

_Review comment by @dscorbett on `crates/ruff_linter/src/rules/flynt/rules/static_join_to_fstring.rs`:115 on 2025-10-28 17:11_

A backslash can safely appear in a raw string as long as it is not the last character. It’s probably common for raw strings to contain backslashes (because their main point is that they handle backslashes differently) so it might be worth trying to support that.

---

_@dscorbett reviewed on 2025-10-28 17:16_

---

_Label `fixes` added by @amyreese on 2025-10-29 01:24_

---

_Label `bug` added by @amyreese on 2025-10-29 01:25_

---
