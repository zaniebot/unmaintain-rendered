```yaml
number: 952
title: simplify main error handling by using the new return Result from main feature
type: pull_request
state: closed
author: rtaycher
labels: []
assignees: []
base: master
head: return-result-from-main
created_at: 2018-06-16T07:34:55Z
updated_at: 2018-06-16T19:56:06Z
url: https://github.com/BurntSushi/ripgrep/pull/952
synced_at: 2026-01-12T18:23:13Z
```

# simplify main error handling by using the new return Result from main feature

---

_@rtaycher_

return Result from main/increase minimum version to 1.26

Is printing `Error: StringError("no matches")` on no matches ok or should I put back `process::exit(1)` and not print anything?

---

_Comment by @BurntSushi on 2018-06-16 11:34_

Thanks for the PR! I appreciate that you're trying to improve things here, but there are several issues with this PR, which is why I'm just going to close this out. Those issues are:

* Compare the output of `rg foo --colors 'invalid'` on master and in this PR. Your PR shows the error as a debug representation. This is unacceptable for end users, and this is why `result-in-main`, as it currently exists, is not allowed in my projects.
* The simplification being made here is, IMO, nominal.
* Increasing the minimum Rust version to the _latest stable release_ isn't acceptable. While ripgrep doesn't have an official policy on this matter, requiring the latest stable release is definitely not the right thing to do.
* Printing an error message when no results are found represents a *significant behavioral change*. I can't imagine any reality in which I would accept a change like that, but in general, for significant changes you really need to open an issue so that it can be properly discussed.
* Your branch name starts with a `U+0096 START OF GUARDED AREA` character, which makes it unreasonably difficult to manage.

---

_Closed by @BurntSushi on 2018-06-16 11:34_

---

_Comment by @rtaycher on 2018-06-16 17:03_

thank you fro the explanation



---

_Comment by @rtaycher on 2018-06-16 17:37_

> Increasing the minimum Rust version to the latest stable release isn't acceptable. While ripgrep doesn't have an official policy on this matter, requiring the latest stable release is definitely not the right thing to do.

I'm sorry about this one, it may have been obvious but maybe it should be mentioned/semi-official (don't increase the minimum version to last stable release/last 2 stable releases)? I tried to find anything on ripgreps version policy in the rpo but could only find that it had been incremented in jumps by looking at the git history.

---

_Comment by @BurntSushi on 2018-06-16 19:52_

The only real policy is "be conservative, within reason." I don't really know how to articulate it more than that. It's a judgment call. Part of this has to do with the _reason_ for bumping the minimum Rust version too. Nominal reasons (like the code change in this PR, barring the other issues I mentioned) are never going to pass my sniff test. The most common reason to bump the minimum Rust version for ripgrep is "because a dependency bumped it, and in due time, it's good to stay up to date with dependencies and move with the ecosystem."

---
