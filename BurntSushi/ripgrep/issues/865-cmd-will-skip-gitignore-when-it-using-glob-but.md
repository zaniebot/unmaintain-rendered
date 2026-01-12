```yaml
number: 865
title: cmd will skip .gitignore when it using glob but bash/zsh
type: issue
state: closed
author: ar1xwe
labels:
  - invalid
  - question
  - wontfix
assignees: []
created_at: 2018-03-20T10:11:20Z
updated_at: 2018-03-20T10:55:34Z
url: https://github.com/BurntSushi/ripgrep/issues/865
synced_at: 2026-01-12T16:13:22Z
```

# cmd will skip .gitignore when it using glob but bash/zsh

---

_@ar1xwe_

#### What version of ripgrep are you using?
ripgrep 0.8.1 (rev c8e9f25b85)

----------------------

#### What operating system are you using ripgrep on?
Windows 10x64
Version 1607 (OS Build 14393.2068)

----------------------

#### Describe your question, feature request, or bug.
When I proceed to search a string with glob, rg will skip my .gitignore for some reason.

----------------------

#### If this is a bug, what are the steps to reproduce the behavior?
Files:
.gitignore "bar*"
foo.txt "Lorem Ipsum"
bar.txt "Lorem Ipsum"

Command: rg -g *.txt -e "Lorem Ipsum"

Output
```
cmd ->
bar.txt
1:Lorem Ipsum
foo.txt
1:Lorem Ipsum

bash/zsh ->
1:Lorem Ipsum
```

#### If this is a bug, what is the expected behavior?
I did updated the ripgrep to latest version, and it fix the issue that glob ignoring .gitignore file.
but the update only fix the problem with bash/zsh.

Help me please. it's a really useful tool, but it's kind of sad that I need to keep watch those result from any unneeded files.

Thanks for taking time to read my question.

---

_Comment by @BurntSushi on 2018-03-20 10:54_

This is intended behavior. Globs specified on the command line override `.gitignore` (and anything else). When you filed this issue, the instructions explicitly asked you to run ripgrep with the `--debug` flag:

> Show the command you ran and the actual output. Include the `--debug` flag in
your invocation of ripgrep.

If you did this, then the output would show you that your files have been whitelisted:

```
DEBUG/ignore::walk/ignore/src/walk.rs:1431: whitelisting ./bar.txt: Whitelist(IgnoreMatch(Override(Glob(Matched(Glob { from: None, original: "*.txt", actual: "**/*.txt", is_whitelist: false, is_only_dir: false })))))
DEBUG/ignore::walk/ignore/src/walk.rs:1431: whitelisting ./foo.txt: Whitelist(IgnoreMatch(Override(Glob(Matched(Glob { from: None, original: "*.txt", actual: "**/*.txt", is_whitelist: false, is_only_dir: false })))))
```

Your results with "bash/zsh" are a red herring because you did not properly quote your glob. `rg -g '*.txt' -e 'Lorem Ipsum'` gives the expected result. `rg -g *.txt -e 'Lorem Ipsum'` actually expands to `rg -g bar.txt foo.txt -e 'Lorem Ipsum'`, which is a totally different comand.

---

_Closed by @BurntSushi on 2018-03-20 10:54_

---

_Label `invalid` added by @BurntSushi on 2018-03-20 10:54_

---

_Label `question` added by @BurntSushi on 2018-03-20 10:54_

---

_Label `wontfix` added by @BurntSushi on 2018-03-20 10:54_

---
