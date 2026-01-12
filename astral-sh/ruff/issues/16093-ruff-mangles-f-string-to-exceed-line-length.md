```yaml
number: 16093
title: "Ruff mangles f-string to exceed line length (format & check, E501)"
type: issue
state: closed
author: vytas7
labels:
  - question
  - formatter
assignees: []
created_at: 2025-02-11T06:39:43Z
updated_at: 2025-02-11T09:48:43Z
url: https://github.com/astral-sh/ruff/issues/16093
synced_at: 2026-01-12T15:54:55Z
```

# Ruff mangles f-string to exceed line length (format & check, E501)

---

_@vytas7_

### Description

I searched for: f-string, format & check, E501.

The issue seems to be caused by the newly introduced ability to format code inside f-strings.

Consider the following file (`test.py`):
```python
variable = 1
items = (
    f"-- This is a format string to demonstrate a Ruff issue: {variable/variable}. --",
)
```

It passes all the checks (I'm also checking line length):
```
$ ruff check test.py 
All checks passed!
$ ruff check --select E501 test.py 
All checks passed!
```

It is not reformatted with an older `ruff` either:
```
$ ruff format test.py
1 file left unchanged
```

Now, using `ruff 0.9.6`:
```
$ ruff format test.py
1 file reformatted

$ ruff check --select E501 test.py 
test.py:3:89: E501 Line too long (89 > 88)
  |
1 | variable = 1
2 | items = (
3 |     f"-- This is a format string to demonstrate a Ruff issue: {variable / variable}. --",
  |                                                                                         ^ E501
4 | )
  |

Found 1 error.
```

---

_Comment by @vytas7 on 2025-02-11 06:55_

Note that #15874 mentions many other issues with f-strings, but not specifically this discrepancy. Also, these issues in #15874 only arise when choosing a specific update rule, this one happens with the default settings.

---

_Label `question` added by @MichaReiser on 2025-02-11 07:13_

---

_Label `formatter` added by @MichaReiser on 2025-02-11 07:13_

---

_Comment by @MichaReiser on 2025-02-11 07:20_

Thanks for opening this issue and the detailed write-up. It made it very easy for me to reproduce your problem.


From our docs

> While the [line-too-long](https://docs.astral.sh/ruff/rules/line-too-long/) (E501) rule can be used alongside the formatter, the formatter only makes a best-effort attempt to wrap lines at the configured [line-length](https://docs.astral.sh/ruff/settings/#line-length). As such, formatted code may exceed the line length, leading to [line-too-long](https://docs.astral.sh/ruff/rules/line-too-long/) (E501) errors. ([source](https://docs.astral.sh/ruff/formatter/#conflicting-lint-rules))

This is one such situation where the formatter requires manual intervention to stay within the configured line length because it doesn't support splitting strings. You can manually rewrite this to:

```py
variable = 1
items = (
    f"-- This is a format string to demonstrate a Ruff issue: "
    f"{variable / variable}. --",
)
```

or 

```py
variable = 1
items = (
    f"-- This is a format string to demonstrate a Ruff issue: {
        variable / variable
    }. --",
)
```

There are other known instances where the formatter may produce code that conflicts with E501, for example:

```py
if True:
    if False:
        if True:
            if False:
                aaaaaaaaaaaaaaaaaaaaaaaaa.aaaaaaaaaaaaaaaaaaaa = "aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa"
```

The way we see the relation between the formatter and `E501` is that the formatter makes its best effort at fitting your code into the configured line length, but you can use `E501` to be warned about instances where the formatter fails to do so and require manual rearranging. 

> Note that https://github.com/astral-sh/ruff/issues/15874 mentions many other issues with f-strings, but not specifically this discrepancy. Also, these issues in https://github.com/astral-sh/ruff/issues/15874 only arise when choosing a specific update rule, this one happens with the default settings.

These issues are unrelated. They're about lint rules. What you describe here is specific to the formatter. 



---

_Comment by @vytas7 on 2025-02-11 07:38_

Hi @MichaReiser!
That is fair that the formatter is not always able to fit strings into the requested line length until (if ever?) the full string rewriting is in place.

However, in this case I would still like to argue this is a bug, because my code was passing the format-check loop without any issues on an older version of `ruff`. Also, the code is passing `ruff check` **before** formatting, but it is not passing **after** `ruff format`.

To give a more practical user story (why I'm filing this at all 😅): we have adopted `ruff` for some repositories, and when a new version of `ruff` is released, the formatting standard sometimes changes, but since automatic reformatting fixes it, one cannot really complain much. However, in this specific case, manual intervention was needed.

---

_Comment by @MichaReiser on 2025-02-11 08:08_

I understand that this case is frustrating and thanks again for opening an issue for it.

> However, in this case I would still like to argue this is a bug, because my code was passing the format-check loop without any issues on an older version of ruff

That's expected because we did release a new formatting style with Ruff 0.9 ([announcement blog post](https://astral.sh/blog/ruff-v0.9.0#the-ruff-2025-style-guide)). The new style now formats f-strings which Ruff didn't do before. Existing code being reformatted (even to something that now exceeds the line length) is, therefore, expected.

We do plan to integrate the formatter into `ruff check` (see https://github.com/astral-sh/ruff/issues/8232) but formatting is currently not checked when running `ruff check`. 

> To give a more practical user story (why I'm filing this at all 😅): we have adopted ruff for some repositories, and when a new version of ruff is released, the formatting standard sometimes changes, but since automatic reformatting fixes it, one cannot really complain much. However, in this specific case, manual intervention was needed

We're aware that breaking formatter changes to create churn for users. That's why we try to limit them to once a year (with the exception of bug fixes). We are considering alternatives to give users more control over when they apply the breaking style guide changes by e.g introducing formatting editions. However, it is important that breaking changes remain possible for the formatter style guide to improve. 

---

_Comment by @vytas7 on 2025-02-11 09:12_

I agree that breaking changes are fair given Ruff hasn't even reached 1.0 yet (regardless of the official SemVer policy).

I do, however, still think this issue is different from the documented examples or the one that you brought up, where Ruff is unable to satisfy the requested line length. Because, to reiterate, in this case, the code passes `ruff check` just fine before `ruff format`, but not after. In other examples that I have seen, the lines are too long to start with too.

I understand this edge case may not be the highest priority on the roadmap, but I would still love to see making `check` and `format` consistent, i.e., that running `format` does not introduce new violations for `check`.

---

_Comment by @MichaReiser on 2025-02-11 09:48_

Here's an example where this happened already before:

```py
if True:
    if False:
        if True:
            if False:
                aaaaaaaaaaaaaaaaaaaaaaaaa.aaaaaaaaaaaaaaaaaaaa = (
"aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa"
                )
```

Get's formatted to 

```py
if True:
    if False:
        if True:
            if False:
                aaaaaaaaaaaaaaaaaaaaaaaaa.aaaaaaaaaaaaaaaaaaaa = "aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa"
```

While this is less likely because I doubt many write the above to begin with, it is the same kind of problem. 

> I understand this edge case may not be the highest priority on the roadmap, but I would still love to see making check and format consistent, i.e., that running format does not introduce new violations for check.


That's our overall goal and is true for almost all rules, except the rules listed in the documentation. 

While I see how this might be desirable, I still consider this as working as intended. It's something we'll look into as part of #8232.


---

_Closed by @MichaReiser on 2025-02-11 09:48_

---
