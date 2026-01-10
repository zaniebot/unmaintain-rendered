```yaml
number: 1166
title: "Guide user towards `-r` flag in `pip install`"
type: issue
state: closed
author: charliermarsh
labels:
  - error messages
assignees: []
created_at: 2024-01-29T11:58:44Z
updated_at: 2024-01-30T18:58:46Z
url: https://github.com/astral-sh/uv/issues/1166
synced_at: 2026-01-10T05:40:31Z
```

# Guide user towards `-r` flag in `pip install`

---

_Issue opened by @charliermarsh on 2024-01-29 11:58_

E.g., this error should suggest the right argument:

```
❯ puffin pip install requirements.txt 
error: Package `requirements-txt` was not found in the registry.
```

---

_Label `error messages` added by @charliermarsh on 2024-01-29 11:58_

---

_Comment by @AlexWaygood on 2024-01-29 12:25_

Pip gives a tailored error message for this:

![image](https://github.com/astral-sh/puffin/assets/66076021/abbe6c25-6c0a-4546-b4f2-fc812b5321eb)

_But_... I find pip's message weirdly aggressive? I feel like there's an implied "... you idiot" after the "(which cannot exist)" subclause 😆

---

_Comment by @MichaReiser on 2024-01-29 12:40_

> But... I find pip's message weirdly aggressive? I feel like there's an implied "... you idiot" after the "(which cannot exist)" subclause 😆

If `requirements.txt` is guaranteed t not be a valid package name, then I would prefer that puffin emits a warning but continues as if I ran `puffin pip install -r requirements.txt`. Aborting while knowing what I wanted to do is just annoying.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-01-29 12:45_

---

_Comment by @charliermarsh on 2024-01-29 12:45_

I can take this one, I have to review the rules.

---

_Comment by @charliermarsh on 2024-01-29 15:32_

Funny, pip only does this for _exactly_ `requirements.txt`. (`requirements.txt` is actually a valid package name per the spec, but it's forbidden by PyPI.) See: https://github.com/pypa/pip/pull/9915.

Meanwhile, `requirements-dev.txt` actually _is_ a valid package name but it's squatted on PyPI (https://pypi.org/project/requirements-dev.txt/) to guard against this.

---

_Closed by @charliermarsh on 2024-01-30 18:58_

---

_Closed by @charliermarsh on 2024-01-30 18:58_

---
