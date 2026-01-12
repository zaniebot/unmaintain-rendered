```yaml
number: 167
title: Default for -j is not clear
type: issue
state: closed
author: durka
labels:
  - doc
assignees: []
created_at: 2016-10-11T17:33:22Z
updated_at: 2016-10-11T23:27:11Z
url: https://github.com/BurntSushi/ripgrep/issues/167
synced_at: 2026-01-12T18:23:11Z
```

# Default for -j is not clear

---

_@durka_

`rg --help` says this:

```
    -j, --threads ARG
        The number of threads to use. Defaults to the number of logical CPUs
        (capped at 6). [default: 0]
```

I have four cores here. Given this documentation I have no idea what the default parallelishness is :)


---

_Comment by @BurntSushi on 2016-10-11 18:10_

I guess it's confusing because of `[default: 0]`?

I guess it should say, "`0` means use the number of logical CPUs, capped at 6."


---

_Label `doc` added by @BurntSushi on 2016-10-11 18:10_

---

_Comment by @durka on 2016-10-11 18:15_

That would be much more clear!


---

_Comment by @durka on 2016-10-11 21:23_

I can make a PR. Are the man pages auto generated somehow or should I make the same edits there?


---

_Comment by @BurntSushi on 2016-10-11 21:24_

@durka Thanks! They aren't auto-generated unfortunately. You'll need to edit both `src/args.rs` and `doc/rg.1.md`, and then run `cd doc && ./convert-to-man`.


---

_Closed by @BurntSushi on 2016-10-11 23:27_

---
