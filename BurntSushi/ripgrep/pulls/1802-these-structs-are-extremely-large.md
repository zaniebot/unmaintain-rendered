```yaml
number: 1802
title: "These structs are extremely large!"
type: pull_request
state: closed
author: hbina
labels: []
assignees: []
base: master
head: fix/box-large-objects
created_at: 2021-02-12T03:06:25Z
updated_at: 2021-05-30T13:30:39Z
url: https://github.com/BurntSushi/ripgrep/pull/1802
synced_at: 2026-01-12T18:23:14Z
```

# These structs are extremely large!

---

_@hbina_

The smallest of them is 600 bytes. Is there a benchmark somewhere?

Signed-off-by: Hanif Bin Ariffin <hanif.ariffin.4326@gmail.com>

---

_Comment by @BurntSushi on 2021-02-12 12:00_

I don't understand this PR. Could you please motivate it? "The structs are large" isn't good enough.

---

_Comment by @hbina on 2021-02-12 12:31_

Sorry about that, from the commit
```
The smallest of them is 600 bytes.
From my (admittedly minimal) knowledge, this causes trashing.

Since these structs are so large, objects before and after this object
won't exist in the same page. Assuming that these values are being used together,
they require 2 pages to actually work instead of (possibly) just 1.

Was the point to prevent a syscall?

Signed-off-by: Hanif Bin Ariffin <hanif.ariffin.4326@gmail.com>
```

---

_Comment by @BurntSushi on 2021-02-12 12:50_

If you can provide a meaningful benchmark motivating this change, then we can think abiut merging. But without that, I don't know that this is a problem, even if your theory is correct.

---

_Comment by @marcospb19 on 2021-02-14 03:12_

@hbina If 600 bytes don't fit in a page, moving the same 600 bytes to the `heap`, they still wouldn't fit in a page, right?

Cause the page size is the same for the stack and the heap, or maybe I'm missing something.

Also, I think that these changes add some problems:
- Unnecessary increase of code complexity

And because we are talking about tiny optimizations:
- Waiting for allocator to allocate (and deallocate) the resources
- One more level of indirection, adding a branch and a possible cache miss

And AFAIK, each one of the above would make code even slower.

Waiting for the allocator to return an address, and then loading that unpredictable page is slower than loading 1 predictable page without waiting.

---

_Comment by @BurntSushi on 2021-05-30 13:30_

While it is plausible that big structs cause perf problems---I've certainly seen and fixed a few of those in the past---primarily by spending more time copying data around, I'm not convinced that that is the case here. I'd need to see a real benchmark to be sure. So with that, I'm going to close it.

---

_Closed by @BurntSushi on 2021-05-30 13:30_

---
