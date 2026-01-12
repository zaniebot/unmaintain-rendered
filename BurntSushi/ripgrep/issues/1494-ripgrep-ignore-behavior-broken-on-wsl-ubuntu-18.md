```yaml
number: 1494
title: Ripgrep ignore behavior broken on WSL Ubuntu 18.04 (failing tests)
type: issue
state: closed
author: kashperanto
labels:
  - wontfix
assignees: []
created_at: 2020-02-20T14:33:21Z
updated_at: 2020-02-22T01:35:20Z
url: https://github.com/BurntSushi/ripgrep/issues/1494
synced_at: 2026-01-12T16:13:23Z
```

# Ripgrep ignore behavior broken on WSL Ubuntu 18.04 (failing tests)

---

_@kashperanto_

#### What version of ripgrep are you using?

~/src/ripgrep $ rg --version
ripgrep 11.0.2 (rev b44554c803)
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)

#### How did you install ripgrep?

I built from source (see commit # above) using the instructions in the README.  There were no problems during the build.  I then moved rg to my /usr/local/bin folder, but the problems are also evident when I run it in the target/release/ folder.

I am using rustc 1.41.0 (5e1a79984 2020-01-27)

I have also installed rg from the debian package(same version), and observed the same results.

#### What operating system are you using ripgrep on?

I am running Ubuntu 18.04 on Windows Subsystem for Linux.

Windows info:  10.0.18363 Build 18363
Ubuntu info: Ubuntu 18.04.3 LTS

#### Describe your question, feature request, or bug.

I think I am experiencing the same issue described in Issue #414, but seeing as that has been closed I figured I would add what information I have in a new issue.

I get no search results unless I specify full file paths (a folder path returns no results).

#### If this is a bug, what are the steps to reproduce the behavior?

I have a git repo for an embedded C project which is fairly typical.  The gitignore of the project is nothing unusual.  It only excludes files with specific extensions (makefiles, object files, and other stuff from Eclipse).  It only excludes build folders.

I have a .global_gitignore in my home directory:
```
*
cscope_update
cscope_out
Session.vim
tags
```

I commonly have local files in the topmost directory (build/flashing scripts, random one-off utility scripts, temp files, and other garbage I don't want committed), so the wildcard keeps them from showing up without me having to worry about naming.  With git itself I never have a problem, because I just add the files that do belong and everything works fine after that (rarely, if ever, is a file added in the topmost folder of the project).

The wildcard ignore in the .gitignore_global file appears to be the culprit here.

#### If this is a bug, what is the actual behavior?

When I have the "*" in the global_gitignore no search results are given when searching for any text.  If I remove the wildcard then I get results as expected.  Also, if I use --no-ignore-global I get results as expected.

I also noticed that if I removed the .git/ folder from the project I would get results, even when .gitignore files are present (I'm assuming that you only look for them when you see that you are in a repo).

With my gitignore rules in place I can only get results when I specify the full path to each file (even specifying a subdirectory returns nothing).

After I saw this problem I decided to run the tests, and found that there are several failing tests when I run "cargo test --all".  All seven of the failures are when running "target/debug/deps/ignore-e88254c05595e4dc" (

The output is fairly large, so I have only included the "failures:" sections below:

```
---- dir::tests::gitignore stdout ----
thread 'dir::tests::gitignore' panicked at 'assertion failed: ig.matched("baz", false).is_none()', crates/ignore/src/dir.rs:908:9

---- dir::tests::git_exclude stdout ----
thread 'dir::tests::git_exclude' panicked at 'assertion failed: ig.matched("baz", false).is_none()', crates/ignore/src/dir.rs:895:9
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace.

---- dir::tests::gitignore_allowed_no_git stdout ----
thread 'dir::tests::gitignore_allowed_no_git' panicked at 'assertion failed: ig.matched("baz", false).is_none()', crates/ignore/src/dir.rs:935:9

---- dir::tests::absolute_parent_anchored stdout ----
thread 'dir::tests::absolute_parent_anchored' panicked at 'assertion failed: ig1.matched("llvm", true).is_none()', crates/ignore/src/dir.rs:1136:9

---- dir::tests::stops_at_git_dir stdout ----
thread 'dir::tests::stops_at_git_dir' panicked at 'assertion failed: ig2.matched("foo", false).is_none()', crates/ignore/src/dir.rs:1094:9

---- walk::tests::gitignore_parent stdout ----
thread 'walk::tests::gitignore_parent' panicked at 'assertion failed: `(left == right)`
  left: `[]`,
 right: `["bar"]`: single threaded', crates/ignore/src/walk.rs:1878:9

---- walk::tests::gitignore stdout ----
thread 'walk::tests::gitignore' panicked at 'assertion failed: `(left == right)`
  left: `[]`,
 right: `["a", "a/bar", "bar"]`: single threaded', crates/ignore/src/walk.rs:1878:9

failures:
    dir::tests::absolute_parent_anchored
    dir::tests::git_exclude
    dir::tests::gitignore
    dir::tests::gitignore_allowed_no_git
    dir::tests::stops_at_git_dir
    walk::tests::gitignore
    walk::tests::gitignore_parent

```

#### If this is a bug, what is the expected behavior?

I believe that this is a bug in that the files I desire to search are not ignored by git itself with my combination of local and global gitignore files.  My guess is that the * is being applied recursively instead of only on the top level folder in the repository.

It looks like the test cases may be checking for the correct behavior, but I am not yet knowledgeable enough about rust to know what is going on.

---

_Comment by @BurntSushi on 2020-02-20 14:46_

This is expected behavior. ripgrep cannot behave like git in all respects. Your `.gitignore` is, as you point out, explicitly ignoring everything. So ripgrep follows that rule. The `*` does not just apply to top-level things; it applies everywhere. This is different than how you're used to using `git`, because `git` will more intelligently interpret your ignore rules with respect to the files and directories that are actually committed in your repo. ripgrep does not know which files are actually committed, so its only choice is to respect your gitignore as written.

There are some work arounds available to you:

* Do something like `echo '!*' > $HOME/.ignore` to whitelist everything, although this may have unintended consequences.
* The most robust work around is to make your `.gitignore` represent more accurately the files you are actually tracking. I used to have the same problem as you. Instead, I removed my `$HOME/.gitignore` and put this in `$HOME/.git/config`:

```
[status]
  showUntrackedFiles = no
```

And that mostly solved the problem I was trying to solve before using `.gitignore`.

Either way, this bug is a `wontfix` because ripgrep is probably never going to know which files are actually being tracked.

---

_Closed by @BurntSushi on 2020-02-20 14:46_

---

_Label `wontfix` added by @BurntSushi on 2020-02-20 14:46_

---

_Comment by @BurntSushi on 2020-02-20 14:47_

Also, the failing tests are possibly due to your `.gitignore` rules being applied, since the tests are perhaps not sufficiently isolated.

---

_Comment by @kashperanto on 2020-02-20 17:03_

@BurntSushi Hey, thanks for the quick response.  I should have double-checked the documentation for the behavior of * before posting... (it's been a few years since I added that hack to ignore all of the junk).  It's slowly coming back to me now.

I think I will update my setup to use your suggestion (that is more in line with my intent, anyway).  Hopefully people will find this Issue if they have the same problem, because I am almost positive I found that as a solution on stackoverflow for dealing with unwanted untracked files.

We are much better now, but when we first started using git where I work people were not very good about using .gitignore files without wiping mine out, so I was constantly getting walls of untracked file reports.  Now we're to the point where that wildcard isn't really necessary anymore.

I didn't even think to check if the tests were affected by the gitignore file.  With the * rule removed all but one test is now passing (relating to json).  I'm not sure if that warrants another bug or not:

```
failures:

---- json::notutf8 stdout ----
thread 'json::notutf8' panicked at 'assertion failed: `(left == right)`
  left: `Begin { path: Some(Text { text: "foo�bar" }) }`,
 right: `Begin { path: Some(Bytes { bytes: "Zm9v/2Jhcg==" }) }`', tests/json.rs:221:5
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace.
```

---

_Comment by @BurntSushi on 2020-02-20 17:13_

With respect to the failing test, I don't know what's going on there. Tests pass on my machine and CI, so I think for it to be actionable bug report, it would need to be reproducible. The way to do that would be to find a problem with actually using `rg --json`.

---

_Comment by @kashperanto on 2020-02-21 20:42_

@BurntSushi The problem seems to be something to do with unicode specifically.  If I knew the intent of the test I might be able to recreate something on my system, but for my actual usage it seems to be working with --json.  I'll add a new issue if I ever figure out what's going on.

Thanks for the help, and for this awesome tool.

---

_Comment by @BurntSushi on 2020-02-21 21:06_

Aye. Basically, that test is checking that the proper JSON is generated if it comes across a file path that is not valid UTF-8. Namely, for the invalid UTF-8 case, ripgrep should serialize the file path in base64. It looks like for whatever reason, when you ran the test, the file path was somehow "fixed" where the invalid UTF-8 portion of the path was replaced with the Unicode replacement codepoint. Not sure how that could happen. I might ascribe this to "a Windows" thing, but I run tests on Windows too. So I'm still a little mystified!

---

_Comment by @kashperanto on 2020-02-22 01:35_

Interesting...I wonder if this might be some path magic related to the interop between WSL and Windows itself?  Or perhaps some of the dependencies behave differently on WSL Ubuntu than on Windows or Ubuntu proper.

So it sounds like this would be a test error as opposed to a true failure.  

---
