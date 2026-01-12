```yaml
number: 7568
title: Allow disabling sub-config required-versions
type: issue
state: open
author: xmo-odoo
labels:
  - configuration
  - needs-decision
assignees: []
created_at: 2023-09-21T11:16:16Z
updated_at: 2023-12-07T12:10:52Z
url: https://github.com/astral-sh/ruff/issues/7568
synced_at: 2026-01-12T15:54:47Z
```

# Allow disabling sub-config required-versions

---

_@xmo-odoo_

From what I understand, ruff currently aborts as soon as it finds a `required-version` mismatch.

This is an issue when e.g. using / submodule-ing multiple repositories with incompatible required-version, as running ruff now requires `--ignore` or `-c`, and the sub-configurations will otherwise be ignored.

Possible improvements (compatible with one another):

- allow disabling required-version on the CLI
- allow a configuration higher up to ignore the sub-configurations' required-version
- only skip (and warn about) directories where an incompatible `required-version` is specified, instead of aborting the run entirely

---

_Comment by @charliermarsh on 2023-09-22 03:53_

I believe your description of current behavior is correct. I'm a little torn on what to change here, if anything -- the current behavior is kind of working as intended, since if you have a submodule with a different required version that the current version, it _should_ error? I'm hesitant to change it to a warning. I'm most open to the idea of allowing a CLI override for `required-version`.

I'm wondering, though -- could you not add your submodules to an `extend-ignore` field in the `pyproject.toml` of the parent project? Wouldn't that be easier than having to pass `--no-required-version` on the CLI every time?

---

_Label `configuration` added by @charliermarsh on 2023-09-22 03:53_

---

_Label `needs-decision` added by @charliermarsh on 2023-09-22 03:53_

---

_Comment by @xmo-odoo on 2023-09-22 05:55_

> I believe your description of current behavior is correct. I'm a little torn on what to change here, if anything -- the current behavior is kind of working as intended, since if you have a submodule with a different required version that the current version, it _should_ error? I'm hesitant to change it to a warning. I'm most open to the idea of allowing a CLI override for `required-version`.

I'm sure it makes sense on CI and thus as default behaviour I guess, but it's an issue when trying to run ruff locally for informational or convenience purpose, especially if the projects being mixed and matched are more or less independent e.g. plugins systems or the like. Modifying the source file can be done if directly vendoring (copying), but it's a risk when using something like submodules and makes updates of the vendored code more complicated (best case scenario the update causes a conflict which has to be resolved, worst the change is lost and has to be re-discovered and reimplemented again).

> I'm wondering, though -- could you not add your submodules to an `extend-ignore` field in the `pyproject.toml` of the parent project? Wouldn't that be easier than having to pass `--no-required-version` on the CLI every time?

I'm not sure I understand the idea, doesn't extend-ignore take rules? And wouldn't the sub-pyproject.toml still be there?

---

_Comment by @ssbarnea on 2023-12-07 12:10_

In fact I would like to propose allowing `required-version` to work like in other tools where is working more like `min-version` required. At least it would fail only if it finds that the version used is older.

Current feasure is incompatible with pre-commit hook of ruff, as when doing auto-update of ruff, the value from the config will get in conflict.

---
