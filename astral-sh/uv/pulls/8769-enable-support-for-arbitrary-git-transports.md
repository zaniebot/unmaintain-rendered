```yaml
number: 8769
title: Enable support for arbitrary git transports
type: pull_request
state: merged
author: rdeaton
labels:
  - enhancement
assignees: []
merged: true
base: main
head: main
created_at: 2024-11-03T03:37:08Z
updated_at: 2024-11-05T07:55:16Z
url: https://github.com/astral-sh/uv/pull/8769
synced_at: 2026-01-12T16:08:29Z
```

# Enable support for arbitrary git transports

---

_@rdeaton_

## Summary

Allows arbitrary git protocols through https://git-scm.com/docs/gitremote-helpers

## Test Plan

I ran the test suite, which hits this path and still succeeds, and I tested quickly locally with a dummy transport. Since this just passes through to the git cli it was fairly straightforward.

---

_Comment by @FishAlchemist on 2024-11-03 06:06_

As a Windows user, I would like to offer my perspective on your PR, even though I am not a member of the review team.

It seems like ``git version 2.47.0.windows.2`` only supports ftp, ftps, http, and https. If you want to widely support multiple protocols using git, I don't think it's a good choice.

Path: ``C:\Program Files\Git\mingw64\libexec\git-core``
![image](https://github.com/user-attachments/assets/3a149ef1-8555-4754-8507-9e579fd51c04)

But perhaps you could try to get all the tests to pass first?
![image](https://github.com/user-attachments/assets/a1d0708c-264d-4755-a944-45c588becd9f)



---

_Comment by @rdeaton on 2024-11-03 10:28_

@FishAlchemist you misunderstand, please read the linked git manpage, additional protocols are supported on Windows as well, but that's a local environment configuration outside the scope of `uv`.

---

_Comment by @rdeaton on 2024-11-03 10:34_

The test failures seem to be a flake or broken on main. They fail on 2ed94745a2cba996c617ffbb686a2d99baf7fecc on my machine as well.

---

_Comment by @konstin on 2024-11-03 15:44_

The test failures are unrelated, https://github.com/astral-sh/uv/pull/8776 fixes them.

@rdeaton Can you explain why you need arbitrary git transports?

---

_Comment by @rdeaton on 2024-11-03 15:53_

They're useful, and used in corporate environments with extra security requirements around exposing git repositories, especially in "zero-trust" like models. There are also a handful of interesting applications of them in various OSS tools.

https://github.com/glandium/git-cinnabar
https://github.com/Git-Mediawiki/Git-Mediawiki/blob/master/docs/Remote-Helpers.md
https://github.com/felipec/git-remote-bzr
https://github.com/git/git/blob/master/contrib/remote-helpers/git-remote-hg
https://docs.gosh.sh/working-with-gosh/git-remote-helper/
https://www.npmjs.com/package/@protocol.land/git-remote-helper

---

_Comment by @rdeaton on 2024-11-03 15:55_

And fwiw, poetry made this possible in 1.2.0 through https://python-poetry.org/docs/configuration/#experimentalsystem-git-client

---

_Comment by @corleyma on 2024-11-04 19:10_

@konstin  Here's an example of how a git remote helper might be used in a corporate environment for security purposes (e.g., dealing with an authenticating proxy like Google's Identity Aware Proxy that might be sitting in front of your VCS): https://github.com/adohkan/git-remote-https-iap

---

_Label `needs-decision` added by @zanieb on 2024-11-04 20:24_

---

_Comment by @konstin on 2024-11-05 07:26_

Thanks @corleyma that's a great example

---

_Label `needs-decision` removed by @konstin on 2024-11-05 07:26_

---

_@konstin approved on 2024-11-05 07:27_

---

_Label `enhancement` added by @konstin on 2024-11-05 07:55_

---

_Merged by @konstin on 2024-11-05 07:55_

---

_Closed by @konstin on 2024-11-05 07:55_

---
