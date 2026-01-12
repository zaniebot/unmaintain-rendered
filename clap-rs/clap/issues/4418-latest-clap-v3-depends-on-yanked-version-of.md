```yaml
number: 4418
title: Latest Clap v3 depends on yanked version of textwrap
type: issue
state: closed
author: Enet4
labels:
  - C-bug
assignees: []
created_at: 2022-10-23T20:44:58Z
updated_at: 2022-10-24T17:17:28Z
url: https://github.com/clap-rs/clap/issues/4418
synced_at: 2026-01-12T16:14:16Z
```

# Latest Clap v3 depends on yanked version of textwrap

---

_@Enet4_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

1.64.0

### Clap Version

3.2.22

### Minimal reproducible code

```toml
clap = { version = "3.2.22", features = ["derive"] }
```


### Steps to reproduce the bug with the above code

```none
cargo update
```


### Actual Behaviour

Version 0.15.1 of `textwrap` [has been yanked](https://github.com/mgeisler/textwrap/blob/master/CHANGELOG.md#version-0151-2022-09-15) due to unexpected breaking changes. Due to pull request #4221, this is the only version that matches the requirement `^0.15.1`, and as such it becomes impossible to `cargo update` a project using Clap v3.

Example error message (obtained while working on DICOM-rs):

```none
error: failed to select a version for the requirement `textwrap = "^0.15.1"`
candidate versions found which didn't match: 0.16.0, 0.15.0, 0.14.2, ...
location searched: crates.io index
required by package `clap v3.2.22`
    ... which satisfies dependency `clap = "3.2.22"` of package `dicom-storescp v0.1.0-rc.1 (.../dicom-rs/storescp)`
```

### Expected Behaviour

It should work fine, probably by having clap v3 depend on textwrap v0.16.0.

### Additional Context

_No response_

### Debug Output

_No response_

---

_Label `C-bug` added by @Enet4 on 2022-10-23 20:44_

---

_Referenced in [clap-rs/clap#4420](../../clap-rs/clap/pulls/4420.md) on 2022-10-24 07:05_

---

_Referenced in [mgeisler/textwrap#453](../../mgeisler/textwrap/issues/453.md) on 2022-10-24 07:35_

---

_Referenced in [jj-vcs/jj#667](../../jj-vcs/jj/pulls/667.md) on 2022-10-24 14:56_

---

_Referenced in [clap-rs/clap#4422](../../clap-rs/clap/pulls/4422.md) on 2022-10-24 15:21_

---

_Comment by @mgeisler on 2022-10-24 15:36_

Sorry abou this! I've just release a 0.15.2 release which is identical to 0.15.0 — and thus a safe semver-compatible upgrade path from the yanked 0.15.1. Please see https://github.com/mgeisler/textwrap/issues/484.

---

_Comment by @epage on 2022-10-24 16:24_

Thanks!

---

_Closed by @epage on 2022-10-24 16:24_

---

_Comment by @Rudxain on 2022-10-24 17:16_

Thanks! All of my workflows (1 workflow per repo) were failing because of this. I appreciate the speed at which this was fixed (other repos would've taken 1 week, lol)

---
