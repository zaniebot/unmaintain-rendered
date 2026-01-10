```yaml
number: 19146
title: Add t-string fixtures for rules that do not need to be modified
type: pull_request
state: merged
author: dylwil3
labels:
  - internal
assignees: []
merged: true
base: main
head: no-change-t-string-rules
created_at: 2025-07-04T15:34:40Z
updated_at: 2025-07-14T14:46:32Z
url: https://github.com/astral-sh/ruff/pull/19146
synced_at: 2026-01-10T18:33:12Z
```

# Add t-string fixtures for rules that do not need to be modified

---

_Pull request opened by @dylwil3 on 2025-07-04 15:34_

I used a script to attempt to identify those rules with the following property: changing f-strings to t-strings in the corresponding fixture altered the number of lint errors emitted. In other words, those rules for which f-strings and t-strings are not treated the same in the current implementation.

This PR documents the subset of such rules where this is fine and no changes need to be made to the implementation of the rule. Mostly these are the rules where it is relevant that an f-string evaluates to type `str` at runtime whereas t-strings do not.

In theory many of these fixtures are not super necessary - it's unlikely t-strings would be used for most of these. However, the internal handling of t-strings is tightly coupled with that of f-strings, and may become even more so as we implement the upcoming changes due to https://github.com/python/cpython/pull/135996 . So I'd like to keep these around as regression tests.

Note: The `flake8-bandit` fixtures were already added during the original t-string implementation.

| Rule(s) | Reason |
| --- | --- |
| [`unused-method-argument` (`ARG002`)](https://docs.astral.sh/ruff/rules/unused-method-argument/#unused-method-argument-arg002) | f-strings exempted for msg in `NotImplementedError` not relevant for t-strings | 
| [`logging-f-string` (`G004`)](https://docs.astral.sh/ruff/rules/logging-f-string/#logging-f-string-g004) | t-strings cannot be used here |
| [`f-string-in-get-text-func-call` (`INT001`)](https://docs.astral.sh/ruff/rules/f-string-in-get-text-func-call/#f-string-in-get-text-func-call-int001) | rule justified by eager evaluation of interpolations |
| [`flake8-bandit`](https://docs.astral.sh/ruff/rules/#flake8-bandit-s)| rules justified by eager evaluation of interpolations |
| [`single-string-slots` (`PLC0205`)](https://docs.astral.sh/ruff/rules/single-string-slots/#single-string-slots-plc0205) | t-strings cannot be slots in general |
| [`unnecessary-encode-utf8` (`UP012`)](https://docs.astral.sh/ruff/rules/unnecessary-encode-utf8/#unnecessary-encode-utf8-up012) | cannot encode t-strings |
| [`no-self-use` (`PLR6301`)](https://docs.astral.sh/ruff/rules/no-self-use/#no-self-use-plr6301) | f-strings exempted for msg in NotImplementedError not relevant for t-strings |
| [`pytest-raises-too-broad` (`PT011`)](https://docs.astral.sh/ruff/rules/pytest-raises-too-broad/) / [`pytest-fail-without-message` (`PT016`)](https://docs.astral.sh/ruff/rules/pytest-fail-without-message/#pytest-fail-without-message-pt016) / [`pytest-warns-too-broad` (`PT030`)](https://docs.astral.sh/ruff/rules/pytest-warns-too-broad/#pytest-warns-too-broad-pt030) | t-strings cannot be empty or used as messages |
| [`assert-on-string-literal` (`PLW0129`)](https://docs.astral.sh/ruff/rules/assert-on-string-literal/#assert-on-string-literal-plw0129) | t-strings are not strings and cannot be empty |
| [`native-literals` (`UP018`)](https://docs.astral.sh/ruff/rules/native-literals/#native-literals-up018) | t-strings are not native literals | 

---

_Label `internal` added by @dylwil3 on 2025-07-04 15:34_

---

_Comment by @github-actions[bot] on 2025-07-04 15:43_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Marked ready for review by @dylwil3 on 2025-07-04 18:23_

---

_@dylwil3 reviewed on 2025-07-04 18:29_

---

_Review comment by @dylwil3 on `crates/ruff_linter/resources/test/fixtures/pylint/single_string_slots.py`:39 on 2025-07-04 18:29_

Somewhat perversely someone _could_ put `__slots__ = t"abc"` since t-strings are iterables that iterate over the string and interpolation objects within them. So that would accidentally be an iterable with a single string object "abc" in it.

If people are doing that (no way they are) we should probably implement a different lint rule for it though, that just says "why have you done this?"

---

_Review comment by @MichaReiser on `crates/ruff_linter/resources/test/fixtures/pylint/assert_on_string_literal.py`:29 on 2025-07-08 06:50_

It's not a 100% clear to me if they should be excluded but I think it's a good starting point and it should be a way less common mistake and they would be a better fit for a `assert-on-always-true` or similar rule.

---

_@MichaReiser approved on 2025-07-08 06:51_

Thanks for the nice summary! 

I do find some of the reasoning a bit confusing:

* [assert-on-string-literal (PLW0129)](https://docs.astral.sh/ruff/rules/assert-on-string-literal/#assert-on-string-literal-plw0129): It's correct that t-strings don't evaluate to strings but that they're never empty is exactly what this rule is trying to catch. I also think it's confusing to say that t-strings can't be empty, t"" is an empty t-string.  I think the reasoning here is simply that they're not a string and it's unlikely that a user would make the same mistake. 
* The same applies for other rules where you reason that a t-string can't be empty. I think the reasoning there mainly is that t-strings aren't allowed/correct in those positions because they don't evaluate to a str
* [flake8-bandit](https://docs.astral.sh/ruff/rules/#flake8-bandit-s): It's unclear to me about which rules you're talking here. Is it the hardcoded password rules (which may apply?)
* [f-string-docstring (B021)](https://docs.astral.sh/ruff/rules/f-string-docstring/#f-string-docstring-b021): I'm not sure how this is different to f-strings that can't be docstrings either. We may need to create a new rule or rename the existing one but it seems the same fundamental problem applies to f- and t-strings.
* [useless-expression (B018)](https://docs.astral.sh/ruff/rules/useless-expression/#useless-expression-b018): I assume this is mainly about the allowing of f-string literals. Which I'm wondering is actually unnecessary/incorrect. But that's unrelated to this PR (and we could make it more accurate by testing if we're in a docstring position and only allow it there rather than always)


---

_@dylwil3 reviewed on 2025-07-08 12:24_

---

_Review comment by @dylwil3 on `crates/ruff_linter/resources/test/fixtures/pylint/assert_on_string_literal.py`:29 on 2025-07-08 12:24_

This is what I meant by "cannot be empty". The actual documentation for this rule justifies it as:

> An assert on a non-empty string literal will always pass, while an assert on an empty string literal will always fail.

which does not apply to t-strings.

---

_Comment by @dylwil3 on 2025-07-14 14:46_

Thanks for catching the oversight re B018/B021 - reverting those fixtures now. The rest looks correct to me - see my clarification of what I meant by "no empty t-string" (essentially that, unlike other string types, `t""` is truthy, so it's not "empty" in the same way).

The bandit rules in question are:

- [hardcoded-bind-all-interfaces (S104)](https://docs.astral.sh/ruff/rules/hardcoded-bind-all-interfaces/#hardcoded-bind-all-interfaces-s104)
- [hardcoded-temp-file (S108)](https://docs.astral.sh/ruff/rules/hardcoded-temp-file/#hardcoded-temp-file-s108)
- [hardcoded-sql-expression (S608)](https://docs.astral.sh/ruff/rules/hardcoded-sql-expression/#hardcoded-sql-expression-s608)

At the moment the hardcoded password rules do not trigger for f-strings. So if we want them to trigger for t-strings, then we should have them trigger for both. Either way that would involve changing the implementation, so out of scope for the current PR.

---

_Merged by @dylwil3 on 2025-07-14 14:46_

---

_Closed by @dylwil3 on 2025-07-14 14:46_

---
