```yaml
number: 1229
title: .gitignore is not used when .git directory does not exist
type: issue
state: closed
author: biodafes
labels:
  - question
assignees: []
created_at: 2019-03-28T19:35:27Z
updated_at: 2019-06-15T00:00:11Z
url: https://github.com/BurntSushi/ripgrep/issues/1229
synced_at: 2026-01-12T16:13:23Z
```

# .gitignore is not used when .git directory does not exist

---

_@biodafes_

#### What version of ripgrep are you using?
ripgrep 0.10.0
-SIMD -AVX (compiled)
+SIMD -AVX (runtime)

#### How did you install ripgrep?
pacman -S ripgrep

#### What operating system are you using ripgrep on?
Linux euler 5.0.4-arch1-1-ARCH #1 SMP PREEMPT Sat Mar 23 21:00:33 UTC 2019 x86_64 GNU/Linux

#### Describe your question, feature request, or bug.
When .git/ does not exist, .gitignore is not used. I am unsure whether is this intentional behavior as this line specifically checks for the existence of the folder:
https://github.com/BurntSushi/ripgrep/blob/09139721047b1cda6ad88dbf89dc5fa74c66a3a2/ignore/src/dir.rs#L200

I ran across this because I have my home directory in version control and access it by setting GIT_DIR and GIT_WORK_TREE.

I would expect that either:
1. Use of .gitignore does not depend on existence of .git/
-or-
2. The directory used to verify the existence of git is read from GIT_DIR if it is defined.

I can work around this easily but wanted to check if it would be considered a bug.

#### If this is a bug, what are the steps to reproduce the behavior?
```
mkdir rgtest
cd rgtest
mkdir a
echo "pattern" > a/test.txt
echo "/a/" > .gitignore
rg pattern # pattern found

mkdir .git
rg pattern # pattern not found

rmdir .git
export GIT_WORK_TREE="$HOME/rgtest"
export GIT_DIR="$HOME/rgtest/.nonstandardname"
git init
rg pattern # pattern found (i would expect this to be ignored)
```


---

_Comment by @BurntSushi on 2019-03-28 21:35_

Thanks for the bug report!

It is definitely intentional that `.gitignore` is not respected when ripgrep does not detect a git repository.

It seems possible for ripgrep to respect `GIT_DIR`, and it might not be particularly hard, but ultimately, ripgrep isn't actually `git`. The `git` command has the luxury of operating on exactly one repository for any given invocation, but ripgrep does not. Moreover, ripgrep uses the `.git` directory to tell when it is has reached the "root" of a git repository, which is how ripgrep knows not to apply `.gitignore` files from a parent git repository to any repositories that might appear in sub-directories. `GIT_DIR` effectively breaks that assumption. So it's not quite clear to me how exactly ripgrep should respect `GIT_DIR`. (And `GIT_WORK_TREE` does not seem relevant, as I understand it.)

Other than creating a dummy `.git` directory (or file), another alternative is to simply symlink `.gitignore` to `.rgignore` (or `.ignore`), and ripgrep should pick that up.

---

_Label `question` added by @BurntSushi on 2019-03-28 21:35_

---

_Comment by @BurntSushi on 2019-04-08 12:30_

It's not clear if there's any actionable steps that can be taken here, given my previous comment. If there's any new information to add, please feel free to comment and we can re-open this.

---

_Closed by @BurntSushi on 2019-04-08 12:30_

---

_Comment by @olson-dan on 2019-04-12 22:27_

> It is definitely intentional that .gitignore is not respected when ripgrep does not detect a git repository.

Can you elaborate on this choice?

I noticed a few months ago that ripgrep completely broke for me with respect to .gitignore use and never understood why until today.

(I discovered that the correct solution was to switch to .ignore so my workflow is working again now, I'm just curious in the reasoning behind splitting the functionality into two files.)

---

_Comment by @BurntSushi on 2019-04-13 01:08_

ripgrep has always supported both `.gitignore` and `.ignore`. They both exist because they aren't always the same. i.e., Just because a file is in your gitignore doesn't mean you don't want to search it, and just because a file is not in gitignore doesn't mean you do want to search it. Plus, not everything you might want to search is in a git repo.

> Can you elaborate on this choice?

Because `.gitignore` only applies to git repositories.

---

_Comment by @aras-p on 2019-05-06 08:27_

FWIW it feels like at least some time ago rg was always respecting `.gitignore`, and that worked excellent for my use case (see below). But now in 11.0 it's ignored and kinda "breaks" rg since each invocation goes and searches multiple gigabytes of stuff.

My use case: we have a repository that can either be cloned from Mercurial or from Git. And normal workflow is to use Mercurial. So the repo has _both_ `.hgignore` and `.gitignore` files at the root, however typical usage ends up _without_ a `.git` folder. However I'd still very much like rg to use the file! Alternatively, if rg would support `.hgignore` that would work too of course, but my impression is that this ain't going to happen.

---

_Comment by @BurntSushi on 2019-05-06 11:09_

> FWIW it feels like at least some time ago rg was always respecting .gitignore

Indeed it was. Commit https://github.com/BurntSushi/ripgrep/commit/e65ca21a6cebceb9ba79fcd164da24478cc01fb0 changed that by fixing #934. 

> But now in 11.0 it's ignored and kinda "breaks" rg

Sometimes folks rely on bugs accidentally and then get bitten when the bug gets fixed. [This is just part of software evolution](https://xkcd.com/1172/) and there are plenty of trivial work-arounds available to you.

> My use case: we have a repository that can either be cloned from Mercurial or from Git. And normal workflow is to use Mercurial. So the repo has both .hgignore and .gitignore files at the root, however typical usage ends up without a .git folder. However I'd still very much like rg to use the file! 

There are several things you can do:

* Create an empty `.git` directory (or even just a file) at the root of the repo.
* Symlink your `.gitignore` to a `.ignore` or `.rgignore`.
* Explicitly copy the rules from the `.gitignore` to a `.ignore` or `.rgignore`.
* Use ripgrep's `--ignore-file` flag to explicitly specify the `.gitignore` file as an ignore file that it should use.

> Alternatively, if rg would support .hgignore that would work too of course, but my impression is that this ain't going to happen.

Indeed, it's unlikely. See #6.

---

_Comment by @aras-p on 2019-05-06 11:23_

yeah I understand that I came to rely on a peculiarity that could have been considered a bug. Some of the workarounds are not without downsides though -- e.g. an empty `.git` folder makes various other tools (e.g. Visual Studio) do "I detected a broken git folder for you!" prompts. Symlinks don't work that well on Windows, etc.

I'll go with manual `.rgignore` file I guess

---

_Comment by @masaeedu on 2019-06-14 21:36_

@BurntSushi I think `rg` respecting the environment variables, at least insofar whether to look for `.gitignore` file, would be quite useful. I'm using a tool called `yadm` for dotfile management, and it provides a `yadm env` command that simply drops you into a shell with the appropriate environment variables to fool most tools into thinking your home folder is actually a git repo.

This works well enough with my editor, `tig`, `git`, etc. but `rg` starts searching everything in the home folder instead, which makes it hard to find things.

As to how this should be implemented, I'm not really familiar with how ripgrep is implemented; is it possible to use something like the appropriate `libgit2` [API](https://libgit2.org/libgit2/#HEAD/group/ignore/git_ignore_path_is_ignored) to determine whether a file is ignored?

---

_Comment by @BurntSushi on 2019-06-15 00:00_

> As to how this should be implemented

This is the crux of this issue, in addition to the UX. Unfortunately, I don't think your comment really addresses any of the points I raised above. I'd suggest picking one of my suggested work arounds above.

> is it possible to use something like the appropriate libgit2 API to determine whether a file is ignored?

No. That's not how ripgrep works.

---
