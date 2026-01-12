```yaml
number: 11899
title: Better message when using aws codeartifact and token has expired
type: issue
state: closed
author: waterworthd-cim
labels:
  - error messages
assignees: []
created_at: 2025-03-03T02:25:58Z
updated_at: 2025-03-03T15:16:50Z
url: https://github.com/astral-sh/uv/issues/11899
synced_at: 2026-01-12T16:00:49Z
```

# Better message when using aws codeartifact and token has expired

---

_@waterworthd-cim_

### Summary

We use AWS CodeArtifact as our PyPI repository. Every 12 hours the CA token expires and I have to re-auth.

When this happens with say poetry I get an error message, with uv I get a very misleading message, i.e.

```
% uv sync
  × No solution found when resolving dependencies for split (python_full_version >= '3.13'):
  ╰─▶ Because only polars[pyarrow]<1.23.0 is available and your project depends on polars[pyarrow]>=1.23.0, we can conclude that your project's requirements are unsatisfiable.
```

This makes no sense - I'm using python 3.10 and "only polars[pyarrow]<1.23.0 is available" is probable because uv has determined no version of poetry is available because the code artifact request failed due to invalid (expired) credentials.

Ideally it would simply bubble up the CA authentication failure exception?

### Example

_No response_

---

_Label `enhancement` added by @waterworthd-cim on 2025-03-03 02:25_

---

_Comment by @charliermarsh on 2025-03-03 02:30_

Unfortunately, CodeArtifact returns a 404 when you provide invalid credentials. We have no way of differentiating "The package doesn't exist on the registry" from "The credentials are invalid".

---

_Comment by @waterworthd-cim on 2025-03-03 03:43_

I get the following from poetry - it's not awesome but it's fairly clear that my credentials have expired. The main difference is the poetry says "Unable to find installation candidates" and uv implies there are candidates.

```
Source (private): Authorization error accessing https://XXX-XXX.d.codeartifact.ap-southeast-2.amazonaws.com/pypi/pypi-cim/simple/referencing/
  - Updating botocore (1.35.97 -> 1.36.6): Failed

  RuntimeError

  Unable to find installation candidates for botocore (1.36.6)

  at ~/.local/pipx/venvs/poetry/lib/python3.12/site-packages/poetry/installation/chooser.py:74 in choose_for
```

---

_Comment by @charliermarsh on 2025-03-03 03:50_

Actually it seems like some routes are returning a 401, so now I'm confused because we do have dedicated error messages for 401s. (I think I was mixing up CodeArtifact with another registry provider when I mentioned 404s above.)

---

_Assigned to @charliermarsh by @charliermarsh on 2025-03-03 03:50_

---

_Comment by @charliermarsh on 2025-03-03 03:51_

Are you on a recent version? I get this:

```
  × No solution found when resolving dependencies:
  ╰─▶ Because black was not found in the package registry and you require black, we can conclude that your requirements
      are unsatisfiable.

      hint: An index URL (https://test-XXXXXXX.d.codeartifact.us-east-2.amazonaws.com/pypi/astral/simple) could not
      be queried due to a lack of valid authentication credentials (401 Unauthorized)
```


---

_Label `enhancement` removed by @charliermarsh on 2025-03-03 03:51_

---

_Label `error messages` added by @charliermarsh on 2025-03-03 03:51_

---

_Comment by @waterworthd-cim on 2025-03-03 04:17_

I'm on 0.5.8 - I'll update and see if that fixes it.

---

_Comment by @waterworthd-cim on 2025-03-03 04:19_

Oh - so I've been running an older version installed with cargo, which was hiding the newer version installed by the shell script...

---

_Comment by @charliermarsh on 2025-03-03 15:16_

👍 Should work as expected in newer versions.

---

_Closed by @charliermarsh on 2025-03-03 15:16_

---
