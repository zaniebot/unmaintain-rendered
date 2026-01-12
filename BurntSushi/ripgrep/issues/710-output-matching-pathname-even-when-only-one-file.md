```yaml
number: 710
title: Output matching pathname even when only one file is searched
type: issue
state: closed
author: rswgnu
labels:
  - question
  - wontfix
assignees: []
created_at: 2017-12-07T17:53:03Z
updated_at: 2017-12-07T18:09:29Z
url: https://github.com/BurntSushi/ripgrep/issues/710
synced_at: 2026-01-12T16:13:22Z
```

# Output matching pathname even when only one file is searched

---

_@rswgnu_

Please make 'rg' output a filename by default if matches are found when searching only one file.  This is important because if an IDE tool uses the output to jump to the associated source line, it will need the filename to do so, just as it does when multiple files are searched and matched.


---

_Comment by @BurntSushi on 2017-12-07 18:03_

This default isn't changing. This matches `grep` behavior, which is the intended behavior in this case. If you want the filename to always be emitted, then you need to pass the `--with-filename` flag (which also exists for grep).

---

_Closed by @BurntSushi on 2017-12-07 18:03_

---

_Label `question` added by @BurntSushi on 2017-12-07 18:03_

---

_Label `wontfix` added by @BurntSushi on 2017-12-07 18:03_

---

_Comment by @BurntSushi on 2017-12-07 18:04_

(Note that the `--vimgrep` option will also achieve this effect, which also includes line numbers.)

---

_Comment by @rswgnu on 2017-12-07 18:09_

On Thu, Dec 7, 2017 at 1:03 PM, Andrew Gallant <notifications@github.com>
wrote:

> This default isn't changing. This matches grep behavior, which is the
> intended behavior in this case. If you want the filename to always be
> emitted, then you need to pass the --with-filename flag (which also
> exists for grep).
>
​Thanks, that will work well.


---
