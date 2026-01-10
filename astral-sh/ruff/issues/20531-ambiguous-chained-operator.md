---
number: 20531
title: ambiguous-chained-operator
type: issue
state: closed
author: kapkekes
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2025-09-23T12:14:43Z
updated_at: 2025-09-29T20:43:47Z
url: https://github.com/astral-sh/ruff/issues/20531
synced_at: 2026-01-10T01:23:01Z
---

# ambiguous-chained-operator

---

_Issue opened by @kapkekes on 2025-09-23 12:14_

### Summary

### Context

A few days ago, I stumbled upon this snippet:

```python
False == False in [False]
```

At first, I thought that it evaluates to `False`, *which is wrong*.

That day, I learned that `in` and `not in` are actually [comparison operators](https://docs.python.org/3/reference/expressions.html#comparisons), which makes them chainable.

You can argue that this snippet is a little unobvious, but not terrible; so, I modified it:

```python
False == False in [False] == [False]
```

This lil' guy also evaluates to `True`, but in my opinion, it doesn't even look like valid Python code.

### Proposal

My feeling is that the knowledge of `in` being a comparison (so chainable) operator is more of a secret, and not something obvious. I want to suggest a rule that forbids using `in` in chained comparisons.

I think, the samples I provided are not typical code anyone would write. Strictly speaking, they are the first examples of chained `in` I have ever seen. However, if the proposed rule makes sense, I am willing to implement it.

Note that the [PLR1716](https://docs.astral.sh/ruff/rules/boolean-chained-comparison/#boolean-chained-comparison-plr1716) rule may conflict with this one, I haven't checked for that yet.

### PS

While writing this proposal, I considered another rule that would prohibit things like `a < b >= c`; more formal definition would be "if a chained comparison contains `<=` or `<`, it should only use `<=` and `<` (same with `>` and `>=`)", so:

- `a < b <= c <= d` - valid;
- `a > b > c >= d` - valid;
- `a < b >= c == d` - invalid.

I would like to hear feedback on this suggestion, thanks in advance.

---

_Comment by @ntBre on 2025-09-23 12:25_

This is interesting and does look confusing to me. It looks like PLR1716 only checks for chained less than and greater than operators, much like your PS suggestion, so it shouldn't conflict, good idea to check though!

---

_Label `rule` added by @ntBre on 2025-09-23 12:25_

---

_Label `needs-decision` added by @ntBre on 2025-09-23 12:25_

---

_Comment by @kapkekes on 2025-09-24 07:39_

I'll start implementing both proposals this weekend as separate rules: `chained-in-usage` and `ambiguous-chained-operator`; I'm too interested to not try.

---

_Comment by @adavis444 on 2025-09-25 19:29_

Although [PLR1716](https://docs.astral.sh/ruff/rules/boolean-chained-comparison/#boolean-chained-comparison-plr1716) comes from Pylint, Pylint has a related warning: [bad-chained-comparison / W3601](https://pylint.readthedocs.io/en/stable/user_guide/messages/warning/bad-chained-comparison.html)

From the tracker in https://github.com/astral-sh/ruff/issues/970, there is no Ruff equivalent.
It would be great to see it implemented!

---

_Referenced in [astral-sh/ruff#20597](../../astral-sh/ruff/pulls/20597.md) on 2025-09-26 21:25_

---

_Comment by @tjkuson on 2025-09-28 12:39_

Should this issue be closed in favour of #8964? FWIW, I wrote a PR for this, and it was rejected until #1774 gets resolved, though that might not be the case any more.

---

_Comment by @serjflint on 2025-09-29 05:09_

I wonder if it can go in the preview category.

---

_Comment by @kapkekes on 2025-09-29 08:46_

@ntBre, hi!

Can you take a look at the situation?

---

_Comment by @ntBre on 2025-09-29 20:43_

Thanks @tjkuson, I missed the earlier issue and PR. I think the reasoning related to #1774 still stands (this is an opinionated/stylistic lint that restricts valid Python syntax), so I think we should continue holding off on implementing this and fold this issue into #8964.

Sorry for not catching this earlier @kapkekes. We should be able to pull from both your and @tjkuson's PRs when we are ready to add this rule.

---

_Closed by @ntBre on 2025-09-29 20:43_

---
