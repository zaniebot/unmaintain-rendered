```yaml
number: 186
title: "[feature request] file type filters for only headers / only source files"
type: issue
state: closed
author: durka
labels: []
assignees: []
created_at: 2016-10-18T16:37:17Z
updated_at: 2016-11-06T17:24:00Z
url: https://github.com/BurntSushi/ripgrep/issues/186
synced_at: 2026-01-12T18:23:11Z
```

# [feature request] file type filters for only headers / only source files

---

_@durka_

Use case: my question is "what do I include to get this function?", so I want to look through `*.h` but not `*.c`/`*.cpp`.


---

_Comment by @BurntSushi on 2016-10-18 16:47_

What is the type name? Of the popular languages, I think only C/C++ have this kind of split, so I imagine it would be specific to them and not a general rule.

Note that you can also search header files by specifying a glob, e.g., `-g '*.h'`, which I kind of think might be good enough.


---

_Comment by @durka on 2016-10-18 18:26_

Yeah, I guess globbing is fine for niche things.

On Tue, Oct 18, 2016 at 12:47 PM, Andrew Gallant notifications@github.com
wrote:

> What is the type name? Of the popular languages, I think only C/C++ have
> this kind of split, so I imagine it would be specific to them and not a
> general rule.
> 
> Note that you can also search header files by specifying a glob, e.g., -g
> '*.h', which I kind of think might be good enough.
> 
> —
> You are receiving this because you authored the thread.
> Reply to this email directly, view it on GitHub
> https://github.com/BurntSushi/ripgrep/issues/186#issuecomment-254568165,
> or mute the thread
> https://github.com/notifications/unsubscribe-auth/AAC3n5pxG2YGf1juvj8I6zIqfG0j-8h_ks5q1PgLgaJpZM4KaBOH
> .


---

_Comment by @gvansickle on 2016-10-26 13:25_

`ack`, `ag`, and `ucg` (and maybe others) have a `--hh` file type for this for C/C++, which only includes '*.h' files.  Which now that I type that, probably should be expanded to include '*.hpp' etc. forms.


---

_Closed by @BurntSushi on 2016-11-06 17:23_

---

_Comment by @BurntSushi on 2016-11-06 17:24_

I added a new `h` file type that matches `*.h` and `*.hpp`.


---
