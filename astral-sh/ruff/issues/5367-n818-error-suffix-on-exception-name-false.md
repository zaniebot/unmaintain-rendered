```yaml
number: 5367
title: "N818 error-suffix-on-exception-name false positives because it doesn't know if a given exception is an error"
type: issue
state: open
author: karolinepauls
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2023-06-26T10:25:05Z
updated_at: 2024-06-29T09:02:37Z
url: https://github.com/astral-sh/ruff/issues/5367
synced_at: 2026-01-10T11:09:47Z
```

# N818 error-suffix-on-exception-name false positives because it doesn't know if a given exception is an error

---

_Issue opened by @karolinepauls on 2023-06-26 10:25_

I believe that this rule does more harm than good.

> you should use the suffix “Error” on your exception names (**if the exception actually is an error**).
- https://peps.python.org/pep-0008/#exception-names

The ratio of non-error exceptions to error exceptions varies depending on the specific task, but I'd assume it to be about 1:1. since Python encourages exceptions for control flow, which is kind of good in absence of Rust-like result types. For example:
- we may be calling some code to add funds to a user's account which may fail for a number of reasons, like: UserIsAnAdmin, UserSuspended, UserUnverified. Those are not errors, they are exceptions from the happy path of successfully adding funds.
- we may have a message processor which raises MessageIgnored for messages which i refuses to process based on its current functionality or configuration.

Meanwhile, there are already many important considerations when raising exceptions, like not reusing the same concrete exception type for more than one error. Having unreliable lint rules (which can be 50% incorrect) takes attention away from those very important considerations which cannot be linted for.


I suggest removing this rule and adding a more limited one which checks that an exception name doesn't end with `Exception` and suggests replacing the `Exception` suffix with `Error`.

At least, the message should be improved to, rather than saying `Exception name <NAME> should be named with an Error suffix`, say `Exception name <NAME> should be named with an Error suffix if it is an error, please ignore otherwise`.

---

_Comment by @charliermarsh on 2023-06-26 16:23_

I'm noticing that this rule is ignored-by-default in pep8-naming -- which makes me think we should consider removing it entirely.

---

_Label `question` added by @charliermarsh on 2023-06-26 16:23_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-06-27 14:07_

---

_Comment by @charliermarsh on 2023-06-27 15:20_

@andersk - Not high-priority, but do you happen to have an opinion on this? (Was looking back at #783.)

---

_Label `question` removed by @charliermarsh on 2023-07-10 01:09_

---

_Label `rule` added by @charliermarsh on 2023-07-10 01:09_

---

_Label `needs-decision` added by @charliermarsh on 2023-07-10 01:09_

---

_Comment by @quassy on 2024-02-12 01:18_

```py
class FoobarException(Exception):
```
I guess it's not mentioned in the PEP but I would like at least expect obviously named exceptions like this not to be flagged.

---

_Comment by @andersk on 2024-02-12 03:18_

PEP 8 [says](https://peps.python.org/pep-0008/#programming-recommendations):

> Class naming conventions apply here, although you should add the suffix “Error” to your exception classes if the exception is an error. Non-error exceptions that are used for non-local flow control or other forms of signaling need no special suffix.

So `FoobarException` doesn’t satisfy either convention; it should be `FoobarError` for an error or just `Foobar` for a non-error.

Clearly it’s difficult for a linter to recognize the intended use case for a class deriving from `Exception`, but errors are the most common ones. Custom non-error exceptions are pretty rare, and someone writing them is likely a more advanced programmer who can determine whether the lint should be ignored or disabled. So I think the rule still has value for most users who have decided to enable `N` rules.

---

_Comment by @karolinepauls on 2024-02-13 10:37_

> Custom non-error exceptions are pretty rare

This is not true, any web backend that specialises its BadRequest exceptions will have plenty.

>  and someone writing them is likely a more advanced programmer who can determine whether the lint should be ignored or disabled

Except that programmers don't work in isolation, in every project there will be more "less advanced" programmers for every, likely overworked, "advanced programmer".

Now the "advanced programmer" is put in the uncomfortable position of making sure that "less advanced" programmers:
- ignore this rule for the "more advanced" errors
- but don't learn the habit of adding a noqa for every slightly confusing lint.

_Moreover, the division between "advanced" and "not advanced" programmers is rather naive. The real division is between programmers of any seniority who want to learn, think, and reason at work and professional clock-watchers who cobble things together until they kind of work._

The cost of false positives is much worse than the cost of false negatives.

---

_Comment by @steve-mavens on 2024-06-29 09:01_

I suggest an opportunity here for two different rules: one to flag exception names ending in `Exception`, saying that this is a violation of PEP-8, and a separate rule (the existing one) to be enabled by people who use exceptions only for errors and therefore want all exception names to end in `Error`. @karolinepauls could use the new rule but not the existing one, @andersk could make an advanced programming decision whether to use the existing rule on a per-project basis (or noqa a few cases), and @quassy prefers an `Exception` suffix and therefore disables both rules.

---
