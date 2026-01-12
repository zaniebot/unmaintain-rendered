```yaml
number: 217
title: "Makefile type list is missing \"*.mak\""
type: issue
state: closed
author: MiguelLatorre
labels:
  - enhancement
assignees: []
created_at: 2016-11-03T21:17:40Z
updated_at: 2016-11-04T00:46:29Z
url: https://github.com/BurntSushi/ripgrep/issues/217
synced_at: 2026-01-12T18:23:11Z
```

# Makefile type list is missing "*.mak"

---

_@MiguelLatorre_

Comparing several _greppers_:

rg --type-list:
`make: *.mk, Gnumakefile, Makefile, gnumakefile, makefile`

ack --help-types
`--[no]make         .mk; .mak; makefile; Makefile; Makefile.Debug; Makefile.Release`

sift --list-types
`make           :Makefile *.mak`

I am missing the ***.mak** pattern.

By the way, this tool is awesome.
Good work. Keep it up!

---

_Comment by @BurntSushi on 2016-11-03 21:49_

If you open a PR and modify [this line](https://github.com/BurntSushi/ripgrep/blob/master/ignore/src/types.rs#L126) then I'd be happy to merge it.

If not, that's OK, and I'll do it next time I think of it!


---

_Label `enhancement` added by @BurntSushi on 2016-11-03 21:49_

---

_Comment by @theamazingfedex on 2016-11-03 21:50_

I was making PRs for filetypes, and saw this issue, so I went ahead and made a PR for this change: #220 


---

_Comment by @BurntSushi on 2016-11-03 21:50_

@theamazingfedex Thanks!


---

_Comment by @MiguelLatorre on 2016-11-03 21:56_

Wow. Perfect timming. Big thanks to both of you!


---

_Closed by @BurntSushi on 2016-11-04 00:46_

---
