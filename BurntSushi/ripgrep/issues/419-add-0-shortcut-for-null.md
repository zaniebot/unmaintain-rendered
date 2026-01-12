```yaml
number: 419
title: Add -0 shortcut for --null
type: issue
state: closed
author: caedn
labels:
  - enhancement
  - help wanted
  - question
assignees: []
created_at: 2017-03-26T20:11:29Z
updated_at: 2017-03-28T22:37:41Z
url: https://github.com/BurntSushi/ripgrep/issues/419
synced_at: 2026-01-12T16:13:22Z
```

# Add -0 shortcut for --null

---

_@caedn_

I've use ag with `-0` a lot to safely pipe it into xargs. Any chance you could add the same alias so I don't have to type as much when switching to rg?

---

_Comment by @BurntSushi on 2017-03-26 20:26_

I'd be willing to add a `-Z` short flag for `--null`, since that's what grep does. (ripgrep obviously takes a certain amount of inspiration from ag, but when there's a choice to be made with respect to flags, I've tended to side with GNU grep because of its ubiquity.)

---

_Label `enhancement` added by @BurntSushi on 2017-03-26 20:26_

---

_Label `help wanted` added by @BurntSushi on 2017-03-26 20:26_

---

_Label `question` added by @BurntSushi on 2017-03-26 20:26_

---

_Comment by @caedn on 2017-03-27 08:14_

That makes sense for GNU grep. Do note that BSD grep (which ships with macOS) behaves differently:

    -Z, -z, --decompress
             Force grep to behave as zgrep.

Neither grep uses `-0`. I'm fine with `-Z` too though.

---

_Comment by @BurntSushi on 2017-03-27 11:04_

Yuck. I'd like ripgrep to support decompression some day. I still lean towards `-Z`... However, `-0` also has precedent, e.g., with `xargs`.

---

_Closed by @BurntSushi on 2017-03-28 22:37_

---
