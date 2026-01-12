```yaml
number: 743
title: "Request: Prefer to search git-tracked files over ignoring them if they match an ignore rule."
type: issue
state: closed
author: unphased
labels:
  - question
assignees: []
created_at: 2018-01-11T22:50:33Z
updated_at: 2018-01-11T23:37:56Z
url: https://github.com/BurntSushi/ripgrep/issues/743
synced_at: 2026-01-12T16:13:22Z
```

# Request: Prefer to search git-tracked files over ignoring them if they match an ignore rule.

---

_@unphased_

Example: 

1. file `a` has the value `x` in it.
2. `~/rg.ignore` specifies file `a`
3. `rg --ignore-file ~/rg.ignore x` does not dredge up `a`.
4. `git add a`
5. now I want `rg --ignore-file ~/rg.ignore x` to pull up `a`.

Maybe it could be implemented as an additional rg flag. So that e.g. `rg --ignore-file ~/rg.ignore --always-search-git-tracked x` will then pull up `a`.

Similarly, imagine the scenario above but where instead of the ignorefile specifying `a`, the file in question is named `.a`. 

The point is to enable the ability to give rg the notion of whether a file is git-tracked or not. And then allow for us to configure rg to *prioritize* it higher than ignore logic, and hidden file logic. This way we can have a more friendly default behavior which is that rg will be able to more readily make sure that it wont skip searching in version controlled code. 

---

_Comment by @BurntSushi on 2018-01-11 23:13_

This has been brought up before and has, for the most part, been rejected. Determining if a file is tracked or not has implementation complexity (I note that you don't suggest an implementation path here), and this is very easily worked around by putting the files you want to search in a `.ignore` whitelist.

---

_Label `question` added by @BurntSushi on 2018-01-11 23:13_

---

_Comment by @unphased on 2018-01-11 23:28_

Was not aware of gitignore whitelisting as a thing. I stumbled upon it independently just now. Thank you. 

---

_Closed by @unphased on 2018-01-11 23:28_

---

_Comment by @unphased on 2018-01-11 23:34_

This is actually slightly off-topic, but... my beautiful new FZF file searching config is this:

    export FZF_DEFAULT_COMMAND="rg --color=never --files --hidden -g '*' -g '!.git/'"

Never dredge up `.git/`. But do show every single other file ever.

It's a different type of configuration compared to when i actually use rg to search file *contents*. I think the decision to not burden rg with "checked into git" is a good one. At this point my fzf and rg configs already make me very happy.

---
