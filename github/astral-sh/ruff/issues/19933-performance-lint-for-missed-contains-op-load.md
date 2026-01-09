---
number: 19933
title: Performance lint for missed CONTAINS_OP LOAD_CONST optimization
type: issue
state: open
author: Zaczero
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2025-08-16T08:40:10Z
updated_at: 2025-08-18T17:20:21Z
url: https://github.com/astral-sh/ruff/issues/19933
synced_at: 2026-01-07T13:12:16-06:00
---

# Performance lint for missed CONTAINS_OP LOAD_CONST optimization

---

_Issue opened by @Zaczero on 2025-08-16 08:40_

### Summary

I've recently found out that some people are unaware that **CONTAINS_OP**'s **LOAD_CONST** optimization applies only to containers with trivial values. As long as the RHS is non-trivial, Python will not apply **LOAD_CONST** optimization and will build the container each time we do **CONTAINS_OP**. This is a *major performance bottlneck* that many people may accidentally stumble upon. I think there's great potential in having a new rule that checks for this kind of inefficiency.

I prepared an example that hopefully explains my idea and labels which cases should be considered for a warning:
https://godbolt.org/z/radT91rdn

For example, `0 in {'foo': 1, 'bar': 2}` is much more efficient in the form of `0 == 'foo' or 0 == 'bar'`, so manual iteration. An alternative would be to declare a variable for computing RHS once but that may not always be feasible.

There is one potential issue: in the original code, all of the RHS is run, which can cause some side effects. After rewriting it to a sequence of `or...or` operations, Python may not run all the cases, potentially changing runtime behavior. Although I think such code would be of very poor quality in the first place.

---

_Label `rule` added by @ntBre on 2025-08-18 14:57_

---

_Label `needs-decision` added by @ntBre on 2025-08-18 14:57_

---

_Comment by @ntBre on 2025-08-18 15:41_

Thanks for the suggestion! This is cool, and I didn't know godbolt had a Python option.

One thing that concerns me about making this a lint rule is that it seems very dependent upon CPython implementation details that might vary between CPython versions and especially other implementations of Python.

This also seems partially covered by two of our other rules:
- [literal-membership (PLR6201)](https://docs.astral.sh/ruff/rules/literal-membership/#literal-membership-plr6201)
- [single-item-membership-test (FURB171)](https://docs.astral.sh/ruff/rules/single-item-membership-test/#single-item-membership-test-furb171)

but not the dict case in your example, as far as I can tell.

---

_Comment by @Zaczero on 2025-08-18 17:20_

> This also seems partially covered by two of our other rules

Not quite, my issue concerns the set/list being constructed from scratch whenever it's being tested. Some people are unaware that this LOAD_CONST optimization only applies to Python trivial values and when such optimization is missed, there's a significant performance degradation.

---

> One thing that concerns me about making this a lint rule is that it seems very dependent upon CPython implementation details that might vary between CPython versions and especially other implementations of Python.

Given that this rule should only be applied to cases that hurt performance (bad cases), the only false-positive I can think of is if there exists a Python version/implementation that can optimize sets/collections with potential creation-time side-effects (or be smarter about it: e.g., understand that `'foo'.lower()` is side-effect-free).

PyPy 3.11 has the same behavior as CPython:
https://godbolt.org/z/sf3xs5aa7

<details>
<summary>Cython Pure Python Mode would result in false positives (it is actually smarter):</summary>

<img height="400" alt="Screenshot from Cython Pure Python Mode HTML annotations" src="https://github.com/user-attachments/assets/d5b2e381-da6f-49eb-981a-7a32216ba3fa" />
</details>

I have run AI research on the topic (https://claude.ai/public/artifacts/3d2baeed-0ad0-4db1-9042-a819ff24425f) and here are the key findings:
- LOAD_CONST optimization here is mostly unchanged since Python 3.7, [the same as Ruff support](https://docs.astral.sh/ruff/faq/#what-versions-of-python-does-ruff-support)
- Other implementations are unlikely to provide false positives here

For cases of Python versions/implementations that do not support the LOAD_CONST optimization, the rule will still be beneficial for spotting performance issues. It would only miss warnings on good cases that would suffer from the same problem.

---

_Referenced in [astral-sh/ruff#21116](../../astral-sh/ruff/pulls/21116.md) on 2025-10-28 22:43_

---
