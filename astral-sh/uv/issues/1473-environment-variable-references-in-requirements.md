```yaml
number: 1473
title: environment variable references in requirements files are not resolved
type: issue
state: closed
author: detlefla
labels:
  - compatibility
assignees: []
created_at: 2024-02-16T10:18:59Z
updated_at: 2024-03-03T23:47:53Z
url: https://github.com/astral-sh/uv/issues/1473
synced_at: 2026-01-10T05:40:31Z
```

# environment variable references in requirements files are not resolved

---

_Issue opened by @detlefla on 2024-02-16 10:18_

If a requirements file contains a line `-r ${OTHERPKG}/requirements/dev.in`, pip-compile tries to replace `${OTHERPKG}` with the contents of an environment variable OTHERPKG. uv, however, uses the string literally and fails to find the location of the file to be included.

Maybe this is a feature uv has not implemented yet; I apologize if it's a documented restriction and I just didn't find it!

---

_Label `compatibility` added by @MichaReiser on 2024-02-16 11:51_

---

_Comment by @charliermarsh on 2024-02-16 14:56_

Ah, we actually do support this, but not for paths to requirements files, only for dependency specifiers (like `-e ${OTHER_PKG}/..`).

---

_Comment by @inoa-jboliveira on 2024-02-17 19:55_

@charliermarsh  I am not sure this issue from @detlefla is the same I have, but I believe uv doesn't support environment variables inside of requirements.txt file as described here: https://pip.pypa.io/en/stable/reference/requirements-file-format/#using-environment-variables

My txt file has the following:

```
mod-wsgi==${MOD_WSGI_WINDOWS_VERSION} ; sys_platform=='win32' and platform_machine=='AMD64'
```

But uv fails with 
```
error: Couldn't parse requirement in `.\requirements\pip-deploy.txt` at position 276
  Caused by: expected version to start with a number, but no leading ASCII digits were found
mod-wsgi==${MOD_WSGI_WINDOWS_VERSION} ; sys_platform=='win32' and platform_machine=='AMD64'
        ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
```

---

_Comment by @charliermarsh on 2024-02-17 19:58_

@inoa-jboliveira - It's similar! We support environment variables in URLs, but not in arbitrary locations like in a version specifier, so that's something we'd have to implement.

---

_Comment by @inoa-jboliveira on 2024-02-17 23:38_

Added support for both cases in the PR above

---

_Closed by @charliermarsh on 2024-03-03 23:47_

---

_Comment by @charliermarsh on 2024-03-03 23:47_

The originating issue here (supporting environment variables in `-r` and `-c` paths, within requirements files) is now fixed by https://github.com/astral-sh/uv/pull/2143. If folks want environment variable expansion for other parts of the file (like version specifiers), we need to handle those on a case-by-case basis, so feel free to open a new issue to track it.

---
