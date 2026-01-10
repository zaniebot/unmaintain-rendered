```yaml
number: 11897
title: explicitly handle ctrl-c in confirmation prompt instead of signal handler
type: pull_request
state: merged
author: ericmarkmartin
labels: []
assignees: []
merged: true
base: main
head: explicit-ctrl-c-handling
created_at: 2025-03-02T19:19:34Z
updated_at: 2025-03-03T15:30:48Z
url: https://github.com/astral-sh/uv/pull/11897
synced_at: 2026-01-10T11:10:39Z
```

# explicitly handle ctrl-c in confirmation prompt instead of signal handler

---

_Pull request opened by @ericmarkmartin on 2025-03-02 19:19_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Follow on to #11706. In the original PR, I tried to solve the issue by getting rid of the `ctrlc::set_handler` call. Unfortunately, this didn't work on windows due to an issue with the console crate. console 0.15.11 includes https://github.com/console-rs/console/pull/235, which resolves the issue, so now we can get rid of the call.

<!-- What's the purpose of the change? What does it do, and why? -->

This change is not super important but I still think it's worthwhile. For one, spinning up a background thread to handle `SIGINT`s when we're going to be raising the `SIGINT` from within the function is more technical complexity than needed, now that there's an easy way to explicitly catch the Ctrl-C from the terminal input. Secondly, `ctrlc::set_handler`'s [docs](https://docs.rs/ctrlc/3.4.5/ctrlc/fn.set_handler.html) advise that you set the handler just once, at the beginning of the program, so this use seems somewhat error prone. In fact, uv already has a second [callsite](https://github.com/astral-sh/uv/blob/461f4d9007160f7061a4fc0c4a5a84c613fdbff7/crates/uv/src/commands/project/add.rs#L596-L611) for this function (though I'm not sure if the two callsites could currently ever both occur on the same run of uv)

## Test Plan

I've tested this manually on linux (WSL ubuntu) and windows, though not on aarch64-apple-darwin as I don't have a machine running that. I would appreciate if someone would double-check that it works on such machines.

As discussed in the original PR, this change is pretty hard to test due to the fact that the behavior only occurs if stderr is connected to a tty. I experimented with using pseudoterminals to test this but it's still quite tricky due to the lack of x-platform non-blocking reads on the pty.

<!-- How was it tested? -->


---

_Review requested from @konstin by @charliermarsh on 2025-03-02 22:39_

---

_Review requested from @geofft by @konstin on 2025-03-03 09:44_

---

_Renamed from "explciitly handle ctrl-c in confirmation prompt instead of signal handler" to "explicitly handle ctrl-c in confirmation prompt instead of signal handler" by @konstin on 2025-03-03 09:45_

---

_@konstin approved on 2025-03-03 10:08_

---

_Comment by @charliermarsh on 2025-03-03 15:18_

I can test it out on my MacBook real quick.

---

_Merged by @charliermarsh on 2025-03-03 15:30_

---

_Closed by @charliermarsh on 2025-03-03 15:30_

---
