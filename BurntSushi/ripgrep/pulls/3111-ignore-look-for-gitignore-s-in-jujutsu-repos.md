```yaml
number: 3111
title: "ignore: look for .gitignore's in Jujutsu repos"
type: pull_request
state: closed
author: atalii
labels: []
assignees: []
base: master
head: jj-gitignore
created_at: 2025-07-28T19:20:42Z
updated_at: 2025-07-28T20:59:42Z
url: https://github.com/BurntSushi/ripgrep/pull/3111
synced_at: 2026-01-12T18:23:15Z
```

# ignore: look for .gitignore's in Jujutsu repos

---

_@atalii_

In the context of a non-colocated Jujutsu (jj) repository, .gitignore's retain their meaning, but .git directories aren't present. This commit amends ignore to look for jj repos when deciding whether or not to also acknowledge a .gitignore.

I've of course tested this patch and everything that I can see works fine, but do let me know if I've missed anything. Thanks a ton!

---

_Comment by @BurntSushi on 2025-07-28 19:46_

Duplicate of #2842 

---

_Closed by @BurntSushi on 2025-07-28 19:46_

---

_Comment by @atalii on 2025-07-28 20:59_

Oh, looks like I missed that. Thanks for the quick response and sorry for the duplicate PR.

---
