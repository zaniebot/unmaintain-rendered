```yaml
number: 762
title: "Make handling of patterns containing a `/` match actual git behaviour"
type: pull_request
state: merged
author: okdana
labels: []
assignees: []
merged: true
base: master
head: dana/761
created_at: 2018-01-27T03:58:49Z
updated_at: 2018-01-29T19:11:00Z
url: https://github.com/BurntSushi/ripgrep/pull/762
synced_at: 2026-01-12T18:23:13Z
```

# Make handling of patterns containing a `/` match actual git behaviour

---

_@okdana_

Fixes #761

This is kind of a scary change, but it *seems* right...?

Per the unit tests, the `ignore` crate assumes that `bar/*.rs` should match both `bar/main.rs` and `foo/bar/main.rs`. However, the `gitignore` documentation seems to state very clearly that it should *not* behave this way:

>Otherwise [if the pattern contains a slash], Git treats the pattern as a shell glob suitable for consumption by fnmatch(3) with the FNM_PATHNAME flag: wildcards in the pattern will not match a / in the pathname. **For example, "Documentation/*.html" matches "Documentation/git.html" but not "Documentation/ppc/ppc.html" or "tools/perf/Documentation/perf.html".**

The real-world behaviour of git 2.16.1 is consistent with this.

Looking at the `ignore` unit tests, the test patterns borrowed from [behnam/gitignore-test](https://github.com/behnam/gitignore-test) seem to have made this assumption as well:

```
# NO_MATCH
dir_deep_02/*

# NO_MATCH
dir_deep_03/**
```

Both patterns say `NO_MATCH`, but the unit tests designed for them *do* in fact expect a match. I don't see any comments anywhere that explain why it should be different, so i'm guessing it wasn't an intentional design decision or anything.

Anyway, *without* this change, if you check out the `gitignore-test` repo, build the file tree, and then compare `rg --files` to `git add . && git status`, you'll see that `rg` is missing the two matches for those directories. *With* the change, they both match exactly the same files.

---

_@BurntSushi approved on 2018-01-29 19:09_

You are indeed correct that this is a scary change! But I cannot find any holes in your analysis or implementation, so I'm happy to go with it. We do have reasonably good test coverage here and a good history of adding regression tests, so my fear is somewhat tempered. :-)

Thanks so much for taking a deep dive on this and figuring it out!

---

_Merged by @BurntSushi on 2018-01-29 19:11_

---

_Closed by @BurntSushi on 2018-01-29 19:11_

---
