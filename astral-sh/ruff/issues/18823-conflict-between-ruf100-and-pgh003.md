```yaml
number: 18823
title: conflict between RUF100 and PGH003
type: issue
state: closed
author: MarcelWilson
labels:
  - question
assignees: []
created_at: 2025-06-20T15:50:25Z
updated_at: 2025-06-20T20:50:07Z
url: https://github.com/astral-sh/ruff/issues/18823
synced_at: 2026-01-10T11:09:58Z
```

# conflict between RUF100 and PGH003

---

_Issue opened by @MarcelWilson on 2025-06-20 15:50_

### Summary

ruff 0.12.0 incorrectly triggers ``RUF100 [*] Unused `noqa` directive (unused: `PGH003`)`` on the following line at the top of a file (using `ruff check .`)

```python
# ruff: noqa: PGH003 type: ignore
```

### Version

0.12.0

---

_Comment by @ntBre on 2025-06-20 16:28_

Does that `type: ignore` comment work? I tested this in the [ty](https://play.ty.dev/0fbab10e-f3b1-413d-a0a7-b74b93a08ca5) and [pyright](https://pyright-play.net/?pythonVersion=3.8&strict=true&enableExperimentalFeatures=true&reportImplicitOverride=true&reportShadowedImports=true&reportUnusedCallResult=true&code=MQAgTgrgZlBcIDsD2BHAhvACgcQBIAZ8BmEAFwE8AHAU3gEsBzZMagKAA96FSQBeEAEQBnAUA) playgrounds, and it doesn't appear to work, i.e. the typing diagnostic still appears unless I remove the `ruff: noqa: PGH003` part. I think both comments will work as intended on separate lines:

```python
# type: ignore
# ruff: noqa: PGH003
```

---

_Label `question` added by @ntBre on 2025-06-20 16:28_

---

_Comment by @MarcelWilson on 2025-06-20 18:03_

Last I checked `# type: ignore` tells mypy to ignore the file (unless that's changed recently).

---

_Comment by @MarcelWilson on 2025-06-20 18:09_

> I think both comments will work as intended on separate lines:

Ahh, you're right, they do work that way.  I guess it's unclear if this issue is actually a bug, then.   Should a single liner work the same way?

---

_Comment by @ntBre on 2025-06-20 18:18_

I'm also seeing the type error in the [mypy playground](https://mypy-play.net/?mypy=latest&python=3.12&gist=9a75314564c4a339eada6cae33d165b9) and [pyrefly playground](https://pyrefly.org/sandbox/?code=MQAgTgrgZlBcIDsD2BHAhvACgcQBIAZ8BmEAFwE8AHAU3gEsBzZMagKAA96FSQBeEAEQBnAUA), so I think the single line form after the noqa isn't parsed by any of these type checkers. I think that means PGH003 is correct not to trigger (no type errors are actually ignored by this comment) and thus the noqa is actually unused. So yeah, I think this is working as intended.

---

_Closed by @dylwil3 on 2025-06-20 20:50_

---
