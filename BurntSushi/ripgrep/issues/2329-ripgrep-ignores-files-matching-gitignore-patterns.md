```yaml
number: 2329
title: ripgrep ignores files matching gitignore patterns even if they are staged or committed
type: issue
state: closed
author: Munksgaard
labels:
  - wontfix
assignees: []
created_at: 2022-10-10T13:54:35Z
updated_at: 2022-10-10T14:26:20Z
url: https://github.com/BurntSushi/ripgrep/issues/2329
synced_at: 2026-01-12T16:13:24Z
```

# ripgrep ignores files matching gitignore patterns even if they are staged or committed

---

_@Munksgaard_

#### What version of ripgrep are you using?

```console
$ rg --version
ripgrep 13.0.0
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
```

#### How did you install ripgrep?

I installed ripgrep through NixOS/nixpkgs

#### What operating system are you using ripgrep on?

```console
$ uname -a
Linux church 5.15.63 #1-NixOS SMP Thu Aug 25 09:40:49 UTC 2022 x86_64 GNU/Linux
```

#### Describe your bug.

Negated patterns are not implemented correctly

#### What are the steps to reproduce the behavior?

In [our project](https://github.com/diku-dk/futhark/) we have a [`.gitignore`](https://github.com/diku-dk/futhark/blob/master/.gitignore) file at the root of the repository. The first few lines serve to ignore all the _files_ in the root. The purpose is to enable using the root as a scratch space for programs, debug-output etc. However, crucially, the `.gitignore` rules do not prevent git from recognizing when some of the files that actually are checked in are changed. 

It seems as if ripgrep does not follow this behavior. Consider the following example:

```console
$ mkdir rgtest
$ cd rgtest/
$ git init
Initialized empty Git repository in /home/munksgaard/rgtest/.git/
$ echo "KAWABUNGA" > README.md
$ git add README.md
$ echo '# Ignore non-directories in root, so you can use it as scratch space.
/*
!/*/
' > .gitignore
$ rg KAWABUNGA
$ git status
On branch master

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
	new file:   README.md


$ echo "Yowza" >> README.md

$ rg Yowza
```

Here, because README.md is staged (though the behavior should be the same when it is committed), we actually do want ripgrep to search the text contained therein, but that does not happen.

The culprit is the double lines of 

```
/*
!/*/
```

in the `.gitignore` file. The first line ignores everything in the root, whereas the second line explicitly unignores all directories (and by extension all files contained therein).

In both of the `rg` calls in the example above I would expect to see the `README.md` file.

---

_Comment by @Munksgaard on 2022-10-10 13:59_

While similar to #1654, I _think_ this issue is different, because we've not ignored the parent directory of the files in the root (how could we), just all of the files (and directories) therein.

---

_Comment by @BurntSushi on 2022-10-10 14:22_

This is correct and it has nothing to do with the implementation of negated patterns. You should try running ripgrep with `--debug` (it's why it's recommended in the issue template). You'll see this line:

```
DEBUG|ignore::walk|crates/ignore/src/walk.rs:1741: ignoring ./README.md: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "/*", actual: "*", is_whitelist: false, is_only_dir: false })))
```

`README.md` matches `/*` and does not match `/*/`, so it is ignored and this is correct.

The issue here is not that negated patterns aren't working as you expect, but that ripgrep respects _only_ your gitignore and not what is actually staged or committed in your git repo. In other words, your gitignore file is _out of sync_ with what is tracked by git. This isn't necessarily a problem with git because once a file is tracked, it is effectively unignored. But ripgrep doesn't (and will never) know it is tracked because all it looks at is the gitignore files.

I suggest explicitly whitelisting files you track in your `.gitignore`.

Also, as an additional note, your rules here are pretty broad. Namely, `!/*/` will whitelist hidden directories such as `.git`, and thus cause ripgrep to search them. You'll want to add `/.git` below `!/*/` to ignore it again.

---

_Closed by @BurntSushi on 2022-10-10 14:22_

---

_Label `wontfix` added by @BurntSushi on 2022-10-10 14:23_

---

_Comment by @Munksgaard on 2022-10-10 14:24_

@BurntSushi Thank you for the quick and thorough reply. You're right, we should probably just change our `.gitignore`....

---

_Renamed from "Negated patterns bug" to "ripgrep ignores files matching gitignore patterns even if they are staged or committed" by @BurntSushi on 2022-10-10 14:26_

---
