```yaml
number: 1181
title: Add .dtx and .ins for -type tex
type: issue
state: closed
author: steffenbanhardt
labels:
  - enhancement
assignees: []
created_at: 2019-01-31T09:34:30Z
updated_at: 2019-01-31T11:49:52Z
url: https://github.com/BurntSushi/ripgrep/issues/1181
synced_at: 2026-01-12T16:13:23Z
```

# Add .dtx and .ins for -type tex

---

_@steffenbanhardt_

#### What version of ripgrep are you using?

ripgrep 0.10.0 (rev 8a7db1a918)

#### Describe your feature request

The --type tex should include documented LaTeX sources (.dtx) and their .ins files

diff for types.rs:

    @@ -283,7 +283,7 @@
         ]),
         ("taskpaper", &["*.taskpaper"]),
         ("tcl", &["*.tcl"]),
    -    ("tex", &["*.tex", "*.ltx", "*.cls", "*.sty", "*.bib"]),
    +    ("tex", &["*.tex", "*.ltx", "*.cls", "*.sty", "*.bib", "*.dtx", "*.ins"]),
         ("textile", &["*.textile"]),
         ("thrift", &["*.thrift"]),
         ("tf", &["*.tf"]),


---

_Comment by @BurntSushi on 2019-01-31 11:49_

Thanks! I don't keep issues open tracking trivial changes to the file type list. Please just submit a PR instead. :-)

---

_Closed by @BurntSushi on 2019-01-31 11:49_

---

_Label `enhancement` added by @BurntSushi on 2019-01-31 11:49_

---
