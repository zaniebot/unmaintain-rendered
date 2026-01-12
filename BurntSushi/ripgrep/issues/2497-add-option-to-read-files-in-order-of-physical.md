```yaml
number: 2497
title: Add option to read files in order of physical location on disk
type: issue
state: closed
author: as-com
labels:
  - wontfix
assignees: []
created_at: 2023-04-22T18:53:26Z
updated_at: 2023-04-23T11:54:13Z
url: https://github.com/BurntSushi/ripgrep/issues/2497
synced_at: 2026-01-12T16:13:24Z
```

# Add option to read files in order of physical location on disk

---

_@as-com_

#### Describe your feature request

It would be nice if ripgrep could support using something like fiemap to sort the files in order of physical location on disk to speed up searches like [fclones](https://github.com/pkolaczk/fclones) does. It would be extremely helpful for hard drives and in some cases could be helpful for SSDs too. 

For reading compressed data with -z and with multiple threads, ripgrep could pre-read the files in order before invoking the decompressors to load the data into the page cache without random accesses.


---

_Comment by @BurntSushi on 2023-04-23 11:54_

I think this is pretty firmly outside the scope of ripgrep. This feature request is also not well motivated. A well motivated version of this request would be to provide a reproducible example that demonstrates the performance benefits of searching files in a different order than what ripgrep does naturally. Is it a 5% difference? If so, clearly, it isn't worth the extra code to do it (almost certainly). Is it an order of magnitude different? Okay, then maybe it's worth adding what I would guess to be some platform specific code to do things a little smarter.

But even then, it still seems out of scope. If this is just about the order of searching, then why not write a program that generates a list of files in the desired order and then feed that to ripgrep? Why does this logic have to be in ripgrep itself?

---

_Closed by @BurntSushi on 2023-04-23 11:54_

---

_Label `wontfix` added by @BurntSushi on 2023-04-23 11:54_

---
