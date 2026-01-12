```yaml
number: 16
title: Ignoring of subdirectories is inconsistent with git
type: issue
state: closed
author: dmit
labels:
  - bug
assignees: []
created_at: 2016-09-23T15:46:25Z
updated_at: 2016-09-24T20:29:31Z
url: https://github.com/BurntSushi/ripgrep/issues/16
synced_at: 2026-01-12T18:23:11Z
```

# Ignoring of subdirectories is inconsistent with git

---

_@dmit_

I have an entry `target/` in .gitignore to ignore all `target` directories in the project. `ag` handles it fine and ignores all matching directories. `rg` only ignores the matching top level directory. If I change the pattern to `**/target/` then `rg` works as expected, but `ag` stops ignoring all `target` directories. In both cases `git` ignores both the top level directory and the subdirectories.

Here's a shell session to illustrate the issue:

```
/p/tmp> rg --version
0.1.16
/p/tmp> git init abc
Initialized empty Git repository in /private/tmp/abc/.git/
/p/tmp> cd abc/
/p/t/abc> mkdir ghi
/p/t/abc> mkdir -p def/ghi
/p/t/abc> echo ghi/ > .gitignore
/p/t/abc> echo xyz > ghi/toplevel.txt
/p/t/abc> echo xyz > def/ghi/subdir.txt
/p/t/abc> ag xyz
/p/t/abc> rg xyz
def/ghi/subdir.txt
1:xyz
/p/t/abc> git status
On branch master

Initial commit

Untracked files:
  (use "git add <file>..." to include in what will be committed)

        .gitignore

nothing added to commit but untracked files present (use "git add" to track)
/p/t/abc> echo '**/ghi/' > .gitignore
/p/t/abc> ag xyz
def/ghi/subdir.txt
1:xyz

ghi/toplevel.txt
1:xyz
/p/t/abc> rg xyz
No files were searched, which means ripgrep probably applied a filter you didn't expect. Try running again with --debug.
/p/t/abc> git status
On branch master

Initial commit

Untracked files:
  (use "git add <file>..." to include in what will be committed)

        .gitignore

nothing added to commit but untracked files present (use "git add" to track)
```

```
DEBUG:rg::ignore: ./ghi ignored by Pattern { from: "./.gitignore", original: "ghi/", pat: "ghi", whitelist: false, only_dir: true }
```


---

_Label `bug` added by @BurntSushi on 2016-09-24 02:20_

---

_Comment by @BurntSushi on 2016-09-24 02:20_

Confirmed. This one is embarrassing.


---

_Comment by @BurntSushi on 2016-09-24 20:07_

The specific problem here was a bug translating the `.gitignore` path into a glob with the right semantics defined by `gitignore`. Specifically, it was being treated as an absolute path, which of course, it's not (because it lacks the leading `/`).


---

_Closed by @BurntSushi on 2016-09-24 20:29_

---
