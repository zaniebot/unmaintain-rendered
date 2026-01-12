```yaml
number: 2915
title: faster exact path match
type: issue
state: closed
author: matepek
labels:
  - question
assignees: []
created_at: 2024-10-22T00:55:37Z
updated_at: 2024-10-22T01:30:56Z
url: https://github.com/BurntSushi/ripgrep/issues/2915
synced_at: 2026-01-12T16:13:25Z
```

# faster exact path match

---

_@matepek_

#### Describe your feature request

Hello,

I'm developing a vscode extension and one of my user started to complain that in case of a large projects the loading takes a long time. `rg.exe` was mentioned so I started digging.

The flow:
- My extension looking for test executables files based on a pattern using the vscode API `vscode.workspace.findFiles`.
- VSCode seems to using this tool under the hood. 

In [this case](https://github.com/matepek/vscode-catch2-test-adapter/issues/444) the user specifies the **exact path, not a pattern**, `rg` still seems to take a lot of time and resources to return with the result.

Since it is not my issue it is not easy to come up with a proper repro and debug the situation myself so I'm fairly unsure that "this is the droid we are looking for" or not. 

Maybe the culprit is vscode or something else but I have to start somewhere. Thank you for your time in advance.

REMARKS:
Might worth to mention that this case `rg` is running in parallel multiple times.



---

_Comment by @BurntSushi on 2024-10-22 01:02_

If you know the exact path, then just run `rg pattern exact/path/to/file`.

---

_Closed by @BurntSushi on 2024-10-22 01:02_

---

_Label `question` added by @BurntSushi on 2024-10-22 01:02_

---

_Comment by @matepek on 2024-10-22 01:04_

Hey,

It's `vscode` which uses this tool to discover files with `--files` option.
So I think your suggestion is about another case. ?

---

_Comment by @matepek on 2024-10-22 01:15_

So we have a couple of exact paths.

My extension could do some check to see it is not a pattern but na exact file and avoid calling `vscode` which would in this case not call this tool with `--files` option and that would do the trick.

But I would not consider it as a best solution because this is a low level optimisation comparing to handing it to the extension which should stay away from file system as much as possible.

The problem is might elsewhere, I didn't read the code of this repo but as first this seemed a good start.
You might say that oh yeah, this case is handled the issue is elsewhere then sure has to search further but if not then fixing here would result that more people would benefit from the improvement.

So, is this tool handing efficiently the exact path case for `--file` option? And if not, are open for a change to handle it?


---

_Comment by @BurntSushi on 2024-10-22 01:27_

This repo is for ripgrep. I can't control how VS Code uses it. I am not capable of providing support for how VS Code uses this tool. I don't use VS Code. If you want my help, please give a repro that I can run using a standard shell environment, along with expected and actual behavior.

---

_Comment by @matepek on 2024-10-22 01:29_

I know, and I understand. 
My question would be that, is this tool handing efficiently the exact path case for `--file` option? 

---

_Comment by @BurntSushi on 2024-10-22 01:30_

I can't answer your question without a command because it's underspecified. Don't tell. _Show_.

---
