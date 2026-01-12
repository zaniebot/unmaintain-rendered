```yaml
number: 1266
title: Validate instead of discovering python patch version
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: main
head: konsti/patch-versions-validate-only
created_at: 2024-02-08T20:25:35Z
updated_at: 2024-02-08T21:38:01Z
url: https://github.com/astral-sh/uv/pull/1266
synced_at: 2026-01-12T16:04:32Z
```

# Validate instead of discovering python patch version

---

_@konstin_

Contrary to our prior assumption, we can't reliably select a specific patch version. With the deadsnakes PPA for example, `python3.12` is installed into `PATH`, but `python3.12.1` isn't. Based on the assumption (or rather, observation) that users have a single python patch version per python minor version installed, generally the latest, we only check if the installed patch version matches the selected patch version, if any, instead of search for one.

In the process, i deduplicated the python discovery logic.

---

_Review requested from @zanieb by @konstin on 2024-02-08 20:25_

---

_@zanieb reviewed on 2024-02-08 20:41_

---

_Review comment by @zanieb on `crates/puffin-interpreter/src/interpreter.rs`:182 on 2024-02-08 20:41_

What's an explicit interpreter here?

---

_@zanieb reviewed on 2024-02-08 20:41_

---

_Review comment by @zanieb on `crates/puffin-interpreter/src/interpreter.rs`:171 on 2024-02-08 20:41_

What does the "platform specific search" mean here?

---

_@zanieb reviewed on 2024-02-08 20:42_

---

_Review comment by @zanieb on `crates/puffin-interpreter/src/lib.rs`:67 on 2024-02-08 20:42_

Perhaps `PatchVersionMismatch` or `PatchVersionNotFound`? The message should probably say "does not have the requested patch version" instead of "has the wrong patch version".

---

_@zanieb reviewed on 2024-02-08 20:45_

---

_Review comment by @zanieb on `crates/puffin-interpreter/src/python_query.rs`:34 on 2024-02-08 20:45_

Is it _often_ not on the path or is it just not on the path in some of the cases you tried? Is there actually a significant cost to _checking_ for the full version in the path? I have a strong preference for seeing if it's available instead of just assuming it can never exist. I would be confused if a tool skipped the more exact executable and then complained that the more general one (i.e. minor version) has an incompatible patch version.

---

_@zanieb reviewed on 2024-02-08 20:46_

---

_Review comment by @zanieb on `crates/puffin-interpreter/src/lib.rs`:64 on 2024-02-08 20:46_

We should update this to match the `NoSuchPython` error message.

---

_@konstin reviewed on 2024-02-08 20:59_

---

_Review comment by @konstin on `crates/puffin-interpreter/src/python_query.rs`:34 on 2024-02-08 20:59_

My thinking was to choose simplicity and consistency over better discovery; Independent of platform and of the way you installed python, we always only check the minor version. For the rare cases where users who actually want exact control over which python interpreter gets picked, they can pass an absolute path. I don't feel it's worth building and maintaining that level of complexity when users shouldn't need to specify patch versions anyway; I'd be in favor of removing explicit patch version support in general if it wasn't for the pip compile overwrites.

---

_@zanieb reviewed on 2024-02-08 21:02_

---

_Review comment by @zanieb on `crates/puffin-interpreter/src/python_query.rs`:34 on 2024-02-08 21:02_

It feels _more_ complex to me to skip that executable file if it's in your path but 🤷‍♀️ we can always revisit this if it's problematic

---

_@zanieb approved on 2024-02-08 21:05_

---

_Merged by @konstin on 2024-02-08 21:38_

---

_Closed by @konstin on 2024-02-08 21:38_

---

_Branch deleted on 2024-02-08 21:38_

---
