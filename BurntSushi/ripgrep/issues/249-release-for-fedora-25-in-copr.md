```yaml
number: 249
title: Release for fedora 25 in copr
type: issue
state: closed
author: SanketDG
labels:
  - question
assignees: []
created_at: 2016-11-25T08:41:02Z
updated_at: 2016-11-27T17:16:44Z
url: https://github.com/BurntSushi/ripgrep/issues/249
synced_at: 2026-01-12T18:23:11Z
```

# Release for fedora 25 in copr

---

_@SanketDG_

The [copr](https://copr.fedorainfracloud.org/coprs/carlgeorge/ripgrep/) is not enabled for the newly released fedora 25!

---

_Comment by @BurntSushi on 2016-11-25 13:02_

I don't know what the copr is and I am sure there is nothing I can do to fix it. I think you need to get in touch with the maintainer of the package. I think that might be @carlwgeorge.

---

_Comment by @carlwgeorge on 2016-11-25 15:02_

Yes, that's my copr.  Copr is a build service provided by the Fedora project.  It is similar to Ubuntu PPA's.  It is just a temporary repo until I can get ripgrep into the main Fedora/EPEL repos.  The [cargo release](https://bodhi.fedoraproject.org/updates/FEDORA-2016-2ca07f1b54) for F25 has not yet been pushed to stable.  Once it is I can enable F25 in the ripgrep copr.

---

_Label `question` added by @BurntSushi on 2016-11-27 00:18_

---

_Comment by @carlwgeorge on 2016-11-27 04:08_

I figured out how to temporarily enable the Fedora updates-testing repository in the copr, so it can now be used with Fedora 25.  This issue can be closed.

---

_Comment by @SanketDG on 2016-11-27 17:16_

Thanks, I just installed `ripgrep` on my Fedora 25. :fire: :boom: 

---

_Closed by @SanketDG on 2016-11-27 17:16_

---
