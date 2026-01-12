```yaml
number: 242
title: "New color handling: switching off colors means bold?"
type: issue
state: closed
author: birkenfeld
labels:
  - bug
assignees: []
created_at: 2016-11-21T19:01:48Z
updated_at: 2016-11-22T01:54:07Z
url: https://github.com/BurntSushi/ripgrep/issues/242
synced_at: 2026-01-12T18:23:11Z
```

# New color handling: switching off colors means bold?

---

_@birkenfeld_

Trying to get rid of the coloring for path and line, only leaving the intra-line matches colored:

```
$ rg  --no-heading --line-number --column --color=always --colors line:none --colors path:none read_to_string | xxd
00000000: 1b5b 6d1b 5b31 6d73 7263 2f63 6f6e 6669  .[m.[1msrc/confi
00000010: 672e 7273 1b5b 6d3a 1b5b 6d1b 5b31 6d34  g.rs.[m:.[m.[1m4
00000020: 341b 5b6d 3a33 353a 2020 2020 2020 2020  4.[m:35:        
00000030: 6673 3a3a 4669 6c65 3a3a 6f70 656e 2866  fs::File::open(f
00000040: 696c 656e 616d 6529 3f2e 1b5b 6d1b 5b33  ilename)?..[m.[3
00000050: 383b 353b 396d 7265 6164 5f74 6f5f 7374  8;5;9mread_to_st
00000060: 7269 6e67 1b5b 6d28 266d 7574 2074 6578  ring.[m(&mut tex
00000070: 7429 3f3b 0a                             t)?;.
```

The `:none` option seems to produce extra `\x1b[m\x1b[1m` before and `\x1b[m` after the text in question. In particular, the former means that the text is shown in bold.

It would be preferable to have no ANSI artifacts in the output at all, when switching color to none.

---

_Comment by @BurntSushi on 2016-11-21 19:12_

Oh interesting. I can reproduce it. `none` should definitely mean that all text styles are removed.

It looks like you don't have a work-around either. e.g., `--colors path:none --colors path:style:nobold` doesn't work.

---

_Label `bug` added by @BurntSushi on 2016-11-21 19:12_

---

_Closed by @BurntSushi on 2016-11-22 01:54_

---
