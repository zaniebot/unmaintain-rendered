```yaml
number: 659
title: Handling of .gitignore files with escaped spaces is inconsistent with git
type: issue
state: closed
author: sedm0784
labels:
  - duplicate
assignees: []
created_at: 2017-10-30T17:09:36Z
updated_at: 2017-10-31T10:16:49Z
url: https://github.com/BurntSushi/ripgrep/issues/659
synced_at: 2026-01-12T16:13:22Z
```

# Handling of .gitignore files with escaped spaces is inconsistent with git

---

_@sedm0784_

In short, git treats paths with escaped spaces in the same way as paths with spaces. ripgrep doesn't.

## Steps to reproduce:

1. `mkdir test_ignore`
2. `cd test_ignore`
3. `git init`
4. `echo "hi there" > "ignore me"`
5. `echo "ignore\\ me" >> .gitignore`
6. `git status` Note that the "ignore me" file is not listed.
7. `rg hi`

Observed results:

    ignore me
    1:hi there

Expected results:

There should be no output, because ripgrep should be ignoring the "ignore me" file.

I've just fixed my repo not to include the unnecessary escaping, so as far as I'm concerned this is pretty low priority ;).

---

_Comment by @okdana on 2017-10-30 17:15_

Relevant: #526

I had a fix for this that seems to work fine on UNIX, but there are unresolved implications for Windows because `rg` supports `\` as a directory separator on that platform

---

_Comment by @BurntSushi on 2017-10-30 17:33_

Should we mark this as a duplicate of #526? I guess the user facing issue isn't quite the same, but I think they share the same resolution, right?

---

_Comment by @okdana on 2017-10-30 17:34_

Seems right, yeah

---

_Label `duplicate` added by @BurntSushi on 2017-10-30 17:35_

---

_Closed by @BurntSushi on 2017-10-30 17:35_

---

_Comment by @sedm0784 on 2017-10-31 10:16_

Yep, sorry. I didn't spot that one when I skimmed the existing issues.

---
