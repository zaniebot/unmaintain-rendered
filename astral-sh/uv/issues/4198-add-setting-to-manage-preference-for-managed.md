---
number: 4198
title: Add setting to manage preference for managed toolchains
type: issue
state: closed
author: zanieb
labels:
  - configuration
assignees: []
created_at: 2024-06-10T14:01:25Z
updated_at: 2024-09-14T21:56:01Z
url: https://github.com/astral-sh/uv/issues/4198
synced_at: 2026-01-10T01:23:35Z
---

# Add setting to manage preference for managed toolchains

---

_Issue opened by @zanieb on 2024-06-10 14:01_

Users should be able to opt in and out of managed toolchains, e.g.

```
managed-toolchains = always | preferred | fallback | never
```



---

_Label `configuration` added by @zanieb on 2024-06-10 14:01_

---

_Label `preview` added by @zanieb on 2024-06-10 14:01_

---

_Comment by @zanieb on 2024-06-10 14:02_

We could also frame this differently, like..

```
toolchain-discovery = prefer-system | prefer-managed | only-system | only-managed
```

---

_Referenced in [astral-sh/uv#2607](../../astral-sh/uv/issues/2607.md) on 2024-06-11 13:12_

---

_Referenced in [astral-sh/uv#4320](../../astral-sh/uv/issues/4320.md) on 2024-06-16 03:45_

---

_Referenced in [astral-sh/uv#4411](../../astral-sh/uv/issues/4411.md) on 2024-06-19 14:56_

---

_Referenced in [astral-sh/uv#4416](../../astral-sh/uv/pulls/4416.md) on 2024-06-19 16:57_

---

_Comment by @baggiponte on 2024-06-20 09:12_

> We could also frame this differently, like..
> 
> ```
> toolchain-discovery = prefer-system | prefer-managed | only-system | only-managed
> ```

What about?

```
toolchains = uv-first | system-first | managed-first | uv-only | system-only | managed-only
```

Or you could explicitly configure python providers like [PDM](https://pdm-project.org/en/latest/usage/config/#configure-the-python-finder). People might (and are) using tools like mise asdf pyenv.


---

_Comment by @zanieb on 2024-06-20 13:12_

What's the difference between `uv-only` and `managed-only` there?

I don't think we'll support discovery of Python interpreters in _other_ tool's installation directories. It seems more reasonable to just expect them to be registered in the PATH (or, on WIndows, in the registry).

---

_Comment by @baggiponte on 2024-06-20 15:08_

> What's the difference between `uv-only` and `managed-only` there?
> 
> I don't think we'll support discovery of Python interpreters in _other_ tool's installation directories. It seems more reasonable to just expect them to be registered in the PATH (or, on WIndows, in the registry).

Makes sense, but then I guess what's the difference between `system` python and other tools? In other words, brew python should just not come up, am I correct?

---

_Comment by @zanieb on 2024-06-20 15:13_

A "system" Python here is any Python toolchain installed on the system i.e. not being managed by uv. Brew Python will come up if it's on the PATH, but we won't try to discover it a Brew-specific location (as we won't, thusfar, attempt to discover `pyenv` Python in their storage directory)

---

_Label `preview` removed by @zanieb on 2024-08-20 18:23_

---

_Comment by @delfick on 2024-09-14 07:27_

It would be useful to be able to tell uv to prefer using asdf/pyenv discovery/installation if that's available rather than only python-build-standalone

---

_Comment by @zanieb on 2024-09-14 13:08_

This issue is actually completed: https://docs.astral.sh/uv/reference/settings/#python-preference

Feel free to open a new one to track support for other managed Python distribution kinds, but we're pretty unlikely to support more.

---

_Closed by @zanieb on 2024-09-14 13:08_

---

_Comment by @delfick on 2024-09-14 21:56_

sure, done https://github.com/astral-sh/uv/issues/7400

Thanks!

---
