```yaml
number: 783
title: N818 ErrorSuffixOnExceptionName should apply recursively to sub-subclasses of Exception
type: issue
state: closed
author: andersk
labels:
  - bug
assignees: []
created_at: 2022-11-17T09:00:11Z
updated_at: 2025-10-17T10:09:26Z
url: https://github.com/astral-sh/ruff/issues/783
synced_at: 2026-01-10T11:09:42Z
```

# N818 ErrorSuffixOnExceptionName should apply recursively to sub-subclasses of Exception

---

_Issue opened by @andersk on 2022-11-17 09:00_

This triggers `N818 exception name 'Bar' should be named with an Error suffix` in pep8-naming but not in Ruff:

```python
class FooError(Exception):
    pass

class Bar(FooError):
    pass
```

pep8-naming checks this by [recursively searching](https://github.com/PyCQA/pep8-naming/blob/b3492eeede12a26d35c0ee9e6b9c37a7e5484bf6/src/pep8ext_naming.py#L283-L293) all ancestor classes, although it may be nearly as effective to just check whether an immediate parent class has a name ending in `Error`.

---

_Label `bug` added by @charliermarsh on 2022-11-17 16:33_

---

_Comment by @charliermarsh on 2022-11-17 17:08_

Yeah let's do the latter for now (checking parent names) as a heuristic.

---

_Closed by @charliermarsh on 2022-11-17 19:51_

---

_Comment by @maltevesper on 2025-10-16 09:48_

@charliermarsh I would kindly ask to reconsider this. Currently the lint wont fire for the following:

```
pydantic.ValidationErrr(ValueError): # typo missing O
```

or for projects defining their own Exception base class on which they base all other exceptions, one example would be [gitlab-python](https://github.com/python-gitlab/python-gitlab/blob/3859aa0ff3a77b31651b770585f5cfe81c9d244a/gitlab/exceptions.py#L37).

```
class GitlabAuthenticationError(GitlabError):
    pass


class RedirectError(GitlabError):
    pass
```

If it is not in the cards it would be nice to have a rational like prohibitve performance statet for reference.
If you can point me to another lint already recursively inspecting bases I *might* have a go at a PR

---

_Comment by @ntBre on 2025-10-16 13:00_

@maltevesper Could you open a separate issue? I think it would be better to discuss this in a new thread.

---

_Comment by @maltevesper on 2025-10-17 10:09_


@ntBre done: #20936 


---
