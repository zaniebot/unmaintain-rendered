```yaml
number: 2235
title: "[Feature request] Just check existance"
type: issue
state: closed
author: fcolecumberri
labels:
  - invalid
assignees: []
created_at: 2022-06-13T16:00:17Z
updated_at: 2022-06-13T16:53:10Z
url: https://github.com/BurntSushi/ripgrep/issues/2235
synced_at: 2026-01-12T16:13:24Z
```

# [Feature request] Just check existance

---

_@fcolecumberri_

Sometimes I just want to check among a lot of files which files have a pattern while not very interested the context of the pattern.

Because of that, I am using a lot:

```bash
rg '...' | cut -d ':' -f 1 | sort | uniq
```

Which could be faster if I could tell ripgrep to stop the search at the first match and only print the file.

Something like:

```bash
rg --only-existance '...' | sort
```


---

_Comment by @BurntSushi on 2022-06-13 16:02_

Why doesn't `-l/--files-with-matches` work for you?

---

_Comment by @fcolecumberri on 2022-06-13 16:33_

Because I am an idiot that for some reason while reading the man page skipped that. Sorry.

---

_Comment by @BurntSushi on 2022-06-13 16:53_

No worries, just wanted to make sure I understood what you were asking for. :)

---

_Closed by @BurntSushi on 2022-06-13 16:53_

---

_Label `invalid` added by @BurntSushi on 2022-06-13 16:53_

---
