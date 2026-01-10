```yaml
number: 4322
title: "Add `--no-build`, `--no-build-package`, and binary variants"
type: pull_request
state: merged
author: charliermarsh
labels:
  - configuration
  - preview
assignees: []
merged: true
base: main
head: charlie/build
created_at: 2024-06-14T01:07:36Z
updated_at: 2024-06-14T04:05:01Z
url: https://github.com/astral-sh/uv/pull/4322
synced_at: 2026-01-10T13:54:02Z
```

# Add `--no-build`, `--no-build-package`, and binary variants

---

_Pull request opened by @charliermarsh on 2024-06-14 01:07_

## Summary

These are now supported on `uv run`, `uv lock`, `uv sync`, and `uv tool run`.

Closes https://github.com/astral-sh/uv/issues/4297.


---

_Label `configuration` added by @charliermarsh on 2024-06-14 01:07_

---

_Label `preview` added by @charliermarsh on 2024-06-14 01:07_

---

_@charliermarsh reviewed on 2024-06-14 01:09_

---

_Review comment by @charliermarsh on `crates/uv-workspace/src/settings.rs`:273 on 2024-06-14 01:09_

So... this means that the `no-build`, `no-build-package`, `no-binary`, and `no-binary-package` settings on `tool.uv` are ignored on the `pip` API (which has its own `--no-binary`, `--only-binary`, etc.).

I guess this makes sense? I'm not sure what else to do, because the APIs are totally different (the types don't line up).

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/lock.rs`:164 on 2024-06-14 01:10_

This is the payoff.

---

_@charliermarsh reviewed on 2024-06-14 01:10_

---

_@zanieb reviewed on 2024-06-14 01:19_

---

_Review comment by @zanieb on `crates/uv-workspace/src/settings.rs`:273 on 2024-06-14 01:19_

 That does seem weird. Do they need to be? Isn't our underlying type the same and we support combination?

---

_@zanieb reviewed on 2024-06-14 01:20_

---

_Review comment by @zanieb on `crates/uv-configuration/src/build_options.rs`:134 on 2024-06-14 01:20_

`PackageNameSpecifier` could maybe be renamed to `PipPackageNameSpecifier` since it's only for pip right?

---

_Review comment by @zanieb on `crates/uv-workspace/src/settings.rs`:273 on 2024-06-14 01:22_

I guess the problem is that we're still at the user-level option here which can't be combined? Should we parse into `NoBinary` and `NoBuild` and combine for `PipOptions`?

---

_@zanieb reviewed on 2024-06-14 01:22_

---

_@charliermarsh reviewed on 2024-06-14 01:23_

---

_Review comment by @charliermarsh on `crates/uv-workspace/src/settings.rs`:273 on 2024-06-14 01:23_

They are certainly not the same type here. Perhaps if I get rid of this method and inline it into `PipSettings` I can... find a way to combine them. I'm not sure. I will try... I don't know if the semantics are clear, lets see if I run into problems.

---

_@charliermarsh reviewed on 2024-06-14 01:25_

---

_Review comment by @charliermarsh on `crates/uv-workspace/src/settings.rs`:273 on 2024-06-14 01:25_

Yeah we can't combine them here. We would need to move this merging logic downstream. It is probably doable.

---

_@zanieb reviewed on 2024-06-14 01:26_

---

_Review comment by @zanieb on `crates/uv-workspace/src/settings.rs`:273 on 2024-06-14 01:26_

As far as I know the semantics should be the same it's just a difference in the user-facing types. I could look at it after too if you want.

---

_@zanieb approved on 2024-06-14 01:26_

---

_@charliermarsh reviewed on 2024-06-14 01:37_

---

_Review comment by @charliermarsh on `crates/uv-workspace/src/settings.rs`:273 on 2024-06-14 01:37_

Yeah it's just a bit less clear because the variables don't line up, so we can't (e.g.) `no_binary.or(other.no_binary)`. So I just need to validate the combination logic.

---

_@zanieb reviewed on 2024-06-14 03:29_

---

_Review comment by @zanieb on `crates/uv-workspace/src/settings.rs`:273 on 2024-06-14 03:29_

Yeah, but they _do_ line up once they're cast into `BuildOptions` (and its children) right? Might be easier to just do that early or pass everything through until we get to the point where we construct that and combine them there.

---

_@charliermarsh reviewed on 2024-06-14 03:42_

---

_Review comment by @charliermarsh on `crates/uv-workspace/src/settings.rs`:273 on 2024-06-14 03:42_

Got this to work as expected, yes.

---

_@charliermarsh reviewed on 2024-06-14 03:42_

---

_Review comment by @charliermarsh on `crates/uv-workspace/src/settings.rs`:273 on 2024-06-14 03:42_

`BuildOptions` would make things more slightly difficult because we actually read _more_ `--no-binary` arguments from `requirements.txt` files later on.

---

_@charliermarsh reviewed on 2024-06-14 03:44_

---

_Review comment by @charliermarsh on `crates/uv-workspace/src/settings.rs`:273 on 2024-06-14 03:44_

(We do convert eagerly to `BuildOptions` for the non-pip APIs. Let me see if we can do the same for the pip APIs.)

---

_@charliermarsh reviewed on 2024-06-14 03:56_

---

_Review comment by @charliermarsh on `crates/uv-workspace/src/settings.rs`:273 on 2024-06-14 03:56_

Done...

---

_Review comment by @charliermarsh on `crates/uv-configuration/src/build_options.rs`:134 on 2024-06-14 03:57_

I left this as-is for now, I wavered on it...

---

_@charliermarsh reviewed on 2024-06-14 03:57_

---

_Merged by @charliermarsh on 2024-06-14 04:05_

---

_Closed by @charliermarsh on 2024-06-14 04:05_

---

_Branch deleted on 2024-06-14 04:05_

---
