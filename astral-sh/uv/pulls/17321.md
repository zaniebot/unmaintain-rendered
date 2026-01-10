```yaml
number: 17321
title: Manually parse and reconcile Boolean environment variables
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - configuration
assignees: []
merged: true
base: main
head: charlie/err-from-env
created_at: 2026-01-05T00:22:05Z
updated_at: 2026-01-06T20:43:06Z
url: https://github.com/astral-sh/uv/pull/17321
synced_at: 2026-01-10T05:49:14Z
```

# Manually parse and reconcile Boolean environment variables

---

_Pull request opened by @charliermarsh on 2026-01-05 00:22_

## Summary

This gives us more flexibility since we can avoid erroring on "conflicts" when one option is disabled (e.g., `UV_FROZEN=0 uv lock --check`).

Closes https://github.com/astral-sh/uv/issues/13385.

Closes https://github.com/astral-sh/uv/issues/13316.


---

_Label `bug` added by @charliermarsh on 2026-01-05 00:22_

---

_Label `configuration` added by @charliermarsh on 2026-01-05 00:22_

---

_Marked ready for review by @charliermarsh on 2026-01-05 01:07_

---

_Comment by @konstin on 2026-01-05 12:47_

This is a lot of churn in uv and moves us to a custom pattern of using clap that requires to manually call a check function. Would it be possible to fix this on the clap side (https://github.com/clap-rs/clap/issues/5591), so we can stay with the regular clap pattern and get the validation at parse time?

---

_Comment by @charliermarsh on 2026-01-05 15:01_

I will try here: https://github.com/clap-rs/clap/pull/6211. If it ends up taking a long time, though, I'm gonna advocate for merging this since it's a big improvement.

Honestly... it's an improvement even if https://github.com/clap-rs/clap/pull/6211 merges because we now track the _source_ of these variables so we can report them in error messages. I might _still_ advocate for this, it's _so_ much better to be able to say `UV_FROZEN` is set than `--frozen` conflicts with whatever else.

---

_@zanieb reviewed on 2026-01-05 15:12_

---

_Review comment by @zanieb on `crates/uv-cli/src/options.rs`:143 on 2026-01-05 15:12_

I kind of wanted to move all the environment variable parsing into `EnvironmentOptions`

https://github.com/astral-sh/uv/blob/13e7ad62cb0d3ca8bf5eeee407d658d6d887d5f9/crates/uv-settings/src/lib.rs#L590

then do combination with our three sources

https://github.com/astral-sh/uv/blob/9949f0801f57a386279b067bae94606e20f60f6f/crates/uv/src/settings.rs#L3453-L3455

---

_@zanieb reviewed on 2026-01-05 15:14_

---

_Review comment by @zanieb on `crates/uv-cli/src/options.rs`:143 on 2026-01-05 15:14_

I _think_ this would fit into your pull request?

---

_Comment by @zanieb on 2026-01-05 15:16_

It's a long-term goal (from my perspective) to disambiguate between environment variables and command-line arguments throughout, so I'm in favor of this kind of change.

---

_Comment by @charliermarsh on 2026-01-05 15:21_

Per https://github.com/clap-rs/clap/issues/5591#issuecomment-3710828617, if this gets solved in Clap, it won't be until Clap v5.

---

_@charliermarsh reviewed on 2026-01-05 15:25_

---

_Review comment by @charliermarsh on `crates/uv-cli/src/options.rs`:143 on 2026-01-05 15:25_

Let me see what I can do!

---

_Review requested from @zanieb by @charliermarsh on 2026-01-05 15:37_

---

_Review requested from @konstin by @charliermarsh on 2026-01-05 15:37_

---

_@charliermarsh reviewed on 2026-01-06 17:41_

---

_Review comment by @charliermarsh on `crates/uv/tests/it/help.rs`:1129 on 2026-01-06 17:41_

I restored all of these (holistically) in a subsequent PR, but I could back it into here.

---

_@zanieb approved on 2026-01-06 17:45_

---

_Merged by @charliermarsh on 2026-01-06 20:43_

---

_Closed by @charliermarsh on 2026-01-06 20:43_

---

_Branch deleted on 2026-01-06 20:43_

---
