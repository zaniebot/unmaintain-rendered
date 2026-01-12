```yaml
number: 227
title: "Allow specifying patterns with `-f FILE` and `-f-`"
type: pull_request
state: merged
author: emk
labels: []
assignees: []
merged: true
base: master
head: issue-7
created_at: 2016-11-09T11:22:21Z
updated_at: 2017-02-19T15:21:23Z
url: https://github.com/BurntSushi/ripgrep/pull/227
synced_at: 2026-01-12T18:23:12Z
```

# Allow specifying patterns with `-f FILE` and `-f-`

---

_@emk_

This is a somewhat basic implementation of `-f-` (#7), with unit tests, just to start a discussion. Your suggestions are very welcome, of course!

Changes include:

1. The internals of the `pattern` function have been refactored to avoid code duplication, but there's a lot more we could do.  Right now we read the entire pattern list into a `Vec`.
2. There's now a `WorkDir::pipe` command that allows sending standard input to `rg` when testing.

Not yet implemented: aho-corasick, or any kind of sensible support for other extremely large inputs.

Thank you for helping me get this far, and for any further suggestions!

---

_Comment by @BurntSushi on 2016-11-10 13:44_

@emk This looks superb! I think this is good to go as is after a rebase (sorry about that, did some bug fixing last night).


---

_Comment by @emk on 2016-11-15 18:01_

As requested, I've rebased this onto `master`! Thank you for helping me figure out what needed to happen.


---

_Comment by @BurntSushi on 2016-11-15 23:15_

All right, let's do this! I'll merge this one before #233 to save you some pain.

I don't have a release plan written down right now, but my vague thoughts are to get this, #233 and another PR fixing various color bugs merged before putting out a `0.3` release.


---

_Merged by @BurntSushi on 2016-11-15 23:15_

---

_Closed by @BurntSushi on 2016-11-15 23:15_

---

_Comment by @BurntSushi on 2016-11-15 23:15_

Thank you!


---

_Comment by @BurntSushi on 2016-11-18 00:45_

@emk Just a heads up, in the process of rebasing #233 I found one thing that I didn't consider during review that I just went ahead and fixed. Specifically, we should permit multiple uses of `-f/--file` _in combination_ with multiple uses of `-e/--regexp`.


---

_Comment by @ravron on 2017-01-27 02:20_

Hey @BurntSushi, just a reminder that you may want to update your [blog post](http://blog.burntsushi.net/ripgrep/), which says in part:

> ripgrep doesn’t yet support searching for patterns/literals from a file, but this is easy to add and should change soon.

---

_Comment by @BurntSushi on 2017-02-19 15:21_

@ravron Thanks for the reminder! Updated. :-)

---
