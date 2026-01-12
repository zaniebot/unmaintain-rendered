```yaml
number: 11514
title: Eagerly reject unsupported Git schemes
type: pull_request
state: merged
author: konstin
labels:
  - error messages
  - no-build
assignees: []
merged: true
base: main
head: konsti/invalid-git-urls
created_at: 2025-02-14T16:40:03Z
updated_at: 2025-02-18T02:14:07Z
url: https://github.com/astral-sh/uv/pull/11514
synced_at: 2026-01-12T16:09:52Z
```

# Eagerly reject unsupported Git schemes

---

_@konstin_


Initially, we were limiting Git schemes to HTTPS and SSH as only supported schemes. We lost this validation in #3429. This incidentally allowed file schemes, which apparently work with Git out of the box.

A caveat for this is that in tool.uv.sources, we parse the git field always as URL. This caused a problem with #11425: repo = { git = 'c:\path\to\repo', rev = "xxxxx" } was parsed as a URL where c: is the scheme, causing a bad error message down the line.

This PR:

* Puts Git URL validation back in place. It bans everything but HTTPS, SSH, and file URLs. This could be a breaking change, if users were using a git transport protocol were not aware of, even though never intentionally supported.
* Allows file: URL in Git: This seems to be supported by Git and we were supporting it albeit unintentionally, so it's reasonable to continue to support it.
* It does not allow relative paths in the git field in tool.uv.sources. Absolute file URLs are supported, whether we want relative file URLs for Git too should be discussed separately.

Closes #3429: We reject the input with a proper error message, while hinting the user towards file:. If there's still desire for relative path support, we can keep it open.


---

_Label `error messages` added by @konstin on 2025-02-14 16:40_

---

_Comment by @charliermarsh on 2025-02-14 16:47_

@konstin -- I'm wondering if we should instead put this in the `GitUrl` constructors?

---

_Comment by @konstin on 2025-02-17 16:49_

Refactored `RequirementSource::Git` for catching this directly in `GitUrl` without adding more error paths, same errors being caught still.

---

_@charliermarsh reviewed on 2025-02-18 01:59_

---

_Review comment by @charliermarsh on `crates/uv/tests/it/pip_install.rs`:8783 on 2025-02-18 01:59_

Stray comment, copy-pasted from above.

---

_@charliermarsh reviewed on 2025-02-18 02:01_

---

_Review comment by @charliermarsh on `crates/uv/tests/it/pip_install.rs`:8781 on 2025-02-18 02:01_

I think this should be `scheme` like the others.

---

_@charliermarsh approved on 2025-02-18 02:02_

---

_Label `no-build` added by @charliermarsh on 2025-02-18 02:05_

---

_Merged by @charliermarsh on 2025-02-18 02:14_

---

_Closed by @charliermarsh on 2025-02-18 02:14_

---

_Branch deleted on 2025-02-18 02:14_

---
