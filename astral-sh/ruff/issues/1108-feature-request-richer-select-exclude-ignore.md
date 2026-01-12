```yaml
number: 1108
title: "[feature-request] Richer `select`/`exclude`/`ignore` specification"
type: issue
state: closed
author: smackesey
labels:
  - configuration
assignees: []
created_at: 2022-12-06T13:16:04Z
updated_at: 2022-12-16T05:25:08Z
url: https://github.com/astral-sh/ruff/issues/1108
synced_at: 2026-01-12T15:54:41Z
```

# [feature-request] Richer `select`/`exclude`/`ignore` specification

---

_@smackesey_

`ruff` inherited flake8's approach to  existing Python linter's approach to building lists from multiple configuration sources. There are default options for `select`/`exclude`/`ignore`, and users can set `extend-{select,exclude,ignore}` to append to this list without overwriting it. This system is not great for certain workflows.

For example, I may wish to have all the sensible default excluded files (from default setting of `exclude`), add to it in my project config (setting `--extend-exclude`), but then during local dev further narrow my target on the command line. This isn't possible to do easily with the current setup, since all I can set on the command line are `--exclude` and `--extend-exclude`, both of which will overwrite my existing config and probably cause a lot of noise to be dumped to the terminal.

Of course one can just specify `--exclude` instead of `--extend-exclude` in the project config file, but then project maintainers need to reiterate all of the exclude defaults in the config file, which is undesirable.

Haven't fully thought through the problem (and linters in other languages probably have good solutions for this), but IMO it would be better if `--extend-*` settings functioned like options that can be passed multiple times on the command line, appending to the target rather than overwriting it:

```
### pyproject.toml

[tool.ruff]
extend-exclude = ["foo"]

### shell

# makes `extend-exclude` include both `foo` and `bar`
$ ruff --extend-exclude=bar  ...
```

---

_Comment by @charliermarsh on 2022-12-06 14:40_

Yeah, I've also considered removing `extend-exclude` from `pyproject.toml` and instead making it a command-line-only option with different semantics (like you've described above). If we support overrides, though, it will also be useful in the `pyproject.toml`, so maybe it just needs the above semantics and that's sufficient.

(See: https://github.com/charliermarsh/ruff/issues/552.)

---

_Label `enhancement` added by @charliermarsh on 2022-12-09 04:50_

---

_Label `configuration` added by @charliermarsh on 2022-12-09 04:50_

---

_Comment by @charliermarsh on 2022-12-16 05:25_

The behavior described above has been implemented in #1245. So those `extend-*` options are treated as "always extending", such that if a `pyproject.toml` has an `extend-ignore`, and you provide `--extend-ignore` on the command-line, that `--extend-ignore` will be layered on top rather than replace the existing `extend-ignore`.

---

_Closed by @charliermarsh on 2022-12-16 05:25_

---
