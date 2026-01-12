```yaml
number: 11706
title: "Don't panic on Ctrl-C in confirm prompt"
type: pull_request
state: merged
author: ericmarkmartin
labels:
  - bug
assignees: []
merged: true
base: main
head: ctrl-c-panicking
created_at: 2025-02-22T07:23:50Z
updated_at: 2025-02-27T02:33:01Z
url: https://github.com/astral-sh/uv/pull/11706
synced_at: 2026-01-12T16:09:57Z
```

# Don't panic on Ctrl-C in confirm prompt

---

_@ericmarkmartin_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->
Resolves #11704 

Propagate errors from `uv_console::confirm` up instead of `unwrap`ping them, causing panics.

## Test Plan

<!-- How was it tested? -->
Regression testing the bug is very difficult, as the behavior of `confirm` changes based on whether `uv` is talking to a `tty`. We can trick it using ptys, but the best rust pty crate I could find only provides blocking reads of the spawned child, which is insufficient to write the regression test.

---

_Comment by @ericmarkmartin on 2025-02-22 08:04_

Took a brief stab at testing, but failed. Not sure how to test the behavior when we need the subprocess to be interactive in order to see the confirmation prompt. For reference, this is what I attempted to no avail (the snapshot looks like what happens if you answer the confirmation prompt with no).

```rust
#[test]
fn ctrl_c_install_confirmation_prompt() -> Result<()> {
    let context = TestContext::new("3.12");

    context.temp_dir.child("pyproject.toml").touch()?;

    let ctrl_c_file = context.temp_dir.child("ctrl_c");

    ctrl_c_file.write_binary(&[0x03])?;

    let file_handle = std::fs::File::open(ctrl_c_file)?;

    uv_snapshot!(context.filters(), context.pip_install()
        .arg("pyproject.toml") // triggers confirmation prompt because user probably wants `-r`
        .stdin(file_handle), @r###"
        "###
    );

    Ok(())
}
```


---

_@ericmarkmartin reviewed on 2025-02-23 05:34_

I've added a test that seems to work using pty, which I get out of wezterm's pty implementation. One thing to note is that the read out of the pty is non-blocking so if our confirmation prompt changes and becomes smaller, the test will hang, which isn't great. Still thinking of ways to fix that.

Part of making the test workable was a drive-by change so that the confirmation prompt respects the global color setting

---

_Converted to draft by @ericmarkmartin on 2025-02-24 03:29_

---

_Comment by @ericmarkmartin on 2025-02-24 03:36_

Apparently the fix doesn't work on windows, so converting back to draft while I workshop a bit

---

_Comment by @konstin on 2025-02-24 14:56_

Can you try forwarding the io error? The handler seems working, but i think the main thread panics too fast.

---

_Comment by @ericmarkmartin on 2025-02-24 19:02_

> Can you try forwarding the io error? The handler seems working, but i think the main thread panics too fast.

Forwarding it out of where? I think the main thread panics because we forward it out of the `confirm` function and `unwrap` the containing `Result`, no?

---

_Comment by @konstin on 2025-02-25 13:18_

Besides the behavior changes to signal handling, we would ideally not have an `.unwrap()` call at all, usually we can just propagate the error to the caller through a thiserror derive enum variant.

---

_Comment by @ericmarkmartin on 2025-02-26 05:20_

Ah okay, that makes sense. I've rolled back the signal handling changes because it requires some changes to the console crate's windows implementation, which I'll try to PR separately but it probably shouldn't block this.

Do you have thoughts on adding back the test with the pseudoterminal? My inclination is that it's not worth it given it's proclivity to hanging

---

_Comment by @ericmarkmartin on 2025-02-26 06:34_

> which I'll try to PR separately

see https://github.com/console-rs/console/pull/235

---

_Marked ready for review by @ericmarkmartin on 2025-02-26 06:36_

---

_Comment by @konstin on 2025-02-26 09:52_

Thanks for making the upstream PR!

I think it is fine this way, it's clear the panic cannot occur anymore given that the `unwrap()` is gone.

I've changed the error handling in `uv/src/lib.rs` to forward the `io::Error` if one occurs.

---

_Label `bug` added by @konstin on 2025-02-26 09:52_

---

_Renamed from "handle Ctrl-C explicitly in confirm prompt" to "Don't panic on Ctrl-C in confirm prompt" by @konstin on 2025-02-26 10:09_

---

_Merged by @konstin on 2025-02-26 10:10_

---

_Closed by @konstin on 2025-02-26 10:10_

---

_Branch deleted on 2025-02-27 02:33_

---
