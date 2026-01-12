```yaml
number: 2014
title: "help message: --sort vs --sortr overrides"
type: issue
state: closed
author: hyperknot
labels:
  - invalid
assignees: []
created_at: 2021-10-05T12:51:09Z
updated_at: 2021-10-05T12:57:23Z
url: https://github.com/BurntSushi/ripgrep/issues/2014
synced_at: 2026-01-12T16:13:24Z
```

# help message: --sort vs --sortr overrides

---

_@hyperknot_

#### What version of ripgrep are you using?

ripgrep 12.1.1

#### Describe your bug.

Unrelated to system or anything, just running `--help` has the following:

```
--sort <SORTBY>
    To sort results in reverse or descending order, use the --sortr flag. Also,
    this flag overrides --sortr.

--sortr <SORTBY>
    To sort results in ascending order, use the --sort flag. Also, this flag
    overrides --sort.
```

I believe the `this flag overrides` cannot be true for both.

---

_Comment by @BurntSushi on 2021-10-05 12:57_

It's correct. They override each other. Just like, e.g., `--ignore` and `--no-ignore` or one of a number of other sets of flags.

To demonstrate:

```
$ tree
.
├── a
├── b
└── c

0 directories, 3 files

$ rg --files --sort path
a
b
c

$ rg --files --sort path --sortr path
c
b
a

$ rg --files --sort path --sortr path --sort path
a
b
c

$ rg --files --sort path --sortr path --sort path --sortr path
c
b
a
```


---

_Closed by @BurntSushi on 2021-10-05 12:57_

---

_Label `invalid` added by @BurntSushi on 2021-10-05 12:57_

---
