```yaml
number: 2851
title: Release the mmap heuristics as a crate
type: issue
state: closed
author: oriongonza
labels: []
assignees: []
created_at: 2024-07-07T14:31:10Z
updated_at: 2024-07-07T14:37:52Z
url: https://github.com/BurntSushi/ripgrep/issues/2851
synced_at: 2026-01-12T16:13:25Z
```

# Release the mmap heuristics as a crate

---

_@oriongonza_

#### Describe your feature request

There is a lot of effort in heuristics of when mmapping will be a bit faster than normal reads.
I would like to use it for my programs so what I did is mostly copypasting the code which is not ideal. Do you think you could release that part as a crate for the ecosystem?

---

_Comment by @BurntSushi on 2024-07-07 14:37_

It's in the grep-searcher crate.

If you're talking about splitting it out from there into an even smaller crate, then I don't really support micro crates like that. I would suggest starting your own crate if you are of that philosophy.

---

_Closed by @BurntSushi on 2024-07-07 14:37_

---
