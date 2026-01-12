```yaml
number: 1445
title: rg does not ignore files listed in .git/info/exclude in git linked worktrees
type: issue
state: closed
author: krobelus
labels: []
assignees: []
created_at: 2019-12-10T23:34:30Z
updated_at: 2020-02-17T22:16:37Z
url: https://github.com/BurntSushi/ripgrep/issues/1445
synced_at: 2026-01-12T16:13:23Z
```

# rg does not ignore files listed in .git/info/exclude in git linked worktrees

---

_@krobelus_

#### What version of ripgrep are you using?

ripgrep 11.0.2 (same in master I think)

#### How did you install ripgrep?

https://www.archlinux.org/packages/community/x86_64/ripgrep/

#### What operating system are you using ripgrep on?

Arch Linux

#### Describe your question, feature request, or bug.

If I exclude a file privately in `.git/info/exclude`, ripgrep ignores that file as expected - but only when searching inside the main worktree. In linked worktrees, the file is not ignored.

#### If this is a bug, what are the steps to reproduce the behavior?

```sh
#!/bin/sh
rm -rf repro repro-worktree

git init repro
cd repro
touch committed
git add committed
git commit -m 'initial commit'
echo ignored > .git/info/exclude
git worktree add ../repro-worktree
cd ../repro-worktree
touch ignored

echo
echo '$ rg --files'
rg --files
echo '$ git ls-files; git ls-files --exclude-standard --others'
git ls-files; git ls-files --exclude-standard --others
```

#### If this is a bug, what is the actual behavior?

```
$ rg --files
ignored
committed
```

#### If this is a bug, what is the expected behavior?

I think it should behave like `git ls-files; git ls-files --exclude-standard --others`:

```
$ rg --files
committed
```


---

_Closed by @BurntSushi on 2020-02-17 22:16_

---
