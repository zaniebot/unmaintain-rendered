```yaml
number: 27
title: Inconsistent behavior on windows when path not specified
type: issue
state: closed
author: brson
labels:
  - bug
assignees: []
created_at: 2016-09-23T18:38:35Z
updated_at: 2016-09-26T00:25:18Z
url: https://github.com/BurntSushi/ripgrep/issues/27
synced_at: 2026-01-12T18:23:11Z
```

# Inconsistent behavior on windows when path not specified

---

_@brson_

This is again possibly by design since ag does the same thing, but I don't understand why.

On linux I can write `rg foo` and rg will search for "foo". On windows if I write `rg foo` rg hands indefinitely, but I can write `rg foo .` to search.


---

_Comment by @brson on 2016-09-23 18:39_

This is in a mingw mintty console.


---

_Comment by @BurntSushi on 2016-09-24 02:10_

In a standard Windows command prompt, I can run `rg` with no problems.

It sounds like `rg` thinks something is being sent to `stdin`, and thus sits there waiting for input.


---

_Label `bug` added by @BurntSushi on 2016-09-24 02:10_

---

_Comment by @BurntSushi on 2016-09-26 00:25_

I think this was a dupe of #19 and has been fixed. But note that I ended up (purposefully) creating a new bug #94.


---

_Closed by @BurntSushi on 2016-09-26 00:25_

---
