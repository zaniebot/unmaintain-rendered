```yaml
number: 2525
title: "Upgrade `aho-corasick` to `1.0.1` in `globset`"
type: pull_request
state: closed
author: zanieb
labels: []
assignees: []
base: master
head: master
created_at: 2023-06-03T19:18:36Z
updated_at: 2023-06-03T22:29:30Z
url: https://github.com/BurntSushi/ripgrep/pull/2525
synced_at: 2026-01-12T18:23:14Z
```

# Upgrade `aho-corasick` to `1.0.1` in `globset`

---

_@zanieb_

Hi!

I noticed that `aho-corasick` was still on version `0.7.20` so it's getting built twice in downstream projects that use the latest version alongside `globset`.

Updating required:

- Change from `AhoCorasick::new_auto_configured` to `AhoCorasick::new`
- A new `GlobSet::ErrorKind` for handling of `AhoCorasick::BuildError`

I named the error `MatchBuild` following the pattern used for `Regex` compilation errors.

I'm new to Rust so any feedback is welcome!

---

_Comment by @BurntSushi on 2023-06-03 22:06_

Hmmm so this isn't quite right. I don't think we need a new error type. The previous code would panic if `AhoCorasick` failed to build, so it seems fine to keep those semantics.

Otherwise, there are other concerns here:

1. I generally prefer to do all dependency upgrades myself so that I can audit any and all changes if necessary. In this case, I'm also the author of `aho-corasick`, so it's fine.
2. I'm planning to transition ripgrep to `regex 1.9` once https://github.com/rust-lang/regex/pull/978 lands. In the mean time, there may indeed be some duplicate dependencies (notably `regex-syntax` and `aho-corasick`) getting compiled. Hopefully it gets resolved soon.

---

_Closed by @BurntSushi on 2023-06-03 22:06_

---

_Comment by @zanieb on 2023-06-03 22:26_

👍 thanks for the reply!

I'm guessing it's not worth upgrading and just changing to `AhoCorasick::new(...).unwrap()`?


---
