```yaml
number: 97
title: "Add option '-l' to only print out the file names from the matched result"
type: issue
state: closed
author: nextlogic-ono
labels: []
assignees: []
created_at: 2016-09-26T03:33:50Z
updated_at: 2016-09-26T03:40:43Z
url: https://github.com/BurntSushi/ripgrep/issues/97
synced_at: 2026-01-12T18:23:11Z
```

# Add option '-l' to only print out the file names from the matched result

---

_@nextlogic-ono_

For grep and the likes including ack, they have an option that only prints the file names from the matched results.

> grep
>           -l, --files-with-matches
>               Suppress  normal  output;  instead  print  the  name  of each input file from which output would normally have been
>               printed.  The scanning will stop on the first match.
> 
> ack
>        -l, --files-with-matches
>            Only print the filenames of matching files, instead of the matching text.

This is useful for example when opening the target files in vim directly from the result, as in;

> vim -p `grep -l foo *`


---

_Comment by @BurntSushi on 2016-09-26 03:40_

This is done and should be in `0.2.0`. Please update. :-)


---

_Closed by @BurntSushi on 2016-09-26 03:40_

---
