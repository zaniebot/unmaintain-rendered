```yaml
number: 12344
title: "Git installs over ssh don't seem to work"
type: issue
state: closed
author: Kroppeb
labels:
  - bug
assignees: []
created_at: 2025-03-20T16:40:08Z
updated_at: 2025-07-11T02:31:40Z
url: https://github.com/astral-sh/uv/issues/12344
synced_at: 2026-01-12T16:01:01Z
```

# Git installs over ssh don't seem to work

---

_@Kroppeb_

### Summary

I was checking out uv, so I looked at the docs on how to install git dependencies (see [here](https://docs.astral.sh/uv/concepts/projects/dependencies/#git)). But it seems to not work for ssh installs?

The docs tell me to add `git+` in front of the git clone compatible url, which seems to work if I use the https url, but not for ssh.

examples:
* `uv add git+https://github.com/astral-sh/uv.git` => does work
* `uv add git+git@github.com:astral-sh/uv.git` => does not work
* `uv add git+ssh://git@github.com/astral-sh/uv.git` => does not work


### Platform

Windows 11 x86_64

### Version

uv 0.6.8 (c1ef48276 2025-03-18)

### Python version

Python 3.12.1

---

_Label `bug` added by @Kroppeb on 2025-03-20 16:40_

---

_Comment by @charliermarsh on 2025-03-20 16:46_

This works for me without issue:

```
❯ uv add git+ssh://git@github.com/astral-sh/uv.git
Using CPython 3.13.0
Creating virtual environment at: .venv
Resolved 2 packages in 49ms
    Updated ssh://git@github.com/astral-sh/uv.git (584dd36e46732cd955ec3e984d97a7ba6569c019)
   Building uv @ git+ssh://git@github.com/astral-sh/uv.git@584dd36e46732cd955ec3e984d97a7ba6569c019
⠧ Preparing packages... (0/1)
```

I suspect there may be an issue in your SSH configuration or similar?

---

_Comment by @Kroppeb on 2025-03-20 17:29_

ugh, my bad. I had converted the `git@github.com:astral-sh/uv.git ` url to and ssh url without changing the `:` into a `/`. Realized my mistake while writing the issue and changed the `:` into a `/` that time, but then apparently forgot to add the `ssh://` 🤦 

And I guess `git+git@github.com:astral-sh/uv.git` doesn't work, because `git@github.com:astral-sh/uv.git` is not a url? It is confusing because it is something you can use in a `git clone` and it's what github gives you if you use the clone button (for ssh). I'm also coming from pipenv which does support this format.

---

_Comment by @charliermarsh on 2025-03-20 17:49_

No worries! Pipenv supports `git+git@github.com:astral-sh/uv.git`? Like it assumes `ssh`?

---

_Comment by @Kroppeb on 2025-03-20 18:00_

Yes it converts them into ssh urls*.

I'm confused about your second question. `git@github.com:astral-sh/uv.git` is an alternative syntax for ssh.


*
pipenv's handling of ssh urls is a bit broken and will result in corrupt lock files if no branch, tag or commit is specified.

---

_Comment by @charliermarsh on 2025-03-20 20:31_

Understood! The context for my clarification is: `pip install git+git@github.com:astral-sh/uv.git` doesn't work (same for `uv pip install`) -- they require `pip install git+ssh://git@github.com:astral-sh/uv.git` (so does `uv pip install`).

---

_Comment by @charliermarsh on 2025-07-11 02:31_

Merging with #14150.

---

_Closed by @charliermarsh on 2025-07-11 02:31_

---
