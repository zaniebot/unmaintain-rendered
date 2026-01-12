```yaml
number: 1420
title: "ripgrep: add --no-ignore-exclude flag"
type: pull_request
state: closed
author: nnathan
labels:
  - rollup
assignees: []
base: master
head: ignore_exclude
created_at: 2019-11-07T00:57:24Z
updated_at: 2020-02-17T22:16:36Z
url: https://github.com/BurntSushi/ripgrep/pull/1420
synced_at: 2026-01-12T18:23:13Z
```

# ripgrep: add --no-ignore-exclude flag

---

_@nnathan_

This is PR arises from a fundamental misunderstanding on my part of how exclude files are handled by ripgrep and ag (silversearcher) (#1417).

1. ripgrep ignores .gitignore and goes further to ignore configured global ignores (toggleable) and local .git/info/exclude (not toggleable).
2. ag ignores .gitignore and ostensibly .git/info/exclude. I say ostensibly because I came to the incorrect conclusion in #1417  that it actually ignores excludes but a cursory reading of source suggests otherwise.

Local excludes are setup because these are files you don't want to see when working in a VCS. So it does make sense to retain default behaviour to ignore them. It is imperative the user however knows this which is explained in the guide but not manpage (this is added as part of this patch).

However when the VCS is shared among others -- local excludes are not. Being able to turn it off allows to get a more consistent view when shared.

The functionality of this patch has been verified manually. It should work because it is essentially a copy of --no-ignore-global behaviour.

I haven't added tests - I didn't see any for --[no-]ignore-global otherwise I would've followed that.

Manual verification:

```
# we don't want doc/rg.1.txt.tpl to match
a:ripgrep $ cat .git/info/exclude
doc/*


# ag respects exclude and doesnt match doc/rg.1.txt.tp
a:ripgrep $ ag -l info.exclude
CHANGELOG.md
ignore/src/dir.rs
ignore/src/walk.rs
GUIDE.md
src/app.rs

# rg respects exclude and doesnt match doc/rg.1.txt.tp
a:ripgrep $ target/release/rg -l info.exclude
CHANGELOG.md
GUIDE.md
src/app.rs
ignore/src/dir.rs
ignore/src/walk.rs

a:ripgrep $ cd doc

# ag does not respect exclude (inconsistent behaviour)
a:doc $ ag -l info.exclude
rg.1.txt.tpl

# rg continues to respect exclude
a:doc $ ../target/release/rg -l info.exclude

# rg now also respects --no-ignore-exclude
a:doc $ ../target/release/rg --no-ignore-exclude -l info.exclude
rg.1.txt.tpl
```

---

_Renamed from "(wip) ripgrep: add --no-ignore-exclude flag" to "ripgrep: add --no-ignore-exclude flag" by @nnathan on 2019-11-07 02:01_

---

_@BurntSushi approved on 2020-02-17 01:19_

Thanks! I'll be merging this with some tweaks as part of my rollup PR in #1486. In particular, I do not feel that details such as `.git/info/exclude` belong in the very first sentences that introduce ripgrep. My opinion is that "respect your .gitignore" is general enough to cover everything. However, I do take your point that this could be documented better than it currently is. As such, I've added a new section on automatic filtering to the man page. (It's mostly a more condensed version of what's in the guide.)

---

_Label `rollup` added by @BurntSushi on 2020-02-17 01:20_

---

_Closed by @BurntSushi on 2020-02-17 22:16_

---
