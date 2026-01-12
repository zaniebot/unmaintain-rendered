```yaml
number: 2146
title: "core: update clap to version 3.1.0"
type: pull_request
state: closed
author: budde25
labels: []
assignees: []
base: master
head: update-clap
created_at: 2022-02-18T06:27:14Z
updated_at: 2023-07-10T04:07:25Z
url: https://github.com/BurntSushi/ripgrep/pull/2146
synced_at: 2026-01-12T18:23:14Z
```

# core: update clap to version 3.1.0

---

_@budde25_

This updates clap to version 3.1.0 following this migration guide
https://github.com/clap-rs/clap/releases/tag/v3.0.0

As far as I can tell it still works the same as before and it passes all the tests.
but the tricky part is the subtle changes section and I am unsure if it still follows all the same expected behavior as before

---

_Comment by @BurntSushi on 2022-03-22 16:59_

Thanks. I'll try to take a look at this soon.

Could you please get rid of merge commits? Thanks!

---

_Comment by @budde25 on 2022-03-22 21:12_


Thanks, sure thing!
How would I go about doing that? I tried to do `git rebase -i master` without much luck. 

---

_Comment by @BurntSushi on 2022-03-22 22:38_

Yes, rebase is the answer. Unfortunately, I don't have the time to mentor someone through that. Perhaps someone else can help?

---

_Comment by @akiirui on 2023-02-22 10:27_

I've rebasing the PR to the latest master branch, try to force push to this PR? https://github.com/akiirui/ripgrep/tree/update-clap

---

_Comment by @BurntSushi on 2023-07-08 14:05_

I'm going to pass on this and stick with clap 2.x for now. I didn't do the upgrade to clap 3 in the first place because I knew clap 4 was just around the corner. And I'm now considering switching to lexopt.

In general it's a good idea to get sign off from maintainers before doing big changes/refactors like this, because I think I probably could have save you the effort.

Thank you for the work though. I appreciate it.

---

_Closed by @BurntSushi on 2023-07-08 14:05_

---

_Branch deleted on 2023-07-10 04:07_

---
