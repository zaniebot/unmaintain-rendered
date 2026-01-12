```yaml
number: 2454
title: "`ignore` doesn't respect .gitignore"
type: issue
state: closed
author: SK83RJOSH
labels: []
assignees: []
created_at: 2023-03-14T20:08:43Z
updated_at: 2023-03-14T20:54:04Z
url: https://github.com/BurntSushi/ripgrep/issues/2454
synced_at: 2026-01-12T16:13:24Z
```

# `ignore` doesn't respect .gitignore

---

_@SK83RJOSH_

#### What version of ignore are you using?

0.4.20

#### What operating system are you using ignore on?

Windows

#### Describe your bug.

This one is a bit of an edge case, but this pattern is used by Unreal and I suspect is valid. When excluding *all* files, but *not* directories, and subsequently whitelisting specific file types, ignored folders seem to be traversed and files which should be otherwise ignored, are not.

I'm building a tool to automate robocopying Unreal's source between git and perforce; and while `ignore` seems to have largely correct output (better than any other library I've tried in any language), this particular bug is blocking me at the moment.

#### What are the steps to reproduce the behavior?

If I have a directory containing a `.gitignore` like this:
```
# Ignore all files by default, but scan all directories
*
!*/

# Text files
!*.txt

# Ignore logs
/Logs/
```

And the following structure:
```
A.txt
B.txt
C.txt
Logs\A.txt
Logs\B.txt
Logs\C.txt
Logs\Subfolder\A.txt
Logs\Subfolder\B.txt
Logs\Subfolder\C.txt
```

#### What is the actual behavior?

`ignore` will include all `*.txt` files within `/Logs/` when it should not.

#### What is the expected behavior?

`ignore` should not include all `*.txt` files within `/Logs/`.



---

_Comment by @BurntSushi on 2023-03-14 20:14_

If you're using the library directly, then you _must_ provide a program that reproduces the problem. Otherwise, how can I fix it? For example, using ripgrep, your example works just as I would expect:

```
$ tree
.
├── A.txt
├── B.txt
├── C.txt
└── Logs
    ├── A.txt
    ├── B.txt
    ├── C.txt
    └── Subfolder
        ├── A.txt
        ├── B.txt
        └── C.txt
$ cat .gitignore
# Ignore all files by default, but scan all directories
*
!*/

# Text files
!*.txt

# Ignore logs
/Logs/
$ rg --files
A.txt
B.txt
C.txt
```

Which is what looks correct to me. Your ignore file whitelists all `*.txt` files, so that explains the `{A,B,C}.txt` in the results. But ignores `/Logs/`, which is correctly ignored here.

---

_Comment by @SK83RJOSH on 2023-03-14 20:26_

> If you're using the library directly, then you _must_ provide a program that reproduces the problem.

My apologies, the issue template didn't mention that, and I didn't want to cut down my program if I didn't have to (I thought this may be reproducible with ripgrep - and unfortunately I didn't think to test it - since it isn't I suspect it's PEBKAC but let's see). 🙂 

[gitignore-repro.zip](https://github.com/BurntSushi/ripgrep/files/10973472/gitignore-repro.zip)

You can simply do cargo run and check the directory for added.txt/removed.txt


---

_Comment by @SK83RJOSH on 2023-03-14 20:50_

Ah okay, I see now that that `add_ignore` doesn't set the root path of `GitignoreBuilder` using the existing `WalkBuilder` root. Which makes sense since as it's intended for global ignores which totally flew over my head for whatever reason. This may be out of scope for the library then, I naively assumed this worked something like filters, and would let me use a given `.gitignore` as if it was already in a given directory.

My apologies! I'll close this for now, and use a `.gitignore` in both source and output instead. I think this would be a rather interesting feature though, as I've seen a few people using `filters` to this effect - but since those act like a ".gitallow" I can't use them here without modifying the source ignore files.

---

_Closed by @SK83RJOSH on 2023-03-14 20:50_

---
