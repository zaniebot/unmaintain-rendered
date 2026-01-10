```yaml
number: 8151
title: new rule - fstring syntax in non-fstring
type: issue
state: closed
author: DetachHead
labels:
  - rule
  - help wanted
  - accepted
assignees: []
created_at: 2023-10-24T02:54:37Z
updated_at: 2024-02-03T00:21:05Z
url: https://github.com/astral-sh/ruff/issues/8151
synced_at: 2026-01-10T11:09:50Z
```

# new rule - fstring syntax in non-fstring

---

_Issue opened by @DetachHead on 2023-10-24 02:54_

```py
asdf = 1
"value is {asdf}" # the user probably meant to make this an f string
```

to avoid false positives, it should only complain when the expression inside the `{}` refers to an existing variable. that way, most usages of the `format` function won't trigger false positives:
```py
x = "{foo}" # no error because `foo` variable does not exist
x.format(foo="bar")
```

---

_Comment by @dhruvmanila on 2023-10-25 05:23_

Interesting! Although not sure how to reliably detect this. One way could be to pass the string as a f-string to the parser and check if there are any `ExprFormattedValue` node in it.

---

_Label `rule` added by @charliermarsh on 2023-10-30 23:37_

---

_Label `needs-decision` added by @charliermarsh on 2023-10-30 23:37_

---

_Comment by @charliermarsh on 2023-10-30 23:37_

I'm worried about false positives on this one.

---

_Comment by @DetachHead on 2023-10-31 01:09_

i feel like they would be rare as long as it only complains when the expression inside the `{}` refers to an existing variable

---

_Comment by @MichaReiser on 2023-10-31 01:18_

I would find this useful. Coming from Rust and JavaScript, I wrote `"{value}"` too many times to then notice my debug print statements are completely useless. So I went back and added the `f` in front of my strings.

---

_Comment by @dhruvmanila on 2023-10-31 01:35_

> I'm worried about false positives on this one.

Can you give an example where there would be false positives? Maybe we could only highlight when the node within `{...}` is an identifier and skip everything else.

---

_Comment by @zanieb on 2023-10-31 01:40_

My common use false positive

```python
x = "{foo}"
x.format(foo="bar")
```

---

_Comment by @DetachHead on 2023-10-31 02:52_

@zanieb in that case it shouldn't trigger a false positive if the rule works how i suggested in https://github.com/astral-sh/ruff/issues/8151#issuecomment-1786278064, because there's no variable in scope called `foo`.

imo the `format` method should only be used instead of f-strings in cases where the variables being formatted are not in scope. i've updated the OP to clarify this

---

_Comment by @zanieb on 2023-10-31 03:04_

I agree requiring the variables to be in scope is a reasonable heuristic for avoiding false positives. I'm not sure I can come up with a common one that meets that requirement.

I found https://github.com/PrefectHQ/prefect/blob/f654a1b23eb5662afb5b8bfd08c9c86668e817ee/tests/agent/test_agent_queue_selection.py#L36 but that probably ought to be an f-string anyway. We can probably hint something else when `format` is being called instead of using a f-string.

---

_Comment by @dhruvmanila on 2023-10-31 03:35_

We could just skip if the said string is part of a large `.format` call or `%` operator.

I think then we could support this as:
1. Skip if this string contains `{` / `}`
2. Skip if it's part of `.format` call or `%` operator
3. Parse the string with the `f` prefix
4. Visit each `ExprFormattedValue` node. If there are none, return
5. Create a diagnostic message only if _all_ of the expression inside the formatted node is
	a. An identifier
	b. It's defined in the scope

What do you think? @zanieb 

---

_Comment by @peterjc on 2023-11-01 15:40_

I'd welcome this. Cross reference https://github.com/peterjc/flake8-sfs/issues/5

See also the opposite request, f-strings without any variables in them - briefly implemented as flake8-pie's PIE782 code but withdrawn due to false positives. Cross reference https://github.com/peterjc/flake8-sfs/issues/3 and https://github.com/sbdchd/flake8-pie/issues/24

---

_Comment by @dhruvmanila on 2023-11-02 04:23_

> See also the opposite request, f-strings without any variables in them

We do support this - https://docs.astral.sh/ruff/rules/f-string-missing-placeholders/ :)

---

_Comment by @hauntsaninja on 2023-12-27 21:58_

pyanalyze supports this check: https://github.com/quora/pyanalyze/blob/647c64e8c9b4eaa81a5309d8e76e72bafe61d687/pyanalyze/name_check_visitor.py#L3096

Here are the heuristics it uses to avoid false positives:
- At least one name is used in the expression and all names used inside the expression are bound
- The string literal is not immediately `.format()`-ed
- It's not standalone expression (i.e. doesn't look like a docstring)
- It's not in a call that passes kwargs corresponding to all names (to avoid false positives with interpolation functions that work like `.format`)

---

_Label `needs-decision` removed by @charliermarsh on 2023-12-31 12:42_

---

_Label `accepted` added by @charliermarsh on 2023-12-31 12:42_

---

_Comment by @charliermarsh on 2023-12-31 12:42_

I'm supportive of adding this with the above heuristics.

---

_Label `help wanted` added by @zanieb on 2024-01-24 21:07_

---

_Assigned to @snowsignal by @snowsignal on 2024-01-29 16:44_

---

_Closed by @snowsignal on 2024-02-03 00:21_

---
