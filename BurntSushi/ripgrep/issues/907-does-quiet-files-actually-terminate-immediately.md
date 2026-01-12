```yaml
number: 907
title: Does --quiet --files actually terminate immediately?
type: issue
state: closed
author: roblourens
labels:
  - enhancement
  - doc
assignees: []
created_at: 2018-05-03T03:25:57Z
updated_at: 2018-07-22T15:05:58Z
url: https://github.com/BurntSushi/ripgrep/issues/907
synced_at: 2026-01-12T16:13:22Z
```

# Does --quiet --files actually terminate immediately?

---

_@roblourens_

VS Code is using a command like `rg --quiet --files -g '**/package.json'` to test whether a workspace contains a certain type of file. Investigating https://github.com/Microsoft/vscode/issues/48599, it seems like ripgrep doesn't actually terminate early when the --quiet flag is used.

One way I'm testing this, in the ripgrep repo:

```

$ rg --quiet --files --debug 2> quiet.txt
$ rg --files --debug 2> notquiet.txt
$ ls -l quiet.txt
-rw-r--r--  1 roblou  staff  18067 May  2 20:22 quiet.txt
$ ls -l notquiet.txt
-rw-r--r--  1 roblou  staff  18067 May  2 20:22 notquiet.txt
```

I'd expect the --quiet version to terminate immediately after one file is found, and leave a shorter log behind.

In the original issue, rg runs for a long time despite the debug log showing that it saw a package.json file very early.

---

_Comment by @BurntSushi on 2018-05-03 10:38_

The docs for `--quiet` say that if a match is found in a file, then ripgrep will stop searching. But if no match is found, then it continues. So with respect to the docs, the behavior you're inquiring about is unspecified, since the `--files` flag doesn't actually do any searching.

With that said, it seems like this is definitely a reasonable interpretation and use of `--files`. :-)

---

_Label `enhancement` added by @BurntSushi on 2018-05-03 10:38_

---

_Label `doc` added by @BurntSushi on 2018-05-03 10:38_

---

_Closed by @BurntSushi on 2018-07-22 15:05_

---
