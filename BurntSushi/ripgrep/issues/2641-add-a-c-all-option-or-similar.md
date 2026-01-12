```yaml
number: 2641
title: Add a ´-C all´ option or similar
type: issue
state: closed
author: eadf
labels: []
assignees: []
created_at: 2023-10-29T13:26:10Z
updated_at: 2023-10-29T14:25:18Z
url: https://github.com/BurntSushi/ripgrep/issues/2641
synced_at: 2026-01-12T16:13:24Z
```

# Add a ´-C all´ option or similar

---

_@eadf_

#### Describe your feature request:
Use case: I occasionally use fgrep or rg for text coloring (to easily see why yanked crates are in the tree, etc): 

```
cargo tree | rg -C 12345 "crossbeam-utils v0.8.5" 
```

Here i use a "large number" to not lose any context in the text, and this works, but it's a bit hackish to provide a "large enough" number like 12345.

So something like this would be cleaner:

```
cargo tree | rg -C all "crossbeam-utils v0.8.5" 
```

What i suggest is a dual mode for the -C option, so that is can be parsed as `[number]` or `all`

---

_Comment by @BurntSushi on 2023-10-29 13:47_

Can you say why the `--passthru` flag doesn't work for your use case?

---

_Comment by @eadf on 2023-10-29 14:25_

That works just fine, thanks

---

_Closed by @eadf on 2023-10-29 14:25_

---
