```yaml
number: 4653
title: "Show tool versions in `uv tool list`"
type: issue
state: closed
author: zanieb
labels:
  - good first issue
  - needs-design
  - cli
  - preview
assignees: []
created_at: 2024-06-29T22:50:09Z
updated_at: 2024-07-03T18:24:39Z
url: https://github.com/astral-sh/uv/issues/4653
synced_at: 2026-01-10T05:31:37Z
```

# Show tool versions in `uv tool list`

---

_Issue opened by @zanieb on 2024-06-29 22:50_

In addition to the name, it'd be nice to show the installed version. Should this be behind a flag? Are there other commands like `uv pip list` or `pipx list` we should take inspiration from?

A simple example:

```
$ uv tool list --show-versions
black v20.0.1
```

Related:

- #4654 

---

_Label `needs-design` added by @zanieb on 2024-06-29 22:50_

---

_Label `cli` added by @zanieb on 2024-06-29 22:50_

---

_Label `preview` added by @zanieb on 2024-06-29 22:50_

---

_Label `good first issue` added by @zanieb on 2024-06-29 22:54_

---

_Comment by @blueraft on 2024-06-30 09:01_

The cargo command which includes both the version and entrypoints could be used as an inspiration too.

```sh
❯ cargo install --list
sqlx-cli v0.7.3:
    cargo-sqlx
    sqlx
```

---

_Comment by @zanieb on 2024-06-30 14:30_

Nice, I like the look of that

---

_Comment by @caiquejjx on 2024-06-30 16:55_

I think the version should be showed by default because it doesn't add that much of information, the entrypoints maybe we should add a flag like `--entrypoints` because for cases where we have a lot of tools it might be too much

---

_Comment by @charliermarsh on 2024-06-30 20:17_

Personally I'd probably do:

```
sqlx-cli==0.7.3:
    cargo-sqlx
    sqlx
```

Only because `sqlx-cli==0.7.3` matches `pip freeze`. But I don't feel strongly.

---

_Comment by @zanieb on 2024-06-30 22:01_

I don't love the pinned requirement syntax for something that will _always_ be a specific version and isn't meant for consumption by another tool.

---

_Comment by @caiquejjx on 2024-07-01 00:13_

I have created a pr based on @blueraft suggestion, lmk if it is the final design we want

---

_Closed by @zanieb on 2024-07-03 18:24_

---
