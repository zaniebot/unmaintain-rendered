```yaml
number: 1333
title: O_NOATIME ?
type: issue
state: closed
author: lilydjwg
labels:
  - enhancement
  - help wanted
  - question
  - icebox
assignees: []
created_at: 2019-07-29T15:23:43Z
updated_at: 2020-01-28T13:10:22Z
url: https://github.com/BurntSushi/ripgrep/issues/1333
synced_at: 2026-01-12T16:13:23Z
```

# O_NOATIME ?

---

_@lilydjwg_

#### What version of ripgrep are you using?

ripgrep 11.0.1 (rev 7ac95c1f50)
+SIMD -AVX (compiled)
+SIMD +AVX (runtime)

#### How did you install ripgrep?

pacman

#### What operating system are you using ripgrep on?

Arch Linux.

#### Describe your question, feature request, or bug.

Would ripgrep try to open files with `O_NOATIME` on Linux so it does not update access times? I want it because:

* it could save some disk writes
* access times are helpful to determine if I should delete / archive those files

Currently ripgrep will update access times for all files in the search space, e.g. my workspace directory. I tend to use a much larger than necessary search space as ripgerp is so fast, but it will mark all files as recently visited.

It's OK for me if this feature is behind an option because I mostly search my own files anyway.

---

_Label `enhancement` added by @BurntSushi on 2019-07-29 15:25_

---

_Label `help wanted` added by @BurntSushi on 2019-07-29 15:25_

---

_Comment by @BurntSushi on 2019-07-29 15:25_

I would potentially be okay with this. Does `grep` has a similar feature? If not, do you know why not?

---

_Comment by @lilydjwg on 2019-07-29 16:36_

There was a [thread](https://lists.gnu.org/archive/html/bug-grep/2014-09/msg00011.html) on that but grep did not get the option though. It seems that adding that option is not very trivial and atime is not very useful.

Actually I didn't want this option when using grep, mostly because I didn't (and still don't) often do the `find | grep` combo. grep was so slow that I only grepped files from a single project (with `-r .`), while with ripgrep I tend to grep all my projects. (And now I only use grep for pipes or a single file unless ripgrep is unavailable on the system.)

---

_Comment by @BurntSushi on 2019-07-29 16:57_

Ah yeah, thanks for that link. It's helpful. In particular, it reminded me that this would require a lot of plumbing work. Given the current state of the world, this would actually require adding the relevant APIs to set `O_NOATIME` in the standard library directory traversal APIs, and then add the corresponding APIs to enable it in both `walkdir` and `ignore`. Then `grep-searcher` and probably ripgrep core would need to be updated to open files using that mode as well.

Overall, this is a ton of work for a niche use case. I am in the process of rewriting `walkdir` such that it no longer depends on standard library APIs, which would probably make this significantly easier. But, it's still a lot of plumbing work and the rewrite itself will take quite a bit of time.

So I'm going to close this for now, since it's unlikely to happen soon, and may not happen ever. Sorry. (It also sounds like you need special options enabled in `ext4` for this to be even useful in the first place.)

---

_Closed by @BurntSushi on 2019-07-29 16:57_

---

_Label `icebox` added by @BurntSushi on 2019-07-29 16:57_

---

_Label `question` added by @BurntSushi on 2019-07-29 16:57_

---

_Comment by @lilydjwg on 2019-07-30 05:03_

OK, I didn't think there are so many libraries involved. I hope it can happen in the future then. And I don't need any special mount options to get a roughly correct atime (for ext4, zfs, xfs and btrfs; relatime is default).

In the meantime maybe I'll get a fancy wrapper to mount the search space as `ro` :-)

---

_Comment by @BurntSushi on 2019-07-30 10:53_

It's not necessarily about libraries, but about layers in the code. The work would be approximately same if ripgrep had no libraries and rolled everything itself. (With modifications to the standard library being an exception, since that would have to go through a review process, and something like this would probably require some careful API design.)

---

_Comment by @lilydjwg on 2020-01-28 11:21_

Using private mounts seems to be harder than I thought and I've gone another way which uses a `LD_PRELOAD` hack: https://github.com/lilydjwg/open-noatime.

---

_Comment by @BurntSushi on 2020-01-28 11:34_

@lilydjwg Nice! If you want to add that to the FAQ in this repo, I'd be happy to link to it.

---

_Comment by @lilydjwg on 2020-01-28 13:10_

Yes, I'd like to, but please note that it's new and may have bugs.

---
