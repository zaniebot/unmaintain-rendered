```yaml
number: 321
title: Switch to avoid crossing filesystem boundaries
type: issue
state: closed
author: wavexx
labels:
  - enhancement
  - icebox
assignees: []
created_at: 2017-01-14T19:41:10Z
updated_at: 2018-12-03T14:23:18Z
url: https://github.com/BurntSushi/ripgrep/issues/321
synced_at: 2026-01-12T16:13:21Z
```

# Switch to avoid crossing filesystem boundaries

---

_@wavexx_

It's often useful to search only files that reside on the same filesystem as the search paths, same as ``find -xdev`` or ``ag --one-device``. I find ag's "one-device" switch to be misleading, as it's more correctly described by find as "do not cross filesystem boundaries", since the restriction applies to each search path in turn (which can span more than *one* device).

---

_Comment by @BurntSushi on 2017-01-18 00:44_

This needs to be implemented in `walkdir` (see: https://github.com/BurntSushi/walkdir/issues/8) and `ignore`'s parallel iterator, and then exposed up through ripgrep as a command line flag.

I agree with the naming choice. Using `find`'s flag name seems fine. It looks like GNU grep doesn't have a similar flag?

---

_Label `enhancement` added by @BurntSushi on 2017-01-18 00:44_

---

_Comment by @wavexx on 2017-01-18 11:23_

On Wed, Jan 18 2017, Andrew Gallant wrote:
> I agree with the naming choice. Using find's flag name seems fine. It looks like GNU grep doesn't have
> a similar flag?

Indeed it doesn't, which is a shame.

If you're working with FUSE and/or remote filesystems, -xdev is very
useful (either to avoid crossing, and/or to escape the local fs).


---

_Label `icebox` added by @BurntSushi on 2017-04-09 13:09_

---

_Comment by @BurntSushi on 2018-08-25 22:15_

I think I'm liking the name `--mount` for this. It's supported by `find`, is relatively short, and feels a bit more generic than `--xdev`.

Does anyone know the provenance of the name `--xdev`? I guess "dev" means "device" (which seems consistent with the `dev` attribute in the `stat` structure) and I guess `x` means "cross" or "don't cross" in this case.

---

_Comment by @ssokolow on 2018-08-25 23:12_

> Does anyone know the provenance of the name --xdev? I guess "dev" means "device" (which seems consistent with the dev attribute in the stat structure) and I guess x means "cross" or "don't cross" in this case.

I suspect you're correct there, given that tools like `ncdu` use the short option `-x` and describe it as follows:

> `-x`  Do not cross filesystem boundaries, i.e. only count files and directories on the same filesystem as the directory being scanned.

Also, I'm not so sure about `--mount`. It doesn't feel at all intuitive to me, which is probably why the manpage for GNU find lists `-mount` it as being a compatibility alias for `-xdev` rather than the other way around.


---

_Comment by @okdana on 2018-08-25 23:24_

>Does anyone know the provenance of the name --xdev?

In 2.11BSD `find` uses a global variable called `Xdev` that is commented as *true if SHOULD cross devices*; the `-xdev` 'primary' flips it off. Probably the latter is named after the former? Seems like `-noxdev` or something would have made more sense; not that `-mount` is especially better

>I suspect you're correct there, given that tools like ncdu use the short option -x

I think `du -x` comes from BSD's `find -x`, which was a later alias/replacement for `-xdev`. GNU `du` gave `-x` the long option name `--one-file-system`, which has also been borrowed into GNU `cp`, GNU `rm`, GNU `tar`, BSD `tar`, and `rsync` (... and `ncdu`, apparently)

---

_Comment by @BurntSushi on 2018-08-25 23:27_

@okdana Ah nice, thanks! The prevalence of `--one-file-system` makes a compelling argument for that name. I guess we should probably go with that? It's also seemingly the most accurate. The only downside is that it's kind of long.

---

_Comment by @ssokolow on 2018-08-25 23:28_

Another thing which just occurred to me is that I like the `x`-based names (`-x`, `-xdev`, etc.) because, in addition to "crossing device boundaries", they also associate it with options like `--exclude` in my mind, which is technically accurate. (You're eXcluding stuff outside the current filesystem.)

---

_Closed by @BurntSushi on 2018-08-26 22:42_

---

_Comment by @wavexx on 2018-12-03 14:23_

I know it's late to comment on this, but if I explicitly pass two directories on different devices to perform searches on:

``$ rg pat /dev1/ /dev2/``

I do expect rg to scan/descend on both, as these were given explicitly. In this sense, I really preferred the original term "same device" or "do not cross boundaries" (aka xdev) more than 'ag --one-device' as I originally pointed out, as "one" in this context is just a special case.

This is just for the sake of discussion, I know the name has already been settled.

---
