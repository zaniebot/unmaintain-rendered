```yaml
number: 2734
title: "bump memchr = \"2.7.1\""
type: pull_request
state: closed
author: vors
labels: []
assignees: []
base: master
head: sergei/bump-memchr
created_at: 2024-02-13T21:16:20Z
updated_at: 2024-02-13T21:52:55Z
url: https://github.com/BurntSushi/ripgrep/pull/2734
synced_at: 2026-01-12T18:23:14Z
```

# bump memchr = "2.7.1"

---

_@vors_

Currently I cannot use `ignore` create with the latest ruff crates. Hence the bump in versions.

Test plan: CI

---

_Comment by @BurntSushi on 2024-02-13 21:25_

Sorry what? Can you elaborate?

---

_Comment by @vors on 2024-02-13 21:28_

sorry, I want to use both https://github.com/astral-sh/ruff and `ignore` crate from this repo in a single project.

Currently I'm getting

```
note: for more details see https://doc.rust-lang.org/cargo/reference/resolver.html#resolver-versions
    Blocking waiting for file lock on package cache
    Updating git repository `https://github.com/astral-sh/ruff.git`
    Updating crates.io index
error: failed to select a version for `memchr`.
    ... required by package `ruff_python_parser v0.0.0 (https://github.com/astral-sh/ruff.git?rev=v0.2.1#0ccca408)`
    ... which satisfies git dependency `ruff_python_parser` of package `bzl_gen_python_extractor v0.1.0 (/Users/sergei/src/bzl-gen-build/crates/python_extractor)`
versions that meet the requirements `^2.7.1` are: 2.7.1

all possible versions conflict with previously selected packages.

  previously selected package `memchr v2.6.3`
    ... which satisfies dependency `memchr = "^2.6.3"` (locked to 2.6.3) of package `ignore v0.4.22`
    ... which satisfies dependency `ignore = "^0.4.22"` (locked to 0.4.22) of package `bzl_gen_build_driver v0.1.0 (/Users/sergei/src/bzl-gen-build/crates/driver)`

failed to select a version for `memchr` which could resolve this conflict
```

I'm fairly new to rust, if there is a better way to deal with it would appreciate a guidance. 


---

_Comment by @BurntSushi on 2024-02-13 21:40_

I'm very confused. The version bumps in this PR really don't have anything to do with what you're trying to do. A version constraint of `2.6.3` is the same as `^2.6.3` and that's the same as `>=2.6.3, < 3.0.0`. Since `memchr 2.7.1` satisfies that constraint, there shouldn't be any issue here.

Please provide a complete reproduction.

Also, `ruff` is a program, not a library.

---

_Comment by @vors on 2024-02-13 21:52_

Oh I though that the repro would be

```
[dependencies]
ruff_python_parser = { git = "https://github.com/astral-sh/ruff.git", rev = "v0.2.1" }
ignore = "0.4.22"
```

but turned out it doesn't repro.

Sorry for the noise and thank you for the explanations.

---

_Closed by @vors on 2024-02-13 21:52_

---
