```yaml
number: 7176
title: "Add `--upgrade-strategy`, i.e., support only upgrading transitive dependencies"
type: issue
state: open
author: zanieb
labels:
  - enhancement
  - configuration
assignees: []
created_at: 2024-09-07T18:41:52Z
updated_at: 2024-10-17T07:58:57Z
url: https://github.com/astral-sh/uv/issues/7176
synced_at: 2026-01-12T15:59:11Z
```

# Add `--upgrade-strategy`, i.e., support only upgrading transitive dependencies

---

_@zanieb_

Originally mentioned at https://github.com/astral-sh/uv/issues/6781#issuecomment-2318265988

e.g. `--upgrade-strategy <transitive | direct>`

---

_Label `enhancement` added by @zanieb on 2024-09-07 18:42_

---

_Label `configuration` added by @zanieb on 2024-09-07 18:42_

---

_Comment by @callegar on 2024-10-17 07:58_

Don't know if this is the correct place to provide this comment, if not, please let me know.

- To many python users, accustomed to `pip`, the option `--upgrade-strategy` is strongly associated to the `only-if-needed` and `eager` options. Having an `--upgrade-strategy` option with a different semantic or different option values may end up confusing.
- As `uv tool` is meant to substitute for `pipx`, it may be valuable to provide options to get behaviors that don't surprise users coming from `pipx`. Specifically, `pipx upgrade` when updating a tool does not update its dependencies unless there is a strict need to do so (under the hood, it works like `pip`, where the update strategy defaults to `only-if-needed`). Apparently, the experience of `pip` and `pipx` developers was that blindly updating dependencies could sometimes lead to breakage that would be avoided with a more conservative approach. I am getting the feeling (please correct me if I am wrong) that `uv tool upgrade` does the opposite (namely works like `pip install -U` with the `--update-strategy` set to `eager`). Having an easy way to get the `pipx` behavior would be great. Apparently `--strategy` and `UV_STRATEGY` don't let you do it. Is there anything I am missing?

---
