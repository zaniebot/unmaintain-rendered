```yaml
number: 17163
title: "[red-knot] Fix `str(…)` calls"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/fix-str-calls
created_at: 2025-04-03T07:51:46Z
updated_at: 2025-04-03T17:44:10Z
url: https://github.com/astral-sh/ruff/pull/17163
synced_at: 2026-01-10T19:40:37Z
```

# [red-knot] Fix `str(…)` calls

---

_Pull request opened by @sharkdp on 2025-04-03 07:51_

## Summary

The existing signature for `str` calls had various problems, one of which I noticed while looking at some ecosystem projects (`scrapy`, added as a project to mypy_primer in this PR).

## Test Plan

- New tests for `str(…)` calls.
- Observed reduction of false positives in ecosystem checks

---

_Label `red-knot` added by @sharkdp on 2025-04-03 07:51_

---

_Review requested from @carljm by @sharkdp on 2025-04-03 07:51_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-04-03 07:51_

---

_Review requested from @dcreager by @sharkdp on 2025-04-03 07:51_

---

_Comment by @github-actions[bot] on 2025-04-03 07:54_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
scrapy (https://github.com/scrapy/scrapy)
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/scrapy/tests/test_http2_client_protocol.py:104:41: No overload of class `str` matches arguments
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/scrapy/tests/test_http2_client_protocol.py:104:60: No overload of class `str` matches arguments
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/scrapy/tests/test_http2_client_protocol.py:159:26: No overload of class `str` matches arguments
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/scrapy/tests/test_http2_client_protocol.py:159:45: No overload of class `str` matches arguments
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/scrapy/tests/test_http2_client_protocol.py:172:21: No overload of class `str` matches arguments
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/scrapy/tests/test_http2_client_protocol.py:172:40: No overload of class `str` matches arguments
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/scrapy/tests/test_http2_client_protocol.py:340:31: No overload of class `str` matches arguments
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/scrapy/tests/test_http2_client_protocol.py:354:25: No overload of class `str` matches arguments
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/scrapy/tests/test_http2_client_protocol.py:356:50: No overload of class `str` matches arguments
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/scrapy/tests/test_http2_client_protocol.py:576:31: No overload of class `str` matches arguments
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/scrapy/tests/test_http2_client_protocol.py:691:43: No overload of class `str` matches arguments
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/scrapy/tests/test_http2_client_protocol.py:694:24: No overload of class `str` matches arguments
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/scrapy/tests/test_http2_client_protocol.py:694:24: No overload of class `str` matches arguments
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/scrapy/tests/test_http2_client_protocol.py:694:24: No overload of class `str` matches arguments
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/scrapy/tests/test_http2_client_protocol.py:694:41: No overload of class `str` matches arguments
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/scrapy/tests/test_http2_client_protocol.py:694:41: No overload of class `str` matches arguments
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/scrapy/tests/test_http2_client_protocol.py:694:41: No overload of class `str` matches arguments
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/scrapy/scrapy/core/http2/stream.py:193:27: No overload of class `str` matches arguments
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/scrapy/scrapy/core/http2/stream.py:194:30: No overload of class `str` matches arguments
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/scrapy/scrapy/core/http2/stream.py:235:25: No overload of class `str` matches arguments
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/scrapy/scrapy/core/http2/stream.py:246:33: No overload of class `str` matches arguments
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/scrapy/scrapy/core/http2/stream.py:467:21: No overload of class `str` matches arguments
- Found 1523 diagnostics
+ Found 1501 diagnostics

```
</details>


---

_@AlexWaygood reviewed on 2025-04-03 10:46_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/call/builtins.md`:53 on 2025-04-03 10:46_

I can tell what you had for breakfast this morning!

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:2877 on 2025-04-03 10:52_

interesting: these overloads differ from typeshed's overloads in that `str(encoding='utf8')` is permitted by these overloads, but not by typeshed's. It's hard to say whether it should or shouldn't be permitted, really: it works at runtime, but arguably the type checker would be doing the user a favour by telling the user off for doing it, since there's no reason why it would ever be useful (it's almost certainly indicative of a user error)

---

_@AlexWaygood approved on 2025-04-03 10:52_

thanks!

---

_@sharkdp reviewed on 2025-04-03 11:18_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:2877 on 2025-04-03 11:18_

Oh, right. I didn't realize that `object` is required in the second overload in typeshed. Should we just ignore the discrepancy? Change it in Red Knot? Change it in typeshed?

I agree that it's useless to do `str(encoding='utf-8')`, but it is valid at runtime, clearly specified as such in the [documentation](https://docs.python.org/3/library/stdtypes.html#str), and seemingly harmless. I also doubt that we would catch actual errors by disallowing it. So I'm inclined to say that it should be changed in typeshed?

---

_@AlexWaygood reviewed on 2025-04-03 11:23_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:2877 on 2025-04-03 11:23_

hmm, that's a fair point about the documentation. I still weakly prefer the typeshed stubs as they are: I just can't think of any reason why a user would want to write `str(encoding='utf8')` when they could just as well write `str()` or `""`. I think the only reason why it would ever appear in a codebase would be as a result of a typo or a bad codemod, so I'd prefer for type checkers to complain about it.

I really don't have a strong opinion, though! If you feel like sending a PR to typeshed, feel free -- the other typeshed maintainers might feel differently 😄

---

_@sharkdp reviewed on 2025-04-03 11:26_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:2877 on 2025-04-03 11:26_

I don't feel strongly about it, and not really familiar with typeshed's policies when it comes to questions like this, so I guess it's fine to just leave it as a discrepancy.

---

_Merged by @sharkdp on 2025-04-03 11:26_

---

_Closed by @sharkdp on 2025-04-03 11:26_

---

_Branch deleted on 2025-04-03 11:26_

---

_@carljm reviewed on 2025-04-03 17:42_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:2877 on 2025-04-03 17:42_

I think typeshed should be changed: this is a classic case of "what should a type checker report" vs "what should a linter report". Perfectly reasonable to write a lint rule saying "this call is silly", but IMO not reasonable for a type checker to claim the call is a type error, when it clearly is not.

---

_@carljm reviewed on 2025-04-03 17:43_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:2877 on 2025-04-03 17:43_

(But "thinking typeshed should be changed" is as far as I will take this, I don't care enough to submit a PR and follow through on debating it 😆 )

---

_@AlexWaygood reviewed on 2025-04-03 17:44_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:2877 on 2025-04-03 17:44_

> Perfectly reasonable to write a lint rule saying "this call is silly", but IMO not reasonable for a type checker to claim the call is a type error, when it clearly is not.

hmm, sounds like there could be a gap in the market for a type-aware linter here 🤔

---
