---
number: 7088
title: Rules D204 cause autofix error
type: issue
state: closed
author: qarmin
labels:
  - bug
  - fuzzer
assignees: []
created_at: 2023-09-03T16:00:38Z
updated_at: 2023-09-11T13:09:49Z
url: https://github.com/astral-sh/ruff/issues/7088
synced_at: 2026-01-10T01:22:46Z
---

# Rules D204 cause autofix error

---

_Issue opened by @qarmin on 2023-09-03 16:00_

Ruff 0.0.287 (latest changes from main branch)
```
ruff  *.py --select D204 --no-cache --fix
```

file content(at least simple cpython script shows that this is valid python file):
```
class ServiceSorter(ABC):
	def sort_services(self,services):0
class DefaultPrioritySorter(ServiceSorter):
	'Default service sorting approach that always give S3 the highest priority.';priorities={'s3':100,'lambda':1}
	def sort_services(A,services):return sorted(services,key=lambda k:A.priorities.get(k,0),reverse=True)
```

error
```
/home/rafal/test/tmp_folder/39521.py:4:2: D204 1 blank line required after class docstring
Found 1 error.

error: Autofix introduced a syntax error. Reverting all changes.

This indicates a bug in `ruff`. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BAutofix%20error%5D

...quoting the contents of `/home/rafal/test/tmp_folder/39521.py`, the rule codes D204, along with the `pyproject.toml` settings and executed command, we'd be very appreciative!


```
    
[python_compressed.zip](https://github.com/astral-sh/ruff/files/12506516/python_compressed.zip)
    

---

_Label `fuzzer` added by @zanieb on 2023-09-03 19:02_

---

_Label `bug` added by @charliermarsh on 2023-09-03 22:31_

---

_Assigned to @konstin by @konstin on 2023-09-05 21:27_

---

_Referenced in [astral-sh/ruff#7174](../../astral-sh/ruff/pulls/7174.md) on 2023-09-11 13:01_

---

_Closed by @konstin on 2023-09-11 13:09_

---
