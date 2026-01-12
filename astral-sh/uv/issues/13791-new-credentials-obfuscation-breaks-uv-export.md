```yaml
number: 13791
title: "New credentials obfuscation breaks `uv export`"
type: issue
state: closed
author: jlconnor
labels:
  - bug
  - needs-mre
assignees: []
created_at: 2025-06-02T22:12:59Z
updated_at: 2025-06-03T16:12:35Z
url: https://github.com/astral-sh/uv/issues/13791
synced_at: 2026-01-12T16:01:37Z
```

# New credentials obfuscation breaks `uv export`

---

_@jlconnor_

### Summary

My team and I are also running into an issue where uv export is not un-obfuscating the credentials and causing pip to fail.

```
  ****@github.com: Permission denied (publickey).
  fatal: Could not read from remote repository.
```

### Platform

Linux x86_64 GitHub Actions build platform, Also MacOS 15.5 (24F74)

### Version

0.7.9

### Python version

3.11.12 + 3.13

---

_Label `bug` added by @jlconnor on 2025-06-02 22:13_

---

_Comment by @zanieb on 2025-06-02 22:16_

Please share a reproducible example, as described in #9452

---

_Comment by @charliermarsh on 2025-06-02 22:16_

Hmm, my assumption is that `uv export` should _always_ have been stripping credentials even before that change. Was it not?

---

_Label `needs-mre` added by @zanieb on 2025-06-02 22:16_

---

_Comment by @stolpa4 on 2025-06-03 06:45_

I can reproduce this, all my private packages are now put into requirements.txt with these stars, which make my requirements.txt file effectively useless. I had no credentials in my dependencies, all the packages are pulled via ssh, with the help of ssh agent. 

---

_Comment by @rivershah on 2025-06-03 08:22_

`0.7.9` fails to propagate credentials for authenticated wheel URLs. When those wheels depend on other authenticated wheels, uv fetches the transitive dependencies without authentication and raises errors

For example I now seeing strange behaviour such as this:
```
$ uv pip install --compile-bytecode --system -r pyproject.toml --all-extras --no-cache
Using Python 3.12.10 environment at: /usr/local
Resolved 159 packages in 3.85s
Bytecode compiled 28515 files in 1.02s

$ uv pip install --compile-bytecode --system -r pyproject.toml --all-extras
Using Python 3.12.10 environment at: /usr/local
  × Failed to download `xxx @ https://<token>@gitlab.com/api/v4/projects/<project-id>/packages/pypi/files/<file-id>/xxx.whl`
  ├─▶ Failed to fetch: `https://gitlab.com/api/v4/projects/<project-id>/packages/pypi/files/<file-id>/xxx.whl`
  ╰─▶ HTTP status client error (401 Unauthorized) for url (https://gitlab.com/api/v4/projects/<project-id>/packages/pypi/files/<file-id>/xxxx.whl)
```

The wheel installation that is rejected is not a direct dependency

---

_Comment by @konstin on 2025-06-03 09:40_

I can reproduce this problem with:
```toml
[project]
name = "debug"
version = "0.1.0"
requires-python = ">=3.12"
dependencies = [
  "tqdm @ git+ssh://ferris:myverysecretpassword@github.com/tqdm/tqdm.git",
]
```

After #13799, we're showing `git` as username.

---

@rivershah can you share a redacted `pyproject.toml`?

---

_Comment by @konstin on 2025-06-03 12:07_

@stolpa4 If you say no credentials, is there a username on the URL, such as the `git` in `git+ssh://git@github.com`?

---

_Comment by @konstin on 2025-06-03 12:15_

If you want to try, https://github.com/astral-sh/uv/pull/13799 should fix the credential redaction for `git+ssh` URLs.

---

_Comment by @rivershah on 2025-06-03 14:03_

@konstin Unfortunately I can't give a nice MRE as internal gitlab. Not sure if this is useful but posting in case it is:

```
[build-system]
    requires      = ["setuptools>=64", "setuptools_scm>=8"]
    build-backend = "setuptools.build_meta"

[project]
    name = "xxx"
    description = "xxx"
    dynamic = ["version"]
    readme = "README.md"
    requires-python = ">=3.12"
    dependencies = [
        "jax[cuda,tpu]==0.6.1",
        "pandas>=2.2.3",
        "xxx @ https://<token>@gitlab.com/api/v4/projects/<project-id>/packages/pypi/files/<file-id>/xxx.whl" # this will download but the subsequent authenticated wheel url deps will not work and raise the error
    ]
.....
```

---

_Comment by @zanieb on 2025-06-03 16:12_

@rivershah let's move to #13823, that is off-topic here

---

_Closed by @zanieb on 2025-06-03 16:12_

---
