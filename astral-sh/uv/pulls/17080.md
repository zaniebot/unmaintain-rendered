```yaml
number: 17080
title: Add value hints to command line arguments to improve shell completion accuracy
type: pull_request
state: merged
author: EliteTK
labels:
  - enhancement
assignees: []
merged: true
base: main
head: tk/completion-improvements
created_at: 2025-12-10T23:11:38Z
updated_at: 2025-12-28T11:29:16Z
url: https://github.com/astral-sh/uv/pull/17080
synced_at: 2026-01-10T05:49:14Z
```

# Add value hints to command line arguments to improve shell completion accuracy

---

_Pull request opened by @EliteTK on 2025-12-10 23:11_

## Summary

This partially addresses #17076 by adding `value_hint` to various arguments.

For cases where an option takes a path to either specifically a file or a directory directory, `ValueHint::FilePath` and `ValueHint::DirPath` are used respectively to try to limit the amount of noise presented by completions in shells which support it.

For cases where a URL (and only a URL, not a path) can be supplied, `ValueHint::Url` is used.

For cases where a python interpreter is to be specified, `ValueHint::CommandName` is used which will tab complete from `$PATH` by default, but will fall back to completing executable filenames if you start typing a path.

Finally, for the many cases where there is no built in completion which would make sense, and where default completion of a path would make no sense (e.g. a package name, or version specifier, or date) `ValueHint::Other` is used to explicitly disable completion.

## Test Plan

Manually tested a bunch of these. These _could_ be automated in the sense that we could snapshot the completion from zsh but I've not thought about how that could be done yet.

---

_Label `enhancement` added by @konstin on 2025-12-11 09:32_

---

_Review requested from @konstin by @EliteTK on 2025-12-15 15:00_

---

_@konstin approved on 2025-12-15 15:28_

Not deep into how shell completions work, but the code looks good.

---

_Review requested from @zanieb by @EliteTK on 2025-12-15 15:31_

---

_@zanieb reviewed on 2025-12-15 15:42_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:1552 on 2025-12-15 15:42_

I think `--python <version>` is the most common use-case here, not passing an executable name.

---

_@zanieb reviewed on 2025-12-15 15:51_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:3604 on 2025-12-15 15:51_

Editables are local directories

---

_@EliteTK reviewed on 2025-12-15 15:52_

---

_Review comment by @EliteTK on `crates/uv-cli/src/lib.rs`:1552 on 2025-12-15 15:52_

The thought process here was that if someone wants to type a version they can still do so, and we have no particular way to complete them, but if they did want to specify a python executable instead, they would have an easier time due to completions being restricted to valid executables.

The default here is to do "_default" completion on zsh, which will complete files and things like that. So CommandName actually restricts this to fewer things.

The only alternative I can think of is to make this `ValueHint::Other` but this would prevent zsh from ever letting you complete paths to programs or programs in your path.

What do you think?

---

_@zanieb reviewed on 2025-12-15 15:54_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:1552 on 2025-12-15 15:54_

I think we should probably use `Other`. I don't want to encourage people to use Python executables from their `PATH` here, I think that's rarely a desirable pattern and can be confusing.

---

_@EliteTK reviewed on 2025-12-15 15:55_

---

_Review comment by @EliteTK on `crates/uv-cli/src/lib.rs`:3604 on 2025-12-15 15:55_

I can't remember why I chose `ValueHint::Other` here ... I think I went into looking what an editable was and found that it could be a directory or some weird syntax. But that doesn't really justify not letting you complete directories.

Will fix.

---

_@EliteTK reviewed on 2025-12-15 16:08_

---

_Review comment by @EliteTK on `crates/uv-cli/src/lib.rs`:1552 on 2025-12-15 16:08_

Fair enough, will change these.

---

_Review requested from @zanieb by @EliteTK on 2025-12-15 17:50_

---

_@zanieb approved on 2025-12-15 18:09_

---

_Merged by @EliteTK on 2025-12-15 18:29_

---

_Closed by @EliteTK on 2025-12-15 18:29_

---

_Branch deleted on 2025-12-15 18:29_

---
