```yaml
number: 1939
title: Default value documentation for --field-match-separator should be colon instead of hyphen
type: issue
state: closed
author: learnbyexample
labels: []
assignees: []
created_at: 2021-07-19T08:25:31Z
updated_at: 2021-07-19T12:08:58Z
url: https://github.com/BurntSushi/ripgrep/issues/1939
synced_at: 2026-01-12T16:13:24Z
```

# Default value documentation for --field-match-separator should be colon instead of hyphen

---

_@learnbyexample_

From `man rg`

>        --field-match-separator SEPARATOR
>            Set the field match separator, which is used to delimit file paths,
>            line numbers, columns and the match itself. The separator may be any
>            number of bytes, including zero. Escape sequences like \x7F or \t may
>            be used. The default value is -.

`The default value is -.` should be `The default value is :.`

```bash
$ seq 3 | rg -n '2'
2:2
$ seq 3 | rg -n '2' --field-match-separator=' '
2 2
```

Also, perhaps the `.` punctuation at the end could be removed? Or may be something like `The : character is the default value.` This formatting would apply for `--field-context-separator` description as well.

---

_Closed by @BurntSushi on 2021-07-19 12:08_

---
