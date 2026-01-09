---
number: 15758
title: skip username and password during pyx upload when user is already authenticated to pyx
type: issue
state: closed
author: JTP3XP
labels:
  - enhancement
assignees: []
created_at: 2025-09-09T15:51:41Z
updated_at: 2025-09-09T19:03:33Z
url: https://github.com/astral-sh/uv/issues/15758
synced_at: 2026-01-07T13:12:19-06:00
---

# skip username and password during pyx upload when user is already authenticated to pyx

---

_Issue opened by @JTP3XP on 2025-09-09 15:51_

### Summary

This is a change in behavior from the alpha build to the v0.8.15 version. On the alpha build, when already authenticated to pyx, the upload command would not ask for a username and password:

```
$ uv publish dist/...-2.0.0-py3-none-any.whl --publish-url https://api.pyx.dev/v1/upload/.../main
Publishing 1 file to https://api.pyx.dev/v1/upload/.../main
Uploading ...-2.0.0-py3-none-any.whl (16.6KiB)
```

This behavior felt great - the upload just runs without further input.

On v0.8.15 we get a username and password prompt that we can just continue past without providing any input and the upload works as before:

```
$ uv publish dist/...-2.0.0-py3-none-any.whl --publish-url https://api.pyx.dev/v1/upload/.../main --verbose
DEBUG uv 0.8.15
Publishing 1 file to https://api.pyx.dev/v1/upload/.../main
DEBUG Using request timeout of 900s
Enter username ('__token__' if using a token):
Enter password:
Uploading ...-2.0.0-py3-none-any.whl (16.6KiB)
DEBUG Hashing dist/...-2.0.0-py3-none-any.whl
DEBUG Performing validation request for https://api.pyx.dev/v1/upload/.../main
DEBUG Using HTTP Basic authentication
DEBUG Access token is up-to-date (`2025-09-09T16:10:58Z`)
DEBUG Response code for https://api.pyx.dev/v1/upload/.../main/validate: 200 OK
DEBUG Using HTTP Basic authentication
DEBUG Response code for https://api.pyx.dev/v1/upload/../main: 200 OK
INFO Upload succeeded
```

I think the current behavior is not intuitive, because after running `uv auth login pyx.dev`, we technically do have a token, so the response `Enter username ('__token__' if using a token):` might make the user think they should enter `__token__` and then provide the token, but this is not necessary (though it works, regardless of whether or not the token is provided in the password field).

### Example

My use case is very simple, so maybe there are situations this would break down, but for me the most intuitive result to running `uv publish` after authenticating to pyx would be that uv sees my publish url is `...pyx.dev...` and uses the token I have saved for `pyx.dev` to attempt the upload without further input from me.

If the username and password prompt makes sense to keep even with pyx authentication complete and a pyx publish url, then maybe we could have an option, like `--non-interactive` when we think uv will be able to figure out the credentials on its own.

---

_Label `enhancement` added by @JTP3XP on 2025-09-09 15:51_

---

_Comment by @charliermarsh on 2025-09-09 15:52_

Sorry about that, will fix today.

---

_Assigned to @charliermarsh by @charliermarsh on 2025-09-09 15:52_

---

_Comment by @charliermarsh on 2025-09-09 15:53_

(I think this was an unintentional regression, likely my fault.)

---

_Referenced in [astral-sh/uv#15759](../../astral-sh/uv/pulls/15759.md) on 2025-09-09 16:01_

---

_Closed by @charliermarsh on 2025-09-09 16:13_

---

_Comment by @JTP3XP on 2025-09-09 16:15_

Thanks for the fast response! h/t to @cmccabe2 for noticing this first

---

_Comment by @charliermarsh on 2025-09-09 19:03_

We'll get this out today.

---
