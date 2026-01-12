```yaml
number: 1127
title: "Please do not ignore file tracked by git, even it's in .gitignore"
type: issue
state: closed
author: co-dh
labels:
  - wontfix
assignees: []
created_at: 2018-11-28T16:03:45Z
updated_at: 2018-11-28T16:35:02Z
url: https://github.com/BurntSushi/ripgrep/issues/1127
synced_at: 2026-01-12T16:13:23Z
```

# Please do not ignore file tracked by git, even it's in .gitignore

---

_@co-dh_

#### What version of ripgrep are you using?

ripgrep 0.10.0 (rev 8a7db1a918)
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)

#### How did you install ripgrep?

tar -xzvf ripgrep-0.10.0-x86_64-unknown-linux-musl.tar.gz


#### Describe your question, feature request, or bug.
More like a feature than bug.
I added a file that is tracked by git to .gitignore by accident.
rg will ignore the file, but git grep will not.
expecting rg do not ignore the file.





---

_Comment by @co-dh on 2018-11-28 16:09_

why not just use git list-files instead code the logic yourself?

---

_Comment by @BurntSushi on 2018-11-28 16:34_

This is unfortunately a bug that is unlikely to be fixed. The work around is to either take the file out of your gitignore or specifically whitelist it in either your gitignore or a separate `.ignore` file.

---

_Closed by @BurntSushi on 2018-11-28 16:34_

---

_Label `wontfix` added by @BurntSushi on 2018-11-28 16:35_

---
