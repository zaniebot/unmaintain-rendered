---
number: 12981
title: "Is there a `--only-dev` or its just in the docs?"
type: issue
state: closed
author: alexey-pelykh
labels:
  - question
assignees: []
created_at: 2025-04-19T11:12:14Z
updated_at: 2025-05-10T20:45:29Z
url: https://github.com/astral-sh/uv/issues/12981
synced_at: 2026-01-10T01:25:27Z
---

# Is there a `--only-dev` or its just in the docs?

---

_Issue opened by @alexey-pelykh on 2025-04-19 11:12_

https://github.com/astral-sh/uv/blob/e9e4ad4d7d96a80bc20ad2efaa9aaab646414171/docs/concepts/projects/dependencies.md?plain=1#L633

but

```
$ uv add --only-dev isort      
error: unexpected argument '--only-dev' found

  tip: to pass '--only-dev' as a value, use '-- --only-dev'
```

with 

```
$ uv --version
uv 0.6.14 (Homebrew 2025-04-09)
```

---

_Comment by @lemorage on 2025-04-19 14:37_

I think `--only-dev` is used when you do `uv sync`.

https://github.com/astral-sh/uv/blob/e9e4ad4d7d96a80bc20ad2efaa9aaab646414171/docs/concepts/projects/sync.md?plain=1#L119-L137

---

_Comment by @alexey-pelykh on 2025-04-19 15:25_

That makes sense, however the way they were mentioned here:

https://github.com/astral-sh/uv/blob/e9e4ad4d7d96a80bc20ad2efaa9aaab646414171/docs/concepts/projects/dependencies.md?plain=1#L620-L635

what lead me to think those flags are for the `add` command as well.

---

_Comment by @zanieb on 2025-04-19 16:07_

What would `uv add --only-dev package` mean?

---

_Label `question` added by @zanieb on 2025-04-19 16:07_

---

_Comment by @alexey-pelykh on 2025-04-19 16:12_

Nothing meaningful really. Unless to imagine a `dev` and `only-dev` groups but that makes no sense at all. So I guess this issue is all about:

```
The `dev` group is special-cased; there are `--dev`, `--only-dev`, and `--no-dev` flags to toggle
inclusion or exclusion of its dependencies. See `--no-default-groups` to disable all default groups
instead. Additionally, the `dev` group is [synced by default](#default-groups).
```

vs

```
The `dev` group is special-cased; there are `--dev`, `--only-dev`, and `--no-dev` flags for `uv sync` to toggle
inclusion or exclusion of its dependencies. See `--no-default-groups` to disable all default groups
instead. Additionally, the `dev` group is [synced by default](#default-groups).
```

to explicitly mark that those flags are about `uv sync` and not the command most recently mentioned (`uv add`)

---

_Comment by @zanieb on 2025-04-19 16:14_

Those flags are available in any interface that allows selecting groups, e.g., `uv export`, `uv run`, etc. They're just not available in `uv add` because it doesn't make sense there. Maybe there's a way to clarify this, but I don't think that suggestion is quite it.

---

_Closed by @charliermarsh on 2025-05-10 20:45_

---
