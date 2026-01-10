```yaml
number: 5007
title: "Add `--no-pager` option in `help` command"
type: pull_request
state: merged
author: silvanocerza
labels:
  - cli
assignees: []
merged: true
base: main
head: add-no-pager-help-option
created_at: 2024-07-12T13:00:09Z
updated_at: 2024-07-13T08:47:58Z
url: https://github.com/astral-sh/uv/pull/5007
synced_at: 2026-01-10T13:42:52Z
```

# Add `--no-pager` option in `help` command

---

_Pull request opened by @silvanocerza on 2024-07-12 13:00_

## Summary

Fixes #4941.

This PR adds a `--no-pager` option in `help` command to explicitly disable the pager.

I noted that the template used for the text printed when calling `help` with no argument or option doesn't show any option. It made sense before this PR since `help` didn't have any available option. Though I'm unsure if it makes sense to update the template as it would make it extremely verbose as all the global options would be shown too.

I leave the decision to you.

## Test Plan

I ran `cargo run -- help` to verify `--isolated` was visible and it.
I ran clippy with `cargo clippy --workspace --all-targets --all-features --locked -- -D warnings` as CI does.

I also ran tests locally with:
```
cargo nextest run \
            --features python-patch \
            --workspace \
            --status-level skip --failure-output immediate-final --no-fail-fast -j 12 --final-status-level slow
```


---

_@zanieb reviewed on 2024-07-12 14:20_

---

_Review comment by @zanieb on `crates/uv/src/commands/help.rs`:70 on 2024-07-12 14:20_

Maybe we should collapse these into a `should_page` bool haha it's getting kind of long.

---

_Comment by @zanieb on 2024-07-12 14:26_

> I noted that the template used for the text printed when calling help with no argument or option doesn't show any option.

Hm yeah this is tough. We may need to manually write out the help template so it includes the options for the command? Unless there's a way to turn off the global options (I don't think there is)

---

_@zanieb reviewed on 2024-07-12 14:27_

---

_Review comment by @zanieb on `crates/uv/tests/help.rs`:673 on 2024-07-12 14:27_

We never page for the root command so `--no-pager` should have no effect here (we also can't page during snapshots so it's kind of moot).

---

_@zanieb approved on 2024-07-12 14:29_

---

_Label `cli` added by @zanieb on 2024-07-12 14:29_

---

_Comment by @zanieb on 2024-07-12 14:29_

Thanks for contributing! Congrats on the first pull request :)

---

_@silvanocerza reviewed on 2024-07-12 15:05_

---

_Review comment by @silvanocerza on `crates/uv/tests/help.rs`:673 on 2024-07-12 15:05_

Should I remove the test then?

---

_@silvanocerza reviewed on 2024-07-12 15:05_

---

_Review comment by @silvanocerza on `crates/uv/src/commands/help.rs`:70 on 2024-07-12 15:05_

Will do! 👍 

---

_Comment by @silvanocerza on 2024-07-12 15:07_

> Hm yeah this is tough. We may need to manually write out the help template so it includes the options for the command? Unless there's a way to turn off the global options (I don't think there is)

Looking at the [template documentation](https://docs.rs/clap/latest/clap/struct.Command.html#method.help_template) doesn't seem like there's a way to separate them. 😕 

---

_@zanieb reviewed on 2024-07-12 15:09_

---

_Review comment by @zanieb on `crates/uv/tests/help.rs`:673 on 2024-07-12 15:09_

Seems nice to confirm that the option exists and works. Maybe just add a comment noting that we can't actually check if the pager is used?

---

_Comment by @silvanocerza on 2024-07-12 15:17_

Fixed all the things you pointed out. 

I also rebased to bring the changes from #5005 or the new test would have failed right after merging in `main`.

---

_Merged by @zanieb on 2024-07-12 16:11_

---

_Closed by @zanieb on 2024-07-12 16:11_

---

_Branch deleted on 2024-07-13 08:47_

---
