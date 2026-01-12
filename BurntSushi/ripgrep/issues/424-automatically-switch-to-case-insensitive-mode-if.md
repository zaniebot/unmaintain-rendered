```yaml
number: 424
title: Automatically switch to case-insensitive mode if the query is all lowercase
type: issue
state: closed
author: alex
labels:
  - duplicate
assignees: []
created_at: 2017-03-30T00:31:20Z
updated_at: 2017-03-30T00:37:47Z
url: https://github.com/BurntSushi/ripgrep/issues/424
synced_at: 2026-01-12T16:13:22Z
```

# Automatically switch to case-insensitive mode if the query is all lowercase

---

_@alex_

This is a great feature that `ag` has. If your search is all lowercase, it'll switch to `-i` automatically, which it turns out is basically always what I want :-)

(PS: `rg` is fantastic, thank you!)

---

_Comment by @BurntSushi on 2017-03-30 00:36_

This is a dupe of #70. The short story is that the `-S/--smart-case` flag already exists, so all you need to do is `alias rg="rg --smart-case"` in your `.bashrc` and you should be good to go.

I'm strongly opposed to making this the default.

> (PS: rg is fantastic, thank you!)

:-)

---

_Closed by @BurntSushi on 2017-03-30 00:36_

---

_Label `duplicate` added by @BurntSushi on 2017-03-30 00:36_

---

_Comment by @alex on 2017-03-30 00:37_

Doh, sorry about the dupe. Thanks!

---
