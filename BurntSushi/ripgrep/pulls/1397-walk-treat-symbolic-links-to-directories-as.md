```yaml
number: 1397
title: "walk: treat symbolic links to directories as directories"
type: pull_request
state: closed
author: nikofil
labels:
  - rollup
assignees: []
base: master
head: iss_1389
created_at: 2019-10-02T09:17:56Z
updated_at: 2020-02-17T22:16:41Z
url: https://github.com/BurntSushi/ripgrep/pull/1397
synced_at: 2026-01-12T18:23:13Z
```

# walk: treat symbolic links to directories as directories

---

_@nikofil_

Due to how walkdir works if symlinks are not followed, symlinks to
directories are seen as simple files by ripgrep. This caused a panic
in some cases due to receiving a WalkEvent::Exit event without a
corresponding WalkEvent::Dir event.

This is fixed by looking at the metadata of the file in the case of a
symlink to determine if it's a directory.

Fixes #1389

---

_Comment by @BurntSushi on 2019-10-02 11:05_

Thanks. It's going to take me a bit to review this, as this adds an additional syscall for every symlink that is seen, as far as I can tell. We've had performance problems in the past (especially on Windows) when doing that.

---

_Comment by @nikofil on 2019-10-02 11:22_

I see, that could indeed be too much overhead.
The only case that this PR fixes should be the case reported (when the given path is a symlink to a directory). Otherwise when the option to follow symlinks isn't given the depth never changes so there are no `WalkEvent::Exit` events caused by symlinks:
https://github.com/BurntSushi/ripgrep/blob/8892bf648cfec111e6e7ddd9f30e932b0371db68/ignore/src/walk.rs#L1026-L1030
Therefore checking only the given path, instead of any symlink encountered, should also fix this case. I can try fixing it that way if you agree.

---

_@BurntSushi reviewed on 2020-02-17 21:14_

Great, thanks! I think this looks good. I'll be merging it with some tweaks as part of #1486. In particular, I made it so the additional stat check only occurred when the directory entry had depth `0`.

---

_Label `rollup` added by @BurntSushi on 2020-02-17 21:15_

---

_Closed by @BurntSushi on 2020-02-17 22:16_

---
