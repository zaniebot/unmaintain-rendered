```yaml
number: 3463
title: "Disallow `pyproject.toml` files for non-project configuration"
type: pull_request
state: merged
author: charliermarsh
labels:
  - configuration
assignees: []
merged: true
base: main
head: charlie/pyproj
created_at: 2024-05-08T17:11:31Z
updated_at: 2024-05-08T18:49:53Z
url: https://github.com/astral-sh/uv/pull/3463
synced_at: 2026-01-10T14:37:54Z
```

# Disallow `pyproject.toml` files for non-project configuration

---

_Pull request opened by @charliermarsh on 2024-05-08 17:11_

## Summary

We already _don't_ discover a `pyproject.toml` in `~/.config/uv` -- it must be `uv.toml`. This PR makes the same change for `--config-file` -- it _has_ to be a `uv.toml`.

I think this is reasonable and more consistent, though I'm not sure. A `pyproject.toml` "means" something -- it defines a project itself, in which case we should be using project configuration. But creating a `pyproject.toml` outside the project and passing it via `--config-file` seems like an anti-pattern.



---

_Review requested from @konstin by @charliermarsh on 2024-05-08 17:11_

---

_Review requested from @zanieb by @charliermarsh on 2024-05-08 17:11_

---

_Label `configuration` added by @charliermarsh on 2024-05-08 17:13_

---

_Comment by @zanieb on 2024-05-08 18:12_

Is there another way to change the "root" of the invocation to a different `pyproject.toml` than the working directory? That might address this use-case. I see `--config-file foo/pyproject.toml` as a desire to change the target entirely rather than just change the config. What does Cargo do here? It allows `--config foo/Cargo.toml` afaik.

---

_Comment by @charliermarsh on 2024-05-08 18:13_

I think Cargo allows `--config foo/config.toml`, which isn't the same as a `Cargo.toml`, unless I'm mistaken?

---

_Comment by @zanieb on 2024-05-08 18:14_

Oh okay nevermind regarding Cargo you're totally right.

---

_Comment by @charliermarsh on 2024-05-08 18:14_

I think... I've never had a need to really internalize the difference.

---

_Comment by @zanieb on 2024-05-08 18:15_

Yeah I checked the docs https://doc.rust-lang.org/cargo/reference/config.html

---

_@zanieb approved on 2024-05-08 18:16_

---

_Merged by @charliermarsh on 2024-05-08 18:49_

---

_Closed by @charliermarsh on 2024-05-08 18:49_

---

_Branch deleted on 2024-05-08 18:49_

---
