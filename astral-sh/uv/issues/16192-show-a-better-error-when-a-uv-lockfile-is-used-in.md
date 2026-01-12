```yaml
number: 16192
title: "Show a better error when a uv lockfile is used in `-r`"
type: issue
state: open
author: zanieb
labels:
  - help wanted
  - error messages
assignees: []
created_at: 2025-10-08T15:28:22Z
updated_at: 2025-10-16T07:45:16Z
url: https://github.com/astral-sh/uv/issues/16192
synced_at: 2026-01-12T16:02:26Z
```

# Show a better error when a uv lockfile is used in `-r`

---

_@zanieb_

e.g., if you try to use `uv lock --script action.py` then `-r action.py.lock`

```
error: Couldn't parse requirement in `action.py.lock` at position 0
  Caused by: no such comparison operator "=", must be one of ~= == != <= >= < > ===
version = 1
        ^^^
```

We can do better by detecting our own lockfile format


---

_Label `help wanted` added by @zanieb on 2025-10-08 15:28_

---

_Label `error messages` added by @zanieb on 2025-10-08 15:28_

---

_Comment by @CakeVision on 2025-10-08 17:42_

I could take a look at this

---

_Comment by @cmnemoi on 2025-10-15 14:41_

Hello,

How do you think this issue should be tackled ? 

I see in https://github.com/Arkenstone999/uv/blob/main/crates/uv-requirements/src/sources.rs that the requirements source format is determined solely based on filename.

However, `uv` is not the only tool which uses lockfiles with `.lock` extension so we can't just check that.

And this MR has been rejected (https://github.com/astral-sh/uv/pull/16282/files) partially (?) by trying to infer the lock format by analyzing its content.

---

_Comment by @CakeVision on 2025-10-16 07:45_

> Hello,
> 
> How do you think this issue should be tackled ? 
> 
> I see in https://github.com/Arkenstone999/uv/blob/main/crates/uv-requirements/src/sources.rs that the requirements source format is determined solely based on filename.
> 
> However, `uv` is not the only tool which uses lockfiles with `.lock` extension so we can't just check that.
> 
> And this MR has been rejected (https://github.com/astral-sh/uv/pull/16282/files) partially (?) by trying to infer the lock format by analyzing its content.

I had a look during the weekend and it's why it took a bit for me to make a pr here. Took the same approach as the pr mentioned in your reply, but realized it will be error prone to detect that way about one day into development. Would appreciate some help designing this. My best idea so far is to use custom names for the locks based on their type when they are generated, but i'm not familiar enough with uv's design to make an educated choice

---
