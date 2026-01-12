```yaml
number: 16500
title: "`C416` triggers leads to `C409` bad preview behavior - false positive"
type: issue
state: open
author: Skylion007
labels:
  - bug
  - preview
assignees: []
created_at: 2025-03-04T15:23:11Z
updated_at: 2025-03-07T09:54:27Z
url: https://github.com/astral-sh/ruff/issues/16500
synced_at: 2026-01-12T15:54:55Z
```

# `C416` triggers leads to `C409` bad preview behavior - false positive

---

_@Skylion007_

### Summary

Ruff triggers bad preview behavior of C409. This behavior we chose not to enable because of issue: https://github.com/astral-sh/ruff/issues/12912 . This is definitely a false positive in this case and should not be reported here as it does lead to a significant performance regression. See this issue for discussion: https://github.com/pytorch/pytorch/pull/148412

Ruff playground for the rule here

https://play.ruff.rs/898a116a-2d60-4afc-b081-a83f5ea72bf8

cc @jansel for finding and flagging this issue

At worst, it should simplify to tuple(range(10)) instead of a useless x for x in range generator

### Version

_No response_

---

_Renamed from "C416 triggers leads to C409 bad preview behavior - false positive" to "`C416` triggers leads to `C409` bad preview behavior - false positive" by @Skylion007 on 2025-03-04 15:29_

---

_Label `bug` added by @ntBre on 2025-03-04 18:39_

---

_Label `preview` added by @ntBre on 2025-03-04 18:42_

---

_Comment by @ntBre on 2025-03-04 18:53_

Ah, without preview and accepting autofixes in the playground the code goes through these iterations:

```python
tuple([x for x in range(10)])  # initial case, C416
```

```python
tuple(list(range(10)))  # after one fix, C414
```

```python
tuple(range(10))  # after two fixes, no error
```

but with `preview = true`, C409 is also flagged and accepting that autofix leads to the problematic generator version.

Based on the name [`unnecessary-literal-within-tuple-call`](https://docs.astral.sh/ruff/rules/unnecessary-literal-within-tuple-call/#unnecessary-literal-within-tuple-call-c409), I think it might make sense if C409 were restricted to actual literals, not to comprehensions, which are covered by C416.

---

_Comment by @ntBre on 2025-03-04 19:02_

Oh I see that's the preview part of the rule now. I think I'm confused here. Is this different from #12912? I think you just want the non-preview behavior (or a way to turn it off on preview), and that seems to be the topic of discussion there. But I could be missing something.

---

_Comment by @Skylion007 on 2025-03-06 14:37_

@ntbre this works for the case where the generator is trivial and come from ranges, but it's still actually slower when a list comprehension is transformed to a generator that cannot be further simplified. See the PyTorch issue and relevant playground for more complicated examples that won't be simplified.

The range thing I provided was just an example, but it still breaks down if the range if filtered at all or chained etc.

---

_Comment by @MichaReiser on 2025-03-07 09:54_

I'm sorry, but I'm struggling to follow this issue. 

Checking the following file with flake8 (`uvx --with flake8-comprehensions flake8 comprehension.py`)

```py
tuple([x for x in range(10)])
```

gives me


```
comprehension.py:2:7: C416 Unnecessary list comprehension - rewrite using list().
```

Which is the same as what you get with Ruff. 

I do understand that C416 and C409 overlap and that the fix suggested by C409 is suboptimal, but that's covered in its own issue.

So is the question to change the C416 rule or is it only about that C416 and C409 don't play nicely together right now but that would be fixed by reverting the preview behavior changes in C409 (or making it its own rule) 

---
