```yaml
number: 1387
title: "Ripgrep's char ordering relation used for `--sort` should be case-insensitive by default"
type: issue
state: closed
author: Boscop
labels:
  - wontfix
assignees: []
created_at: 2019-09-24T09:58:25Z
updated_at: 2019-10-22T12:19:10Z
url: https://github.com/BurntSushi/ripgrep/issues/1387
synced_at: 2026-01-12T16:13:23Z
```

# Ripgrep's char ordering relation used for `--sort` should be case-insensitive by default

---

_@Boscop_

#### What version of ripgrep are you using?
11.0.2

#### How did you install ripgrep?
cargo install

#### What operating system are you using ripgrep on?
openSUSE

#### Describe your question, feature request, or bug.

Ripgrep `--sort path` seems to order uppercase chars before lowercase chars, e.g. I was running `rg -l -i --sort path 'some pattern' > results.txt` and the file contained `ClassMappings.sql` before `Classifications.sql` even though `M` comes after `i` in the alphabet (I even passed `-i` but I guess that's only for the actual text matching). 
(Yes, I know, in ascii it's `ord('M') < ord('i')`, but not in the human alphabet.)

Ripgrep's char ordering relation should have `i` before `M`, unless otherwise specified with a flag.
Just like the unix `sort` command does (e.g. when I pipe ripgrep's output through `sort`).


---

_Comment by @BurntSushi on 2019-09-24 10:25_

> Just like the unix `sort` command does

That isn't quite right. `sort` only does this when you have your locale set properly. When using the `C` locale, it sorts just like ripgrep:

```
$ cat /tmp/ripgrep-1387
M
i

$ LC_ALL=en_US.UTF-8 sort /tmp/ripgrep-1387
i
M

$ LC_ALL=C sort /tmp/ripgrep-1387
M
i
```

ripgrep does not respect your locale settings and likely never will. Sorry.

---

_Closed by @BurntSushi on 2019-09-24 10:25_

---

_Label `wontfix` added by @BurntSushi on 2019-09-24 10:25_

---

_Comment by @Boscop on 2019-09-24 10:52_

It doesn't have to read the locale, but it'd be nice to have the option to sort by filenames case-insensitively (ignoring the locale).
Or do you know another way how I can sort ripgrep's output by filenames case-insensitively, but keeping the output lines associated with each file ordered by original line number? 
(When I pipe through `sort` and I have multiple matches per file, it changes the order of the lines for each file.)
Don't you think this is a desirable feature to have this kind of output?

---

_Comment by @Simran-B on 2019-10-22 12:19_

Couldn't additional parameters `--sorti` and `sortri` be added? Or another parameter value `path_ci` (or similar)?

---
