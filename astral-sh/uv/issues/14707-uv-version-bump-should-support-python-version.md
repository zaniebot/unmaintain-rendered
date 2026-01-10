```yaml
number: 14707
title: uv version --bump should support python version numbers convention
type: issue
state: closed
author: leandrodamascena
labels:
  - question
assignees: []
created_at: 2025-07-18T09:37:45Z
updated_at: 2025-07-18T11:37:07Z
url: https://github.com/astral-sh/uv/issues/14707
synced_at: 2026-01-10T03:32:45Z
```

# uv version --bump should support python version numbers convention

---

_Issue opened by @leandrodamascena on 2025-07-18 09:37_

### Summary

I'm using uv in my project and I want to release alpha versions every day so customers can consume and test new features that will be released in production soon.

Currently, the `uv version --bump` command only supports the values `major, minor, patch`; it should support at least beta, alpha, and rc, as described here: https://packaging.python.org/en/latest/discussions/versioning/.

Thanks

### Example

```sh
uv version --bump alpha aws_lambda_powertools 
error: invalid value 'alpha' for '--bump <BUMP>'
  [possible values: major, minor, patch]

For more information, try '--help'.
```


---

_Label `enhancement` added by @leandrodamascena on 2025-07-18 09:37_

---

_Comment by @konstin on 2025-07-18 10:09_

Which version of uv are you using? This is supported in the latest version of uv.

---

_Label `enhancement` removed by @konstin on 2025-07-18 10:09_

---

_Label `question` added by @konstin on 2025-07-18 10:09_

---

_Comment by @leandrodamascena on 2025-07-18 11:18_

> Which version of uv are you using? This is supported in the latest version of uv.

Hey @konstin indeed it is, thanks a lot for replying so quickly. One question: it's only support bumping the pyproject.toml file or if I have a `version.py` or `__init__.py` file with my version it also supports to bump this file? Thanks.

---

_Comment by @zanieb on 2025-07-18 11:37_

See #13827 for that — not right now.


---

_Closed by @zanieb on 2025-07-18 11:37_

---
