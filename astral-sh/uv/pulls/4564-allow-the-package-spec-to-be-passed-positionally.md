```yaml
number: 4564
title: "Allow the package spec to be passed positionally in `uv tool install`"
type: pull_request
state: merged
author: zanieb
labels:
  - cli
  - preview
assignees: []
merged: true
base: main
head: zb/tool-install-name
created_at: 2024-06-26T19:12:20Z
updated_at: 2024-06-27T12:35:02Z
url: https://github.com/astral-sh/uv/pull/4564
synced_at: 2026-01-12T16:06:18Z
```

# Allow the package spec to be passed positionally in `uv tool install`

---

_@zanieb_

Moves `--from` to a hidden argument — we allow it still but we validate that it is compatible with whatever is passed to `uv tool install <package>`. The positional package can now be a full specification, allowing things like `uv tool install black==24.2.0`.

---

_Label `cli` added by @zanieb on 2024-06-26 19:12_

---

_Label `preview` added by @zanieb on 2024-06-26 19:12_

---

_@charliermarsh reviewed on 2024-06-26 19:29_

---

_Review comment by @charliermarsh on `crates/uv-cli/src/lib.rs`:1890 on 2024-06-26 19:29_

`package`, like in `PipInstallArgs`? But up to you.

---

_@charliermarsh approved on 2024-06-26 19:29_

---

_@zanieb reviewed on 2024-06-26 19:33_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:1890 on 2024-06-26 19:33_

`package` makes more sense here 👍 

---

_Comment by @zanieb on 2024-06-26 20:05_

I'm going to add a custom error for `--from` to help users that are transitioning from `uv tool run`.

---

_Comment by @charliermarsh on 2024-06-26 20:06_

It might be easier to just support both (hide `--from` on the CLI help) and warn.

---

_Comment by @charliermarsh on 2024-06-26 20:06_

You know what they want to do, so it seems fine to proceed with the command IMO.

---

_Comment by @zanieb on 2024-06-26 20:19_

As long as it matches the name requested positionally I think we could continue without warning even. I worry it's a little confusing, but I'll see how it feels. Probably going to do this separately.

---

_Comment by @charliermarsh on 2024-06-26 20:22_

Honestly I bet it's a lot easier than hacking it into Clap hah.

---

_Renamed from "Move `uv tool install --from` to a positional argument" to "Allow the package specification to be passed positionally in `uv tool install`" by @zanieb on 2024-06-26 21:59_

---

_@zanieb reviewed on 2024-06-26 22:01_

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/install.rs`:63 on 2024-06-26 22:01_

I presume there are some utilities around for checking if two requirement specifications are compatible with each other?

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/install.rs`:245 on 2024-06-26 22:01_

This seemed like an oversight.

---

_@zanieb reviewed on 2024-06-26 22:01_

---

_Renamed from "Allow the package specification to be passed positionally in `uv tool install`" to "Allow the package spec to be passed positionally in `uv tool install`" by @zanieb on 2024-06-26 22:19_

---

_Review requested from @charliermarsh by @zanieb on 2024-06-26 22:25_

---

_Comment by @zanieb on 2024-06-26 23:23_

This changed substantively.

---

_Review requested from @konstin by @zanieb on 2024-06-26 23:23_

---

_@konstin approved on 2024-06-27 07:29_

---

_Merged by @zanieb on 2024-06-27 12:35_

---

_Closed by @zanieb on 2024-06-27 12:35_

---

_Branch deleted on 2024-06-27 12:35_

---
