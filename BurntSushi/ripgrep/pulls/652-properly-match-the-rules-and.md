```yaml
number: 652
title: "Properly match the rules \"**/\" and \"!**/\""
type: pull_request
state: merged
author: lambda
labels: []
assignees: []
merged: true
base: master
head: 649-match-all-directories
created_at: 2017-10-24T03:03:14Z
updated_at: 2017-11-15T11:44:04Z
url: https://github.com/BurntSushi/ripgrep/pull/652
synced_at: 2026-01-12T18:23:13Z
```

# Properly match the rules "**/" and "!**/"

---

_@lambda_

When processing a rule that ends in a slash, we strip it off and set the
`is_only_dir` flag.  We then apply the rule that paths that aren't
absolute should be given an implicit `**/` prefix, while avoiding
adding that prefix if it already exists.

However, this means that we miss the case in which we had already
stripped off the trailing slash and set `is_only_dir`.  Correct this
by also explicitly checking for that case.

Fixes #649

---

_Comment by @lambda on 2017-10-24 04:18_

I think the build failure here on macOS is spurious. The same test passes on my own machine (running 10.12.6, while the build machine is 10.11.6), and the test that failed is one that is supposed to panic but failed with a `SIGBUS` instead.

---

_Comment by @lambda on 2017-10-24 04:46_

Huh, failed twice the exact same way, which I can't reproduce here with the exact same nightly compiler, though a different kernel. That's strange.

---

_Comment by @BurntSushi on 2017-10-24 11:24_

That's... strange. I just kicked it again.

Looks like a great fix, thank you. :-)

---

_Comment by @BurntSushi on 2017-10-24 11:47_

Damn, defeated again. I'm not sure what's going on. I'm at work so I can't really dig into this at the moment, but I would at least like to try to get to the bottom of this before merging. (I can't imagine how this PR is causing this error though!)

---

_Comment by @lambda on 2017-10-24 14:55_

Yeah, it's really strange, I also don't see how this change could have caused this error, but it makes sense to get it worked out before merging since you do need to keep the CI working happily.

I might try spinning up a 10.11 VM this evening and see if I can repro the issue.

---

_Comment by @BurntSushi on 2017-11-01 11:10_

The same CI problem is happening with other PRs, so it's clearly not provoked by this specific PR. Therefore, I shall merge! Thanks again @lambda!

---

_Merged by @BurntSushi on 2017-11-01 11:10_

---

_Closed by @BurntSushi on 2017-11-01 11:10_

---

_Comment by @lambda on 2017-11-15 11:03_

Wonder if the CI problem was rust-lang/rust#45866?

---

_Comment by @BurntSushi on 2017-11-15 11:44_

@lambda Ah yup, I believe it is! That particular test is also a `should_panic` test, which means it was invoking the backtrace stuff, which is consistent with that bug. Nice find. :-)

---
