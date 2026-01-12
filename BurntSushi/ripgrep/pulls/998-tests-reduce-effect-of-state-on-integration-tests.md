```yaml
number: 998
title: "tests: reduce effect of state on integration tests"
type: pull_request
state: merged
author: BurntSushi
labels: []
assignees: []
merged: true
base: master
head: ag/fix-448
created_at: 2018-07-29T14:24:34Z
updated_at: 2018-07-29T18:28:35Z
url: https://github.com/BurntSushi/ripgrep/pull/998
synced_at: 2026-01-12T18:23:13Z
```

# tests: reduce effect of state on integration tests

---

_@BurntSushi_

tests: reduce reliance on state in tests

This commit improves the integration test setup by running tests inside
the system's temporary directory instead of within ripgrep's `target`
directory. The motivation here is to attempt to reduce the effect of
unanticipated state on ripgrep's integration tests, such as the presence
of `.gitignore` files in ripgrep's checkout directory hierarchy
(including parent directories).

This doesn't remove all possible state. For example, there's no
guarantee that the system's temporary directory isn't itself within a
git repository. Moreover, there may still be other ignore rules in the
directory tree that might impact test behavior. Fixing this seems
somewhat difficult. Conceptually, it seems like ripgrep should run each
test in its own `chroot`-like environment, but doing this in a
non-annoying and portable way (including Windows) doesn't appear to be
possible.

Another approach to take here might be to teach ripgrep itself that a
particular directory should be treated as root, and therefore, never
look at anything outside that directory. This also seems complex to
implement, but tractable. Let's see how this approach works for now.

Fixes #448, Fixes #996


---

_Merged by @BurntSushi on 2018-07-29 14:41_

---

_Closed by @BurntSushi on 2018-07-29 14:41_

---

_Branch deleted on 2018-07-29 14:41_

---

_Comment by @igor-raits on 2018-07-29 15:44_

@BurntSushi almost!

```
 	thread 'no_parent_ignore_git' panicked at 'assertion failed: `(left == right)`
  left: `"sherlock:For the Doctor Watsons of this world, as opposed to the Sherlock\nsherlock:be, to a very large extent, the result of luck. Sherlock Holmes\nwatson:For the Doctor Watsons of this world, as opposed to the Sherlock\nwatson:be, to a very large extent, the result of luck. Sherlock Holmes\n"`,
 right: `"sherlock:For the Doctor Watsons of this world, as opposed to the Sherlock\nsherlock:be, to a very large extent, the result of luck. Sherlock Holmes\n"`', tests/tests.rs:696:5
note: Run with `RUST_BACKTRACE=1` for a backtrace.
```

---

_Comment by @BurntSushi on 2018-07-29 16:47_

@ignatenkobrain If you're patching `tests/tests.rs` with this PR, then I think you also need another fix, which was included in https://github.com/BurntSushi/ripgrep/commit/e65ca21a6cebceb9ba79fcd164da24478cc01fb0#diff-475a9530a9c1e888d097445199b74c2f

---

_Comment by @igor-raits on 2018-07-29 18:28_

oh right, now everything works ;)

---
