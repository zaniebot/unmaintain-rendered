```yaml
number: 8682
title: Alternative to pip install --no deps
type: issue
state: closed
author: tauferp
labels:
  - question
assignees: []
created_at: 2024-10-29T19:44:58Z
updated_at: 2025-07-19T10:54:08Z
url: https://github.com/astral-sh/uv/issues/8682
synced_at: 2026-01-12T15:59:32Z
```

# Alternative to pip install --no deps

---

_@tauferp_

Is there a way to mark some dependencies to install without their dependencies with uv? This is similar to https://github.com/pypa/pip/pull/10837 .

For example installing `pgmpy` but without `pytorch`. It's ok to add rest of `pgmpy` dependendecies directly.

---

_Comment by @notatallshaw on 2024-10-29 19:54_

You can create an override file this this: https://github.com/astral-sh/uv/issues/4422#issuecomment-2254182800

---

_Label `question` added by @samypr100 on 2024-10-31 23:58_

---

_Closed by @charliermarsh on 2025-03-03 03:18_

---

_Comment by @m-legrand on 2025-03-27 14:45_

Sorry why was it marked as completed? Is there an equivalent to `pip install <package> --no-deps` available now?

The hack mentioned above is hardly a solution to the problem,
Not only does it rely on an implementation detail (see the comment below: https://github.com/astral-sh/uv/issues/4422#issuecomment-2255411693), but it doesn't serve the same purpose.

For instance, I want to use an internal library which has plently of "<package> == <version>" requirements which I know for a fact are abusive, and I want to relax. 
But I don't want uv to not install at all those packages (e.g. `pandas`).



---

_Comment by @notatallshaw on 2025-03-27 14:48_

>  Is there an equivalent to pip install <package> --no-deps available now?

Yes:

```
uv pip install <package> --no-deps 
```

The question was about overriding a specific dependency, which pip doesn't support, but uv does by using an override file or manually specifying the dependency metadata: https://docs.astral.sh/uv/reference/settings/#dependency-metadata

---
