```yaml
number: 1109
title: .gitignore not respected outside of git repositories
type: issue
state: closed
author: Tim-at-AST
labels: []
assignees: []
created_at: 2018-11-16T03:06:49Z
updated_at: 2021-04-02T16:56:59Z
url: https://github.com/BurntSushi/ripgrep/issues/1109
synced_at: 2026-01-12T16:13:22Z
```

# .gitignore not respected outside of git repositories

---

_@Tim-at-AST_

#### What version of ripgrep are you using?

````
$ rg --version
ripgrep 0.10.0
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
````

#### How did you install ripgrep?
macports


#### What operating system are you using ripgrep on?
mac osx 10.14.1 (mojave)


#### Describe your question, feature request, or bug.
here is my .gitignore file:

````
$ cat ~/.gitignore
**/*.pdf
*.pdf
````


I am trying to ignore pdf files in my rg search.

Here is a test directory:
````
$ cd /tmp/test
$ touch a.pdf b.txt
$ ls
a.pdf
b.txt

$ rg --files --debug
b.txt
a.pdf
````

The problem is that the file `a.pdf` should be ignored.




---

_Comment by @BurntSushi on 2018-11-16 13:35_

Your `.gitignore` is in your home directory and you are searching outside your home directory. Therefore, what you're observing is expected behavior. Try using a standard git repository setup instead. See `man gitignore` for how to specify global gitignore rules.

---

_Closed by @BurntSushi on 2018-11-16 13:35_

---

_Comment by @Tim-at-AST on 2018-11-16 20:18_

The example was badly chosen.  But the problem persists inside the home directory also.  Here is a better example:

````
$ cat ~/.gitignore
**/*.pdf
*.pdf
**/*.jpg
$ cd /Users/tim/documents/rg/test
$ touch a.pdf b.txt
$ ls
a.pdf
b.txt
$ rg --files --debug
DEBUG|rg::config|src/config.rs:37: /Users/tim/gdrive/tim/app-support/ripgreprc: arguments loaded from config file: ["--max-columns=150", "--smart-case"]
DEBUG|rg::args|src/args.rs:508: final argv: ["rg", "--max-columns=150", "--smart-case", "--files", "--debug"]
DEBUG|globset|globset/src/lib.rs:429: built glob set; 0 literals, 0 basenames, 2 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
b.txt
a.pdf

`````

---

_Comment by @BurntSushi on 2018-11-16 20:25_

Well, is `/Users/tim/documents/rg/test` a git repository? If not, then `.gitignore` isn't going to apply to it because, well, it's not a git repository. This is expected behavior. Here is a transcript showing this:

```
$ touch a.pdf b.txt

$ rg --files
b.txt
a.pdf

$ echo '*.pdf' > .gitignore

$ rg --files
b.txt
a.pdf

$ git init
Initialized empty Git repository in /tmp/ripgrep-1109/.git/

$ rg --files
b.txt

$ rm -rf .git

$ rg --files
b.txt
a.pdf

$ mv .gitignore .ignore

$ rg --files
b.txt
```

---

_Renamed from ".gitignore not respected at all." to ".gitignore not respected outside of git repositories" by @BurntSushi on 2018-11-16 20:25_

---

_Comment by @Tim-at-AST on 2018-11-16 21:15_

ok, I changed .gitignore to .rgignore and problem solved.  
The documentation seems to me to indicate that .gitignore was used by rg.

> After recursive search, ripgrep's most important feature is what it doesn't search. By default, when you search a directory, ripgrep will ignore all of the following:

> 1. Files and directories that match the rules in your .gitignore glob pattern.

---

_Comment by @BurntSushi on 2018-11-16 21:17_

> The documentation seems to me to indicate that .gitignore was used by rg.

Right, _in a git repository_. I agree that the docs could be more precise on this point. In particular, I do not think ripgrep always behaved this way, but I can't find the corresponding bug report that prompted the change to require a git repository before respecting `.gitignore`.

---

_Comment by @anntzer on 2019-02-07 21:12_

Note that currently, if rg follows a symlink that points to a subdirectory of a git repo, it will not detect that it is in a git repo, and thus won't respect a gitignore file in that subdirectory (even though that gitignore is indeed in a git repo), as shown by:
```
$ tree -a -L 3
.
├── a
│   └── subdir -> ../c/subdir
└── c
    ├── .git
    │   ├── branches
    │   ├── config
    │   ├── description
    │   ├── HEAD
    │   ├── hooks
    │   ├── info
    │   ├── objects
    │   └── refs
    └── subdir
        ├── .gitignore
        └── somefile
$ cat a/subdir/.gitignore
*
$ rg --follow --files
a/subdir/somedir  # oops, not ignored.
```

I would thus suggest revisiting the choice of not respecting gitignores outside of git repos -- the alternative being, I guess, to check for each *resolved* symlink whether it is in a git repo.

---

_Comment by @BurntSushi on 2019-02-07 22:00_

@anntzer Yes, that's interesting, although it's definitely a distinct bug from this ticket. My guess is that I'd probably mark it as `wontfix`. I don't think I'm willing to start messing with the resolved paths of symlinks.

---

_Comment by @anntzer on 2019-02-07 22:11_

That's why I listed checking the resolved paths as the alternative solution -- if you don't remember what change prompted you to ignore gitignores outside of git repositories, perhaps you can consider the above an argument in favor of always taking them into account?

---

_Comment by @BurntSushi on 2019-02-07 22:20_

I'm not going to change that. It's semantically incorrect and leads to other types of bugs. I wrote "I don't remember" above because it's often inconvenient for me to spend the time to remember. Looking at revision history, I found this commit https://github.com/BurntSushi/ripgrep/commit/e65ca21a which fixed this bug https://github.com/BurntSushi/ripgrep/issues/934

---

_Comment by @anntzer on 2019-02-07 22:31_

Sure, thanks for the reference.

---

_Comment by @EndlessDex on 2021-04-02 16:56_

A note for future readers:  The ~/.ignore file DOES work for non-git repos. It is just the .gitignore file that does not work

---
