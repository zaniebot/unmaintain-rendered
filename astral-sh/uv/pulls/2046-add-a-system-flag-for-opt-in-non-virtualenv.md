```yaml
number: 2046
title: "Add a `--system` flag for opt-in non-virtualenv installs"
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
  - cli
assignees: []
merged: true
base: main
head: charlie/system
created_at: 2024-02-28T16:11:19Z
updated_at: 2024-02-28T19:49:47Z
url: https://github.com/astral-sh/uv/pull/2046
synced_at: 2026-01-12T16:04:51Z
```

# Add a `--system` flag for opt-in non-virtualenv installs

---

_@charliermarsh_

## Summary

This is essentially a wrapper around something like `--python $(which python3)`, but gives users a portable and streamlined way to solve the common pain point of using `uv` in GitHub Actions or a Docker container.

See: https://github.com/astral-sh/uv/issues/1526.


---

_Label `enhancement` added by @charliermarsh on 2024-02-28 16:11_

---

_Label `cli` added by @charliermarsh on 2024-02-28 16:11_

---

_Review comment by @AlexWaygood on `crates/uv/src/main.rs`:464 on 2024-02-28 17:17_

Possibly over the top, but you could do something like this:

```suggestion
    /// WARNING: `--system` is intended for use in continuous integration (CI) environments.
    /// It should be used with caution, as it can modify the system Python installation.
```

---

_@zanieb approved on 2024-02-28 17:21_

The testing looks like a good start to me!

---

_@AlexWaygood reviewed on 2024-02-28 17:22_

---

_Review comment by @zanieb on `.github/workflows/system-install.yml`:31 on 2024-02-28 17:22_

This update looks like it has some weird conflicts — I think we also don't do `update` elsewhere and just do `rustup show`?

---

_@zanieb reviewed on 2024-02-28 17:22_

---

_Comment by @charliermarsh on 2024-02-28 17:26_

@zanieb - Do you want me to check-in the testing (perhaps making it `workflow_dispatch` for now)? I was considering removing it.

---

_Review comment by @charliermarsh on `.github/workflows/system-install.yml`:31 on 2024-02-28 17:30_

Oops, thanks.

---

_@charliermarsh reviewed on 2024-02-28 17:30_

---

_Comment by @zanieb on 2024-02-28 17:45_

@charliermarsh I can take ownership of productionizing them from wherever you want to leave it — workflow dispatch seems fine

---

_@AlexWaygood reviewed on 2024-02-28 18:24_

---

_Review comment by @AlexWaygood on `.github/workflows/system-install.yml`:82 on 2024-02-28 18:24_

I don't think Windows has an `echo` command or a `which` command

---

_@AlexWaygood reviewed on 2024-02-28 18:24_

---

_Review comment by @AlexWaygood on `.github/workflows/system-install.yml`:82 on 2024-02-28 18:24_

It has a `where` command

---

_@charliermarsh reviewed on 2024-02-28 18:29_

---

_Review comment by @charliermarsh on `.github/workflows/system-install.yml`:82 on 2024-02-28 18:29_

It turns out that it doesn't matter on GHA:

![Screenshot 2024-02-28 at 1 29 42 PM](https://github.com/astral-sh/uv/assets/1309177/12b76e62-2af4-463c-8891-c20bbb2988cb)


---

_@zanieb reviewed on 2024-02-28 18:30_

---

_Review comment by @zanieb on `.github/workflows/system-install.yml`:82 on 2024-02-28 18:30_

🤯 

---

_Merged by @charliermarsh on 2024-02-28 19:48_

---

_Closed by @charliermarsh on 2024-02-28 19:48_

---

_Branch deleted on 2024-02-28 19:48_

---

_Comment by @tylerlaprade on 2024-02-28 19:49_

🙏 

---
