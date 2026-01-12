```yaml
number: 1889
title: "Add support for pip-compile's `--unsafe-package` flag"
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
  - compatibility
assignees: []
merged: true
base: main
head: charlie/unsafe-package
created_at: 2024-02-23T00:13:36Z
updated_at: 2024-02-23T19:27:03Z
url: https://github.com/astral-sh/uv/pull/1889
synced_at: 2026-01-12T16:04:46Z
```

# Add support for pip-compile's `--unsafe-package` flag

---

_@charliermarsh_

## Summary

In uv, we're going to use `--no-emit-package` for this, to convey that the package will be included in the resolution but not in the output file. It also mirrors flags like `--emit-index-url`.

We're also including an `--unsafe-package` alias.

Closes https://github.com/astral-sh/uv/issues/1415.


---

_Label `enhancement` added by @charliermarsh on 2024-02-23 00:13_

---

_Label `compatibility` added by @charliermarsh on 2024-02-23 00:13_

---

_Comment by @charliermarsh on 2024-02-23 00:14_

I'm torn on the name `--unsafe-package`, and the use of "unsafe" in the footer comment (which matches pip-compile), like:

```
# The following packages are considered to be unsafe in a requirements file:
# jinja2
```

---

_Review requested from @zanieb by @charliermarsh on 2024-02-23 00:14_

---

_Comment by @hauntsaninja on 2024-02-23 08:18_

Yeah :-/ It's unfortunate that pip-compile puts those words in the file. I do think it'd be nice to have an `--exclude-package` alias.

---

_Merged by @charliermarsh on 2024-02-23 18:47_

---

_Closed by @charliermarsh on 2024-02-23 18:47_

---

_Branch deleted on 2024-02-23 18:47_

---

_Review comment by @hauntsaninja on `crates/uv/src/commands/pip_compile.rs`:404 on 2024-02-23 19:07_

Thank you for implementing the feature!

nit: I was confused by "include" here, maybe `The following packages were omitted from the result:` (although maybe omit vs no-emit confuses folks too, so maybe just "excluded")

---

_@hauntsaninja reviewed on 2024-02-23 19:07_

---

_@charliermarsh reviewed on 2024-02-23 19:27_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/pip_compile.rs`:404 on 2024-02-23 19:27_

Good idea -- changed in https://github.com/astral-sh/uv/pull/1935.

---
