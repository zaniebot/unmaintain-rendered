```yaml
number: 841
title: Include info on how to get the man page
type: issue
state: closed
author: ferhtgoldaraz
labels:
  - invalid
  - doc
assignees: []
created_at: 2018-02-28T18:30:33Z
updated_at: 2018-02-28T19:05:41Z
url: https://github.com/BurntSushi/ripgrep/issues/841
synced_at: 2026-01-12T16:13:22Z
```

# Include info on how to get the man page

---

_@ferhtgoldaraz_

Info on how to build the man page would be a good improvement over the already very complete readme. Maybe also mention the install methods that include it (for example the .deb doesn't seem to install it).

I use this command in mac (installed through brew) and the man page is a godsend.

Once I figure that out maybe I'll make a pull request :)

---

_Comment by @BurntSushi on 2018-02-28 18:33_

I think this [FAQ item](https://github.com/BurntSushi/ripgrep/blob/master/FAQ.md#manpage) might answer most of your question?

The deb really *should* include the man page. In fact, *every* distro package should include the man page. The deb is under my control, but truthfully, I generate it via the `cargo deb` tool, and I'm not sure anyone has taught it about man pages yet. I don't have the bandwidth to wade into Debian packaging territory, so it's basically up to someone else at this point.

---

_Comment by @BurntSushi on 2018-02-28 18:33_

(Also, `rg --help` is very nearly the same as the contents of the man page. At least, all of the flags are documented using precisely the same text. The man page does contain some additional prose for other sections like config knobs, shell completions and the like. Also, `rg -h` shows a much more condensed view.)

---

_Comment by @ferhtgoldaraz on 2018-02-28 18:46_

Perfect, I'm in the process of building from source now. I imagine we can close this.

---

_Closed by @ferhtgoldaraz on 2018-02-28 18:46_

---

_Comment by @ferhtgoldaraz on 2018-02-28 18:48_

(maybe open some issue for the deb and a help wanted tag or somethting, I'm not even a newbie regarding deb packaging)

---

_Comment by @BurntSushi on 2018-02-28 19:05_

@ferhtgoldaraz Great! Good idea. Done: https://github.com/BurntSushi/ripgrep/issues/842

---

_Label `invalid` added by @BurntSushi on 2018-02-28 19:05_

---

_Label `doc` added by @BurntSushi on 2018-02-28 19:05_

---
