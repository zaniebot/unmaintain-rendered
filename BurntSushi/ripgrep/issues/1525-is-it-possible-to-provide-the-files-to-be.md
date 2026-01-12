```yaml
number: 1525
title: Is it possible to provide the files to be searched in a file rather than on the command line?
type: issue
state: closed
author: jborgh
labels:
  - duplicate
assignees: []
created_at: 2020-03-19T16:55:49Z
updated_at: 2020-05-25T09:24:42Z
url: https://github.com/BurntSushi/ripgrep/issues/1525
synced_at: 2026-01-12T16:13:23Z
```

# Is it possible to provide the files to be searched in a file rather than on the command line?

---

_@jborgh_

#### What version of ripgrep are you using?
ripgrep 11.0.2 (rev 3de31f7527)

#### How did you install ripgrep?
scoop 

#### What operating system are you using ripgrep on?
Windows 10

#### Describe your question, feature request, or bug.
I am interested in searching a specific set of files which I am currently specifying on the command line as the `[PATH ...]` argument. I'm wondering if there is something similar to the `--file` flag (which is for patterns) to specify a file containing a list of file paths to search instead?

Currently, I sometimes have to invoke `rg` multiple times since I'm hitting the max command line length.


---

_Label `duplicate` added by @BurntSushi on 2020-03-19 16:59_

---

_Comment by @BurntSushi on 2020-03-19 16:59_

Duplicate of #273. 

---

_Closed by @BurntSushi on 2020-03-19 16:59_

---

_Comment by @jborgh on 2020-03-19 17:09_

Thanks for the quick reponse! 

---

_Comment by @habamax on 2020-05-25 09:24_

@jborgh how did you resolve the issue?



---
