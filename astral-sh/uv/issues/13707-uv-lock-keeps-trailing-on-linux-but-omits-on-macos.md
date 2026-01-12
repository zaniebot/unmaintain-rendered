```yaml
number: 13707
title: "uv lock keeps trailing `/` on Linux, but omits on macOS"
type: issue
state: closed
author: ivanychev
labels:
  - bug
assignees: []
created_at: 2025-05-28T20:25:26Z
updated_at: 2025-06-27T15:11:22Z
url: https://github.com/astral-sh/uv/issues/13707
synced_at: 2026-01-12T16:01:35Z
```

# uv lock keeps trailing `/` on Linux, but omits on macOS

---

_@ivanychev_

### Summary

Hey! Thanks for amazing work and this very useful project. I'm facing behavior where local macOS `uv lock` generates a different lock file from what is done on Linux.

My `pyproject.toml` file contains


```toml
[[tool.uv.index]]
name = "mycompany"
url = "https://mycompany-123.d.codeartifact.us-east-1.amazonaws.com/pypi/mycompany/simple/"
default = true
authenticate = "always"
```

and when executing `uv lock` on macOS, it contains entries like

```
[[package]]
name = "tomli"
version = "2.2.1"
source = { registry = "https://mycompany-123.d.codeartifact.us-east-1.amazonaws.com/pypi/mycompany/simple" }
```


However, when doing 

```
uv sync --extra torch-gpu --locked --no-dev --no-install-project
```

during Docker build on Ubuntu 22.04 with same uv version, it fails because it tries to change the lock file. The `diff -u` showed this:

```
-source = { registry = "https://mycompany-123.d.codeartifact.us-east-1.amazonaws.com/pypi/mycompany/simple/" }
+source = { registry = "https://mycompany-123.d.codeartifact.us-east-1.amazonaws.com/pypi/mycompany/simple" }
```

During docker build, I have `UV_INDEX_MYCOMPANY_USERNAME` and `UV_INDEX_MYCOMPANY_PASSWORD` specified. 

Do you have any idea what might have gone wrong here?

### Platform

macOS 15.5 and Ubuntu 22.04

### Version

0.7.8

### Python version

3.11

---

_Label `bug` added by @ivanychev on 2025-05-28 20:25_

---

_Comment by @charliermarsh on 2025-05-30 03:55_

Can you include the verbose output of `uv sync --extra torch-gpu --locked --no-dev --no-install-project`? There might be something else going on here. There should be a line that indicates "why" the `--locked` fails.

---

_Comment by @lucas-m-1 on 2025-06-18 13:00_

I've got an additional issue, maybe related to this. On my machine (Ubuntu 22.04) I get different results (trailing slashes or not on the https://) when using `uv sync` or the pre-commit hook `uv-sync` from https://github.com/astral-sh/uv-pre-commit. There is obviously something wrong going on here, and it might be better to normalize everywhere...

Version: 0.7.13 for both local and pre-commit

Python version: 3.12

---------------------

Nevermind that came from my setting `UV_EXTRA_INDEX_URL=https://pypi.org/simple/`
Might be nice to normalize still 😃 


---

_Assigned to @jtfmumm by @zanieb on 2025-06-20 14:55_

---

_Comment by @jtfmumm on 2025-06-24 07:04_

Following up on this, @ivanychev if this is still an issue for you, would it be possible to share the verbose output of `uv sync --extra torch-gpu --locked --no-dev --no-install-project`?

---

_Label `needs-mre` added by @jtfmumm on 2025-06-24 07:04_

---

_Comment by @ivanychev on 2025-06-24 10:23_

Let me know if I could share anything else helpful. Our `pyproject.toml` has

```
[[tool.uv.index]]
name = "constructor"
url = "https://constructor-012345.d.codeartifact.us-east-1.amazonaws.com/pypi/mycompany/simple/"
default = true
authenticate = "always"
```

but we sometimes lock with

```
export UV_DEFAULT_INDEX=https://constructor-012345.d.codeartifact.us-east-1.amazonaws.com/pypi/mycompany/simple
```

The script that fails doesn't have `UV_DEFAULT_INDEX` set but has `UV_INDEX_MYCOMPANY_USERNAME`, `UV_INDEX_MYCOMPANY_PASSWORD`. Maybe that's the issue — I can fix our `UV_DEFAULT_INDEX` or `pyproject.toml` to both have trailing `/` or not, but I'd like to understand if it's default behavior that these urls are not normalized or not. IIUC the actual implementation does normalization somewhere because both urls work.

---

_Label `needs-mre` removed by @jtfmumm on 2025-06-24 14:17_

---

_Comment by @jtfmumm on 2025-06-24 14:42_

Thanks for sharing more detail @ivanychev. I don't think this is related to macOS/Linux differences, but is just a matter of uv using the exact URL you provided (as you indicate in your last comment about using `UV_DEFAULT_INDEX` sometimes). I'll look into normalizing the URL in the lockfile to prevent this issue.

---

_Comment by @ivanychev on 2025-06-24 17:58_

Thanks a lot @jtfmumm !

---

_Closed by @jtfmumm on 2025-06-27 15:11_

---
