```yaml
number: 2397
title: "Add `uv pip check`"
type: pull_request
state: merged
author: ChannyClaus
labels: []
assignees: []
merged: true
base: main
head: uvcheck
created_at: 2024-03-13T00:03:54Z
updated_at: 2024-03-16T14:11:59Z
url: https://github.com/astral-sh/uv/pull/2397
synced_at: 2026-01-10T14:49:08Z
```

# Add `uv pip check`

---

_Pull request opened by @ChannyClaus on 2024-03-13 00:03_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary
Resolves https://github.com/astral-sh/uv/issues/2391
<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan
Added a few tests to make sure that the exit code returned is 0 when there's no conflict; 1 when there's any conflict.
<!-- How was it tested? -->


---

_Comment by @danielhollas on 2024-03-13 00:05_

Thanks for taking this on @ChannyClaus, much appreciated! :love_you_gesture: 

---

_Marked ready for review by @ChannyClaus on 2024-03-13 00:18_

---

_Review requested from @zanieb by @zanieb on 2024-03-13 00:34_

---

_@zanieb reviewed on 2024-03-13 22:51_

---

_Review comment by @zanieb on `crates/uv/tests/pip_check.rs`:75 on 2024-03-13 22:51_

Is this an extra newline being omitted?

---

_@zanieb reviewed on 2024-03-13 22:51_

---

_Review comment by @zanieb on `crates/uv/src/commands/pip_check.rs`:59 on 2024-03-13 22:51_

Could we use a positive message instead? e.g. "Installed packages passed checks"

---

_@ChannyClaus reviewed on 2024-03-13 23:36_

---

_Review comment by @ChannyClaus on `crates/uv/tests/pip_check.rs`:75 on 2024-03-13 23:36_

```
chankang@chans-MacBook-Air ~/repos/uv -  (uvcheck)
$ cargo run --quiet pip check
No broken requirements found.
chankang@chans-MacBook-Air ~/repos/uv -  (uvcheck)
$ 
```

```
chankang@chans-MacBook-Air ~/repos/uv -  (uvcheck)
$ target/debug/uv pip check
No broken requirements found.
chankang@chans-MacBook-Air ~/repos/uv -  (uvcheck)
$ 
```

_I think_ it's just one

---

_@ChannyClaus reviewed on 2024-03-13 23:37_

---

_Review comment by @ChannyClaus on `crates/uv/src/commands/pip_check.rs`:59 on 2024-03-13 23:37_

The intention was to match `pip`s' behavior - can still make the change if the positive one is preferred!
```
$ pip check
No broken requirements found.
```

---

_@zanieb reviewed on 2024-03-13 23:42_

---

_Review comment by @zanieb on `crates/uv/src/commands/pip_check.rs`:59 on 2024-03-13 23:42_

I don't think we need to match their output (we're not elsewhere) and I have a minor preference for happy sounding success messages haha

---

_@zanieb reviewed on 2024-03-13 23:42_

---

_Review comment by @zanieb on `crates/uv/src/commands/pip_check.rs`:59 on 2024-03-13 23:42_

Thanks for considering that though!

---

_@ChannyClaus reviewed on 2024-03-14 01:05_

---

_Review comment by @ChannyClaus on `crates/uv/src/commands/pip_check.rs`:59 on 2024-03-14 01:05_

changed it to `Installed packages pass the check.` 👍 

---

_Comment by @ChannyClaus on 2024-03-14 01:10_

hmm pushed a benign-looking commit and CI is suddenly very unhappy 😭 - fixing

---

_Review requested from @zanieb by @ChannyClaus on 2024-03-14 11:33_

---

_Assigned to @zanieb by @zanieb on 2024-03-15 03:21_

---

_@zanieb approved on 2024-03-15 17:39_

---

_Merged by @zanieb on 2024-03-15 17:39_

---

_Closed by @zanieb on 2024-03-15 17:39_

---

_Branch deleted on 2024-03-16 14:11_

---
