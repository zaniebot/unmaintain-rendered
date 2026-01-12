```yaml
number: 31
title: "git: explicitly added files are ignored"
type: issue
state: closed
author: Istvan91
labels:
  - question
assignees: []
created_at: 2016-09-23T19:10:00Z
updated_at: 2016-09-24T23:31:09Z
url: https://github.com/BurntSushi/ripgrep/issues/31
synced_at: 2026-01-12T18:23:11Z
```

# git: explicitly added files are ignored

---

_@Istvan91_

I'm not sure if this is in scope of ripgrep, but explicitly added files in git repositories are ignored.

The script

```
#!/bin/bash
mkdir testfolder
pushd testfolder
    git init
    echo "some text" > testfile
    echo "*" > .gitignore
    git add -f testfile .gitignore
    git commit -m "Initial commit"
    echo "ripgrep:"
    rg some
    echo "git grep:"
    git --no-pager grep some
popd

```

yields following output:

```
Initialized empty Git repository in /full/path/testfolder/.git/
[master (root-commit) 350428e] Initial commit
 2 files changed, 2 insertions(+)
 create mode 100644 .gitignore
 create mode 100644 testfile
ripgrep:
No files were searched, which means ripgrep probably applied a filter you didn't expect. Try running again with --debug.
git grep:
testfile:some text
```


---

_Comment by @BurntSushi on 2016-09-24 02:09_

I don't think there's a way to fix this without digging deeping into `git`. We might have to live with it.

Note that you can add files to your `.rgignore` (soon to be `.gitignore`) that will override this. You can also whitelist them in your `.gitignore`, which honestly kind of seems like the right thing to do anyway.


---

_Label `question` added by @BurntSushi on 2016-09-24 02:09_

---

_Comment by @Istvan91 on 2016-09-24 05:43_

Have you actually evaluated to (optionally) call git and ask for all files? I guess this would also fix all other gitignore related problems.

Though I partially disagree, I leave it like this since ag, silver_searcher and co. can't do this as well. (In the future you could mention this limitation in the README.)


---

_Comment by @BurntSushi on 2016-09-24 05:54_

It just seems like bad juju for `rg` to have a runtime dependency on another command line tool. Moreover, implementing _optional_ support for it is a recipe for monstrous code complexity and innumerable bugs.

If we really wanted to get serious about this, I'd probably insist researching something like `libgit2` to see if we could use that to implement this stuff. Depending on a C library doesn't sound great to me either, but slightly more palatable than another command line tool.


---

_Comment by @BurntSushi on 2016-09-24 23:31_

I am going to close this because I don't see an answer to this problem that I'm comfortable with. If anything new comes to light, we can revisit it.


---

_Closed by @BurntSushi on 2016-09-24 23:31_

---
