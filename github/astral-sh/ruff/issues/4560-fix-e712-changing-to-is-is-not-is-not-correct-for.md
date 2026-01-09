---
number: 4560
title: "Fix (E712) changing `==`/`!=` to `is`/`is not` is not correct for some types"
type: issue
state: open
author: zanieb
labels:
  - bug
  - type-inference
assignees: []
created_at: 2023-05-21T15:51:41Z
updated_at: 2025-12-24T13:54:26Z
url: https://github.com/astral-sh/ruff/issues/4560
synced_at: 2026-01-07T13:12:14-06:00
---

# Fix (E712) changing `==`/`!=` to `is`/`is not` is not correct for some types

---

_Issue opened by @zanieb on 2023-05-21 15:51_

## Summary

Generally, comparisons to `True`, `False`, and `None` singletons should use `obj is True` instead of `obj == True`.

However, it is common for libraries to override the `==`/`__eq__` operator to create simple APIs for filtering data. In these cases, correcting `==` to `is` changes the meaning of the program and breaks the user's code. The same applies for `!=` and `is not`.

This is a tracking issue for all invalid corrections from this rule.


## Types with the issue

- `pandas.DataFrame` often used with `DataFrame.mask`, `DataFrame.where`
- `pandas.Series` often used with `Series.mask`, `Series.where`
- `numpy.Array` often used with `Array.where`
- `sqlalchemy.Column` often used with `Query.having`, `Query.filter`, `Query.where`

_If an issue with an unlisted type is encountered please reply and I will edit to add it here._

## Resolution

Eventually, ruff is likely to detect these cases by inferring the datatype involved and exclude it from the suggested fix.

In the meantime, you may:

- Disable rule [E712](https://beta.ruff.rs/docs/rules/true-false-comparison/)
- Use an alternative comparison method that is not ambiguous (e.g. `pandas.Series.eq`)

## Examples

```python
import numpy

numpy.array([True, False]) == False
# array([False,  True])

numpy.array([True, False]) is False
# False
```

```python
import pandas

pandas.Series([True, False]) == False
# 0    False
# 1    True
# dtype: bool

pandas.Series([True, False]) is False
# False

# Alternative safe syntax
pandas.Series([True, False]).eq(False)
```

```python
pandas.DataFrame({"x": [True, False]}) == False
#       x
# 0  False
# 1  True

pandas.DataFrame({"x": [True, False]}) is False
# False

# Alternative safe syntax
pandas.DataFrame({"x": [True, False]}).eq(False)
```

```python
import sqlalchemy
c = sqlalchemy.Column("foo", sqlalchemy.Boolean)

c == True
# <sqlalchemy.sql.elements.BinaryExpression object at 0x12ed532e0>

c is True
# False

# Alternative safe syntax
c.is_(True)
c.is_not(False)
```

## Related issues

- https://github.com/charliermarsh/ruff/issues/2443
- https://github.com/charliermarsh/ruff/issues/1852
- https://github.com/charliermarsh/ruff/issues/4356


---

_Referenced in [astral-sh/ruff#2443](../../astral-sh/ruff/issues/2443.md) on 2023-05-21 16:01_

---

_Label `bug` added by @charliermarsh on 2023-05-21 16:06_

---

_Label `type-inference` added by @charliermarsh on 2023-05-21 16:06_

---

_Renamed from "Fix changing `==`/`!=` to `is`/`is not` is not correct for some types" to "Fix (E712) changing `==`/`!=` to `is`/`is not` is not correct for some types" by @zanieb on 2023-05-21 16:07_

---

_Comment by @ndevenish on 2023-06-19 14:36_

I just spent a while tracking down this exact issue, an error introduced by ruff and reduced to the almost identical:
```python
import numpy as np

arr = np.array([False, True, False, True])
print(repr(arr == False))
# array([ True, False,  True, False])
print(repr(arr is False))
# False
```

Reading the other thread, it sounds like this autofix can't be made safe. I would suggest disabling it completely then, because using truthiness in numpy comparison isn't a rare operation, and expecting everyone to "know" not to do this seems to defeat the point of an autocorrecting linter.


---

_Comment by @charliermarsh on 2023-06-19 14:44_

> Reading the other thread, it sounds like this autofix can't be made safe. I would suggest disabling it completely then, because using truthiness in numpy comparison isn't a rare operation, and expecting everyone to "know" not to do this seems to defeat the point of an autocorrecting linter.

I think making this a suggested fix (as it is now) will have the same effect, once we introduce `--fix` and `--fix-unsafe` (the former of which will only make automatic fixes, while the latter will include suggested fixes, which may include changes in behavior).

The problem with removing the autofix entirely is that it doesn't really reduce the burden or expectation on the user, because this diagnostic will still be _raised_, and so users will still be required to look at the code and understand whether or not to change it. Performing the fix _automatically_ has the downside of silently breaking the code, but requiring users to opt-in to the change explicitly seems (to me) as safe as pointing them to the diagnostic without including any possible fix.


---

_Referenced in [astral-sh/ruff#5526](../../astral-sh/ruff/issues/5526.md) on 2023-07-05 13:33_

---

_Comment by @nicornk on 2023-07-18 07:10_

Any conclusion on this one? This issue broke a bunch of our `pyspark` code by converting code similar to:

```
df = df.where(F.col("colName") == True)
```

to

```
df = df.where(F.col("colName") is True)
```

which leads to `TypeError: condition should be string or Column`

Thank you

---

_Comment by @dstoeckel on 2023-07-18 07:15_

Adding to @nicornk: PEP-8 actually strongly discourages using `is` with boolean constants:

```
Don’t compare boolean values to True or False using ==:
# Correct:
if greeting:
# Wrong:
if greeting == True:
Worse:

# Wrong:
if greeting is True:
```

So while the autofix may be unsafe, I would argue the diagnostics itself is harmful. The correct suggestion would be to drop `== True` entirely (though PySpark is certainly another special case, as `== False` would need a conversion to the unary not `~` operator ...)

---

_Comment by @zanieb on 2023-07-18 13:53_

@nicornk you can use the unambiguous alternate syntax e.g. `df = df.where(F.col("colName").is_(True))` or disable the rule. Additionally, once Ruff has type inference, we will avoid suggesting a change to `col is True`.

@dstoeckel while I agree that `if foo` is definitely better than `if foo == True`, there are entirely valid uses for checking if something is `True` or `False` without having a `__bool__` cast occur and there are cases outside of `if` statments where the user must perform the cast. Unfortunately the diagnostic must try to guess the intent of the user when suggesting a fix which puts us in a tough spot. I'm not sure this issue is the correct place to discuss the validity of the rule as a whole though, I'd like to keep this scoped to a discussion of false positives based on type inference.


---

_Comment by @conbrad on 2023-07-18 15:38_

> Additionally, once Ruff has type inference

Is there an existing ongoing effort for this that's public?




---

_Referenced in [chaoss/augur#2474](../../chaoss/augur/pulls/2474.md) on 2023-08-02 13:59_

---

_Referenced in [astral-sh/ruff#8023](../../astral-sh/ruff/issues/8023.md) on 2023-10-17 20:30_

---

_Comment by @zanieb on 2023-10-17 20:32_

Note [as of v0.1.0](#7769 ) we do not apply unsafe fixes by default — so this fix will not be applied by default.

---

_Comment by @NeilGirdhar on 2023-11-06 13:36_

> there are entirely valid uses for checking if something is `True` or `False` without having a `__bool__` cast occur

Of course this is a matter of opinion, but if you want to check this, I think the Pythonic way to do so is to use:
```
if isinstance(x, bool) and x:
# or
match x:
    case True:
```
`is True` is far more often misused in my opinion.

---

_Comment by @zanieb on 2023-11-06 16:05_

Please let's not make this issue a debate about how `if _ == True`, `if _ is True`, and `if _` should be used or whether `E712` is valid in general.

The libraries that are the focus of this issue have designed APIs where specific comparisons are necessary. This issue is intended to track support for patterns in those APIs. I'd recommend creating a new [discussion](https://github.com/astral-sh/ruff/discussions) if you want to discuss broader concerns.

---

_Referenced in [astral-sh/ruff#9063](../../astral-sh/ruff/issues/9063.md) on 2023-12-09 00:49_

---

_Referenced in [NREL/flasc#154](../../NREL/flasc/pulls/154.md) on 2023-12-15 23:45_

---

_Comment by @VictorGob on 2023-12-22 10:23_

Just to add an example, of an error that took me a while to fix.

```python3
import pandas as pd

# Example dataframe
df = pd.DataFrame({"id": [1, 2, 3, 4, 5], "col_2": [True, False, True, False, True]})

# This works, but ruff raises: Comparison to `False` should be `cond is False`
a = df[df["col_2"] == False]
print(a)

# This does not work, pandas will raise 'KeyError: False'
b = df[df["col_2"] is False]
print(b)
```


---

_Referenced in [astral-sh/ruff#9883](../../astral-sh/ruff/issues/9883.md) on 2024-02-08 03:32_

---

_Comment by @simonpanay on 2024-02-16 09:43_

Another example with sqlalchemy2 ( where syntax has changed a lot comparing with versions 1.x): 

~~~python
    query = request.dbsession.execute(
        select(Station, func.min(Check.result))  # pylint: disable=E1102
        .join(Check.channel)
        .join(Channel.station)
        .where(
            Station.triggered == False,
        )
    ).all()
 ~~~

Here the `Station.triggered == False` raises the E712
If replaced by `Station.triggered is False` the result is not what is expected

---

_Referenced in [astral-sh/ruff#10344](../../astral-sh/ruff/issues/10344.md) on 2024-03-11 18:04_

---

_Comment by @psychedelicious on 2024-06-12 23:44_

Same thing with [E711](https://docs.astral.sh/ruff/rules/none-comparison/).

Would be nice to have a brief mention in the rule's docs calling out common situations where this is unsafe and why

---

_Comment by @zanieb on 2024-06-13 00:48_

Thanks @psychedelicious. Would you be willing to open a pull request?

---

_Referenced in [astral-sh/ruff#11859](../../astral-sh/ruff/pulls/11859.md) on 2024-06-13 13:49_

---

_Referenced in [astral-sh/ruff#12290](../../astral-sh/ruff/issues/12290.md) on 2024-07-11 18:11_

---

_Comment by @torzsmokus on 2024-08-02 12:04_

But why do we change `==`/`!=` to `is`/`is not` at all??  [PEP8 says it is even **worse**](https://peps.python.org/pep-0008/#:~:text=%3D%3D%20True%3A-,Worse,-%3A).
![image](https://github.com/user-attachments/assets/437a4cce-d707-49fb-9a23-10559972ec4d)


---

_Comment by @torzsmokus on 2024-08-02 12:07_

oh, checking #8164 that seems to deal with the same question…

---

_Comment by @NeilGirdhar on 2024-08-02 12:25_

Now that Ruff is moving towards having type information, this issue may eventually warrant some refinement?  If `x` has Boolean type, then `if x` is appropriate and `if x is True` is inappropriate.  If `x` has a broader type, then either could be fine.

---

_Comment by @jbcpollak on 2024-08-02 12:54_

> If x has a broader type, then either could be fine.

I would argue that if x is not Boolean type, "is True" is always wrong - for example if x is `numpy.bool_`, comparing it with `is` will be wrong.

---

_Comment by @dangotbanned on 2024-08-03 07:40_

I ran into this when writing [this example](https://github.com/vega/altair/blob/6c4c7856a5b134103d3db1205035d08a83fc3aa6/altair/vegalite/v5/api.py#L1111-L1130) for the next version of `altair` - based on [upstream example](https://vega.github.io/editor/#/url/vega-lite/N4IgJAzgxgFgpgWwIYgFwhgF0wBwqgegIDc4BzJAOjIEtMYBXAI0poHsDp5kTykBaADZ04JAKyUAVhDYA7EABoQAEySYUqUAwBOgtCrVICCNsRpwIUmfIC+S5NoDW+nGxqzMikHFlQ2y9zI0UAAPYJAAM3NBZX0ASQBZABEAIQACACU1QK9MAE8cOH0ARwYkDzps0hA7EDzwqLgY-Qy2bB80gBU2ZEw2C0zs2SClfMKSsor1TBpq2r9BNm1wv1kAmblwzAtPdFVMBgQAbQByRNTBmeGTgF00gF5HtNkGQUE0gB8PtP3D09b2rIuj01P0IJdArcHk8Xm8vMQkIIGEV0ABiJAYmo2eZyKJBTQgBzOAnuBHCWKoWGCbE2IA)

Would be applicable to:
- All of the types in [altair.expr.core](https://github.com/vega/altair/blob/6c4c7856a5b134103d3db1205035d08a83fc3aa6/altair/expr/core.py)
- Derived in `altair.vegalite.v5.api`
	- [altair.Parameter](https://github.com/vega/altair/blob/6c4c7856a5b134103d3db1205035d08a83fc3aa6/altair/vegalite/v5/api.py#L344)
	- [altair.ParameterExpression](https://github.com/vega/altair/blob/6c4c7856a5b134103d3db1205035d08a83fc3aa6/altair/vegalite/v5/api.py#L448)
	- [altair.SelectionExpression](https://github.com/vega/altair/blob/6c4c7856a5b134103d3db1205035d08a83fc3aa6/altair/vegalite/v5/api.py#L462)

---

_Comment by @NeilGirdhar on 2024-08-03 08:37_

> I would argue that if x is not Boolean type, "is True" is always wrong - for example if x is numpy.bool_, comparing it with is will be wrong.

By broader, I mean something like `Any` or `bool | int` or `np.bool | bool`, etc.

---

_Referenced in [astral-sh/ruff#12765](../../astral-sh/ruff/issues/12765.md) on 2024-08-09 01:33_

---

_Referenced in [astral-sh/ruff#12769](../../astral-sh/ruff/pulls/12769.md) on 2024-08-09 01:37_

---

_Comment by @subnix on 2024-09-18 12:07_

Note about SQLAlchemy:

The `.is_` and `.is_not` methods may not be safe alternatives to the equality (`==`/`!=`) operators, because `IS` and `=` are different SQL operators. Consider the following example in MySQL:
```sql
SELECT 10 IS TRUE;
/* 1 */
SELECT 10 = TRUE;
/* 0 */
SELECT NULL IS NOT FALSE;
/* 1 */
SELECT NULL != FALSE;
/* NULL */
```

Thus, the safe alternative for comparing to booleans is:
```python
c == True
# <sqlalchemy.sql.elements.BinaryExpression object at 0x1026203e0>
c == sqlalchemy.true()
# <sqlalchemy.sql.elements.BinaryExpression object at 0x1026222a0>
```

---

_Referenced in [blacklanternsecurity/bbot#1997](../../blacklanternsecurity/bbot/pulls/1997.md) on 2024-11-21 10:23_

---

_Referenced in [darrenburns/posting#158](../../darrenburns/posting/pulls/158.md) on 2024-12-15 11:03_

---

_Referenced in [astral-sh/ruff#17014](../../astral-sh/ruff/issues/17014.md) on 2025-03-27 17:01_

---

_Referenced in [unb-mds/VIDAProject#75](../../unb-mds/VIDAProject/pulls/75.md) on 2025-06-17 01:22_

---

_Comment by @yomajo on 2025-07-08 17:06_

Just adding my case in sqlalchemy world:
```
>>> len(db.session.scalars(select(SKU).where(SKU.left <= 0).where(SKU.discontinued == False)).all())
437
>>> len(db.session.scalars(select(SKU).where(SKU.left <= 0).where(not SKU.discontinued)).all())
0
```

---

_Referenced in [redpanda-data/redpanda#28275](../../redpanda-data/redpanda/pulls/28275.md) on 2025-10-30 03:16_

---

_Comment by @oscarbenjamin on 2025-12-24 13:51_

The rule E712 throws about 1000 errors on the sympy codebase some of which could be changed to `is True` but many (most?) of which should not. The reason that `== True` is often used is that sympy has its own symbolic versions of True and False:
```python
In [1]: from sympy import *

In [2]: S.true
Out[2]: True

In [3]: x = Symbol('x')

In [4]: e = x > 0

In [5]: e
Out[5]: x > 0

In [6]: e.subs(x, 1)
Out[6]: True

In [7]: type(e.subs(x, 1))
Out[7]: sympy.logic.boolalg.BooleanTrue

In [8]: e.subs(x, 1) == True
Out[8]: True

In [9]: e.subs(x, 1) is True
Out[9]: False
```
Symbolic true is needed because e.g.:
```python
In [11]: S.true.subs(x, 1)
Out[11]: True

In [12]: True.subs(x, 1)
...
AttributeError: 'bool' object has no attribute 'subs'
```
Calling `__bool__` on a symbolic relational can raise an error:
```python
In [10]: bool(x > 0)
...
TypeError: cannot determine truth value of Relational: x > 0
```
Given an arbitrary expression `e` then it is common to write something like
```python
if (e > 0) == True
```
to test if the expression is definitely positive without raising a potential exception if the check is undecidable.

The fix suggested by `E712` is incorrect in this case because the correct is condition would be with sympy's symbolic True:
```python
if (e > 0) is S.true
```
I think that `E712` is a valid rule but in the sympy codebase (or downstream code using sympy) the unsafe fix would be wrong much of the time and instead someone would need to through all 1000 cases manually and figure out whether each one should be `is True` or `is S.true`. I am not particularly motivated to do that and I wouldn't even want to review a PR if someone else did it so probably not worth it.

I also expect that many sympy contributors would find `E712` confusing and end up writing the wrong code so leaving aside the current 1000 errors I'm not sure it would be worth enabling the rule unless we could tell ruff somehow to mention `S.true` in a custom error message. At least I suppose the E712 doc page links to this issue.

---

_Comment by @oscarbenjamin on 2025-12-24 13:54_

The E711 doc page links to this issue:
https://docs.astral.sh/ruff/rules/true-false-comparison/

In principle this issue applies to both E711 (None) and E712 (True and False). In practice I am not aware of any situation where there would be an issue with the E711 fixer and I don't see any actual examples listed above. I'm not sure what the bar is for considering a fix to be safe but for my own codebases I would be happy to autofix E711 but not E712.

---
