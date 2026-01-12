```yaml
number: 4694
title: Show dedicated message for tools with no entrypoints
type: pull_request
state: merged
author: charliermarsh
labels:
  - cli
  - preview
assignees: []
merged: true
base: main
head: charlie/empty
created_at: 2024-07-01T13:43:09Z
updated_at: 2024-07-01T16:22:49Z
url: https://github.com/astral-sh/uv/pull/4694
synced_at: 2026-01-12T16:06:24Z
```

# Show dedicated message for tools with no entrypoints

---

_@charliermarsh_

## Summary

Closes https://github.com/astral-sh/uv/issues/4688.

## Test Plan

```
❯ cargo run tool install ruff
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.14s
     Running `target/debug/uv tool install ruff`
warning: `uv tool install` is experimental and may change without warning.
Resolved 1 package in 136ms
Installed 1 package in 3ms
 + ruff==0.5.0
No entrypoints to install for tool `ruff`
```


---

_Review requested from @zanieb by @charliermarsh on 2024-07-01 13:43_

---

_Review requested from @konstin by @charliermarsh on 2024-07-01 13:43_

---

_Label `cli` added by @charliermarsh on 2024-07-01 13:43_

---

_Label `preview` added by @charliermarsh on 2024-07-01 13:43_

---

_Comment by @zanieb on 2024-07-01 13:52_

Should we... fail? I feel like it doesn't "mean" anything to install a tool with no entry points.

---

_Comment by @charliermarsh on 2024-07-01 13:55_

I mildly prefer not failing.

---

_Comment by @zanieb on 2024-07-01 14:14_

Separately, I just noticed your example: we should probably have a message for installation of Ruff's executable.

---

_Comment by @zanieb on 2024-07-01 14:14_

If there are no provided executables or entry points.. what is installing a tool?

---

_Comment by @charliermarsh on 2024-07-01 14:16_

Ruff is fixed in https://github.com/astral-sh/uv/pull/4693.

---

_Comment by @charliermarsh on 2024-07-01 14:17_

> If there are no provided executables or entry points.. what is installing a tool?

It puts the tool in a dedicated environment on your machine. In theory you could run that environment if you wanted an isolated environment with the tool? I don't know the use-case but failing seems user-hostile. Anyway you can decide, I don't feel the need to keep discussing.

---

_Comment by @charliermarsh on 2024-07-01 14:26_

One data point:

```
❯ pipx install requests
Note: Dependent package 'charset-normalizer' contains 1 apps
  - normalizer

No apps associated with package requests. Try again with '--include-deps' to include apps of dependent packages, which
are listed above. If you are attempting to install a library, pipx should not be used. Consider using pip or a similar
tool instead.
```

---

_Comment by @charliermarsh on 2024-07-01 14:27_

That seems like two votes for erroring, so I'll do that.

---

_@zanieb approved on 2024-07-01 16:11_

Thanks! 

I think we can revisit later, but yeah right now the environments are intended not to be user-facing and I'd recommend some other command for the no-entrypoints use case.

---

_Comment by @charliermarsh on 2024-07-01 16:22_

Yeah all good!

---

_Merged by @charliermarsh on 2024-07-01 16:22_

---

_Closed by @charliermarsh on 2024-07-01 16:22_

---

_Branch deleted on 2024-07-01 16:22_

---
