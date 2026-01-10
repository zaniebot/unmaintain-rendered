```yaml
number: 2228
title: "Add a `uv self update` command"
type: pull_request
state: merged
author: charliermarsh
labels:
  - cli
assignees: []
merged: true
base: main
head: charlie/updater
created_at: 2024-03-06T05:13:03Z
updated_at: 2024-03-19T20:02:54Z
url: https://github.com/astral-sh/uv/pull/2228
synced_at: 2026-01-10T14:49:08Z
```

# Add a `uv self update` command

---

_Pull request opened by @charliermarsh on 2024-03-06 05:13_

## Summary

Powered by Axo: https://github.com/axodotdev/axoupdater.

Closes https://github.com/astral-sh/uv/issues/1591.

## Test Plan

To test locally:

- `rm -f ~/.config/uv/uv-receipt.json /Users/crmarsh/.cargo/bin/uv`
- `curl --proto '=https' --tlsv1.2 -LsSf https://github.com/astral-sh/uv/releases/download/0.1.14/uv-installer.sh | sh`
- `cargo run self update`

Up-to-date:

![Screenshot 2024-03-06 at 12 13 36 AM](https://github.com/astral-sh/uv/assets/1309177/04bb7a11-6557-4317-8e86-18288fbc13c6)

Updated:

![Screenshot 2024-03-06 at 12 13 54 AM](https://github.com/astral-sh/uv/assets/1309177/c08ad739-5a2b-47cf-bf13-018a8d708330)

No receipt:

![Screenshot 2024-03-06 at 12 14 13 AM](https://github.com/astral-sh/uv/assets/1309177/317bbfaf-a787-4cbf-9f93-a4ce8ca7a988)


---

_Marked ready for review by @charliermarsh on 2024-03-06 05:20_

---

_Review requested from @zanieb by @charliermarsh on 2024-03-06 05:40_

---

_Review requested from @konstin by @charliermarsh on 2024-03-06 05:40_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/self_update.rs`:40 on 2024-03-06 06:00_

This is actually wrong, because it'll be the release of the version that's being replaced IIUC. I've filed a request to get `receipt.run()` to return the _new_ version.

---

_@charliermarsh reviewed on 2024-03-06 06:00_

---

_Review comment by @konstin on `crates/uv/src/commands/self_update.rs`:21 on 2024-03-06 09:01_

nit: long line

---

_Review comment by @konstin on `crates/uv/src/main.rs`:125 on 2024-03-06 09:03_

Why is it hidden?

---

_Comment by @konstin on 2024-03-06 09:29_

Found an upstream bug: https://github.com/axodotdev/axoupdater/issues/34

---

_Review comment by @konstin on `crates/uv/src/commands/self_update.rs`:29 on 2024-03-06 09:57_

```suggestion
    let was_updated = receipt.run().await.map_err(|err| {
        if let AxoupdateError::Reqwest(err) = err {
            anyhow::Error::new(BetterReqwestError::from(err))
        } else {
            anyhow::Error::new(err)
        }
    })?;
    if was_updated {
```

**before**

```
error: error sending request for url (https://api.github.com/repos/astral-sh/uv/releases): error trying to connect: dns error: failed to lookup address information: Temporary failure in name resolution
  Caused by: error trying to connect: dns error: failed to lookup address information: Temporary failure in name resolution
  Caused by: dns error: failed to lookup address information: Temporary failure in name resolution
  Caused by: failed to lookup address information: Temporary failure in name resolution
```

**after**

```
error: Could not connect, are you offline?
  Caused by: error sending request for url (https://api.github.com/repos/astral-sh/uv/releases): error trying to connect: dns error: failed to lookup address information: Temporary failure in name resolution
  Caused by: error trying to connect: dns error: failed to lookup address information: Temporary failure in name resolution
  Caused by: dns error: failed to lookup address information: Temporary failure in name resolution
  Caused by: failed to lookup address information: Temporary failure in name resolution
```

Still too verbose but clearer as to what went wrong.

---

_@konstin approved on 2024-03-06 09:57_

---

_Review comment by @bluetech on `crates/uv/src/commands/self_update.rs`:14 on 2024-03-06 13:20_

If I understand correctly...

```suggestion
    // Load the "install receipt" for the current binary. If the receipt is not found, then
```

---

_@bluetech reviewed on 2024-03-06 13:20_

---

_Comment by @charliermarsh on 2024-03-06 13:44_

We may want to consider waiting for https://github.com/axodotdev/axoupdater/issues/34, and we definitely need to wait for axoupdater to give us the updated version back (since the current messages are incorrect).

---

_Comment by @konstin on 2024-03-07 10:06_

Thinking about https://github.com/axodotdev/axoupdater/issues/34 again, should we feature gate the installer, so that pip/conda/distros can turn it off entirely? Seem safer than relying on recipe.

---

_Label `do-not-merge` added by @zanieb on 2024-03-07 18:07_

---

_Comment by @mistydemeo on 2024-03-08 03:52_

I've shipped a new version of axoupdater with a fix for axodotdev/axoupdater#34, but providing a way to turn off the `self update` feature entirely for certain distributions isn't a bad idea either.

---

_Comment by @charliermarsh on 2024-03-08 03:54_

One challenge we'd have there is that we build the standalone binaries as a side-effect of the PyPI wheels. So we'd need to do separate builds (which is doable).

---

_Review comment by @charliermarsh on `crates/uv/src/commands/self_update.rs`:14 on 2024-03-08 05:02_

Thanks!

---

_@charliermarsh reviewed on 2024-03-08 05:02_

---

_Review comment by @mistydemeo on `crates/uv/src/commands/self_update.rs`:68 on 2024-03-08 05:08_

The actual tag is available too, in case it differs from the version number.

```suggestion
                        result.new_version_tag
```

---

_@mistydemeo reviewed on 2024-03-08 05:08_

---

_Review comment by @mistydemeo on `crates/uv/src/commands/self_update.rs`:17 on 2024-03-08 05:13_

The return value here isn't actually the receipt - it's `self`. (This was written for chaining, eg `AxoUpdater::new_for("uv").load_receipt().run()`.) The actual install receipt's contents are members of the struct.

Doesn't matter much, just wanted to clarify in case this was confusing - I can update the naming in axoupdater in case that's helpful.

---

_@mistydemeo reviewed on 2024-03-08 05:13_

---

_@charliermarsh reviewed on 2024-03-08 05:15_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/self_update.rs`:17 on 2024-03-08 05:15_

Ahh right thank you! Also explains why `check_receipt_is_for_this_executable` can return an error if not loaded.

---

_Review comment by @JacobCoffee on `crates/uv/src/commands/self_update.rs`:80 on 2024-03-10 02:57_

```suggestion
                    "{}{} Upgraded `uv` to {}! {}",
```
everywhere else besides these two places you wrap `uv` as \`uv`

---

_Review comment by @JacobCoffee on `crates/uv/src/commands/self_update.rs`:97 on 2024-03-10 02:57_

```suggestion
                    "{}{} You're on the latest version of `uv` ({}).",
```
also here

---

_@JacobCoffee reviewed on 2024-03-10 02:58_

---

_@konstin approved on 2024-03-19 09:40_

---

_Merged by @charliermarsh on 2024-03-19 20:02_

---

_Closed by @charliermarsh on 2024-03-19 20:02_

---

_Branch deleted on 2024-03-19 20:02_

---

_Label `do-not-merge` removed by @charliermarsh on 2024-03-19 20:02_

---

_Label `cli` added by @charliermarsh on 2024-03-19 20:02_

---
