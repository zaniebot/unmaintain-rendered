```yaml
number: 1574
title: specifying --ignore-file .gitignore works, but the normal gitignore logic does not
type: issue
state: closed
author: Kagami
labels:
  - bug
  - gitignore
assignees: []
created_at: 2020-05-08T11:13:25Z
updated_at: 2020-05-20T11:50:30Z
url: https://github.com/BurntSushi/ripgrep/issues/1574
synced_at: 2026-01-12T16:13:23Z
```

# specifying --ignore-file .gitignore works, but the normal gitignore logic does not

---

_@Kagami_

Sorry if it was already asked.

Explanation of feature request with the code:

```bash
# Init structure
$ git init t
$ cd t
$ echo a > 1
$ echo a > 2
$ mkdir sub
$ echo a > sub/1
$ echo a > sub/2
$ echo sub/1 > .gitignore

# Search for a
$ rg -l a
1
2
sub/2

# Want a only in sub
$ rg -l a sub
sub/1 # < don't want this
sub/2

# Forcing ignore file
$ rg -l --ignore-file .gitignore a sub
sub/2 # < ok

# Works ok too
$ cd sub
$ rg -l a
2

# Don't know how to find gitignore (possible, but needs additional code)
$ cd sub
$ rg -l --ignore-file .gitignore a
.gitignore: No such file or directory (os error 2)

# Would be nice to have
$ alias rgi='rg --force-ignore'
$ rgi -l a sub
sub/2 # < ok
$ cd sub
$ rgi -l a
2 # < ok
```

---

_Renamed from "Add option to respect ignore files with positional arguments" to "specifying --ignore-file .gitignore works, but the normal gitignore logic does not" by @BurntSushi on 2020-05-08 11:44_

---

_Label `bug` added by @BurntSushi on 2020-05-08 11:44_

---

_Label `gitignore` added by @BurntSushi on 2020-05-08 11:44_

---

_Comment by @BurntSushi on 2020-05-08 11:46_

I think there are two separate issues here. One is your request for ignore files to apply to explicitly given file paths. That won't ever happen, but it should still work when the explicitly given arguments are _directories_, which is the case here.

I think the bug here is that `--ignore-file .gitignore` works where as the normal gitignore logic does not. That's quite strange to me and I don't know off-hand why they would be inconsistent with one another.

This issue looks very similar to #278, #829 and #1050.

---

_Comment by @Kagami on 2020-05-08 11:56_

Ah yes, it's the same as #829, thanks.

I thought it's working as designed due to https://github.com/BurntSushi/ripgrep/issues/1531.

Closing.

---

_Closed by @Kagami on 2020-05-08 11:56_

---

_Comment by @rolfb on 2020-05-20 11:15_

How are you supposed to do `rg --files **/index.js` and always apply ignores (.gitignore) to the result?

---

_Comment by @BurntSushi on 2020-05-20 11:19_

@rolfb You can't, by design.

---

_Comment by @rolfb on 2020-05-20 11:21_

@BurntSushi does that mean it's not possible to search for all index.js files recursively that respects the ignores?

---

_Comment by @BurntSushi on 2020-05-20 11:23_

No it does not mean that. Why not open a new discussion question with the actual problem you're trying to solve instead of commenting on a closed issue?

---

_Comment by @rolfb on 2020-05-20 11:50_

@BurntSushi my apologies for this, created a discussion at #1589 and linking it here in case people land here the same way I did.

---
