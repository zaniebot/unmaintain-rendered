```yaml
number: 8390
title: "[SIM401] Produces false-positive when the `else` value is not constant"
type: issue
state: closed
author: sshishov
labels: []
assignees: []
created_at: 2023-10-31T20:07:55Z
updated_at: 2023-11-01T13:27:52Z
url: https://github.com/astral-sh/ruff/issues/8390
synced_at: 2026-01-12T15:54:48Z
```

# [SIM401] Produces false-positive when the `else` value is not constant

---

_@sshishov_

The easy example to reproduce:

```python
value = mydict['attr'] if 'attr' in mydict else instance.some_value
```
the rule is saying to transform it to dict's `get()`
```python
value = mydict.get('attr', instance.some_value)
```
The second example produces `AttributeError` even if `attr` is inside dict.
The problem is dict's default value is evaluated always when the ternary's `...if...else...` else block is executed only if the condition is falsy.

The same issue is applied if expensive computation is done in `else` block, or some other logic which is not plain constant.

Similar issue on "parent" repo: https://github.com/MartinThoma/flake8-simplify/issues/177

---

_Comment by @charliermarsh on 2023-10-31 21:42_

We do avoid flagging `SIM401` is the `else` value has a side effect -- for example, if it's a function call, we don't flag `SIM401`:

```python
value = mydict['attr'] if 'attr' in mydict else instance.some_value()
```

But we don't consider attribute accesses to be effectful... They _are_, but they're almost always fine, and it's kind of a tradeoff with regards to how conservative we want to be.

---

_Comment by @charliermarsh on 2023-10-31 21:42_

I think this is okay because it's already marked as an unsafe edit.

---

_Comment by @zanieb on 2023-11-01 03:54_

I agree with Charlie, attribute access is _sometimes_ expensive in Python but I would highly recommend not writing code with expensive attribute access (i.e. properties that do significant computation).

---

_Comment by @dhruvmanila on 2023-11-01 04:06_

FWIW, using cached property is somewhat common if the computation is expensive. Either using the `@functools.cached_property` or `self._cached_value: T | None`.

---

_Comment by @sshishov on 2023-11-01 08:40_

That is okay for us, guys.
We will just mark it `# noqa: SIM401` if the resolution is to ignore / be okay with this.

I was just flagging that the suggestion is not always correct and can produce false positive.
In my original case let's assume the following:
- `mydict` does have `attr` key
- `instance` does not have `some_attr` attribute/property

```python
1. value = mydict['attr'] if 'attr' in mydict else instance.some_attr
2. value = mydict.get('attr', instance.some_attr)
```
In this situation 1st example will not cause any problems and work without errors, The 2nd example will crash with AttributeError. And therefore you have to ignore this `SIM401` rule. That is why I recommed to apply this SIM401 only for `constant` types


---

_Comment by @charliermarsh on 2023-11-01 13:27_

Totally understand. There's a tradeoff to make... Right now I feel like flagging attributes is correct, but we'll definitely revisit that decision if we see more of these issues in the future.

---

_Closed by @charliermarsh on 2023-11-01 13:27_

---
