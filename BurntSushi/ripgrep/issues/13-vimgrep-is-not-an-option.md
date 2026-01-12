```yaml
number: 13
title: "--vimgrep is not an option"
type: issue
state: closed
author: davidosomething
labels:
  - bug
assignees: []
created_at: 2016-09-23T15:05:15Z
updated_at: 2016-09-25T00:51:42Z
url: https://github.com/BurntSushi/ripgrep/issues/13
synced_at: 2026-01-12T18:23:11Z
```

# --vimgrep is not an option

---

_@davidosomething_

_No description provided._

---

_Comment by @BurntSushi on 2016-09-23 15:08_

Could you please show the exact command you're running? And also, the output of `rg --version`?


---

_Comment by @davidosomething on 2016-09-23 15:33_

Installed via homebrew formula

```
david@elitedavid:~projects/elitedaily/ed-videoplayer                                                                                                                                                                                                [node:v6.6.0][py:3.5.2][rb:2.3.1]
11:31:57I(vast*)% rg --version
0.1.15
```

Just for testing:

```
11:32:09I(vast*)% rg --vimgrep video
Unknown flag: '--vimgrep'

Usage: rg [options] -e PATTERN ... [<path> ...]
       rg [options] <pattern> [<path> ...]
       rg [options] --files [<path> ...]
       rg [options] --type-list
       rg --help
       rg --version
```

expected output in format `filename:LINENUMBER:grep-result`


---

_Comment by @BurntSushi on 2016-09-23 15:37_

Oh, the `--vimgrep` option was added in `0.1.16`. How did you install `rg`? (It looks like something is out of date.)


---

_Comment by @davidosomething on 2016-09-23 15:46_

oh the brew package is referencing 0.1.15 still
https://github.com/BurntSushi/ripgrep/blob/master/pkg/brew/ripgrep.rb


---

_Label `bug` added by @BurntSushi on 2016-09-24 02:19_

---

_Closed by @BurntSushi on 2016-09-25 00:51_

---

_Comment by @BurntSushi on 2016-09-25 00:51_

I've updated the brew package to `0.1.17`, so you should get the `--vimgrep` option now.


---
