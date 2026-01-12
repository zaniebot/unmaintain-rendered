```yaml
number: 4163
title: "Add `uv toolchain list`"
type: pull_request
state: merged
author: zanieb
labels:
  - preview
assignees: []
merged: true
base: main
head: zb/toolchain-v-i
created_at: 2024-06-08T15:51:16Z
updated_at: 2024-06-10T16:25:57Z
url: https://github.com/astral-sh/uv/pull/4163
synced_at: 2026-01-12T16:06:04Z
```

# Add `uv toolchain list`

---

_@zanieb_

Adds the `uv toolchain` namespace and a `list` command to get us started.

```
❯ cargo run -q -- toolchain list
warning: `uv toolchain list` is experimental and may change without warning.
3.8.12   (cpython-3.8.12-macos-aarch64-none)
3.8.13   (cpython-3.8.13-macos-aarch64-none)
3.8.14   (cpython-3.8.14-macos-aarch64-none)
3.8.15   (cpython-3.8.15-macos-aarch64-none)
3.8.16   (cpython-3.8.16-macos-aarch64-none)
3.8.17   (cpython-3.8.17-macos-aarch64-none)
3.8.18   (cpython-3.8.18-macos-aarch64-none)
3.8.18   (cpython-3.8.18-macos-aarch64-none)
3.8.19   (cpython-3.8.19-macos-aarch64-none)
3.9.2    (cpython-3.9.2-macos-aarch64-none)
3.9.3    (cpython-3.9.3-macos-aarch64-none)
3.9.4    (cpython-3.9.4-macos-aarch64-none)
3.9.5    (cpython-3.9.5-macos-aarch64-none)
3.9.6    (cpython-3.9.6-macos-aarch64-none)
3.9.7    (cpython-3.9.7-macos-aarch64-none)
3.9.10   (cpython-3.9.10-macos-aarch64-none)
3.9.11   (cpython-3.9.11-macos-aarch64-none)
3.9.12   (cpython-3.9.12-macos-aarch64-none)
3.9.13   (cpython-3.9.13-macos-aarch64-none)
3.9.14   (cpython-3.9.14-macos-aarch64-none)
3.9.15   (cpython-3.9.15-macos-aarch64-none)
3.9.16   (cpython-3.9.16-macos-aarch64-none)
3.9.17   (cpython-3.9.17-macos-aarch64-none)
3.9.18   (cpython-3.9.18-macos-aarch64-none)
3.9.19   (cpython-3.9.19-macos-aarch64-none)
3.10.0   (cpython-3.10.0-macos-aarch64-none)
3.10.2   (cpython-3.10.2-macos-aarch64-none)
3.10.3   (cpython-3.10.3-macos-aarch64-none)
3.10.4   (cpython-3.10.4-macos-aarch64-none)
3.10.5   (cpython-3.10.5-macos-aarch64-none)
3.10.6   (cpython-3.10.6-macos-aarch64-none)
3.10.7   (cpython-3.10.7-macos-aarch64-none)
3.10.8   (cpython-3.10.8-macos-aarch64-none)
3.10.9   (cpython-3.10.9-macos-aarch64-none)
3.10.11  (cpython-3.10.11-macos-aarch64-none)
3.10.12  (cpython-3.10.12-macos-aarch64-none)
3.10.13  (cpython-3.10.13-macos-aarch64-none)
3.10.14  (cpython-3.10.14-macos-aarch64-none)
3.11.1   (cpython-3.11.1-macos-aarch64-none)
3.11.3   (cpython-3.11.3-macos-aarch64-none)
3.11.4   (cpython-3.11.4-macos-aarch64-none)
3.11.5   (cpython-3.11.5-macos-aarch64-none)
3.11.6   (cpython-3.11.6-macos-aarch64-none)
3.11.7   (cpython-3.11.7-macos-aarch64-none)
3.11.8   (cpython-3.11.8-macos-aarch64-none)
3.11.9   (cpython-3.11.9-macos-aarch64-none)
3.12.0   (cpython-3.12.0-macos-aarch64-none)
3.12.1   (cpython-3.12.1-macos-aarch64-none)
3.12.2   (cpython-3.12.2-macos-aarch64-none)
3.12.3   (cpython-3.12.3-macos-aarch64-none)
```

Closes https://github.com/astral-sh/uv/issues/4189

---

_Label `preview` added by @zanieb on 2024-06-08 15:51_

---

_Renamed from "zb/toolchain v i" to "Add `uv toolchain list`" by @zanieb on 2024-06-08 15:52_

---

_@zanieb reviewed on 2024-06-08 15:55_

---

_Review comment by @zanieb on `crates/uv/src/commands/toolchain/list.rs`:19 on 2024-06-08 15:55_

This implementation feels kind of sloppy. cc @snowsignal maybe a good one for you to critique :)

---

_@zanieb reviewed on 2024-06-08 15:56_

---

_Review comment by @zanieb on `crates/uv/src/cli.rs`:2002 on 2024-06-08 15:56_

No attachment to these defaults yet, probably going to change this interface as we understand the use cases.

Probably going to add support for listing all the system toolchains we can find too.

---

_Review comment by @zanieb on `crates/uv/src/cli.rs`:2002 on 2024-06-08 16:56_

Maybe by default we should only list the latest patch version for the downloads.

---

_@zanieb reviewed on 2024-06-08 16:56_

---

_@charliermarsh reviewed on 2024-06-09 18:25_

---

_Review comment by @charliermarsh on `crates/uv/src/cli.rs`:2002 on 2024-06-09 18:25_

Should these be marked as conflicting?

---

_@charliermarsh reviewed on 2024-06-09 18:26_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/toolchain/list.rs`:22 on 2024-06-09 18:26_

Can omit if you want.

---

_@charliermarsh approved on 2024-06-09 18:26_

---

_@zanieb reviewed on 2024-06-09 21:51_

---

_Review comment by @zanieb on `crates/uv/src/cli.rs`:2002 on 2024-06-09 21:51_

Good call yeah they should be

---

_@zanieb reviewed on 2024-06-09 21:52_

---

_Review comment by @zanieb on `crates/uv/src/commands/toolchain/list.rs`:22 on 2024-06-09 21:52_

We'll use it soon (e.g. #4172)

---

_Merged by @zanieb on 2024-06-10 14:22_

---

_Closed by @zanieb on 2024-06-10 14:22_

---

_Branch deleted on 2024-06-10 14:22_

---

_Review comment by @snowsignal on `crates/uv/src/commands/toolchain/list.rs`:39 on 2024-06-10 16:20_

This could be refactored to avoid an allocation:
```rust
    let download_request = match includes {
        ToolchainListIncludes::All => Some(PythonDownloadRequest::default()),
        ToolchainListIncludes::Installed => None,
        ToolchainListIncludes::Default => Some(PythonDownloadRequest::from_env()?)
    };

    let downloads = download_request
        .as_ref()
        .map(|request| request.iter_downloads())
        .into_iter()
        .flatten();
```

---

_Review comment by @snowsignal on `crates/uv/src/commands/toolchain/list.rs`:63 on 2024-06-10 16:21_

We could use `BTreeSet` here to skip the sorting / de-duplication steps.

```rust
    let mut output = BTreeSet::new();
    for toolchain in installed {
        output.insert((
            toolchain.python_version().version().clone(),
            toolchain.key().to_owned(),
        ));
    }
    for download in downloads {
        output.insert((
            download.python_version().version().clone(),
            download.key().to_owned(),
        ));
    }
```

---

_@snowsignal reviewed on 2024-06-10 16:22_

Leaving a post-merge review with some suggestions for the future 😄 

---

_Review comment by @zanieb on `crates/uv/src/commands/toolchain/list.rs`:39 on 2024-06-10 16:25_

Thanks!

---

_@zanieb reviewed on 2024-06-10 16:25_

---
