```yaml
number: 8594
title: "Add `--all-group` option to `uv sync`"
type: issue
state: closed
author: shoucandanghehe
labels:
  - help wanted
  - cli
assignees: []
created_at: 2024-10-26T14:22:01Z
updated_at: 2024-11-20T16:07:37Z
url: https://github.com/astral-sh/uv/issues/8594
synced_at: 2026-01-10T04:36:20Z
```

# Add `--all-group` option to `uv sync`

---

_Issue opened by @shoucandanghehe on 2024-10-26 14:22_

When installing multiple dependency groups you need to specify `--group` multiple times, which can be annoying if you want to install many dependency groups at the same time

Also `--all-group` should be able to be used in conjunction with `--no-group` in case you don't want to actually install all the dependencies.


---

_Comment by @zanieb on 2024-10-26 14:50_

Sure, we could add `--all-groups` throughout to match `--all-extras`

---

_Label `help wanted` added by @zanieb on 2024-10-26 14:50_

---

_Label `cli` added by @zanieb on 2024-10-26 14:50_

---

_Comment by @frfahim on 2024-10-26 18:07_

The `--all-group` command would be very helpful to install all groups with a single command.
Also it would be great to support multiple groups together within one `--group` argument like this `uv sync --group "lint,docs"` instead of `uv sync --group lint --group docs`

---

_Comment by @zanieb on 2024-10-27 00:42_

We probably won't support multiple groups in a single option, that doesn't match the design of the rest of our CLI.

---

_Comment by @drmikehenry on 2024-10-27 12:15_

Though it's not completely portable, you can use brace expansion in many Unix shells (Bash, Zsh, Ksh, Fish, etc.) to get the desired behavior because the `--group groupname` syntax may use an `=` as in `--group=groupname`.  To show this using the `echo` command:

```
echo uv sync --group={lint,docs}
```

with output:

```
uv sync --group=lint --group=docs
```


---

_Comment by @bluss on 2024-10-27 14:03_

Groups can also include each other, so you can add a group that includes all other groups, if you want to.

---

_Comment by @bluss on 2024-10-27 18:47_

The PEP has the syntax for group inclusion here https://peps.python.org/pep-0735/#dependency-group-include

---

_Comment by @amoralesc on 2024-10-29 00:20_

Just referencing here that I temporally solved this issue by defining [default groups](https://docs.astral.sh/uv/concepts/dependencies/#default-groups):

```toml
# pyproject.toml
[tool.uv]
default-groups = ["lint", "docs"]
```

This makes the `uv sync` command sync all groups contained there. This obviously has the disadvantage that now I need to specify `--no-group` or `--only-group` when I don't want any of those groups.

---

_Comment by @amoralesc on 2024-10-29 00:34_

@zanieb In line with the [above comment](https://github.com/astral-sh/uv/issues/8594#issuecomment-2442915709), I also can't find a way to **only** install the project-level dependencies (those under `[project].dependencies`), regardless of what the default groups property defines.

For example, Poetry achieves this behavior by using the `--only main` flag (although badly documented). This seems equivalent to the `--only-group` flag, with the caveat that the project-level dependencies don't seem to belong to a group.

My workaround for achieving both behaviors (sync with default groups and sync only main) is creating a `main` group.

**Edit:** This seems loosely related to #8582

---

_Comment by @zanieb on 2024-10-29 01:47_

@amoralesc it seems like we might want a `--no-default-groups` flag, which I was sort of hoping we could avoid (just because we have so many flags)

---

_Closed by @charliermarsh on 2024-11-20 16:07_

---
