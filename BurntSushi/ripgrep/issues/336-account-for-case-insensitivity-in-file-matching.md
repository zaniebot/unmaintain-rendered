```yaml
number: 336
title: Account for case-insensitivity in file matching
type: issue
state: closed
author: thijsvandien
labels:
  - duplicate
assignees: []
created_at: 2017-01-20T03:23:08Z
updated_at: 2017-01-20T12:31:14Z
url: https://github.com/BurntSushi/ripgrep/issues/336
synced_at: 2026-01-12T16:13:21Z
```

# Account for case-insensitivity in file matching

---

_@thijsvandien_

I'm on a Mac that has a case-insensitive file system (and so does Windows). When I have a folder containing `a.txt` and `b.TXT`, both `rg --ttxt --files` and `rg -g '*.txt' --files` list only `a.txt`. 

A user on such system would expect both filenames to be matched, so that's what I would vouch for. Perhaps, in addition there could be a flag to either force case-sensitive or case-insensitive filename matching. 

If detecting case-sensitivity of the filesystem is terribly inconvenient to do—a search may span multiple volumes with different case-sensitivity!—I suppose case-sensitive should remain the default always, and case-insensitivity could optionally be enabled with a flag (to become a very popular one on those platforms then).

For a point or reference, Python 3's `glob` module respects the filesystem. It's possible that this is an issue to address in the library used for globbing rather than this consuming project. 

---

_Comment by @thijsvandien on 2017-01-20 03:37_

Excuse me, I missed https://github.com/BurntSushi/ripgrep/issues/163.

---

_Comment by @BurntSushi on 2017-01-20 12:31_

Yup, this is a dupe. Thanks for contributing to the discussion. :-)

(In case it isn't clear, I'm fully supportive of taking action on this issue. The UX just isn't quite clear to me...)

---

_Closed by @BurntSushi on 2017-01-20 12:31_

---

_Label `duplicate` added by @BurntSushi on 2017-01-20 12:31_

---
