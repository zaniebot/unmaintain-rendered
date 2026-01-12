```yaml
number: 1734
title: Negate an entry in .gitignore
type: issue
state: closed
author: zachriggle
labels:
  - invalid
assignees: []
created_at: 2020-11-18T01:43:29Z
updated_at: 2020-11-20T02:06:18Z
url: https://github.com/BurntSushi/ripgrep/issues/1734
synced_at: 2026-01-12T16:13:24Z
```

# Negate an entry in .gitignore

---

_@zachriggle_

I would like the ability to instruct `ripgrep` to search a directory, even though it's in a repo's `.gitignore`.  I know I can ignore the *entire* ignore file, but there's really just one directory that I want to omit.

There are several workarounds below, but none of them work ideally for my workflow.

## Remove it from the .gitignore

I can remove the line from my local copy, but it would be better for my `ripgrep` tooling / process to be able to descend into that directory.  This also means that other users of my tooling would also have to modify the `.gitignore`.

## Specify the directory explicitly

 I know I can also explicitly search that directory by providing it on the command-line, but this negates all of the other e.g. `.rgignore` and e.g. `-t c` flags.

## Specify its negation in an `--ignore-file`

This would be ideal, but it appears that this doesn't work.

## Use `--no-ignore-vcs`

This works, but affects the entire `.gitignore` file, while I only care about one specific entry.

## Perform the search from outside the repo

This works, but is not ideal.  If I have `foo/myrepo/.gitignore`, performing the search from `foo` does not pull in the .gitignore file, and the search works as I'd like it to.

---

_Comment by @BurntSushi on 2020-11-18 01:55_

>  If I have foo/myrepo/.gitignore, performing the search from foo does not pull in the .gitignore file, and the search works as I'd like it to.

That sounds like a bug to me. ripgrep should still respect your gitignore in that case. Could you please file a new bug with a test case that I can reproduce?

As for this issue, just put `!dir` in a `.ignore` or `.rgignore` file to whitelist it. If that doesn't work, then please provide a reproducible test case.

Your `--ignore-file` approach probably isn't working because local gitignore rules take precedent. But `.ignore` and `.rgignore` take precedent over `.gitignore`.

---

_Label `question` added by @BurntSushi on 2020-11-18 01:55_

---

_Comment by @zachriggle on 2020-11-18 01:59_

This is pretty simple to repro, here you go:

*EDIT*: Realized this isn't what you're looking for, see second post.  This shows how I would like this Feature Request to accomplish, though.

```
$ git init
Initialized empty Git repository in ...

$ cat .gitignore
ignored-dir

$ cat .rgiginore
!ignored-dir

$ cat ignored-dir/foo.c
hello

$ tree
.
├── [   6]  foo.c
└── [  96]  ignored-dir
    └── [   6]  foo.c

1 directory, 2 files

$ rg hello
foo.c
1:hello

$ rg --no-ignore-vcs hello
ignored-dir/foo.c
1:hello

foo.c
1:hello
```

---

_Comment by @zachriggle on 2020-11-18 02:14_

Ah I see, you wanted me to repro something else entirely.  I can't repro it with a basic case, unfortunately.



---

_Comment by @BurntSushi on 2020-11-18 17:34_

But what you posted was what I was looking for? I asked for two reproductions. One of them was about ripgrep ignoring `.gitignore` when a search starts outside a git directory. The other was whether a whitelisted directory in an `.rgignore` file worked or not. I can't reproduce it though:

```
$ tree
.
├── foo.c
└── ignored-dir
    └── foo.c

1 directory, 2 files

$ cat .gitignore
ignored-dir

$ cat .rgignore
!ignored-dir

$ rg hello
foo.c
1:hello

ignored-dir/foo.c
1:hello
```

ripgrep is, as far as I can tell, behaving not only as expected, but is doing exactly what you want. Putting `ignored-dir` in the `.rgignore` file whitelists `ignored-dir` and causes ripgrep to search it.

So either there is more info about your environment that you've left out (I don't see `rg --version` or `rg --debug` output anywhere, for example), or I don't understand what you're asking for.

---

_Comment by @zachriggle on 2020-11-18 21:13_

Look a bit closer, `ignored-dir/foo.c` is not discovered unless `--no-ignore-vcs` is passed.  The behavior you described is indeed what I want, but not what happens on my end.  I do see that it's working as-desired in your example.

I've attached my test directory, and version information.

```
$ rg --version
ripgrep 12.1.1
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
```

I've attached the exact working directory I have.

[issue-1734.zip](https://github.com/BurntSushi/ripgrep/files/5562752/issue-1734.zip)

```
$ cd issue-1734
$ rg hello
foo.c
1:hello
```

It shouldn't make any difference, but this is happening on macOS.

---

_Comment by @BurntSushi on 2020-11-18 22:10_

> Look a bit closer, `ignored-dir/foo.c` is not discovered unless `--no-ignore-vcs` is passed. The behavior you described is indeed what I want, but not what happens on my end.

Yes, I saw that. Now look at my transcript. `ignored-dir/foo.c` is discovered _without_ passing `--no-ignore-vcs`. I presumed that was the point.

As for your zip file, it's not a ripgrep problem. It's a user error:

```
$ rg hello
foo.c
1:hello

$ mv .rgiginore .rgignore

$ rg hello
foo.c
1:hello

ignored-dir/foo.c
1:hello
```

You misspelled `.rgignore`.

---

_Closed by @BurntSushi on 2020-11-18 22:10_

---

_Label `question` removed by @BurntSushi on 2020-11-18 22:10_

---

_Label `invalid` added by @BurntSushi on 2020-11-18 22:10_

---

_Comment by @zachriggle on 2020-11-19 07:34_

Now I feel dumb.  Thanks for setting me straight, I appreciate your time <3 

---

_Comment by @zachriggle on 2020-11-19 23:05_

Following up on this, your example does work (I had a misnamed .rgignore), however the negation does NOT work when using `--ignore-file`.

Using the same example as above, with the corrected filename:

```
$ rg hello
ignored-dir/foo.c
1:hello

foo.c
1:hello

$ mv .rgignore rgignore

$ rg hello --ignore-file ./rgignore
foo.c
1:hello
```

---

_Comment by @BurntSushi on 2020-11-20 00:22_

Right. As I said, `--ignore-file` has lower precedence than `.gitignore`, so the ignore rule in the gitignore wins. The `--ignore-file` flag is meant for global ignore rules which can be overridden by more specific rules in your directory tree. If you need higher precdent rules, use the `-g/--glob` flag.

---

_Comment by @zachriggle on 2020-11-20 01:19_

I missed the precedence comment, thanks for reiterating why it doesn't work.

As a follow-up, is there a *reason* that `.gitignore` takes precedence over `--ignore-file`, but not `.rgignore`?  I'd expect `.rgignore` to just be a "default" `--ignore-file`.

---

_Comment by @BurntSushi on 2020-11-20 02:06_

The reason is that it's the scheme I chose. `--ignore-file`, as I said, is meant to be a set of global rules given to every invocation, usually via a config file or an alias. `.gitignore` is the approximate "ground truth." `.rgignore` is meant to be a way to tailor it even further on a _per directory_ basis.

There is also an `.ignore` file, which is meant to be an application agnostic way to added gitignore overrides. `.rgignore` is meant to be specific to ripgrep.

Think about it this way. Let's say you built a search tool that respected your gitignore rules. What's the _first_ thing you do? Well, you provide a way to override those gitignore rules because you might want to search something that is ignored by git. That's what `.ignore`/`.rgignore` are. The `--ignore-file` flag came much later to satisfy a more niche use case.

---
