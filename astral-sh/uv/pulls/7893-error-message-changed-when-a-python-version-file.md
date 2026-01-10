```yaml
number: 7893
title: Error message changed when a .python-version file requirement is incompatible with the project.
type: pull_request
state: closed
author: mstepan
labels: []
assignees: []
base: main
head: dot-python-version-incompatible-with-project-version-better-error
created_at: 2024-10-03T12:38:50Z
updated_at: 2024-12-02T08:45:27Z
url: https://github.com/astral-sh/uv/pull/7893
synced_at: 2026-01-10T11:59:59Z
```

# Error message changed when a .python-version file requirement is incompatible with the project.

---

_Pull request opened by @mstepan on 2024-10-03 12:38_

Fix issue described in https://github.com/astral-sh/uv/issues/7737

<!--
- Does this pull request include a summary of the change? Yes.
- Does this pull request include a descriptive title? Yes.
- Does this pull request include references to any relevant issues? Yes.
-->

## Summary

1. Change error message when python version inside `.python-version` is incompatible with `pyproject.yaml` as proposed in https://github.com/astral-sh/uv/issues/7737
2. IntelliJ RustRover generated files added to `.gitignore`

## Test Plan

<!-- How was it tested? -->
```
cargo nextest run
```


---

_Review requested from @zanieb by @charliermarsh on 2024-10-03 13:08_

---

_Assigned to @zanieb by @charliermarsh on 2024-10-03 13:08_

---

_@zanieb reviewed on 2024-10-03 14:23_

---

_Review comment by @zanieb on `.gitignore`:40 on 2024-10-03 14:23_

Generally these things should go in your user `.gitignore` file instead, since they're not specific to the project https://stackoverflow.com/questions/5724455/can-i-make-a-user-specific-gitignore-file

---

_@zanieb reviewed on 2024-10-03 14:24_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/mod.rs`:77 on 2024-10-03 14:24_

I don't think we want to change the existing message, we should add a line _after_ it suggesting use of `uv python pin`.

---

_Comment by @zanieb on 2024-10-03 14:25_

Welcome to the project! Thanks for contributing :)

---

_@mstepan reviewed on 2024-10-03 15:02_

---

_Review comment by @mstepan on `.gitignore`:40 on 2024-10-03 15:02_

Yes, I'm aware of user-specific `.gitignore`. 
I just thought if `.gitignore` contains some mac specific files it's also valid to add `IntelliJ` files too:
https://github.com/astral-sh/uv/blob/main/.gitignore#L37
B/c mac os files are also environment specific and not related to a project. )))

---

_@mstepan reviewed on 2024-10-03 15:04_

---

_Review comment by @mstepan on `crates/uv/src/commands/project/mod.rs`:77 on 2024-10-03 15:04_

ACK.

---

_@zanieb reviewed on 2024-10-03 15:25_

---

_Review comment by @zanieb on `.gitignore`:40 on 2024-10-03 15:25_

Yes, arguably it shouldn't contain those either, but I am not the only one managing the contents of the file :D

---

_Comment by @mstepan on 2024-10-03 15:36_

Comments addressed. Error message changed to:
```
error: The Python request from `.python-version` resolved to Python 3.9.6, which is incompatible with the project's Python requirement: `>=3.12`. Use `uv python pin`
```

---

_Review requested from @zanieb by @mstepan on 2024-10-04 11:08_

---

_Comment by @mstepan on 2024-10-04 11:09_

#7737 

---

_@zanieb approved on 2024-11-12 13:58_

Thanks! And sorry for the delay, it's been busy :)

I expanded the message a bit.

---

_Closed by @mstepan on 2024-12-02 08:45_

---
