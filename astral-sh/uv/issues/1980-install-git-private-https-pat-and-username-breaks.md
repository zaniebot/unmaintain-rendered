```yaml
number: 1980
title: "`install_git_private_https_pat_and_username` breaks git"
type: issue
state: closed
author: konstin
labels:
  - help wanted
  - testing
assignees: []
created_at: 2024-02-26T09:35:48Z
updated_at: 2025-12-01T11:41:23Z
url: https://github.com/astral-sh/uv/issues/1980
synced_at: 2026-01-12T15:58:33Z
```

# `install_git_private_https_pat_and_username` breaks git

---

_@konstin_

Running `install_git_private_https_pat_and_username` inserts the user and password for astral-test-user into my keyring:

![image](https://github.com/astral-sh/uv/assets/6826232/3507bf99-4003-4130-ba1f-ba2be453cedf)

This breaks my git setup because now _all_ invocations in any repository with a github remote try to use the astral-test-user for github:

```
$ git push
remote: Permission to astral-sh/uv.git denied to astral-test-bot.
fatal: unable to access 'https://github.com/astral-sh/uv/': The requested URL returned error: 403
```

The test suite should not change a developer's user wide settings, especially if that breaks git.

---

_Label `bug` added by @konstin on 2024-02-26 09:35_

---

_Label `internal` added by @zanieb on 2024-02-28 18:15_

---

_Comment by @zanieb on 2024-02-28 18:20_

This is annoying... sorry.

The solution is probably to create a temporary [git config](https://git-scm.com/docs/git-config#SCOPES) then configure or disable the [git credential helper](https://git-scm.com/docs/gitcredentials) — I don't think we should need one enabled at all this test.

---

_Label `bug` removed by @charliermarsh on 2024-03-22 04:14_

---

_Label `internal` removed by @charliermarsh on 2024-03-22 04:14_

---

_Label `testing` added by @charliermarsh on 2024-03-22 04:14_

---

_Closed by @konstin on 2024-07-01 14:44_

---

_Comment by @zanieb on 2024-07-01 14:50_

@konstin is this fixed.... or did we just turn the test off?

---

_Comment by @konstin on 2024-07-01 14:52_

You're right, we only turned the test off

---

_Reopened by @konstin on 2024-07-01 14:52_

---

_Label `help wanted` added by @zanieb on 2024-07-01 21:52_

---

_Comment by @sid-38 on 2025-11-30 03:49_

@zanieb @konstin , I have created a PR to resolve this. Take a look and let me know if it needs any changes

---

_Closed by @konstin on 2025-12-01 11:41_

---
