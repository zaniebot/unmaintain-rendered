```yaml
number: 580
title: missing example of many positional arguments
type: issue
state: closed
author: ghost
labels: []
assignees: []
created_at: 2016-07-14T13:40:46Z
updated_at: 2018-08-02T03:29:51Z
url: https://github.com/clap-rs/clap/issues/580
synced_at: 2026-01-12T16:14:09Z
```

# missing example of many positional arguments

---

_@ghost_

i cant find example handling many positional arguments, like

```
rm file1 file2 file3 file4 (and so on)
```

is it possible with clap-rs now?


---

_Comment by @clux on 2016-07-14 14:26_

Yes, should be just `Arg::with_name("files").multiple(true))` on your `rm` app/subcommand.

There are some examples under the arg documentation at http://kbknapp.github.io/clap-rs/clap/struct.Arg.html#examples-19


---

_Comment by @ghost on 2016-07-15 13:31_

Yup, it works. :)
I missed it because i searched it here: https://github.com/kbknapp/clap-rs/tree/96869dfd5772639182048d5f020437303f2e40ac/examples
However your link:
http://kbknapp.github.io/clap-rs/clap/struct.Arg.html#examples-19
clearifies that, thanks.


---

_Closed by @ghost on 2016-07-15 13:31_

---
