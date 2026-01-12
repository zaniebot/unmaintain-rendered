```yaml
number: 1897
title: Ripgrep applies a root level pattern inside subdirectories. Git does not.
type: issue
state: closed
author: pmontrasio
labels: []
assignees: []
created_at: 2021-06-16T08:35:08Z
updated_at: 2021-06-16T11:51:27Z
url: https://github.com/BurntSushi/ripgrep/issues/1897
synced_at: 2026-01-12T16:13:24Z
```

# Ripgrep applies a root level pattern inside subdirectories. Git does not.

---

_@pmontrasio_

#### What version of ripgrep are you using?

`0.1.16`

#### How did you install ripgrep?

```
sudo apt install ripgrep
```

#### What operating system are you using ripgrep on?

```
$ lsb_release -a
No LSB modules are available.
Distributor ID:	Ubuntu
Description:	Ubuntu 20.04.2 LTS
Release:	20.04
Codename:	focal
```

#### Describe your bug.

Ripgrep applies root level ignore patterns such as `/a/**` to subdirectories. A `.gitignore` like that in the root of a repository prevents ripgrep to look into any `a` subdirectory at any level. Git ignores `a` only at the root level of the repository.

#### What are the steps to reproduce the behavior?

```
$ mkdir bug-demo
$ cd bug-demo
$ mkdir a a/a b b/a
$ echo a > a/a/a.txt
$ echo a > b/a/a.txt
$ echo '/a/**' > .gitignore
$ git init
$ git status
On branch master

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)
	.gitignore
	b/
nothing added to commit but untracked files present (use "git add" to track)
```

Git ignores `a` at the root level, this is correct.

```
$ git add b
$ git status
On branch master

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
	new file:   b/a/a.txt

Untracked files:
  (use "git add <file>..." to include in what will be committed)
	.gitignore
```

So git doesn't ignore `b/a/a.txt` because of `/a/**`

From within a subdirectory:

```
$ cd b
$ echo b > a/b.txt
$ git status
On branch master

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
	new file:   a/a.txt

Untracked files:
  (use "git add <file>..." to include in what will be committed)
	../.gitignore
	a/b.txt
```

So git doesn't ignore `b/a/b.txt` when called from within `b` even if it sees an `a` directory there.

#### What is the actual behavior?

This is what happens with ripgrep, called from within the `b` directory

```
$ rg a
No files were searched, which means ripgrep probably applied a filter you didn't expect. Try running again with --debug.

$ rg a --debug
DEBUG:grep::search: regex ast:
Literal {
    chars: [
        'a'
    ],
    casei: false
}
DEBUG:grep::literals: literal prefixes detected: Literals { lits: [Complete(a)], limit_size: 250, limit_class: 10 }
DEBUG:rg::ignore: ./a ignored by Pattern { from: "/home/me/bug-demo/.gitignore", original: "/a/**", pat: "a/**", whitelist: false, only_dir: false }
No files were searched, which means ripgrep probably applied a filter you didn't expect. Try running again with --debug.
```

This is inconsistent with git's behavior. It makes difficult or impossible to use ripgrep in some git repositories.

I wonder if the key is `original: "/a/**", pat: "a/**"`: The original is `/a/**` but pat (pattern?) is `a/**` without the leading slash.



#### What is the expected behavior?

```
$ rg a
a/a.txt
1:a
```

---

_Comment by @pmontrasio on 2021-06-16 09:14_

I'm closing the issue. I did install ripgrep with `sudo apt install ripgrep` but I didn't realize that I downloaded a binary two years ago into a directory with higher precedence in my PATH, then forgot about it.
I removed the old binary and the newer version (ripgrep 11.0.2) works well.

Sorry for wasting your time reading this.


---

_Closed by @pmontrasio on 2021-06-16 09:14_

---

_Comment by @BurntSushi on 2021-06-16 11:51_

@pmontrasio Nice! And no problem. Bugs that fix themselves before I even have a chance to read them are great. :-)

Do note though that ripgrep 11.0.2 is itself almost two years old: https://github.com/BurntSushi/ripgrep/blob/master/CHANGELOG.md#1102-2019-08-01

---
