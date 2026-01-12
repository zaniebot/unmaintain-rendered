```yaml
number: 1762
title: Add warning if nothing was searched
type: pull_request
state: closed
author: goto-engineering
labels:
  - rollup
assignees: []
base: master
head: nothing-searched-warning
created_at: 2020-12-15T08:05:02Z
updated_at: 2021-06-01T01:51:22Z
url: https://github.com/BurntSushi/ripgrep/pull/1762
synced_at: 2026-01-12T18:23:14Z
```

# Add warning if nothing was searched

---

_@goto-engineering_

Ok, another attempt at fixing https://github.com/BurntSushi/ripgrep/issues/1404.

I somehow messed up the other PR in the Github UI, not sure how. You can delete it.

Let me know if this is on the right track, and I'll look into tests.

---

_Comment by @BurntSushi on 2020-12-15 13:06_

> I somehow messed up the other PR in the Github UI, not sure how. You can delete it.

Please don't delete PRs or issues. I'm really surprised GitHub gives you the ability to delete issues/PRs filed on another repo. Please don't do that. Because you deleted it, we've now lost some historical context and some helpful info on a false start. Sure, the PR might have been wrong, but we don't just sweep wrong things under the rug. We learn from them.

Aside from that, you shouldn't have needed to open a new PR. For a small change like this, it's enough to keep it inside a single commit. When you make a change, just amend the commit. (`git commit -a --amend` for example.) And since the history is altered, you'll need to force push it, e.g., `git push <remote> <branch> --force`.

---

_@BurntSushi requested changes on 2020-12-15 13:21_

I think this looks correct to me! Have you tried it? Does it work?

I think adding some tests would be a good idea. They should go in `tests/feature.rs`. I think you'll find `assert_non_empty_stderr()` helpful in this case. Here's an example of another test that uses it: https://github.com/BurntSushi/ripgrep/blob/a6d05475fb353c756e88f605fd5366a67943e591/tests/feature.rs#L44-L47

Take a look at other tests in that file to see how to setup directory trees and what not.

---

_Comment by @goto-engineering on 2020-12-15 18:13_

> > I somehow messed up the other PR in the Github UI, not sure how. You can delete it.
> 
> Please don't delete PRs or issues. I'm really surprised GitHub gives you the ability to delete issues/PRs filed on another repo. Please don't do that. Because you deleted it, we've now lost some historical context and some helpful info on a false start. Sure, the PR might have been wrong, but we don't just sweep wrong things under the rug. We learn from them.
> 
> Aside from that, you shouldn't have needed to open a new PR. For a small change like this, it's enough to keep it inside a single commit. When you make a change, just amend the commit. (`git commit -a --amend` for example.) And since the history is altered, you'll need to force push it, e.g., `git push <remote> <branch> --force`.

Sorry, I got confused with the whole fork thing on github. Since there was only 1 commit, I deleted the branch on my fork, not realizing that would close the PR. I had intended to just force push my new commit to the same branch name.

---

_Comment by @goto-engineering on 2020-12-15 18:15_

> I think this looks correct to me! Have you tried it? Does it work?
> 
> I think adding some tests would be a good idea. They should go in `tests/feature.rs`. I think you'll find `assert_non_empty_stderr()` helpful in this case. Here's an example of another test that uses it:
> 
> https://github.com/BurntSushi/ripgrep/blob/a6d05475fb353c756e88f605fd5366a67943e591/tests/feature.rs#L44-L47
> 
> Take a look at other tests in that file to see how to setup directory trees and what not.

Yes, I've tried it with this setup of files:
```
.ignore - (ignored-dir/**)
ignored-dir/file.txt ("needle")
other-file.txt (doesn't contain needle)
```
Then I do `rg needle`. If `other-file.txt` is there, it doesn't show the warning. If only `ignored-dir` is there, it does show up.

Thanks for the hints, I'll look into the tests later!

---

_Comment by @goto-engineering on 2020-12-18 04:48_

@BurntSushi I added a test case for the warning and it passes. But another test (`regression::r428_unrecognized_style`) fails because its setup also triggers the warning.

I think it's because the case being tested there throws an exception and therefore never triggers my `searched = true`. I'm not really sure how to handle that. Since it's about an unrecognized style, is this happening in the builder itself?

---

_Comment by @goto-engineering on 2020-12-27 19:07_

@BurntSushi Ping just in case you hadn't seen this

---

_Label `rollup` added by @BurntSushi on 2021-05-30 14:17_

---

_Closed by @BurntSushi on 2021-06-01 01:51_

---
