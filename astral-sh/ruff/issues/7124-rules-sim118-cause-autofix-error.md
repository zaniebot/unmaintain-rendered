```yaml
number: 7124
title: Rules SIM118 cause autofix error
type: issue
state: closed
author: qarmin
labels:
  - bug
  - fuzzer
assignees: []
created_at: 2023-09-04T06:05:13Z
updated_at: 2023-09-05T11:51:36Z
url: https://github.com/astral-sh/ruff/issues/7124
synced_at: 2026-01-10T11:09:49Z
```

# Rules SIM118 cause autofix error

---

_Issue opened by @qarmin on 2023-09-04 06:05_

Ruff 0.0.287 (latest changes from main branch)
```
ruff  *.py --select SIM118 --no-cache --fix
```

file content(at least simple cpython script shows that this is valid python file):
```
class DockerVmManager(VmManager):
		try:U=H.image_from_ami_id(A,verify=_A)
		except InvalidAMIIdError as J:
			if A in B.amis.keys()and B.amis[A].get_filter_value(_H)==_C:LOG.debug(f"Deregistering AMI '{A}' because it no longer exists in Docker");B.amis.pop(A,_B)
```

error
```
/home/rafal/test/tmp_folder/1378887.py:4:7: SIM118 Use `A in B.amis` instead of `A in B.amis.keys()`
Found 1 error.

error: Autofix introduced a syntax error. Reverting all changes.

This indicates a bug in `ruff`. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BAutofix%20error%5D

...quoting the contents of `/home/rafal/test/tmp_folder/1378887.py`, the rule codes SIM118, along with the `pyproject.toml` settings and executed command, we'd be very appreciative!

```

[python_compressed.zip](https://github.com/astral-sh/ruff/files/12509793/python_compressed.zip)


---

_Label `bug` added by @MichaReiser on 2023-09-04 06:27_

---

_Label `fuzzer` added by @MichaReiser on 2023-09-04 06:27_

---

_Comment by @dhruvmanila on 2023-09-04 07:38_

Again a whitespace problem, we're removing the `.keys()` method but there's no whitespace between the call and the keyword `and`, it get's merged into the attribute:
```diff
- if A in B.amis.keys()and B.amis[A]
+ if A in B.amisand B.amis[A]
```

---

_Assigned to @charliermarsh by @charliermarsh on 2023-09-05 11:41_

---

_Closed by @charliermarsh on 2023-09-05 11:51_

---
