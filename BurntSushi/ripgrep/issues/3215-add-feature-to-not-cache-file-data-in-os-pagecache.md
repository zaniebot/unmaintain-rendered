```yaml
number: 3215
title: Add feature to not cache file data in OS pagecache
type: issue
state: closed
author: mckellygit
labels:
  - wontfix
assignees: []
created_at: 2025-11-01T15:17:38Z
updated_at: 2025-11-01T15:24:00Z
url: https://github.com/BurntSushi/ripgrep/issues/3215
synced_at: 2026-01-12T16:13:25Z
```

# Add feature to not cache file data in OS pagecache

---

_@mckellygit_

Hi,

Thank you for awesome rg.  I have done this with my own ag and grep, but to have it in rg would be awesome.  I will try to find out how in rust to do it myself soon, but will request the feature here.  I didn't see this feature in rg currently.

Basically its to add -

posix_fadvise(fd, 0, 0, POSIX_FADV_DONTNEED);

after each read() or at least after some ok amount of reads (such as 64k bytes etc)

And could also add -

posix_fadvise(fd, 0, 0, POSIX_FADV_RANDOM);

after opening a file, before any reads.

I dont even know if rg uses mmap() instead of read(), so perhaps this isn't quite applicable and please forgive me.

This could be added as an option (--no-pgcache or some such)

But the point here for my use is to be able to rg some really large (log) files on live production systems and not have that push out application data from the OS pagecache, which can affect application performance.

NB there are other ways, such as opening files with O_DIRECT or using preadv2 and RWF_UNCACHED, but those are more involved methods and the above works well enough.

I suppose some might say just export the log files to another system and rg them there to avoid disturbing production, and they would be right, but still to have this option would be for me very helpful for those crisis type sessions.

thx,
-m


---

_Comment by @BurntSushi on 2025-11-01 15:22_

I don't think this is a good fit for ripgrep. It seems extremely niche and it isn't really something I want to maintain. It would be pretty easy for you, I think, to maintain a fork of ripgrep that does this though.

---

_Closed by @BurntSushi on 2025-11-01 15:22_

---

_Label `wontfix` added by @BurntSushi on 2025-11-01 15:23_

---

_Comment by @mckellygit on 2025-11-01 15:24_

ok.  Thanks again for rg.
-m

---
