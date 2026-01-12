```yaml
number: 898
title: add support for lz4 decoding
type: pull_request
state: closed
author: njaard
labels: []
assignees: []
base: master
head: master
created_at: 2018-04-27T09:39:42Z
updated_at: 2018-07-24T16:59:14Z
url: https://github.com/BurntSushi/ripgrep/pull/898
synced_at: 2026-01-12T18:23:13Z
```

# add support for lz4 decoding

---

_@njaard_

Requires the `lz4` binary which I published in
https://crates.io/crates/lz4util. It has semantics
similar to `gzip`.

Add a test for lz4 files.

Update the readme

---

_Comment by @BurntSushi on 2018-04-27 11:26_

I'm not sure I follow the choices made in this PR. It seems like lz4 already has a standard command line tool: https://github.com/lz4/lz4 It is even already installed on my system as a core package (Archlinux). If we are going to add support for it, then it should be against the standard binary and not a binary released on crates.io yesterday.

---

_Comment by @njaard on 2018-04-27 11:29_

Since the `lz4` command that you cited is compatible with my lz4 (for decompression purposes) due to `gzip` compatibility, I can simply remove the reference to my crate in the manual. Is that acceptable?

---

_Comment by @BurntSushi on 2018-04-27 11:33_

@njaard OK, yes, that's fine. It wasn't clear from your PR whether your binary was compatible with the standard one. But yes, might as well remove the crate reference, which I think would be confusing.

Also, please install the standard lz4 binary in CI so that the test for it runs.

---

_@BurntSushi reviewed on 2018-04-27 11:34_

---

_Review comment by @BurntSushi on `README.md`:109 on 2018-04-27 11:34_

Please follow the prevailing format of this file and wrap lines to 79 columns inclusive.

---

_Comment by @njaard on 2018-04-27 11:52_

* I removed the reference to my crate
* The line wraps to 79 characters, consequently
* added `liblz4-tool` as a dependency for travis
* found a few more instances to document lz4 support

---

_Review comment by @mqudsi on `src/app.rs`:1439 on 2018-04-29 17:02_

Can we sort this list alphabetically, just like it is in `FAQ.md`?

(Sorry for being so OCD!)

---

_@mqudsi reviewed on 2018-04-29 17:02_

---

_@njaard reviewed on 2018-04-29 17:09_

---

_Review comment by @njaard on `src/app.rs`:1439 on 2018-04-29 17:09_

I kept the original ordering because it seemed to be by popularity

---

_Comment by @njaard on 2018-06-08 21:02_

@BurntSushi Just a quick reminder that I believe my pull request has satisfied all of your concerns.

---

_Closed by @BurntSushi on 2018-07-21 20:27_

---

_Comment by @BurntSushi on 2018-07-21 20:28_

@njaard Thanks for working on this! I rebased this on top of master, added a CHANGELOG entry and removed your Oxford commas. :-)

---

_Comment by @njaard on 2018-07-24 16:59_

Great!

Also, they were your own Oxford commas :)

---
