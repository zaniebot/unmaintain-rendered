```yaml
number: 279
title: "ripgrep never terminates when passing `-q` or `--quiet` flags"
type: issue
state: closed
author: lilianmoraru
labels: []
assignees: []
created_at: 2016-12-12T09:43:08Z
updated_at: 2016-12-12T11:56:57Z
url: https://github.com/BurntSushi/ripgrep/issues/279
synced_at: 2026-01-12T16:13:21Z
```

# ripgrep never terminates when passing `-q` or `--quiet` flags

---

_@lilianmoraru_

`rustc 1.15.0-nightly (daf8c1dfc 2016-12-05)`
`ripgrep 0.3.2`

ripgrep never terminates when passing `-q` or `--quiet` flags.
Here is an example run(I added `--debug` later):
```
rg --debug --quiet "GCC"
DEBUG:grep::search: regex ast:
Literal {
    chars: [
        'G',
        'C',
        'C'
    ],
    casei: false
}
DEBUG:grep::literals: literal prefixes detected: Literals { lits: [Complete(GCC)], limit_size: 250, limit_class: 10 }
```

---

_Closed by @BurntSushi on 2016-12-12 11:55_

---

_Comment by @BurntSushi on 2016-12-12 11:56_

Wow. Can't believe this one slipped through. This required there to be at least one match. There were tests for `-q`, but none of them involved finding matches. Thanks for reporting!

---
