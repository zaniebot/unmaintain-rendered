```yaml
number: 1745
title: Support searching through git history
type: issue
state: closed
author: Jab2870
labels:
  - wontfix
assignees: []
created_at: 2020-11-25T09:51:19Z
updated_at: 2021-04-25T20:00:48Z
url: https://github.com/BurntSushi/ripgrep/issues/1745
synced_at: 2026-01-12T16:13:24Z
```

# Support searching through git history

---

_@Jab2870_

Currently, to search through the history of a git repository, you need to do something like

```
git log -S 'search term'
```

This technically searches through diffs. However, it would be very nice to be able to use rg with its speed and pretty output to accomplish the same.

Is this something you'd consider implementing?

Obviously this feature request could be extended to support other version control systems although I am only familier with git.


---

_Comment by @krobelus on 2020-11-25 11:39_

You can already do that by piping Git commands into `rg`. A tighter integration seems out of scope.

---

_Comment by @BurntSushi on 2020-11-25 12:20_

Definitely way out of scope. Use shell pipelines.

This is also the kind of feature that begets more features. Which makes it very unlikely that I'll change my mind.

---

_Closed by @BurntSushi on 2020-11-25 12:20_

---

_Label `wontfix` added by @BurntSushi on 2020-11-25 12:20_

---

_Comment by @Jab2870 on 2020-11-25 14:01_

No worries, thanks for replying anyway. :+1: 

---

_Comment by @venkatd on 2021-04-25 19:28_

Is there a recommended bash command with pipes and ripgrep to efficiently search all the contents of the git history? If so, I would be happy to submit a PR to the user guide.

---
