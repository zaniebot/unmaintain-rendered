```yaml
number: 1000
title: Release a new version
type: issue
state: closed
author: infinity0
labels:
  - question
assignees: []
created_at: 2018-08-03T10:48:59Z
updated_at: 2018-08-05T11:51:52Z
url: https://github.com/BurntSushi/ripgrep/issues/1000
synced_at: 2026-01-12T16:13:22Z
```

# Release a new version

---

_@infinity0_

Hi, please release a new version 0.8.2 of ripgrep.

We've packaged [all of the dependencies in Debian](https://qa.debian.org/developer.php?login=pkg-rust-maintainers@alioth-lists.debian.net) now and are ready to upload ripgrep. However the latest release 0.8.1 still uses old versions of some dependencies. I see git master has updated, so it would be nice to publish to crates.io so we can upload ripgrep to Debian so `apt-get install ripgrep` works.

---

_Comment by @BurntSushi on 2018-08-03 12:23_

> so we can upload ripgrep to Debian so `apt-get install ripgrep` works.

Wow! That's awesome news! I'm super appreciative of all the hard work that must have gone into that. :-)

With that said, I don't think a 0.8.2 release is practical. Too much has has been added from 0.8.1, including an increase in the minimum supported version of Rust from 1.20 to 1.23. Are you OK with a 0.9.0 release instead?

---

_Label `question` added by @BurntSushi on 2018-08-03 12:24_

---

_Comment by @kpcyrd on 2018-08-03 12:42_

A 0.9.0 release is fine, right now we have rustc 1.27 in sid, it's only the dependencies that need a bump. :)

---

_Comment by @BurntSushi on 2018-08-03 12:44_

Sounds great. 1.27 is especially good news, because that means ripgrep will automatically get (some) SIMD optimizations. I will start the release process soon.

---

_Comment by @BurntSushi on 2018-08-03 21:10_

Done: https://github.com/BurntSushi/ripgrep/releases/tag/0.9.0

---

_Closed by @BurntSushi on 2018-08-03 21:10_

---

_Comment by @infinity0 on 2018-08-04 08:21_

Could you publish to crates.io as well? Our automated scripts take the tarball from there and not github.

---

_Comment by @BurntSushi on 2018-08-04 11:33_

Ah right, all set!

---

_Comment by @infinity0 on 2018-08-05 04:21_

Thanks! ripgrep is [now in Debian](https://packages.debian.org/sid/ripgrep)

---

_Comment by @BurntSushi on 2018-08-05 11:51_

w00t! Thank you!

---
