```yaml
number: 3200
title: Fix mistake in man page for -w/--word-regexp
type: pull_request
state: closed
author: KlaymenDK
labels:
  - duplicate
assignees: []
base: master
head: patch-1
created_at: 2025-10-22T10:06:24Z
updated_at: 2025-10-22T11:51:59Z
url: https://github.com/BurntSushi/ripgrep/pull/3200
synced_at: 2026-01-12T18:23:15Z
```

# Fix mistake in man page for -w/--word-regexp

---

_@KlaymenDK_

This is a fix for the documentation for the `-w` flag. The feature itself works as intended.

The output of `man rg | grep -B 3 end-half` states that:

       -w, --word-regexp
           When  enabled,  ripgrep will only show matches surrounded by word boundaries.  This is equiva‐
           lent to surrounding every pattern with \b{start-half} and \b{end-half}.

in which the last bit seems incorrect -- `\b{end-half}` should be `{end-half}\b` meaning the END of the pattern match aligns with a word boundary. This seems like a typo or simple mistake.

---

_Label `duplicate` added by @BurntSushi on 2025-10-22 11:51_

---

_Comment by @BurntSushi on 2025-10-22 11:51_

Duplicate of #3198.

`{end-half}\b` isn't a valid pattern. Why would you change a valid pattern to an invalid one?

---

_Closed by @BurntSushi on 2025-10-22 11:51_

---
