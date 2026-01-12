```yaml
number: 1786
title: Updates all dependencies; replaces memmap with memmap2
type: pull_request
state: closed
author: brightly-salty
labels: []
assignees: []
base: master
head: replace-memmap
created_at: 2021-01-17T22:00:21Z
updated_at: 2021-01-18T00:03:39Z
url: https://github.com/BurntSushi/ripgrep/pull/1786
synced_at: 2026-01-12T18:23:14Z
```

# Updates all dependencies; replaces memmap with memmap2

---

_@brightly-salty_

Fixes #1785 

Originally replaced memmap with memmap2 per `cargo-audit`;
Also upgraded all dependencies

---

_Closed by @BurntSushi on 2021-01-17 23:58_

---

_Comment by @BurntSushi on 2021-01-18 00:01_

Thanks, I've done this myself. While I don't document this anywhere, I _always_ do dependency updates myself. That's so I can carefully review the changes and inspect any new dependencies that may sneak in and understand why they are needed. Accepting a PR like this that not only updates every dependency but _also_ bumps the minimal version required for each dependency would make it very very easy to miss if a new dependency ended up sneaking in.

I say all this in an effort to help others understand and appreciate that dependencies should be carefully scrutinized because they are a liability.

Thanks for bringing the RUSTSEC advisory to my attention and submitting a patch though. I appreciate it!

---

_Comment by @BurntSushi on 2021-01-18 00:01_

Note though that I did not bump the minimal version of each dependency. I don't see a strong reason to do this, and if I were consistent about it, it would lead to an annoyingly high rate of churn.

---

_Branch deleted on 2021-01-18 00:02_

---

_Comment by @brightly-salty on 2021-01-18 00:03_

> Note though that I did not bump the minimal version of each dependency. I don't see a strong reason to do this, and if I were consistent about it, it would lead to an annoyingly high rate of churn.

All I did was run `cargo-upgrade` (part of `cargo-edit`) which automatically did this. I completely understand and appreciate your quick response.

---
