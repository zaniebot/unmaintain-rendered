```yaml
number: 2390
title: Update dependencies
type: pull_request
state: closed
author: stmcginnis
labels: []
assignees: []
base: master
head: deps
created_at: 2023-01-05T09:13:42Z
updated_at: 2023-04-29T13:31:45Z
url: https://github.com/BurntSushi/ripgrep/pull/2390
synced_at: 2026-01-12T18:23:14Z
```

# Update dependencies

---

_@stmcginnis_

This updates a few dependencies to pull in their latest versions for bugfixes and other improvements.

---

_Comment by @BurntSushi on 2023-01-05 12:28_

I'm sorry, but I don't accept PRs that update dependencies. I always do that myself so that I can see how it changes the dependency tree. It is far too easy to update a version number somewhere and have that result in a new dependency (or whole mess of dependencies) getting pulled into the tree. When that happens, it's not uncommon for me to seek to eliminate that dependency. During this process, I also check that no dependencies with copyleft licenses sneak into the tree.

Moreover, many of the changes here appear to be increasing the minimal versions of dependencies for no particular reason. I usually do that for crates within this repo because there is high coupling between them and it is usually required, but otherwise, if an older version _can_ be used then It seems fine to permit that.

---

_Closed by @BurntSushi on 2023-01-05 12:28_

---

_Comment by @stmcginnis on 2023-01-05 12:42_

No worries! Should I file an issue instead asking for this instead? Just trying to do some of the work.

See https://github.com/bottlerocket-os/bottlerocket-test-system/pull/650#issuecomment-1310849255 for the motivation for wanting to do this. Basically, trying to avoid multiple dependencies on different bstr versions.

---

_Comment by @BurntSushi on 2023-01-05 14:21_

OK, I've put `globset 0.4.10` up on crates.io with an updated `bstr` dependency.

> Just trying to do some of the work.

Yeah unfortunately most of the work here is not in updating version numbers, but doing dependency review. There's exactly one reason why ripgrep has so "few" (if ~80 can be considered few) dependencies and has reasonable compile times: because I spend a lot of time pruning the tree and being careful when updating dependencies. It is _very_ easy for new dependencies to sneak into the tree.

I just went through the process myself for example and was able to remove both `num_cpus` and `crossbeam-utils`. The former was flagged because an update brought a new dependency in, and the latter was flagged as a normal part of review.

I don't really know how this can be done by anyone by the maintainers (which is just me at the moment). I'd have to somehow write down and codify my process, but much of it is "just" judgment and opinions.

---

_Comment by @AlexTMjugador on 2023-04-29 13:28_

My two cents here: [`cargo deny`](https://github.com/EmbarkStudios/cargo-deny) can be used to automate some of the tedious review tasks of dependency management mentioned.

---

_Comment by @BurntSushi on 2023-04-29 13:31_

Thank you. I know about `cargo deny`. It is indeed true that it can automate some parts of my process, but not the time consuming ones.

---
