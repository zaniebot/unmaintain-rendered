```yaml
number: 6335
title: Preserve Git username for SSH dependencies
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/git-at
created_at: 2024-08-21T15:01:04Z
updated_at: 2024-08-21T15:22:48Z
url: https://github.com/astral-sh/uv/pull/6335
synced_at: 2026-01-10T13:09:51Z
```

# Preserve Git username for SSH dependencies

---

_Pull request opened by @charliermarsh on 2024-08-21 15:01_

## Summary

We're gonna work on a more comprehensive review of whether we should preserve the username here, but for now, `git@` is effectively a convention for GitHub and GitLab etc.

Closes https://github.com/astral-sh/uv/issues/6305.

## Test Plan

I guess we don't have infrastructure for testing SSH private keys right now, but...

```
❯ cargo run init foo
❯ cd foo
❯ cargo run add git+ssh://git@github.com/astral-sh/mkdocs-material-insiders.git
```


---

_Review requested from @zanieb by @charliermarsh on 2024-08-21 15:01_

---

_Comment by @charliermarsh on 2024-08-21 15:01_

Adding tests...

---

_Review comment by @zanieb on `crates/pypi-types/src/requirement.rs`:773 on 2024-08-21 15:03_

This seems weird, shouldn't we always drop the password and conditionally drop the username? Or is this just for SSH credentials? A comment would be helpful if so.

---

_@zanieb reviewed on 2024-08-21 15:03_

---

_@charliermarsh reviewed on 2024-08-21 15:04_

---

_Review comment by @charliermarsh on `crates/pypi-types/src/requirement.rs`:773 on 2024-08-21 15:04_

The password is required to be `None` in the return case though.

---

_Review requested from @zanieb by @charliermarsh on 2024-08-21 15:05_

---

_Label `bug` added by @charliermarsh on 2024-08-21 15:05_

---

_@zanieb reviewed on 2024-08-21 15:06_

---

_Review comment by @zanieb on `crates/pypi-types/src/requirement.rs`:773 on 2024-08-21 15:06_

Yes.. I expect:

```
let _ = url.set_password(None);
if url.username() != "git" {
    let _ = url.set_username("");
}
```

or for the documentation to properly declare that we only allow `git` if it doesn't have an associated password. Otherwise.. `git:foo@github.com` would redact both `git` and `foo`.

---

_@charliermarsh reviewed on 2024-08-21 15:06_

---

_Review comment by @charliermarsh on `crates/pypi-types/src/requirement.rs`:773 on 2024-08-21 15:06_

Added a comment + `ssh` guard.

---

_@zanieb reviewed on 2024-08-21 15:08_

---

_Review comment by @zanieb on `crates/pypi-types/src/requirement.rs`:764 on 2024-08-21 15:08_

Can you mention SSH here too? Sorry.

---

_@charliermarsh reviewed on 2024-08-21 15:09_

---

_Review comment by @charliermarsh on `crates/pypi-types/src/requirement.rs`:764 on 2024-08-21 15:09_

Ofc

---

_@zanieb approved on 2024-08-21 15:18_

---

_Merged by @charliermarsh on 2024-08-21 15:22_

---

_Closed by @charliermarsh on 2024-08-21 15:22_

---

_Branch deleted on 2024-08-21 15:22_

---
