```yaml
number: 682
title: Different handling of .gitignore in comparison to git
type: issue
state: closed
author: nmoehrle
labels: []
assignees: []
created_at: 2017-11-16T13:31:05Z
updated_at: 2017-11-16T17:05:46Z
url: https://github.com/BurntSushi/ripgrep/issues/682
synced_at: 2026-01-12T16:13:22Z
```

# Different handling of .gitignore in comparison to git

---

_@nmoehrle_

I stumbled over a case where ripgrep ignores files that are not ignored by git. I am unsure if this is actually an issue of ripgreps interpretation of the .gitignore file, something that cannot be realized without further information about the repository or even a issue of git itself. I think this is best explained by a minimal example:

```
$ tree
.
├── a.txt
└── sub
    ├── b.txt
    ├── c.txt
    └── d.md

1 directory, 4 files
```
```
$ rg -u test
sub/d.md
1:test

sub/c.txt
1:test

sub/b.txt
1:test

a.txt
1:test
```
```
$ rg test
a.txt
1:test
```
```
$ cat .gitignore
# Ignore everything
*
# Exceptions for txt files
!*.txt
# Apply above to subfolders
!**/
```
```
$ git status
On branch master

Initial commit

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)

	new file:   sub/b.txt

Untracked files:
  (use "git add <file>..." to include in what will be committed)

	a.txt
	sub/c.txt
```

The files *b.txt* and *c.txt* are ignored by ripgrep even tough they are not ignored by git. Since *b.txt* is staged it is clear that it cannot be ignored by git, but git also does not ignore c.txt which suggests to me that the 6th line of the *.gitignore* does what the comment says. Note that *d.md* is correctly ignored by both ripgreg and git.

I absolutely love ripgrep and use it everyday to quickly navigate foreign code bases --- or my own --- its often faster than waiting for the (re)indexing to complete. Thank you for sharing the project!

---

_Comment by @okdana on 2017-11-16 13:34_

I think this was fixed by PR #652

---

_Comment by @BurntSushi on 2017-11-16 13:45_

@nmoehrle Thanks so much for the awesome bug report! I can indeed confirm that this is fixed on master by #652. Yay!

---

_Closed by @BurntSushi on 2017-11-16 13:45_

---

_Comment by @nmoehrle on 2017-11-16 17:05_

Wow you guys are quick --- should have looked into the commit history :-D Next release then --- thanks!
Also shout-out to @lambda nice catch :-)

---
