```yaml
number: 260
title: Add sort option?
type: issue
state: closed
author: nerdrew
labels:
  - duplicate
assignees: []
created_at: 2016-12-01T08:00:29Z
updated_at: 2016-12-01T12:10:49Z
url: https://github.com/BurntSushi/ripgrep/issues/260
synced_at: 2026-01-12T16:13:21Z
```

# Add sort option?

---

_@nerdrew_

When using `rg` with `nvim` for my quickfix list, the results are no longer sorted. Thoughts on adding a sort option when sending output to a non-terminal? (It looks like the results do get sorted when it detects output is going to a terminal.)

---

_Comment by @ci70 on 2016-12-01 08:02_

This is a must for both input and output files. Please add!

Something like this is already done but not sure if it's still developed.
http://sgrep.sourceforge.net/

---

_Comment by @BurntSushi on 2016-12-01 10:38_

ripgrep has never done any sorting of any kind.

If you run ripgrep with `-j1`, then it will disable parallelism and should
result in deterministic output.

On Dec 1, 2016 3:02 AM, "Joe Parker" <notifications@github.com> wrote:

> This is a must for both input and output files. Please add!
>
> Something like this is already done but not sure if it's still developed.
> http://sgrep.sourceforge.net/
>
> —
> You are receiving this because you are subscribed to this thread.
> Reply to this email directly, view it on GitHub
> <https://github.com/BurntSushi/ripgrep/issues/260#issuecomment-264104084>,
> or mute the thread
> <https://github.com/notifications/unsubscribe-auth/AAb34qOPZyvdo5XjOWuQPeofDT1QIFZ7ks5rDn8cgaJpZM4LBGcQ>
> .
>


---

_Comment by @BurntSushi on 2016-12-01 10:39_

sgrep appears to be a completely different kind of search tool and also looks completely unrelated to this specific issue.

---

_Comment by @BurntSushi on 2016-12-01 12:10_

This appears to be a dupe of #263.

Notably, there's no regression here because ripgrep has *never* done sorting.

I don't know what ttys have to do with sorting. :-/ @nerdrew Perhaps you could clarify in #263.

---
