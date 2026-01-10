```yaml
number: 2842
title: Replace Python bootstrapping script with Rust implementation
type: pull_request
state: merged
author: zanieb
labels:
  - internal
assignees: []
merged: true
base: main
head: zb/uv-toolchain
created_at: 2024-04-05T22:08:45Z
updated_at: 2024-04-10T16:22:42Z
url: https://github.com/astral-sh/uv/pull/2842
synced_at: 2026-01-10T14:43:31Z
```

# Replace Python bootstrapping script with Rust implementation

---

_Pull request opened by @zanieb on 2024-04-05 22:08_

See https://github.com/astral-sh/uv/issues/2617

Note this also includes:
- #2918 
- #2931 (pending)

A first step towards Python toolchain management in Rust.

First, we add a new crate to manage Python download metadata:

- Adds a new `uv-toolchain` crate
- Adds Rust structs for Python version download metadata
- Duplicates the script which downloads Python version metadata
- Adds a script to generate Rust code from the JSON metadata
- Adds a utility to download and extract the Python version

I explored some alternatives like a build script using things like `serde` and `uneval` to automatically construct the code from our structs but deemed it to heavy. Unlike Rye, I don't generate the Rust directly from the web requests and have an intermediate JSON layer to speed up iteration on the Rust types.

Next, we add add a `uv-dev` command `fetch-python` to download Python versions per the bootstrapping script.

- Downloads a requested version or reads from `.python-versions`
- Extracts to `UV_BOOTSTRAP_DIR`
- Links executables for path extension

This command is not really intended to be user facing, but it's a good PoC for the `uv-toolchain` API. Hash checking (via the sha256) isn't implemented yet, we can do that in a follow-up.

Finally, we remove the `scripts/bootstrap` directory, update CI to use the new command, and update the CONTRIBUTING docs.



<img width="1023" alt="Screenshot 2024-04-08 at 17 12 15" src="https://github.com/astral-sh/uv/assets/2586601/57bd3cf1-7477-4bb8-a8e9-802a00d772cb">



---

_Label `internal` added by @zanieb on 2024-04-05 22:08_

---

_@zanieb reviewed on 2024-04-05 22:11_

---

_Review comment by @zanieb on `crates/uv-toolchain/fetch-version-metadata.py`:132 on 2024-04-05 22:11_

This is the only change from the standard bootstrap script

---

_@zanieb reviewed on 2024-04-05 22:12_

---

_Review comment by @zanieb on `crates/uv-toolchain/fetch-version-metadata.py`:1 on 2024-04-05 22:12_

Nearly unchanged from the `scripts/boostrap` script which will be deleted in the near future.

---

_@zanieb reviewed on 2024-04-05 22:13_

---

_Review comment by @zanieb on `crates/uv-toolchain/src/python_versions.inc.mustache`:4 on 2024-04-05 22:13_

Maybe I should generate this header fully in Python so there's not a DO NOT EDIT line in the template itself

---

_@zanieb reviewed on 2024-04-05 22:13_

---

_Review comment by @zanieb on `crates/uv-toolchain/src/downloads.rs`:57 on 2024-04-05 22:13_

Not sure what we'll need here. Just using the same representation we have in the JSON for now.

---

_Marked ready for review by @zanieb on 2024-04-05 22:17_

---

_Review comment by @konstin on `crates/uv-toolchain/src/downloads.rs`:9 on 2024-04-08 11:07_

These should be `u_` types

---

_Review comment by @konstin on `crates/uv-toolchain/src/downloads.rs`:143 on 2024-04-08 11:08_

windows?

---

_Review comment by @konstin on `crates/uv-toolchain/src/downloads.rs`:15 on 2024-04-08 11:09_

Can we sync the names with https://github.com/astral-sh/uv/blob/c296da34bffd19d41b895bd5aa06e78d79c71bd4/crates/platform-tags/src/platform.rs#L75 and uses aliases where they don't match? So that we have consistent names across uv

---

_Review comment by @konstin on `crates/uv-toolchain/src/downloads.rs`:163 on 2024-04-08 11:11_

Shared?

---

_Review comment by @konstin on `crates/uv-toolchain/src/downloads.rs`:160 on 2024-04-08 11:12_

Is there a reason this is called `.inc` and not `.rs`?

---

_@konstin approved on 2024-04-08 11:13_

---

_@zanieb reviewed on 2024-04-08 14:23_

---

_Review comment by @zanieb on `crates/uv-toolchain/src/downloads.rs`:160 on 2024-04-08 14:23_

Coping Rye, but it seems clearer that it's generated.

---

_@zanieb reviewed on 2024-04-08 14:24_

---

_Review comment by @zanieb on `crates/uv-toolchain/src/downloads.rs`:15 on 2024-04-08 14:24_

Yeah I plan on syncing with other crates, just didn't start with that yet. Thanks for the link.

---

_@zanieb reviewed on 2024-04-08 14:24_

---

_Review comment by @zanieb on `crates/uv-toolchain/src/downloads.rs`:143 on 2024-04-08 14:24_

Maybe there's a parsing problem with the "windows-shared" builds.

---

_Converted to draft by @zanieb on 2024-04-08 14:25_

---

_Comment by @zanieb on 2024-04-08 14:25_

Switching back to a draft because I've made a lot of changes in follow-ups already.

---

_Renamed from "Add Python version metadata to new `uv-toolchain` crate" to "Replace Python bootstrapping script with Rust implementation" by @zanieb on 2024-04-08 20:25_

---

_@zanieb reviewed on 2024-04-08 21:36_

---

_Review comment by @zanieb on `crates/uv-toolchain/src/downloads.rs`:15 on 2024-04-08 21:36_

I'd like to address this in a follow-up so we can have these in a consistent location? I'm unsure what the best way to go about it is.

---

_Comment by @zanieb on 2024-04-08 22:13_

I need to fix Windows yeesh gosh it's a thorn in my side to have proper Python discovery in Windows tests.

Regardless... this is roughly ready for review. I'll either fix Windows before merging or revert CI and fix afterwards.

---

_Review requested from @konstin by @zanieb on 2024-04-08 22:14_

---

_Marked ready for review by @zanieb on 2024-04-08 22:14_

---

_@zanieb reviewed on 2024-04-08 23:35_

---

_Review comment by @zanieb on `crates/uv-toolchain/src/downloads.rs`:15 on 2024-04-08 23:35_

Working on this in https://github.com/astral-sh/uv/pull/2918

---

_Review comment by @konstin on `crates/uv-dev/src/fetch_python.rs`:51 on 2024-04-09 09:01_

```suggestion
        .map(|version| {
            PythonDownloadRequest::from_str(version).and_then(PythonDownloadRequest::fill)
        })
```

---

_Review comment by @konstin on `crates/uv-dev/src/fetch_python.rs`:86 on 2024-04-09 09:03_

```suggestion
                info!("Downloaded v{} to {}", version, path.user_display());
                downloaded += 1;
```

---

_Review comment by @konstin on `crates/uv-dev/src/fetch_python.rs`:160 on 2024-04-09 09:08_

```suggestion
    let lines: Vec<String> = fs::tokio::read_to_string(".python-versions")
        .await?
        .lines()
        .map(ToString::to_string)
        .collect();
```

---

_Review comment by @konstin on `crates/uv-dev/src/fetch_python.rs`:97 on 2024-04-09 09:14_

`bin\python3.8.exe` and `bin\python3.12.exe` work on windows, while 3.9 - 3.11 don't work, you run them but they just exit immediately without any message

---

_@konstin approved on 2024-04-09 09:16_

Code looks good but the windows launchers need fixing

---

_Review comment by @zanieb on `crates/uv-dev/src/fetch_python.rs`:86 on 2024-04-09 13:34_

Why this change?

---

_@zanieb reviewed on 2024-04-09 13:34_

---

_@zanieb reviewed on 2024-04-09 13:35_

---

_Review comment by @zanieb on `crates/uv-dev/src/fetch_python.rs`:97 on 2024-04-09 13:35_

Like you tested this PR and that's the case? Or that was what you observed with the previous hard link bootstrapping?

---

_@konstin reviewed on 2024-04-09 14:14_

---

_Review comment by @konstin on `crates/uv-dev/src/fetch_python.rs`:86 on 2024-04-09 14:14_

The `&` is not required, `format!` always borrows the value. Normally clippy would flag this (`needless_borrow`), but apparently it can't look through this macro.

---

_@konstin reviewed on 2024-04-09 14:21_

---

_Review comment by @konstin on `crates/uv-dev/src/fetch_python.rs`:97 on 2024-04-09 14:21_

I tested this PR only. The ones with the previous script don't have `.exe` suffixes and don't seem to work at all. The strategy of populating the PATH this way doesn't really work on windows.

---

_@konstin reviewed on 2024-04-09 14:23_

---

_Review comment by @konstin on `crates/uv-dev/src/fetch_python.rs`:97 on 2024-04-09 14:23_

On windows, normally all pythons are `python.exe` and we either add each folder to PATH or us the `py` launcher, which is a very different approach from the unix one of adding version suffixes to executable and putting them all in the same directory.

---

_@zanieb reviewed on 2024-04-09 14:28_

---

_Review comment by @zanieb on `crates/uv-dev/src/fetch_python.rs`:97 on 2024-04-09 14:28_

👍 makes sense, thanks!

---

_@zanieb reviewed on 2024-04-09 16:38_

---

_Review comment by @zanieb on `crates/uv-dev/src/fetch_python.rs`:86 on 2024-04-09 16:38_

Thanks! Got confused by the `downloaded` removal.

---

_@konstin reviewed on 2024-04-10 10:46_

---

_Review comment by @konstin on `.github/workflows/ci.yml`:86 on 2024-04-10 10:46_

Related to the other windows comment, i think this line doesn't work as intended on windows

---

_@zanieb reviewed on 2024-04-10 13:55_

---

_Review comment by @zanieb on `.github/workflows/ci.yml`:86 on 2024-04-10 13:55_

I think it actually is working otherwise the tests would fail to find Python 3.12 when a test does not request specific executables (because we no longer setup a default Python in the runner)?

---

_Review comment by @zanieb on `crates/uv-dev/src/fetch_python.rs`:97 on 2024-04-10 13:56_

This is addressed in #2931 

---

_@zanieb reviewed on 2024-04-10 13:56_

---

_@konstin reviewed on 2024-04-10 14:02_

---

_Review comment by @konstin on `.github/workflows/ci.yml`:86 on 2024-04-10 14:02_

We will find `python.exe` in this directory, i'd have to double check, i don't think this applies equally to the other launcher we create, while they work fine on unix

---

_@zanieb reviewed on 2024-04-10 14:06_

---

_Review comment by @zanieb on `.github/workflows/ci.yml`:86 on 2024-04-10 14:06_

Actually I removed the hard-linking entirely so I just think this is a no-op on Windows?

---

_@zanieb reviewed on 2024-04-10 14:08_

---

_Review comment by @zanieb on `.github/workflows/ci.yml`:86 on 2024-04-10 14:08_

Like here are all the links

```
2024-04-10T14:06:10.657299Z  INFO run: Linked `bin\python@3.10.13` to `bin\cpython-3.10.13-windows-x86_64-none`
2024-04-10T14:06:10.657361Z  INFO run: Linked `bin\python@3.11.7` to `bin\cpython-3.11.7-windows-x86_64-none`
2024-04-10T14:06:10.657382Z  INFO run: Linked `bin\python@3.12.1` to `bin\cpython-3.12.1-windows-x86_64-none`
2024-04-10T14:06:10.657399Z  INFO run: Linked `bin\python@3.8.12` to `bin\cpython-3.8.12-windows-x86_64-none`
2024-04-10T14:06:10.657415Z  INFO run: Linked `bin\python@3.8.18` to `bin\cpython-3.8.18-windows-x86_64-none`
2024-04-10T14:06:10.657431Z  INFO run: Linked `bin\python@3.9.18` to `bin\cpython-3.9.18-windows-x86_64-none`
```

There are no executables in here on Windows

---

_@konstin reviewed on 2024-04-10 14:11_

---

_Review comment by @konstin on `.github/workflows/ci.yml`:86 on 2024-04-10 14:11_

Can you add a comment that this is unix only and why we need it there but not on windows?

---

_@zanieb reviewed on 2024-04-10 14:14_

---

_Review comment by @zanieb on `.github/workflows/ci.yml`:86 on 2024-04-10 14:14_

Yeah I'll investigate, it was needed but maybe not anymore.

---

_@konstin reviewed on 2024-04-10 14:16_

---

_Review comment by @konstin on `.github/workflows/ci.yml`:86 on 2024-04-10 14:16_

I think i moved all the PATH handling to explicit overwrites when adding windows support

---

_Merged by @zanieb on 2024-04-10 16:22_

---

_Closed by @zanieb on 2024-04-10 16:22_

---

_Branch deleted on 2024-04-10 16:22_

---
