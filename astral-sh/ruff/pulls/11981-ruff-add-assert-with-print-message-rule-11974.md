```yaml
number: 11981
title: "[`ruff`] Add `assert-with-print-message` rule (#11974)"
type: pull_request
state: merged
author: denwong47
labels:
  - rule
  - preview
  - great writeup
assignees: []
merged: true
base: main
head: assert-with-print-expression
created_at: 2024-06-22T12:22:30Z
updated_at: 2024-06-23T20:56:04Z
url: https://github.com/astral-sh/ruff/pull/11981
synced_at: 2026-01-10T21:56:00Z
```

# [`ruff`] Add `assert-with-print-message` rule (#11974)

---

_Pull request opened by @denwong47 on 2024-06-22 12:22_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Addresses #11974 to add a `RUF` rule to replace `print` expressions in `assert` statements with the inner message.

An autofix is available, but is considered unsafe as it changes behaviour of the execution, notably:
- removal of the printout in `stdout`, and
- `AssertionError` instance containing a different message.

While the detection of the condition is a straightforward matter, deciding how to resolve the print arguments into a string literal can be a relatively subjective matter. The implementation of this PR chooses to be as tolerant as possible, and will attempt to reformat any number of `print` arguments containing single or concatenated strings or variables into either a string literal, or a f-string if any variables or placeholders are detected.

## Test Plan

`cargo test`.

## Examples
For ease of discussion, this is the diff for the tests:

```diff
 # Standard Case
 # Expects:
 # - single StringLiteral
-assert True, print("This print is not intentional.")
+assert True, "This print is not intentional."
 
 # Concatenated string literals
 # Expects:
 # - single StringLiteral
-assert True, print("This print" " is not intentional.")
+assert True, "This print is not intentional."
 
 # Positional arguments, string literals
 # Expects:
 # - single StringLiteral concatenated with " "
-assert True, print("This print", "is not intentional")
+assert True, "This print is not intentional"
 
 # Concatenated string literals combined with Positional arguments
 # Expects:
 # - single stringliteral concatenated with " " only between `print` and `is`
-assert True, print("This " "print", "is not intentional.")
+assert True, "This print is not intentional."
 
 # Positional arguments, string literals with a variable
 # Expects:
 # - single FString concatenated with " "
-assert True, print("This", print.__name__, "is not intentional.")
+assert True, f"This {print.__name__} is not intentional."

 # Mixed brackets string literals
 # Expects:
 # - single StringLiteral concatenated with " "
-assert True, print("This print", 'is not intentional', """and should be removed""")
+assert True, "This print is not intentional and should be removed"
 
 # Mixed brackets with other brackets inside
 # Expects:
 # - single StringLiteral concatenated with " " and escaped brackets
-assert True, print("This print", 'is not "intentional"', """and "should" be 'removed'""")
+assert True, "This print is not \"intentional\" and \"should\" be 'removed'"
 
 # Positional arguments, string literals with a separator
 # Expects:
 # - single StringLiteral concatenated with "|"
-assert True, print("This print", "is not intentional", sep="|")
+assert True, "This print|is not intentional"
 
 # Positional arguments, string literals with None as separator
 # Expects:
 # - single StringLiteral concatenated with " "
-assert True, print("This print", "is not intentional", sep=None)
+assert True, "This print is not intentional"
 
 # Positional arguments, string literals with variable as separator, needs f-string
 # Expects:
 # - single FString concatenated with "{U00A0}"
-assert True, print("This print", "is not intentional", sep=U00A0)
+assert True, f"This print{U00A0}is not intentional"
 
 # Unnecessary f-string
 # Expects:
 # - single StringLiteral
-assert True, print(f"This f-string is just a literal.")
+assert True, "This f-string is just a literal."
 
 # Positional arguments, string literals and f-strings
 # Expects:
 # - single FString concatenated with " "
-assert True, print("This print", f"is not {'intentional':s}")
+assert True, f"This print is not {'intentional':s}"
 
 # Positional arguments, string literals and f-strings with a separator
 # Expects:
 # - single FString concatenated with "|"
-assert True, print("This print", f"is not {'intentional':s}", sep="|")
+assert True, f"This print|is not {'intentional':s}"
 
 # A single f-string
 # Expects:
 # - single FString
-assert True, print(f"This print is not {'intentional':s}")
+assert True, f"This print is not {'intentional':s}"
 
 # A single f-string with a redundant separator
 # Expects:
 # - single FString
-assert True, print(f"This print is not {'intentional':s}", sep="|")
+assert True, f"This print is not {'intentional':s}"
 
 # Complex f-string with variable as separator
 # Expects:
 # - single FString concatenated with "{U00A0}", all placeholders preserved
 condition = "True is True"
 maintainer = "John Doe"
-assert True, print("Unreachable due to", condition, f", ask {maintainer} for advice", sep=U00A0)
+assert True, f"Unreachable due to{U00A0}{condition}{U00A0}, ask {maintainer} for advice"
 
 # Empty print
 # Expects:
 # - `msg` entirely removed from assertion
-assert True, print()
+assert True
 
 # Empty print with separator
 # Expects:
 # - `msg` entirely removed from assertion
-assert True, print(sep=" ")
+assert True
 
 # Custom print function that actually returns a string
 # Expects:
@@ -100,4 +100,4 @@
 # Use of `builtins.print`
 # Expects:
 # - single StringLiteral
-assert True, builtins.print("This print should be removed.")
+assert True, "This print should be removed."
```

## Known Issues

The current implementation resolves all arguments and separators of the `print` expression into a single string, be it `StringLiteralValue::single` or a `FStringValue::single`. This:

- potentially joins together strings well beyond the ideal character limit for each line, and
- does not preserve multi-line strings in their original format, in favour of a single line `"...\n...\n..."` format.

These are purely formatting issues only occurring in unusual scenarios.

Additionally, the autofix will tolerate `print` calls that were previously invalid:

```python
assert True, print("this", "should not be allowed", sep=42)
```

This will be transformed into
```python
assert True, f"this{42}should not be allowed"
```
which some could argue is an alteration of behaviour.

---

_Renamed from "[`ruff`] Add `assert-with-print-expression` flag (#11974)" to "[`ruff`] Add `assert-with-print-expression` rule (#11974)" by @denwong47 on 2024-06-22 12:37_

---

_Review requested from @charliermarsh by @charliermarsh on 2024-06-23 15:01_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-06-23 15:01_

---

_Label `rule` added by @charliermarsh on 2024-06-23 15:01_

---

_Comment by @charliermarsh on 2024-06-23 15:01_

Thanks! I will review.

---

_@charliermarsh approved on 2024-06-23 16:44_

This is excellent work, nicely done.

---

_Comment by @MichaReiser on 2024-06-23 16:48_

I think I would rename the rule to `assert-with-print-message`, it's otherwise unclear if it also captures `assert print("test")`

---

_Comment by @charliermarsh on 2024-06-23 16:50_

Makes sense to me.

---

_Label `preview` added by @charliermarsh on 2024-06-23 16:50_

---

_Merged by @charliermarsh on 2024-06-23 16:54_

---

_Closed by @charliermarsh on 2024-06-23 16:54_

---

_Renamed from "[`ruff`] Add `assert-with-print-expression` rule (#11974)" to "[`ruff`] Add `assert-with-print-message` rule (#11974)" by @MichaReiser on 2024-06-23 17:09_

---

_Label `great writeup` added by @zanieb on 2024-06-23 17:25_

---

_Branch deleted on 2024-06-23 20:56_

---
