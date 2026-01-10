```yaml
number: 11484
title: "Short name for \"--group\" option"
type: pull_request
state: open
author: hongquan
labels:
  - cli
assignees: []
base: main
head: feature/short-group
created_at: 2025-02-13T16:38:36Z
updated_at: 2025-04-14T08:00:31Z
url: https://github.com/astral-sh/uv/pull/11484
synced_at: 2026-01-10T11:10:38Z
```

# Short name for "--group" option

---

_Pull request opened by @hongquan on 2025-02-13 16:38_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Add short name `-G` for `--group` option in `uv add` command.

The `uv add --group` command is used quite often. Having short alternative form `uv add -G` will let user type less.
The `-G` short name also exists in Poetry and PDM.

## Test Plan

```console
$ uv help add

...

Options:
  -G, --group <GROUP>
          Add the requirements to the specified dependency group.
```

---

_Label `cli` added by @zanieb on 2025-02-13 16:54_

---

_Assigned to @zanieb by @zanieb on 2025-02-13 16:54_

---

_@zanieb reviewed on 2025-02-13 17:43_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:2712 on 2025-02-13 17:43_

This looks duplicated
```suggestion
    #[arg(long, short = 'G', conflicts_with_all = ["only_group", "only_dev"])]
```

---

_Comment by @zanieb on 2025-02-13 17:43_

I'm a little hesitant to use an uppercase short flag, I'm not sure we're really doing that elsewhere. Why do they use `-G` instead of `-g`? What's `pip` going to do for the short flag?

---

_@hongquan reviewed on 2025-02-13 17:45_

---

_Review comment by @hongquan on `crates/uv-cli/src/lib.rs`:2712 on 2025-02-13 17:45_

Thank you. Fixing!

---

_Comment by @hongquan on 2025-02-13 17:47_

PDM uses `-G` instead of `-g` because it already has `-g / --global`.

---

_Comment by @hongquan on 2025-02-23 09:57_

@zanieb should I change it to lowercase?

---

_Comment by @zanieb on 2025-02-24 15:45_

I'm not sure. I'm hesitant to add a short flag at all since we don't know what we might want to use it for. It might make sense to wait and see if there's more demand for it.

---
