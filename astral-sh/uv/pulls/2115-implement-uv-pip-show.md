```yaml
number: 2115
title: "Implement `uv pip show`"
type: pull_request
state: merged
author: ChannyClaus
labels:
  - enhancement
  - compatibility
  - cli
assignees: []
merged: true
base: main
head: main
created_at: 2024-03-01T15:57:33Z
updated_at: 2024-03-06T03:17:59Z
url: https://github.com/astral-sh/uv/pull/2115
synced_at: 2026-01-10T14:54:43Z
```

# Implement `uv pip show`

---

_Pull request opened by @ChannyClaus on 2024-03-01 15:57_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary
Implementation for https://github.com/astral-sh/uv/issues/1594
The output will contain only the name, version and location of the packages for now but it should be extendable to include other information in the future.

Quite inexperienced with Rust, so please forgive me if there are things that obviously don't make sense 😭 

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan
Added a bunch of unit tests. The exit code behavior matches `pip`'s behavior:
- When the package is found -> exit code 0
- When the package isn't found -> exit code 1
- When one package is found but another isn't -> exit code 0
<!-- How was it tested? -->


---

_Converted to draft by @ChannyClaus on 2024-03-01 19:32_

---

_Marked ready for review by @ChannyClaus on 2024-03-01 20:51_

---

_Comment by @ChannyClaus on 2024-03-06 01:38_

Dug around a bit and it seems like it's not too difficult to add the additional metadata, particularly `Required-by` and `Requires`. Would it be preferred that I include those changes in this PR or in a separate follow-up? 

---

_Review requested from @charliermarsh by @charliermarsh on 2024-03-06 01:39_

---

_Label `enhancement` added by @charliermarsh on 2024-03-06 01:39_

---

_Label `compatibility` added by @charliermarsh on 2024-03-06 01:39_

---

_Label `cli` added by @charliermarsh on 2024-03-06 01:39_

---

_Comment by @charliermarsh on 2024-03-06 01:39_

@ChannyClaus - Separate PR would be preferred, thank you!

---

_Comment by @charliermarsh on 2024-03-06 01:39_

Marking myself as reviewer on this, sorry for the delay.

---

_Comment by @ChannyClaus on 2024-03-06 01:43_

No worries! Thanks for taking a look.

---

_Comment by @ChannyClaus on 2024-03-06 02:33_

```
        FAIL [   4.616s] uv::pip_install install_git_public_https_missing_commit
```
Hmm, seems unrelated, let me try re-triggering via an empty commit.

---

_Comment by @ChannyClaus on 2024-03-06 02:43_

CI is green again 😄 - seems like it was transient indeed.

---

_@charliermarsh approved on 2024-03-06 02:51_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/pip_show.rs`:21 on 2024-03-06 02:51_

@ChannyClaus - I simplified things a bit here because this method only needs to accept a list of package names (unlike `uninstall` which needs to accept a list of requirements files, etc.).

---

_@charliermarsh reviewed on 2024-03-06 02:51_

---

_@charliermarsh reviewed on 2024-03-06 02:52_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/pip_show.rs`:120 on 2024-03-06 02:52_

@ChannyClaus - I think `pip show` _always_ shows the `site-packages` location regardless of whether it's editable. It looks like `pip show` has a separate `Editable location` field for the editable source, so we can add that in a separate PR?

---

_@charliermarsh reviewed on 2024-03-06 03:00_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/pip_show.rs`:108 on 2024-03-06 03:00_

I also had to change these back to using `println`, because the `printer` always prints to `stderr`, but I think it's important that this info goes to `stdout`, so we have to do it "manually" (by passing in `quiet`, etc.). Something we could try to improve in the future.

---

_Renamed from "Add `uv pip show`" to "Implement `uv pip show`" by @charliermarsh on 2024-03-06 03:04_

---

_Merged by @charliermarsh on 2024-03-06 03:08_

---

_Closed by @charliermarsh on 2024-03-06 03:08_

---

_Comment by @charliermarsh on 2024-03-06 03:08_

Thanks @ChannyClaus!

---

_@ChannyClaus reviewed on 2024-03-06 03:10_

---

_Review comment by @ChannyClaus on `crates/uv/src/commands/pip_show.rs`:120 on 2024-03-06 03:10_

Ah, you're absolutely right, thanks for catching that - will look into that sometime, probably sometime this weekend.

---

_@charliermarsh reviewed on 2024-03-06 03:17_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/pip_show.rs`:108 on 2024-03-06 03:17_

I improved the situation a bit here: https://github.com/astral-sh/uv/pull/2227

---
