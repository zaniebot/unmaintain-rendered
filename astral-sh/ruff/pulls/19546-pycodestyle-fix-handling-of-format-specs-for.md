```yaml
number: 19546
title: "[`pycodestyle`] Fix handling of format specs for `invalid-escape-sequence (W605)`"
type: pull_request
state: open
author: MeGaGiGaGon
labels:
  - bug
assignees: []
base: main
head: fix-format-spec-W605-raw-fix
created_at: 2025-07-25T01:34:03Z
updated_at: 2025-08-04T13:23:33Z
url: https://github.com/astral-sh/ruff/pull/19546
synced_at: 2026-01-10T17:52:17Z
```

# [`pycodestyle`] Fix handling of format specs for `invalid-escape-sequence (W605)`

---

_Pull request opened by @MeGaGiGaGon on 2025-07-25 01:34_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

This PR fixes #11491, cleaning up the last of the interpolated string related issues with [invalid-escape-sequence (W605)](https://docs.astral.sh/ruff/rules/invalid-escape-sequence/#invalid-escape-sequence-w605). Interpolated string format specs behave very strangely, in that their contents are not affected by the raw-ness of their containing string, unless they are adjacent to a `{` or `}`, in which case they are affected.

The fix functions by separating out all of the non-raw-affected escapes into their own vec, since the only option for them is to be escaped via adding another `\`. The edge cases of `{` and `}` adjacent `\`s are left with the rest of the invalid escapes.

Passing the inside-format-spec-ness via a bool is a bit ugly, but I could not think of a better solution. Ideas welcome.

## Test Plan

<!-- How was it tested? -->

Added 4 new test cases for the non-`{`-`}` edge cases, and 2 other new test cases for those edge cases.

---

_Comment by @github-actions[bot] on 2025-07-25 01:43_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Renamed from "[`pycodestyle`] Fix handling of format specs for `invalid-escape-sequence (W605`" to "[`pycodestyle`] Fix handling of format specs for `invalid-escape-sequence (W605)`" by @MeGaGiGaGon on 2025-07-25 02:22_

---

_Review requested from @ntBre by @ntBre on 2025-07-25 21:57_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pycodestyle/rules/invalid_escape_sequence.rs`:274 on 2025-07-29 20:37_

```suggestion
/// affected by whether or not the string is raw.
```

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pycodestyle/rules/invalid_escape_sequence.rs`:209 on 2025-07-29 20:41_

Will this check handle escaped braces like `f"{{x}}"`?

---

_@ntBre reviewed on 2025-07-29 20:44_

Thanks! This looks plausible to me in a quick skim. I just had one tiny docs nit and a question about doubled braces.

@MichaReiser or @dylwil3 if either of you remembers much context from #14748, you might be better suited to review this. If not, I'll do a deeper dive here myself.

---

_Label `bug` added by @ntBre on 2025-07-29 20:45_

---

_@MeGaGiGaGon reviewed on 2025-07-29 21:44_

---

_Review comment by @MeGaGiGaGon on `crates/ruff_linter/src/rules/pycodestyle/rules/invalid_escape_sequence.rs`:209 on 2025-07-29 21:44_

Format specs operate under different rules for whatever reason, so `{{` escapes don't work in format specs. `{{`s outside format specs shouldn't be affected because of the `in_format_spec` check.
```pycon
>>> f"{1: {{ }"
  File "<python-input-0>", line 1
    f"{1: {{ }"
              ^
SyntaxError: f-string: expecting '}'
```

---

_@ntBre reviewed on 2025-07-29 22:21_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pycodestyle/rules/invalid_escape_sequence.rs`:209 on 2025-07-29 22:21_

Ohhh, gotcha. I forgot only the part after the colon was the format spec. I was thinking it was the whole set of curly braces.

---

_Review requested from @dylwil3 by @MichaReiser on 2025-07-30 06:58_

---

_Comment by @MichaReiser on 2025-07-30 06:58_

Not really. But it makes sense that @dylwil3 takes a look

---

_Review comment by @dylwil3 on `crates/ruff_linter/resources/test/fixtures/pycodestyle/W605_1.py`:144 on 2025-08-01 17:30_

Could you add a test for something like this?

```python
rf"xyz{a:\XFF}"
```

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/pycodestyle/rules/invalid_escape_sequence.rs`:114 on 2025-08-01 17:31_

Might be a little clearer at the call site to make this an enum with two variants

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/pycodestyle/rules/invalid_escape_sequence.rs`:327 on 2025-08-01 17:33_

Can you give this a more descriptive name and/or a doc-comment to differentiate it from `check`? Something to indicate that this function is specifically for adding escapes rather than making strings raw.

---

_@dylwil3 requested changes on 2025-08-01 17:55_

Thanks this looks good! A few suggestions. Also, can you point me to some documentation around the comment you made about format specifiers being the same whether or not there is a raw prefix on the f-string?

---

_Comment by @MeGaGiGaGon on 2025-08-01 21:44_

> Thanks this looks good! A few suggestions. Also, can you point me to some documentation around the comment you made about format specifiers being the same whether or not there is a raw prefix on the f-string?

I did some more digging, and it looks like there is no documentation on this, see https://github.com/python/cpython/issues/137314 .

So looks like the fix might need to behave differently dependent on the python version.

---

_Comment by @dylwil3 on 2025-08-03 15:06_

Thank you for the thorough investigation! It looks like there is now an open PR to change the behavior for some subset of Python versions where the change is still allowed to be made (https://github.com/python/cpython/pull/137328). So let's wait to see how that pans out and then revisit this - I've subscribed to that PR so hopefully will be pinged if and when it's merged.

---

_Comment by @dylwil3 on 2025-08-03 15:09_

That said - if you are optimistic that the PR will merge in (as I am), then you could make the changes now. It seems like after the PR is merged, the raw prefix will behave as expected _except_ in version 3.12, where your custom logic is required.

Update: PR has merged to main, and will be backported to 3.13 and 3.14. There was a leftover question of whether it would also be backported to 3.12, so we'll see if in the end we don't need any version-specific logic after all.

---

_Comment by @dylwil3 on 2025-08-04 13:23_

@MeGaGiGaGon sorry for so many pings - it looks like the CPython changes are done and Python 3.12 is the only version that has this strange behavior going forward. Let me know if you need help implementing any of the updates, otherwise feel free to ping me when it's ready for re-review. Thank you!

---
