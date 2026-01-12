```yaml
number: 10028
title: "[`ruff`] Implement compound comparison rule (`RUF029`)"
type: pull_request
state: closed
author: tjkuson
labels: []
assignees: []
base: main
head: non-transitive-compound-comparison
created_at: 2024-02-18T15:57:21Z
updated_at: 2024-04-05T15:08:39Z
url: https://github.com/astral-sh/ruff/pull/10028
synced_at: 2026-01-12T15:55:31Z
```

# [`ruff`] Implement compound comparison rule (`RUF029`)

---

_@tjkuson_

## Summary

Implements `non-transitive-compound-comparison` (`RUF029`) that disallows certain compound comparisons.

As [suggested](https://github.com/astral-sh/ruff/issues/8964#issuecomment-1837194529),

> Arguably we should lint against all compound Compares _except_ the following:
> 
>     * Both compares are `is`: `a is b is c`
> 
>     * Both compares are `==`: `a == b ==c`
> 
>     * Both compares are one of `<`, `<=`: `a <= b < c` (probably the most common use  of compound Compare)
> 
>     * Both compares are one of `>`, `>=`: `a >= b > c`

Not a huge fan of the name I gave it, though. Chose `RUF029` as `RUF028` has been used in a bunch of open PRs.

Closes #8964. 

## Test Plan

`cargo nextest run`


---

_Comment by @tjkuson on 2024-02-18 16:08_

I don't think the `ecosystem` CI failure is to do with me...

---

_Comment by @sbrugman on 2024-02-18 22:37_

This pylint rule is related (to the introduction mostly):
https://pylint.pycqa.org/en/latest/user_guide/messages/refactor/chained-comparison.html

Perhaps you'd be interested to implement it too

---

_Comment by @tjkuson on 2024-02-18 23:18_

Huh, I thought that was already implemented in Ruff. Thanks for the idea @sbrugman, I'll implement it tomorrow.

---

_Comment by @Skylion007 on 2024-02-21 21:54_

Make sense for this to be a pylint rule more than a RUF rule.

---

_Comment by @tjkuson on 2024-02-22 09:34_

> Make sense for this to be a pylint rule more than a RUF rule.

I am not aware of there being a corresponding Pylint rule on which to map, is there one?

---

_Comment by @Skylion007 on 2024-02-22 22:16_

Yeah, in RUFF it would be `PLR1716` See https://github.com/astral-sh/ruff/issues/970

---

_Comment by @tjkuson on 2024-02-22 22:26_

I might be misunderstanding something, sorry. That turns things into compound comparisons (have a PR for that tomorrow). This rule does the opposite. They should be separate rules, no?

---

_Comment by @sbrugman on 2024-02-22 22:59_

Indeed, I understand it the same way. The current PR implements a new rule that does not exist in pylint (although it would be a good fit there) and thus should be assigned the latest available RUFF code, e.g. RUF029. 

The PLR1716 is the linked related rule that is included by Pylint, but not implemented in the PR.

---

_Comment by @Skylion007 on 2024-02-23 01:16_

Ah you are right, my bad.

---

_Review requested from @charliermarsh by @MichaReiser on 2024-02-23 07:48_

---

_Comment by @MichaReiser on 2024-04-05 10:31_

I'm undecided how we should categorize this rule. To me it's a restriction rule because it disallows a subset of Python's grammar. However, I could also see that the rule fits into suspicious because the code probably doesn't do what you intended.  @AlexWaygood what do you think?

---

_Comment by @AlexWaygood on 2024-04-05 10:33_

I think it's a useful rule to enforce a Python style that's more readable, but I'd certainly agree that it should be disabled by default.

---

_Comment by @MichaReiser on 2024-04-05 13:08_

Thank you @tjkuson for working on this rule and sorry for the late feedback. 

We agree that the rule itself is valuable and think it should be added to Ruff eventually. But we don't feel comfortable doing it today before #1774 is merged. The rule restricts the allowed Python syntax in an opinionated way, which we aren't comfortable enabling by default but all users that use `RUFF` or `ALL` today would automatically opt in to the rule. 

That's why we want to hold back with the rule for now but hope to get back to it after #1774 is completed. Sorry that we only decided this after you implemented the rule (without mentioning on the issue)

---

_Closed by @MichaReiser on 2024-04-05 13:08_

---

_Branch deleted on 2024-04-05 15:07_

---

_Comment by @tjkuson on 2024-04-05 15:08_

@MichaReiser No worries!

---
