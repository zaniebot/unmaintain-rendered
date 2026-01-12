```yaml
number: 1111
title: Attempt to add tar support
type: pull_request
state: closed
author: upsuper
labels: []
assignees: []
base: master
head: search-tar
created_at: 2018-11-17T09:39:57Z
updated_at: 2019-01-23T02:44:39Z
url: https://github.com/BurntSushi/ripgrep/pull/1111
synced_at: 2026-01-12T18:23:13Z
```

# Attempt to add tar support

---

_@upsuper_

Some preliminary attempt of adding tar support.

The current approach is simple, but probably not ideal. We probably should handle tarball like directory rather than hacking into the file searching...

I can investigate further if you agree this is something worth doing.

---

_Comment by @BurntSushi on 2018-11-17 12:17_

Thanks for working on this! This really should have an issue for it where we can discuss what the actual semantics should be, if any. I've historically been skeptical of adding tar archive support, but if we can nail down what the desired behavior should be in detail, then I could be convinced otherwise. :)

---

_Comment by @upsuper on 2018-11-17 20:48_

Filed #1112 for this.

---

_Comment by @BurntSushi on 2019-01-23 02:44_

I'm going to close this for now. In terms of the implementation here, this mostly looks good, but unfortunately it does not interact well with how ignore rules are currently implemented. There's more discussion in #1112.

---

_Closed by @BurntSushi on 2019-01-23 02:44_

---
