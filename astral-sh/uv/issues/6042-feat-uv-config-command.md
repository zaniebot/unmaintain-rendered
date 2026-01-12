```yaml
number: 6042
title: "[feat] `uv config` command"
type: issue
state: open
author: baggiponte
labels:
  - enhancement
  - configuration
  - cli
assignees: []
created_at: 2024-08-12T19:46:51Z
updated_at: 2025-10-09T07:03:46Z
url: https://github.com/astral-sh/uv/issues/6042
synced_at: 2026-01-12T15:59:01Z
```

# [feat] `uv config` command

---

_@baggiponte_

Now that `uv` is XDG compliant it might matter less; but I'd argue it'd be helpful to have a `uv config` namespace to access uv configurations:

```
# list all configurations
uv config

# see current value for foo
uv config foo

# set value
uv config foo bar

# alternatively
uv config get foo
uv config set foo bar
```

---

_Comment by @zanieb on 2024-08-12 20:09_

Yeah we're interested in doing this eventually.

---

_Label `enhancement` added by @zanieb on 2024-08-12 20:09_

---

_Label `configuration` added by @zanieb on 2024-08-12 20:09_

---

_Label `cli` added by @zanieb on 2024-08-12 20:09_

---

_Comment by @kwaegel on 2024-11-04 18:31_

I'm also interested in this. Especially with the addition of multiple packages indexes, it would be really helpful to know what configuration values `uv` is picking up and where they're coming from (e.g. `uv config list`).

---

_Comment by @sebastian-correa on 2025-03-11 18:12_

I'd like to upvote this suggestion. Is there a system to do voice my support or is it just upvoting the comment?

If providing the full interface is too complicated, which I'd guess it is (where do you `set` values to?), I'd limit a first implementation to just listing and getting settings.

A flag like `git config`'s `--show-origin` would be **extremely** useful in this context to understand where each setting is being loaded from.

---

_Comment by @CharString on 2025-10-09 07:03_

This feature would make issues like https://github.com/tox-dev/tox-uv/issues/253 much easier to fix. Projects down stream wouldn't have to reimplement the environment and configuration file parsing and merging.

---
