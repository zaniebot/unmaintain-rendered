```yaml
number: 581
title: ripgrep may abort unexpectedly when using memory maps
type: issue
state: closed
author: BurntSushi
labels: []
assignees: []
created_at: 2017-08-23T23:07:24Z
updated_at: 2020-12-16T14:24:13Z
url: https://github.com/BurntSushi/ripgrep/issues/581
synced_at: 2026-01-12T16:13:22Z
```

# ripgrep may abort unexpectedly when using memory maps

---

_@BurntSushi_

The issue is that ripgrep *may* use memory maps by default while searching, and memory maps come with the intrinsic downside that if the underlying file is truncated, ripgrep might receive a SIGBUS signal and therefore terminate. This has two possible negative consequences:

1. ripgrep might abort sporadically if it happens to be searching a file that is being truncated by some other process. This is a bug.
2. A malicious user on a multi-user system could truncate a file while ripgrep is running to cause a local denial of service attack. This seems somewhat limited in scope since it would require searching files that another user has write access to, although one good example of where it might happen is if one is running ripgrep from a root account.

My view is that while this is technically a security concern because it could result in a DoS, it seems to me like it's quite mild and that changing ripgrep's default behavior to never use memory maps in turn seems like a pretty extreme reaction to said vulnerability. (The point of using memory maps is that they can be faster than normal `read` calls in some circumstances.) In particular, it is trivial for an end user to protect themselves from this bug/DoS by passing the `--no-mmap` flag. If we don't change the default, then it seems to me like a good alternative solution is to **document this flaw in the man page and the help output.**

There has also been a claim that this could prevent some Linux distros from shipping ripgrep with this particular default. Some Linux distros are already shipping ripgrep with this default, so the concern seems a bit exaggerated here, but if a distro was so inclined I would be willing to accept a small patch that added a Cargo feature to disable memory map searching by default (but kept memory map functionality available via the opt-in `--mmap` flag). Distros could then enable this feature when building ripgrep and opt out of any undesirable default behavior. However, I personally don't care enough to implement this functionality since I currently perceive it as quite niche. I would rather wait for someone else to convince me that a significant number of people care.

See #579 for related discussion.

---

_Comment by @okdana on 2017-08-23 23:26_

As a point of reference, `ag` uses `mmap()` by default (on Linux), and neither Debian nor Ubuntu disable that behaviour in their official packages. I don't have much recent experience with other distributions, but it doesn't look like the official Fedora package disables it either.

---

_Closed by @BurntSushi on 2017-08-23 23:55_

---

_Comment by @shachaf on 2020-12-16 13:26_

Instead of terminating unexpectedly, it should be possible to handle SIGBUS, print an error, and keep searching other files. Was that considered?

---

_Comment by @BurntSushi on 2020-12-16 14:24_

@shachaf I considered it and dismissed it due to implementation complexity. As I said above (and in the linked issue), this problem rarely occurs in practice, has fairly nominal repercussions and can be avoided entirely with the `--no-mmap` flag.

---
