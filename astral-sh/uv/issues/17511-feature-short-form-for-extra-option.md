```yaml
number: 17511
title: "Feature: short form for --extra option"
type: issue
state: open
author: osma
labels:
  - enhancement
  - compatibility
assignees: []
created_at: 2026-01-16T08:56:26Z
updated_at: 2026-01-16T13:04:06Z
url: https://github.com/astral-sh/uv/issues/17511
synced_at: 2026-01-16T13:57:15Z
```

# Feature: short form for --extra option

---

_@osma_

### Summary

Our project ([Annif](https://github.com/NatLibFi/Annif)) has just [switched](https://github.com/NatLibFi/Annif/pull/923) from Poetry to uv. It has quite a few extras (optional dependencies) so the `--extra` option is needed a lot. We also had to write [a little for loop in our Dockerfile](https://github.com/NatLibFi/Annif/blob/27e4ac72f220e7b88fc7011046a965882de07dc4/Dockerfile#L24-L27) just to construct the `--extra` options for `uv sync`, where previously (with Poetry) a simple variable substitution did the trick.

Unlike Poetry, uv currently only supports a long `--extra` option that may be repeated. To improve the developer experience, I would like to propose shorter forms of that option, matching what Poetry supports.

In Poetry, I can run `poetry install -E foo,bar` to install the package with the extras `foo` and `bar`.

`uv sync` (current version 0.9.26 on Linux) doesn't support the short form `-E`:

```
$ uv sync -E foo
error: unexpected argument '-E' found

Usage: uv sync [OPTIONS]

For more information, try '--help'.
```

`uv sync --extra` also doesn't support a comma-separated list of extras:

```
$ uv sync --extra foo,bar
error: invalid value 'foo,bar' for '--extra <EXTRA>': Extra names must start and end with a letter or digit and may only contain -, _, ., and alphanumeric characters

For more information, try '--help'.
```

Since both of these currently trigger errors, I think it should be unproblematic to add support for the shorter forms, i.e.

1. A short option `-E` that works the same way as `--extra`
2. Support for a comma-separated value for `--extra` / `-E` as an alternative for repeating the option

PS. I tried looking for existing issues with similar proposals but couldn't find any. I hope I didn't miss something.

### Example

I would like this command to work (when extras `foo` and `bar` are defined):

    uv sync -E foo,bar


---

_Label `enhancement` added by @osma on 2026-01-16 08:56_

---

_Label `compatibility` added by @EliteTK on 2026-01-16 13:04_

---
