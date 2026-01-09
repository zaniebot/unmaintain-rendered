---
number: 2387
title: PIE804 with PEP8 naming conflict
type: issue
state: closed
author: spaceone
labels:
  - wontfix
assignees: []
created_at: 2023-01-31T11:24:43Z
updated_at: 2023-01-31T21:47:19Z
url: https://github.com/astral-sh/ruff/issues/2387
synced_at: 2026-01-07T13:12:14-06:00
---

# PIE804 with PEP8 naming conflict

---

_Issue opened by @spaceone on 2023-01-31 11:24_

```python
def foo(NotPep8NamingCompliant=True, **kwargs):
    pass


foo(**{
    "NotPep8NamingCompliant": True,
})
foo(
    NotPep8NamingCompliant=True,
)
```

The function parameter name is not compliant to PEP8 naming:
`foo.py:1:10: N803 argument name 'NotPep8NamingCompliant' should be lowercase`
But functions sometimes allows to receive `**keyword arguments` instead of non-pep8-conform names.

When calling the function with keyword-arguments `PIE804` complains about:
```foo.py:5:1: PIE804 Unnecessary `dict` kwargs```

I would like to allow giving keyword arguments via `**keyword_args` if the dict-keys aren't PEP8 naming conform.
And therefore only complain about names which look OK when calling the function.

---

_Comment by @charliermarsh on 2023-01-31 12:37_

Hate saying no to things that have a reasonable motivation but to me this feels like it'd add more complexity than is worthwhile for a somewhat specific context.

---

_Comment by @spaceone on 2023-01-31 13:21_

OK, your decision. If there is no other need for this in the community: My alternative is to add `noqa` comments then.


---

_Closed by @charliermarsh on 2023-01-31 21:28_

---

_Comment by @charliermarsh on 2023-01-31 21:28_

I'm going to close for now. If this comes up a few more times, I'll definitely reconsider!

---

_Comment by @spaceone on 2023-01-31 21:42_

yes, can you set the issue status to `wontfix` ? (easier to filter then).

---

_Label `wontfix` added by @charliermarsh on 2023-01-31 21:47_

---

_Comment by @charliermarsh on 2023-01-31 21:47_

Good call, done.

---
