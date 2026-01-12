```yaml
number: 10639
title: "[Python Formatter] Preserve empty lines"
type: pull_request
state: closed
author: robincaloudis
labels:
  - bug
  - formatter
assignees: []
base: main
head: rc-semi
created_at: 2024-03-27T22:15:22Z
updated_at: 2024-09-08T18:36:15Z
url: https://github.com/astral-sh/ruff/pull/10639
synced_at: 2026-01-12T15:55:32Z
```

# [Python Formatter] Preserve empty lines

---

_@robincaloudis_

## Summary
https://github.com/astral-sh/ruff/issues/9958 reported that code like
```python
b = """
    This looks like a docstring but is not in a notebook because notebooks can't be imported as a module.
    Ruff should leave it as is
""";


a = "another normal string"
```
gets formatted as
```python
b = """
    This looks like a docstring but is not in a notebook because notebooks can't be imported as a module.
    Ruff should leave it as is
""";
a = "another normal string"
```
This is unexpected and the expected behaviour of the formatter should preserve the empty lines between b and a.

## What
* Modified `lines_after` such that it respects the semicolon. In my opinion, the extension makes a lot of sense in this function as it should not break counting of lines due to a semicolon
* Test call sides
* I intentionally did not modify `lines_before` as alone standing `;`'s are not allowed by the Python syntax anyway, e.g.
  ```python
  b = """
      This looks like a docstring but is not in a notebook because notebooks can't be imported as a module.
      Ruff should leave it as is
  """;
  
  ;
  a = "another normal string"
  ```
## Test Plan
* Correct tests
* Add tests

Closes https://github.com/astral-sh/ruff/issues/9958.


---

_Review requested from @MichaReiser by @robincaloudis on 2024-03-27 22:15_

---

_@robincaloudis reviewed on 2024-03-27 22:21_

---

_Review comment by @robincaloudis on `crates/ruff_python_formatter/tests/snapshots/format@notebook_docstring.py.snap`:45 on 2024-03-27 22:21_

Interestingly, this empty like break was previously checked in. 

---

_Renamed from "Formatter: Preserve empty lines" to "[Pyton Formatter] Preserve empty lines" by @robincaloudis on 2024-03-27 22:28_

---

_Comment by @github-actions[bot] on 2024-03-27 22:42_

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

_Label `formatter` added by @charliermarsh on 2024-03-28 01:24_

---

_Comment by @robincaloudis on 2024-03-28 08:31_

Some formatting has changed in the ruff-ecosystem. Seems like the backslash("\\") is not recognized correctly with this change. Probably `lines_after_ignoring_end_of_line_trivia` needs to be adapted such that it interprets [SimpleTokenKind::Continuation](https://github.com/robincaloudis/ruff/blob/40186a26ef388ee12de38b514460dce9fd6ad1f3/crates/ruff_python_trivia/src/tokenizer.rs#L219) correctly. I will look into that. 

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/suite.rs`:363 on 2024-03-28 09:12_

I'm a bit worried about changing this to `lines_after_ignoring_end_of_line_trivia`. I haven't been able to come up with an example where it is causing troubles but my worry is about that this now counts the empty lines after a trailing comment, but `FormatTrailingComments` sometimes counts the lines up to the previous comment... where it could be possible that we duplicate the empty lines. 

There are also many existing call sites to `lines_after` that I think would need to be changed to. For example here

https://github.com/astral-sh/ruff/blob/7b6d7abc3fa81d912a07311257d8b8950692dd14/crates/ruff_python_formatter/src/statement/suite.rs#L263-L284

We need to have a closer look how the two play together. 

---

_Review comment by @MichaReiser on `crates/ruff_python_trivia/src/tokenizer.rs`:124 on 2024-03-28 09:15_

Nit
```suggestion
            matches!(token.kind, SimpleTokenKind::Newline | SimpleTokenKind::Whitespace | SimpleTokenKind::Semi)
```

---

_Review comment by @MichaReiser on `crates/ruff_python_trivia/src/tokenizer.rs`:122 on 2024-03-28 09:16_

Did you verify that all call sites require the "skipping over semicolons" behavior? I recommend adding tests that exercise each possible call site.

---

_@MichaReiser requested changes on 2024-03-28 09:18_

Thanks for looking into this. 

I'm a bit afraid of merging this without more extensive test coverage because the empty lines logic is complicated and using the right combinations of `lines_after` and `lines_before` variants is essential to avoid instabilities. 

I recommend you to add more tests that also include comments, as well as tests that cover suppressed statements with trailing semicolons

For example, the following comment is misplaced:

```python
if True:
	a = test; # comment

	# trailing end of block comment

c
```

Get's reformated to 

```python
if True:
    a = test  # comment

# trailing end of block comment

c

```

---

_Comment by @dhruvmanila on 2024-03-28 09:37_

Thank you for working on this! I agree with Micha that blank lines are hard to get right even if it seems to be working. Even if the implementation work in isolation, it breaks when mixed with other things (speaking from personal experience ;)). As suggested, testing this out thoroughly would be really helpful for us to evaluate the change.

---

_Comment by @zanieb on 2024-03-29 03:35_

I'm surprised this _never_ occurs in the current ecosystem checks. Is there a project that uses this syntax that we could test on for a real world case?

---

_Renamed from "[Pyton Formatter] Preserve empty lines" to "[Pyhton Formatter] Preserve empty lines" by @zanieb on 2024-03-29 03:36_

---

_Renamed from "[Pyhton Formatter] Preserve empty lines" to "[Python Formatter] Preserve empty lines" by @zanieb on 2024-03-29 03:36_

---

_@dhruvmanila reviewed on 2024-03-29 11:05_

---

_Review comment by @dhruvmanila on `crates/ruff_python_formatter/tests/snapshots/format@notebook_docstring.py.snap`:45 on 2024-03-29 11:05_

I think the existing behavior correct because the source type is a Jupyter Notebook which doesn't have a concept of module-level docstring. So, it's just a string followed by another string.

---

_@robincaloudis reviewed on 2024-04-04 19:17_

---

_Review comment by @robincaloudis on `crates/ruff_python_formatter/tests/snapshots/format@notebook_docstring.py.snap`:45 on 2024-04-04 19:17_

Hello @dhruvmanila, thanks for the review! Shouldn't the formatter keep the two empty lines between the two strings even though the concept of module-level docstring is missing? From what I understood so far, the existing behavior is not correct and should respect the empty lines as is.

---

_Comment by @robincaloudis on 2024-04-04 19:42_

Hi @dhruvmanila and @MichaReiser, thank you both for your review. I agree. I rethought my current approach of extending `lines_after_ignoring_end_of_line_trivia ` and decided to extend `lines_after` instead. Note that I intentionally did not extend `lines_before`. Find details in the PR description. This new approach should not mess up any corner cases, as it does not touches the current combination of `lines_after` and `lines_before`. @MichaReiser, do you mind to re-review? Thank you. 

---

_Review requested from @MichaReiser by @robincaloudis on 2024-04-05 12:38_

---

_Review requested from @dhruvmanila by @robincaloudis on 2024-04-05 12:38_

---

_Review comment by @MichaReiser on `crates/ruff_python_trivia/src/tokenizer.rs`:79 on 2024-04-08 13:19_

The function comment is outdated. 

The `lines_after_ignoring_trivia` is now no longer a more specific version of `lines_after` because it doesn't implement the "skip over commas". I think we should keep the two in sync. This possibly also applies to `lines_after_ignoring_end_of_line_trivia` but I would need to dig deeper into the actual usage. 



---

_@MichaReiser reviewed on 2024-04-08 13:20_

Thanks for following up on this PR.

Would you mind taking a look at why 

```
if True:
	a = test; # comment

	# trailing end of block comment

c
```

gets formatted differently when the semicolon is present? We don't need to fix it as part of this PR but I would like to understand the reason for it (to know if it makes sense to address it in a separate PR). 

Is there any github project that has some usage of semicolons in Python? I would feel more comfortable when we could test the impact of this change on a larger code base.

---

_Label `bug` added by @MichaReiser on 2024-04-08 13:21_

---

_Comment by @MichaReiser on 2024-09-08 18:36_

I'm going to close this PR as it has become stale. Please feel free to re-open if you plan to keep working on it.

---

_Closed by @MichaReiser on 2024-09-08 18:36_

---
