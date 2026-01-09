---
number: 8788
title: "Inline error/violation suppression doesn't work with existing comments"
type: issue
state: closed
author: dAnjou
labels:
  - question
  - suppression
assignees: []
created_at: 2023-11-20T15:21:26Z
updated_at: 2023-11-25T17:43:35Z
url: https://github.com/astral-sh/ruff/issues/8788
synced_at: 2026-01-07T13:12:15-06:00
---

# Inline error/violation suppression doesn't work with existing comments

---

_Issue opened by @dAnjou on 2023-11-20 15:21_

Hi,

at work, our client requires us to use their SonarQube. At the same time, I'd like to use Ruff as well.

These two clash when you're trying to add inline suppression comments for both, at least this way:
```python
except:  # NOSONAR python:S5754 noqa: E722
```
In this particular case, switching them works:
```python
except:  # noqa: E722 NOSONAR python:S5754
```

However, this might not work with other inline comment based metadata.

I'm not sure what a good solution for this might look like, but I have two ideas.

You could look for the `noqa` pattern _anywhere_ in a comment.

And/or, you could add a feature like [coverage](https://coverage.readthedocs.io/en/7.3.2/excluding.html#advanced-exclusion), that lets you exclude lines that match a provided pattern. This way, I could provide a suppression pattern `NOSONAR` to Ruff and both tools would be happy.

---

_Comment by @charliermarsh on 2023-11-20 15:22_

Would you mind trying this? `except:  # NOSONAR python:S5754  # noqa: E722`

We allow the `noqa` anywhere, but we require that it's prefixed by its own `#`.

---

_Comment by @dAnjou on 2023-11-20 15:30_

That does work, thanks 🙏 

---

_Comment by @charliermarsh on 2023-11-25 17:43_

I'm gonna close since we do ask for the leading `#` to clearly disambiguate the suppression. Hopefully that's ok for your use-case.

---

_Closed by @charliermarsh on 2023-11-25 17:43_

---

_Label `question` added by @charliermarsh on 2023-11-25 17:43_

---

_Label `noqa` added by @charliermarsh on 2023-11-25 17:43_

---
