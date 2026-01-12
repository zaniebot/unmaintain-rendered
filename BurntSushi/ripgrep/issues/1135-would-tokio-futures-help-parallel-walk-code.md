```yaml
number: 1135
title: Would tokio/futures help parallel walk code?
type: issue
state: closed
author: jessegrosjean
labels: []
assignees: []
created_at: 2018-12-07T20:48:40Z
updated_at: 2018-12-07T21:36:34Z
url: https://github.com/BurntSushi/ripgrep/issues/1135
synced_at: 2026-01-12T16:13:23Z
```

# Would tokio/futures help parallel walk code?

---

_@jessegrosjean_

This is more of a learning question for me, if there's a better place for me to ask please point me in that direction.

Ripgrep's parallel walk code is documented to be non-ideal and maybe in need of an eventual rewrite. I've recently been trying to understand Tokio/futures and am wondering if they would provide a better foundation for implementing a parallel walk?

If I understand things correctly the potential benefits would be:

1. Get to replace ripgrep's work stealing and thread management with a standardized framework.
2. Potential speed up because ripgrep threads won't get blocked waiting to read directories and files. They can instead spend all time just searching.

But in practice I'm not really sure that it would help. When I profile ripgrep it's already using almost all cpu cores... so I "think" that means it's not getting stuck blocking on io very often. Also I wonder if a thread doesn't get blocked on io then I think what it would do is go request more io while waiting for that other io to complete... so end result would maybe be that ripgrep would just try to open a bunch more files at the same time? Which doesn't seem good.

Any thoughts and comments appreciated. Would Tokio help ripgrep's parallel implementation or not?

Thanks,
Jesse



---

_Renamed from "Would tokio/futures help?" to "Would tokio/futures help parallel walk code?" by @jessegrosjean on 2018-12-07 20:48_

---

_Comment by @BurntSushi on 2018-12-07 21:14_

No, I don't think so. Async I/O doesn't usually translate well to disk I/O, since the underlying implementations don't tend to implement blocking/polling semantics. That is, AFAIK, you can't ask to read or write to a file such that if it would "block" on hardware, then have the call should return immediately and let you listen to future events using something like `epoll` on Linux. In most async I/O systems I know of, file I/O is typically implemented via a separate worker pool dedicated for handling blocking I/O operations, [including tokio](https://docs.rs/tokio-fs/0.1.4/tokio_fs/). To a first approximation, "async I/O" means "async network I/O." This is a common source of confusion. :-)

It is possible that something designed around futures would be useful though, since futures aren't tied to any specific I/O model. With that said, it seems like an abstraction that is probably unnecessary. I think the future path forward for the parallel recursive directory iterator is to try and use something like [rayon](https://docs.rs/rayon) until it either works well or we convince ourselves that it isn't appropriate.

> Get to replace ripgrep's work stealing and thread management with a standardized framework.

FWIW, in general, I do not think this is an unmitigated benefit. In order for me to be convinced to use a framework, I'd want some super compelling benefits that the framework itself brings in exchange for increasing the complexity budget of the code (by stacking more abstractions). Even if tokio worked like you hope in this scenario and was faster, if it was only faster by a near imperceptible amount, then I'd be very strongly opposed to bringing it in just for that.

> Potential speed up because ripgrep threads won't get blocked waiting to read directories and files. They can instead spend all time just searching.

This is just fundamentally impossible, AFAIK, in the context of Mac/Windows/Linux. One possible tweak in this space, if you are actually running into I/O problems, is to add more threads with the `-j` flag. With that said, my guess is that most uses of ripgrep are run on directories that are in the I/O cache, so it's plausible that not only is the directory tree metadata in cache, but potentially the contents of all files searched are also in cache. At that point, you start running into performance issues like, "I ran too many syscalls per file" or "I'm copying the file path N times" and so on.

---

_Comment by @jessegrosjean on 2018-12-07 21:36_

Thanks helps my understanding a lot.

---

_Closed by @jessegrosjean on 2018-12-07 21:36_

---
