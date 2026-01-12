```yaml
number: 256
title: Cannot search in symlink
type: issue
state: closed
author: phiresky
labels:
  - bug
assignees: []
created_at: 2016-11-27T13:30:32Z
updated_at: 2016-12-11T04:11:12Z
url: https://github.com/BurntSushi/ripgrep/issues/256
synced_at: 2026-01-12T16:13:21Z
```

# Cannot search in symlink

---

_@phiresky_

`db` is a symlink to a directory.

```
$ rg --version
ripgrep 0.3.1
$ rg test db
db: Is a directory (os error 21)
$ rg test db/
(works as expected)
```

I would expect it to traverse the symlink in the first example, or at least to show an error message that is less confusing ;)

---

_Comment by @BurntSushi on 2016-11-27 13:46_

Oh interesting. I'm pretty sure this used to work. I wonder if it's a regression in 0.3. I'm on mobile, but can you try `rg -j1 test db`? Does that work?

---

_Comment by @phiresky on 2016-11-27 13:47_

Same thing with `rg -j1 test db`. The problem already existed in 0.2.9

2016-11-27 14:46 GMT+01:00 Andrew Gallant <notifications@github.com>:

> Oh interesting. I'm pretty sure this used to work. I wonder if it's a
> regression in 0.3. I'm on mobile, but can you try rg -j1 test db? Does
> that work?
>
> —
> You are receiving this because you authored the thread.
> Reply to this email directly, view it on GitHub
> <https://github.com/BurntSushi/ripgrep/issues/256#issuecomment-263122990>,
> or mute the thread
> <https://github.com/notifications/unsubscribe-auth/ACMnYf9KOvxbl5rsOIeANAjx2Wdqyf-Mks5rCYmlgaJpZM4K9Jco>
> .
>


---

_Comment by @jFransham on 2016-11-28 14:10_

I tried to have a look at this issue since it looked like a decent beginner bug - looks like running `rg` with `--follow` fixes this issue. It seems like this is intended behaviour, at least to some extent, but it's undeniably unintuitive that `somelink` and `somelink/` act differently. Maybe a fix would be to traverse symbolic links for all input paths, but then respect `--follow` when walking the resultant paths, but that might not play nice with globbing (like, you might accidentally end up traversing symlinks and causing a loop, and have no way to disable that without manually excluding the path).

I'd just make it so that it displays a better message, something like `somelink: is a symlinked directory - run again with --follow to traverse symbolic links`. If you're up for this I'll implement it (I'd like to start contributing to this project, since I use it daily).

---

_Comment by @BurntSushi on 2016-11-28 17:56_

> Maybe a fix would be to traverse symbolic links for all input paths

This is indeed the intended behavior I think.

> (like, you might accidentally end up traversing symlinks and causing a loop, and have no way to disable that without manually excluding the path)

The only way a loop can happen is if you use `--follow`, and when you do that, the underlying recursive iterator will detect loops. I don't think you can wind up in a loop if you follow only symlinks that have been explicitly given though?

> (I'd like to start contributing to this project, since I use it daily)

Great! I haven't actually looked into this bug yet (just got back from vaca), but I do think we should just do the right thing here and follow symlinks if they've been explicitly given as a positional parameter. If I've missed some complication here that you see and I don't, then don't hesitate to push back! :-)

---

_Label `bug` added by @BurntSushi on 2016-11-28 22:41_

---

_Assigned to @BurntSushi by @BurntSushi on 2016-11-28 22:42_

---

_Unassigned @BurntSushi by @BurntSushi on 2016-11-28 22:42_

---

_Comment by @BurntSushi on 2016-11-28 22:42_

@jFransham Let me know if you want to take a crack at this!

---

_Comment by @jFransham on 2016-11-29 08:55_

@BurntSushi Yeah, I'll get on it now. I was wondering whether you'd hang on opening if you had a recursive set of links (i.e. not a directory that _contains_ a link to itself, but a link that transitively links to itself) but it looks like the Linux kernel just throws an exception. I don't know what the behaviour is on other operating systems (*BSD, Windows, OS X) but I'd guess that at best it's the same and at worst it's not considered to be a problem for the application developer. Either way, that can be tackled as a seperate bug if it ever becomes a problem.

I'll get back with a PR soon.

---

_Closed by @BurntSushi on 2016-12-06 01:06_

---

_Comment by @jdub on 2016-12-11 04:11_

Thanks for fixing this interaction bug in such a user friendly manner! 😃

---
