```yaml
number: 1273
title: fix some clippy warning
type: pull_request
state: closed
author: p13marc
labels: []
assignees: []
base: master
head: master
created_at: 2019-05-04T18:34:07Z
updated_at: 2019-08-01T21:05:45Z
url: https://github.com/BurntSushi/ripgrep/pull/1273
synced_at: 2026-01-12T18:23:13Z
```

# fix some clippy warning

---

_@p13marc_

We can now run: 
cargo clippy -- -A clippy::if_same_then_else -A clippy::cast_lossless -A clippy::collapsible_if -A clippy::redundant_closure -A clippy::trivially_copy_pass_by_ref -A clippy::wrong_self_convention -A clippy::new_without_default -A clippy::useless_let_if_seq -A clippy::large_enum_variant -A clippy::ptr_arg -A clippy::never_loop -A clippy::type_complexity -A clippy::write_with_newline

without warnings.
tests: cargo build && cargo test

---

_Comment by @BurntSushi on 2019-05-04 20:06_

I appreciate the amount of work you put into this, but big PRs like this---especially ones that change style---really should be discussed first. There are a large number of changes in this PR that I disagree with (which is why I haven't gotten around to using Clippy yet), including **breaking API changes**.

Some of these changes I agree with though. But not all.

---

_Comment by @BurntSushi on 2019-08-01 21:05_

Closing this since I haven't heard back since my last comment.

---

_Closed by @BurntSushi on 2019-08-01 21:05_

---
