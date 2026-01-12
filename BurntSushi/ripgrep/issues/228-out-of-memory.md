```yaml
number: 228
title: Out of memory
type: issue
state: closed
author: andyleejordan
labels:
  - bug
assignees: []
created_at: 2016-11-09T21:19:32Z
updated_at: 2016-11-09T21:57:18Z
url: https://github.com/BurntSushi/ripgrep/issues/228
synced_at: 2026-01-12T18:23:11Z
```

# Out of memory

---

_@andyleejordan_

I managed to break `ripgrep` somehow.

```
$ git clone https://github.com/docker/docker.git
$ cd docker

$ rg --ignore-file vendor tar
fatal runtime error: out of memory
^C^C^C^CIllegal instruction (core dumped)

$ rg --version
ripgrep 0.2.8

$ uname -r
4.8.6-1.el7.elrepo.x86_64
```

This is on CentOS 7 virtualized with Hyper-V.

I have since realized that `--ignore-file` was not what I wanted (I'm trying to exclude files). It's actual intention is a file which lists what to be ignored. However, by giving it a directory (any directory) I manage to crash ripgrep pretty easily.

---

_Comment by @andyleejordan on 2016-11-09 21:26_

@BurntSushi is it intended that `--ignore-file` accepts and walks a directory for ignore files? If it is not, I think I can fix it rather simply by denying the use of directories.


---

_Comment by @BurntSushi on 2016-11-09 21:44_

This is an awesome bug, thanks for the report!

The issue was that I got a bit overzealous with partial error reporting. Namely, when processing a gitignore file, we should try to use every pattern even if some patterns are invalid globs (e.g., `a**b`). In the process, I applied the same logic to I/O errors. In this case, it manifest by attempting to read lines from a directory, which appears to yield `Result`s forever, where each `Result` is an error of the form "you can't read from a directory silly." Since I treated it as a partial error, ripgrep was just spinning and accruing each error in memory, which caused the OOM killer to kick in.


---

_Label `bug` added by @BurntSushi on 2016-11-09 21:44_

---

_Closed by @BurntSushi on 2016-11-09 21:46_

---

_Comment by @andyleejordan on 2016-11-09 21:57_

Yay! This is why I love `ripgrep`. It's already fixed! Thanks @BurntSushi.


---
