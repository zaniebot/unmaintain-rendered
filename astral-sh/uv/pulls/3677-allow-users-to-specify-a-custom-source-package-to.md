```yaml
number: 3677
title: "Allow users to specify a custom source package to `uv tool run`"
type: pull_request
state: merged
author: zanieb
labels:
  - preview
assignees: []
merged: true
base: main
head: zb/tool-run-spec
created_at: 2024-05-20T23:04:17Z
updated_at: 2024-05-22T19:48:55Z
url: https://github.com/astral-sh/uv/pull/3677
synced_at: 2026-01-12T16:05:47Z
```

# Allow users to specify a custom source package to `uv tool run`

---

_@zanieb_

We usually infer the package the tool is pulled from to be the same name as the tool itself, but that's not always the case. This allows users to provide a custom package.

---

_Label `preview` added by @zanieb on 2024-05-20 23:04_

---

_@zanieb reviewed on 2024-05-20 23:08_

---

_Review comment by @zanieb on `crates/uv/src/cli.rs`:1951 on 2024-05-20 23:08_

Open to some bike-shedding here.

pipx says

> --spec SPEC           
>
> The package name or specific installation source passed to pip. Runs `pip install -U SPEC`. For
> example `--spec mypackage==2.0.0` or `--spec git+https://github.com/user/repo.git@branch`

which I find very unintuitive
                        

---

_Marked ready for review by @zanieb on 2024-05-21 15:24_

---

_@charliermarsh reviewed on 2024-05-21 20:30_

---

_Review comment by @charliermarsh on `crates/uv/src/cli.rs`:1951 on 2024-05-21 20:30_

Hmm. `package` isn't bad but it might be confusing with `-p` in the workspace API. I find `spec` okay but don't feel strongly.

---

_@zanieb reviewed on 2024-05-21 20:49_

---

_Review comment by @zanieb on `crates/uv/src/cli.rs`:1951 on 2024-05-21 20:49_

Like `-p` being short for `--python`?

---

_@charliermarsh reviewed on 2024-05-21 20:57_

---

_Review comment by @charliermarsh on `crates/uv/src/cli.rs`:1951 on 2024-05-21 20:57_

Oh, I was assuming `-p` would be short for package in the workspace API, as it is in cargo (`cargo test -p uv`, etc.).

---

_@zanieb reviewed on 2024-05-21 20:59_

---

_Review comment by @zanieb on `crates/uv/src/cli.rs`:1951 on 2024-05-21 20:59_

Uh oh haha it's short for `--python` right now. We might need to reconsider that?

I'll think about this some more.

---

_@zanieb reviewed on 2024-05-21 21:03_

---

_Review comment by @zanieb on `crates/uv/src/cli.rs`:1951 on 2024-05-21 21:03_

`--from`? e.g. `uv tool run --from foo -- bar` or `uv tool run --from git+https://github.com/user/repo.git@branch -- bar`

---

_Review comment by @charliermarsh on `crates/uv/src/cli.rs`:1951 on 2024-05-21 21:04_

Dumb question, but how does it differ from `--with`?

---

_@charliermarsh reviewed on 2024-05-21 21:04_

---

_@zanieb reviewed on 2024-05-21 21:10_

---

_Review comment by @zanieb on `crates/uv/src/cli.rs`:1951 on 2024-05-21 21:10_

If you use `--with` (as in #3678), we will still infer the package that provides the tool from the command name so `--with` is additive while this flag _replaces_ the inferred requirement.

I considered having the first `--with` package replace the inferred requirement but that seems confusing.


---

_@zanieb reviewed on 2024-05-21 21:11_

---

_Review comment by @zanieb on `crates/uv/src/cli.rs`:1951 on 2024-05-21 21:11_

This is especially important for installation where we won't install entry points from additional dependencies, just the tool package (and I'd expect the `uv tool run` semantics to match)

---

_Renamed from "Allow users to specify a custom package spec to `uv tool run`" to "Allow users to specify a custom source package to `uv tool run`" by @zanieb on 2024-05-21 21:55_

---

_Merged by @zanieb on 2024-05-22 19:48_

---

_Closed by @zanieb on 2024-05-22 19:48_

---

_Branch deleted on 2024-05-22 19:48_

---
