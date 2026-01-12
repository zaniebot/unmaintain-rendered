```yaml
number: 20805
title: Implement safety checks for affix removal in slice_to_remove_prefix_or_suffix rule
type: pull_request
state: open
author: tinkerer-shubh
labels:
  - fixes
assignees: []
base: main
head: furb188-unsafe-fix
created_at: 2025-10-10T14:10:19Z
updated_at: 2025-10-15T14:15:45Z
url: https://github.com/astral-sh/ruff/pull/20805
synced_at: 2026-01-12T15:57:10Z
```

# Implement safety checks for affix removal in slice_to_remove_prefix_or_suffix rule

---

_@tinkerer-shubh_

<!--
Thank you for contributing to Ruff/ty! To help us with reviewing, please ensure the following:

- Does this pull request include a clear summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull requests.)
- Does this pull request reference any relevant issues?
-->

## Summary
closes: https://github.com/astral-sh/ruff/issues/20724

This pull request implements **safety checks for affix removal** in the `slice_to_remove_prefix_or_suffix` rule.

It introduces a new `FixSafety` enum to determine whether an affix removal is *safe* or *unsafe*. Diagnostic fixes now use these safety checks when applying replacements for prefix and suffix removals.  
This prevents unsafe modifications that could alter runtime behavior — for instance, when dealing with custom sequence types that may not behave like built‑in `str` or `bytes`.

Additionally, helper functions have been added to:
- Classify expression types
- Assess safety based on semantic analysis

## Test Plan

The changes were tested by:
- Adding new test cases to `crates/ruff_linter/resources/test/fixtures/refurb/FURB188.py` to cover the new functionality and verify correct affix removal behavior.
- Updating snapshot tests in  
  `crates/ruff_linter/src/rules/refurb/snapshots/ruff_linter__rules__refurb__tests__FURB188_FURB188.py.snap`  
  to reflect the new diagnostics and unsafe fix annotations.


---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/refurb/rules/slice_to_remove_prefix_or_suffix.rs`:545 on 2025-10-10 19:06_

I don't think we need to re-implement a check like this. We have existing helpers for `str` and `bytes` here:

https://github.com/astral-sh/ruff/blob/bbd3856de8f83c31db84ef8a104d67ae104732ee/crates/ruff_python_semantic/src/analyze/typing.rs#L1065-L1073

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/refurb/rules/slice_to_remove_prefix_or_suffix.rs`:521 on 2025-10-10 19:07_

We can reuse our existing `Applicability` type for this:

https://github.com/astral-sh/ruff/blob/bbd3856de8f83c31db84ef8a104d67ae104732ee/crates/ruff_diagnostics/src/fix.rs#L14

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/refurb/rules/slice_to_remove_prefix_or_suffix.rs`:141 on 2025-10-10 19:12_

Once we use `Applicability` we can also use `Fix::applicable_edit`.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/refurb/snapshots/ruff_linter__rules__refurb__tests__FURB188_FURB188.py.snap`:25 on 2025-10-10 19:16_

We should double-check some of these. Hopefully using the `typing` helpers I mentioned below will help because this seems like a case where we could know that they are both strings. `filename` is annotated in the function signature, and the suffix is a string literal.

---

_@ntBre requested changes on 2025-10-10 19:16_

Thanks for working on this! I think we can reuse more existing code and hopefully improve the accuracy of our detection too.

---

_Label `fixes` added by @ntBre on 2025-10-10 19:17_

---

_@dscorbett reviewed on 2025-10-10 19:58_

---

_Review comment by @dscorbett on `crates/ruff_linter/src/rules/refurb/snapshots/ruff_linter__rules__refurb__tests__FURB188_FURB188.py.snap`:25 on 2025-10-10 19:58_

Although the annotation says that `isinstance(filename, str)`, it is not guaranteed that `type(filename) is str`. `filename` could be an instance of a subclass of `str` for which FURB188’s assumptions don’t hold. Example:
```console
$ cat >furb188.py <<'# EOF'
class StringX(str):
    def __getitem__(self, key, /):
        return type(self)(super().__getitem__(key))

    def parenthesize(self):
        return type(self)(f"({self})")

def remove_extension_via_slice(filename: str) -> str:
    if filename.endswith(".txt"):
        filename = filename[:-4]
    return filename

print(remove_extension_via_slice(StringX("foo.txt")).parenthesize())
# EOF

$ python furb188.py
(foo)

$ ruff --isolated check furb188.py --select FURB188 --fix
Found 1 error (1 fixed, 0 remaining).

$ python furb188.py 2>&1 | tail -n 1
AttributeError: 'str' object has no attribute 'parenthesize'
```

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/refurb/snapshots/ruff_linter__rules__refurb__tests__FURB188_FURB188.py.snap`:25 on 2025-10-10 20:30_

I could be wrong, but my initial thought is that this is overly defensive. I think many of our checks that rely on type annotations, like the `typing` helpers, would be susceptible to this kind of thing.

---

_@ntBre reviewed on 2025-10-10 20:30_

---

_@tinkerer-shubh reviewed on 2025-10-11 08:48_

---

_Review comment by @tinkerer-shubh on `crates/ruff_linter/src/rules/refurb/rules/slice_to_remove_prefix_or_suffix.rs`:545 on 2025-10-11 08:48_

ahh yess. thanks sm! addressed in https://github.com/astral-sh/ruff/pull/20805/commits/a5694f1ef0f794627090cbce94a479b1147ab210

---

_@tinkerer-shubh reviewed on 2025-10-11 08:49_

---

_Review comment by @tinkerer-shubh on `crates/ruff_linter/src/rules/refurb/rules/slice_to_remove_prefix_or_suffix.rs`:521 on 2025-10-11 08:49_

addressed in https://github.com/astral-sh/ruff/commit/a5694f1ef0f794627090cbce94a479b1147ab210

---

_@tinkerer-shubh reviewed on 2025-10-11 08:49_

---

_Review comment by @tinkerer-shubh on `crates/ruff_linter/src/rules/refurb/rules/slice_to_remove_prefix_or_suffix.rs`:141 on 2025-10-11 08:49_

addressed in https://github.com/astral-sh/ruff/commit/a5694f1ef0f794627090cbce94a479b1147ab210

---

_Review comment by @tinkerer-shubh on `crates/ruff_linter/src/rules/refurb/snapshots/ruff_linter__rules__refurb__tests__FURB188_FURB188.py.snap`:25 on 2025-10-11 08:53_

agreed. I did revisit the PR with this feedback in mind and it does make sense. thanks! 




---

_@tinkerer-shubh reviewed on 2025-10-11 08:53_

---

_Review requested from @ntBre by @MichaReiser on 2025-10-14 06:35_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/refurb/rules/slice_to_remove_prefix_or_suffix.rs`:556 on 2025-10-15 13:40_

Did you try using the existing helpers I recommended in https://github.com/astral-sh/ruff/pull/20805#discussion_r2421775622? This is quite a bit different from what I had in mind, but  I'd be curious to know if you ran into problems with that route.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/refurb/rules/slice_to_remove_prefix_or_suffix.rs`:538 on 2025-10-15 13:48_

I'm surprised that we need to special-case tuples. Shouldn't we treat any definite non-strings and non-bytes the same way?

A small variant of your `tuple_affix_should_be_suppressed` test case doesn't get suppressed, as one example:

```py
def object_affix_should_be_suppressed(text: str) -> str:
    prefix = object()
    if text.startswith(prefix):
        text = text[len(prefix):]
    return text
```

I don't think we have a good way to infer a static but non-specific type to handle  this part of the issue:

> If it is statically known that they are instances of unrelated classes, the rule should be suppressed.

I think I'd be fine with checking that the affix and suffix are both bytes or str and then marking the fix  unsafe if that's not true. We could also suppress the rule entirely in that case. There's precedent for that in another rule:

https://github.com/astral-sh/ruff/blob/b562fe030d61e65e595fbf393bd06299cab87cde/crates/ruff_linter/src/rules/pylint/rules/bad_str_strip_call.rs#L76-L96

I think we need basically the same thing here.

---

_@ntBre reviewed on 2025-10-15 14:15_

---
