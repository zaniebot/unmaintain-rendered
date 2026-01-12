```yaml
number: 889
title: "[QUESTION] How do I exclude a filetype from a search?"
type: issue
state: closed
author: andradei
labels:
  - question
assignees: []
created_at: 2018-04-19T16:31:55Z
updated_at: 2018-04-19T16:34:26Z
url: https://github.com/BurntSushi/ripgrep/issues/889
synced_at: 2026-01-12T16:13:22Z
```

# [QUESTION] How do I exclude a filetype from a search?

---

_@andradei_

This isn't an issue as far as I'm concerned, but I don't know where else to ask this.

I'm trying to make search that excludes XML files without success: Here's what I tried:

`rg -i someword --iglob \!.xml .`

I use the exclamation mark to exclude `.xml` files from the search. Also, I escape it because my shell tries to expand it (or do some stuff with it that I don't understand).

But the `.xml` files are still included in the search result.

---

_Comment by @andradei on 2018-04-19 16:33_

Looks like I was missing a `*`. This worked: `rg -i someword --iglob \!*.xml .`

---

_Closed by @andradei on 2018-04-19 16:33_

---

_Comment by @BurntSushi on 2018-04-19 16:34_

Also, you might find that single quotes will help. Please see the [guide](https://github.com/BurntSushi/ripgrep/blob/master/GUIDE.md#manual-filtering-globs)'s treatment on manual filtering.

---

_Label `question` added by @BurntSushi on 2018-04-19 16:34_

---
