```yaml
number: 3083
title: "There's a broken link in the README"
type: issue
state: closed
author: crazyczy
labels: []
assignees: []
created_at: 2025-07-02T12:30:43Z
updated_at: 2025-09-23T01:17:34Z
url: https://github.com/BurntSushi/ripgrep/issues/3083
synced_at: 2026-01-12T16:13:25Z
```

# There's a broken link in the README

---

_@crazyczy_

While installing ripgrep, I noticed a typo in a hyperlink within the documentation. I traced it back to PR #3058 and will submit a fix shortly.

---

_Comment by @Liu-Eroteme on 2025-07-04 11:44_

Just ran into that myself!

```If you're a Debian user (or a user of a Debian derivative like Ubuntu), then ripgrep can be installed using a binary .deb file provided in each [ripgrep release](https://github.com/BurntSushi/ripgrep/releases).

$ curl -LO https://github.com/BurntSushi/ripgrep/releases/download/14.1.0/ripgrep_14.1.1-1_amd64.deb
$ sudo dpkg -i ripgrep_14.1.1-1_amd64.deb
```

It's

`https://github.com/BurntSushi/ripgrep/releases/download/14.1.1/ripgrep_14.1.1-1_amd64.deb`

not

`https://github.com/BurntSushi/ripgrep/releases/download/14.1.0/ripgrep_14.1.1-1_amd64.deb`

---

_Comment by @Propfend on 2025-08-12 18:46_

The documentation uses the `ripgrep_14.1.1-1_amd64.deb` file from `https://github.com/BurntSushi/ripgrep/releases/download/14.1.0/ripgrep_14.1.1-1_amd64.deb` URL. But when we try to run the command `sudo dpkg -i ripgrep_14.1.1-1_amd64.deb` to install it from `.deb` file we run  into error:
```bash
dpkg-deb: error: unexpected end of file in archive magic version number in ripgrep_14.1.1-1_amd64.deb
dpkg: error processing archive ripgrep_14.1.1-1_amd64.deb (--install):
 dpkg-deb --control subprocess returned error exit status 2
Errors were encountered while processing:
 ripgrep_14.1.1-1_amd64.deb
```
That is because the second command was not updated. I created a [PR](https://github.com/BurntSushi/ripgrep/pull/3123) to fix that.

---

_Comment by @BurntSushi on 2025-09-23 01:17_

Fixed by https://github.com/BurntSushi/ripgrep/pull/3058

---

_Closed by @BurntSushi on 2025-09-23 01:17_

---
