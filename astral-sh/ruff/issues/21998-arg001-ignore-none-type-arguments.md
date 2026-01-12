```yaml
number: 21998
title: ARG001 - Ignore None type arguments
type: issue
state: closed
author: Philipp-Thiel
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2025-12-16T09:11:40Z
updated_at: 2026-01-05T12:49:53Z
url: https://github.com/astral-sh/ruff/issues/21998
synced_at: 2026-01-12T15:54:58Z
```

# ARG001 - Ignore None type arguments

---

_@Philipp-Thiel_

### Summary

Pytest fixtures can be used for side effects and reasonably often those fixtures do not need to return anything, but are still included in the test relying on it. Can function arguments with type None be excluded from ARG001, as they can not in any meaningful way be "used" anyway?

If you are interested in this change, I can implement it.

---

_Comment by @ntBre on 2025-12-16 14:13_

Do you mean a parameter annotated with `None`?

```py
import pytest

@pytest.fixture
def some_fixture(): ...

def test_something(some_fixture: None):
    do_something()
```

It sounds kind of interesting, but it also seems like an unintuitive way to suppress the error and not too much different from using a more clear `noqa` comment, at least to me. But I'm curious what others think.

cc @amyreese 

---

_Label `question` added by @ntBre on 2025-12-16 14:13_

---

_Comment by @Philipp-Thiel on 2025-12-16 14:31_

Exactly like that.

I'm a bit torn on the unintuitive point. I mean, what else would the point of adding an argument that can only take one value be? Using it as an argument directly makes little sense.

---

_Comment by @ntBre on 2025-12-16 15:25_

Sorry I should have expanded a bit more. I just meant that it would feel non-obvious to me, if I saw an ARG001 diagnostic, that I could annotate the parameter as `None` to suppress the diagnostic. Usually the fix is to prefix the parameter name with `_` (or whatever [lint.dummy-variable-rgx](https://docs.astral.sh/ruff/settings/#lint_dummy-variable-rgx) matches), but that would require changing the name of the fixture in this case.

But maybe that pattern is common in other contexts or only feels unintuitive to me.

---

_Comment by @amyreese on 2025-12-16 17:53_

I've never seen this sort of pattern used outside of pytest, and I would generally expect a parameter annotated as `None` to actually cause a type error in a well-typed codebase (that wasn't depending on magic like pytest to call/pass arguments).

I'd be more inclined to say the "correct" fix is to disable ARG001 on your test cases, using something like [`per-file-ignores`](https://docs.astral.sh/ruff/settings/#lint_per-file-ignores), rather than adding edge cases to the rule itself.

---

_Comment by @Philipp-Thiel on 2025-12-17 07:59_

> I just meant that it would feel non-obvious to me, if I saw an ARG001 diagnostic, that I could annotate the parameter as None to suppress the diagnostic.

Ah that makes sense. I was thinking about it from the other side, already having a mostly typed codebase and wanting to introduce the rule to it. It's understandable though if you don't want to introduce edge cases for that.

> I'd be more inclined to say the "correct" fix is to disable ARG001 on your test cases, using something like [per-file-ignores](https://docs.astral.sh/ruff/settings/#lint_per-file-ignores), rather than adding edge cases to the rule itself.

I'd prefer not to blanket disable the rule for tests, but I'm looking into that.

---

_Comment by @Philipp-Thiel on 2025-12-17 16:28_

OK, so I spent the day trying stuff and here are my results:

Trying to exclude the fixtures based on names has not really worked out. Since fixtures can but don't have to return values, sometimes things called "fixture" should be excluded, sometimes not. I tried some other regexes but, at least in our code base, there is no consistent scheme that would allow me to just select the side effect fixtures.

The issue I have with excluding the lint for the tests is that while applying the rule to the tests I found quite a few places where duplicate work was done, because fixture returns were unused. So I would love to have the rule on tests, but it turns out way too many false positives for that to be feasible.

That basically returns me to the start. The thing I want is for the linter to ignore variables with a None type annotation, because that is the only way I found to identify fixtures that exclusively produce side effects. 
If that is not a compelling reason for you, feel free to close the issue.

---

_Comment by @ntBre on 2025-12-17 17:08_

Thanks for looking into it. I'll label this explicitly as `needs-decision` and we can see what others think or if they've also run into this. I'm still not totally convinced we should skip parameters annotated with `None`, but I also empathize and think it would be nice to do something here.

Another option that comes to mind is skipping arguments that match `@pytest.fixture`-decorated functions, but this would require multi-file analysis that Ruff doesn't currently support unless they're defined in the same file.

One other workaround you could try is to apply `noqa` comments automatically with `ruff check --add-noqa`. That's a bit more targeted than ignoring the rule outright but could also cause a lot of changes if you use this pattern a lot.

---

_Label `question` removed by @ntBre on 2025-12-17 17:08_

---

_Label `rule` added by @ntBre on 2025-12-17 17:08_

---

_Label `needs-decision` added by @ntBre on 2025-12-17 17:08_

---

_Comment by @Philipp-Thiel on 2025-12-17 23:08_

The fixture solution seems technically more difficult and also prone to some false negatives, but would be a major improvement.

---

_Comment by @ntBre on 2025-12-22 21:06_

I realized today that we have a somewhat related rule in [pytest-fixture-param-without-value (PT019)](https://docs.astral.sh/ruff/rules/pytest-fixture-param-without-value/#pytest-fixture-param-without-value-pt019) that recommends using `@pytest.mark.usefixtures` ([pytest docs](https://docs.pytest.org/en/stable/how-to/fixtures.html#use-fixtures-in-classes-and-modules-with-usefixtures)). That could be another workaround here that also seems to be recommended by pytest.

---

_Comment by @Philipp-Thiel on 2026-01-05 10:53_

Hey, that looks pretty good. Still lots of work fixing all our cases, but a lot nicer.

I'll try implementing the PT set, and then circle back to ARG, as on a second look some of our issues are not addressed by PT019, namely fixtures using fixtures returning `None`. As those cannot use `@pytest.mark.usefixtures`. Will reopen once I know more and can confidently state whether we still have issues.

---

_Closed by @Philipp-Thiel on 2026-01-05 10:53_

---
