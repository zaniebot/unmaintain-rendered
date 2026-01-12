```yaml
number: 1049
title: "-z archive search option does not work for foo.gz files"
type: issue
state: closed
author: kaushalmodi
labels:
  - question
assignees: []
created_at: 2018-09-12T18:44:45Z
updated_at: 2018-09-12T21:28:30Z
url: https://github.com/BurntSushi/ripgrep/issues/1049
synced_at: 2026-01-12T16:13:22Z
```

# -z archive search option does not work for foo.gz files

---

_@kaushalmodi_

#### What version of ripgrep are you using?

ripgrep 0.10.0 (rev 7ebed3ace6)
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)

#### How did you install ripgrep?

Built from git repo.

#### What operating system are you using ripgrep on?

RHEL 6.8

#### Describe your question, feature request, or bug.

I tried searching an archive file (help-mode.el.gz) for the first time today, but it didn't work.

#### If this is a bug, what are the steps to reproduce the behavior?

1. Download the attached (see below) `help-mode.el.gz` file (it's from a regular Emacs build) to a dir of your choice.
2. cd to that dir
3. `rg -z 'easymenu'`.
  - I also tried `rg -z -a 'easymenu' help-mode.el.gz`. But that didn't work either.
4. Nothing will be returned.

#### If this is a bug, what is the actual behavior?

`help-mode.el.gz` is a .gz of `help-mode.el` which contains this line on line 34: `(eval-when-compile (require 'easymenu))`.

I was expecting the `rg -z` to find that line, but it didn't. 

```
km²~/temp/:> rg -z -a 'easymenu' help-mode.el.gz --debug

DEBUG|grep_regex::literal|grep-regex/src/literal.rs:100: required literals found: [Cut(EASY), Cut(eASY), Cut(EaSY), Cut(eaSY), Cut(EAsY), Cut(eAsY), Cut(EasY), Cut(easY), Cut(EAſY), Cut(eAſY), Cut(EaſY), Cut(eaſY), Cut(EASy), Cut(eASy), Cut(EaSy), Cut(eaSy), Cut(EAsy), Cut(eAsy), Cut(Easy), Cut(easy), Cut(EAſy), Cut(eAſy), Cut(Eaſy), Cut(eaſy)]
DEBUG|globset|globset/src/lib.rs:429: built glob set; 0 literals, 0 basenames, 8 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|globset/src/lib.rs:424: glob converted to regex: Glob { glob: "**/*.*~", re: "(?-u)^(?:/?|.*/).*\\..*\\~$", opts: GlobOptions { case_insensitive: false, literal_separator: false, backslash_escape: true }, tokens: Tokens([RecursivePrefix, ZeroOrMore, Literal('.'), ZeroOrMore, Literal('~')]) }
DEBUG|globset|globset/src/lib.rs:424: glob converted to regex: Glob { glob: "**/*#*#", re: "(?-u)^(?:/?|.*/).*\\#.*\\#$", opts: GlobOptions { case_insensitive: false, literal_separator: false, backslash_escape: true }, tokens: Tokens([RecursivePrefix, ZeroOrMore, Literal('#'), ZeroOrMore, Literal('#')]) }
DEBUG|globset|globset/src/lib.rs:424: glob converted to regex: Glob { glob: "**/*#*.*#", re: "(?-u)^(?:/?|.*/).*\\#.*\\..*\\#$", opts: GlobOptions { case_insensitive: false, literal_separator: false, backslash_escape: true }, tokens: Tokens([RecursivePrefix, ZeroOrMore, Literal('#'), ZeroOrMore, Literal('.'), ZeroOrMore, Literal('#')]) }
DEBUG|globset|globset/src/lib.rs:424: glob converted to regex: Glob { glob: "**/sos.log*", re: "(?-u)^(?:/?|.*/)sos\\.log.*$", opts: GlobOptions { case_insensitive: false, literal_separator: false, backslash_escape: true }, tokens: Tokens([RecursivePrefix, Literal('s'), Literal('o'), Literal('s'), Literal('.'), Literal('l'), Literal('o'), Literal('g'), ZeroOrMore]) }
DEBUG|globset|globset/src/lib.rs:429: built glob set; 1 literals, 22 basenames, 12 extensions, 0 prefixes, 4 suffixes, 3 required extensions, 4 regexes
DEBUG|grep_cli::decompress|grep-cli/src/decompress.rs:202: help-mode.el.gz: error spawning command '"gzip" "-d" "-c" "help-mode.el.gz"': Not a directory (os error 20) (falling back to uncompressed reader)
```

This seems to be the key:

> DEBUG|grep_cli::decompress|grep-cli/src/decompress.rs:202: help-mode.el.gz: error spawning command '"gzip" "-d" "-c" "help-mode.el.gz"': Not a directory (os error 20) (falling back to uncompressed reader)

#### If this is a bug, what is the expected behavior?

It should have found that line with `easymenu` string.


### Attachments

[help-mode.el.gz](https://github.com/BurntSushi/ripgrep/files/2376479/help-mode.el.gz)

---

**Updates**

- Fix typo *s/easymode/easymenu/*

---

_Comment by @BurntSushi on 2018-09-12 19:59_

Thanks for the detailed bug report! I appreciate you filing it.

I can't reproduce this. In particular, `zcat help-mode.el.gz | rg easymode` returns nothing, so therefore, `rg -z easymode` will necessarily also return nothing.

If I pick something that is in your file, then ripgrep works as expected:

```
$ rg emacs-lisp-mode
$ rg emacs-lisp-mode -z
help-mode.el.gz
466:          (set-syntax-table emacs-lisp-mode-syntax-table)
619:    (with-syntax-table emacs-lisp-mode-syntax-table
```

With that said, I do not get the same error in my `--debug` logs as you with respect to gzip not being a directory. The obvious thing for you to try is to actually run the gzip command that ripgrep is running and debug it from there. For example, `gzip -d -c help-mode.el.gz`. Does that fail? If so, then why? How should ripgrep cause your version of `gzip` to decompress the given file and print the contents to stdout?

---

_Label `question` added by @BurntSushi on 2018-09-12 19:59_

---

_Comment by @kaushalmodi on 2018-09-12 20:05_

> zcat help-mode.el.gz | rg easymode

Sorry, typo in the report *s/easymode/easymenu/*, but the same issue still persists.. I will fix the typo.

That said,

- `zcat help-mode.el.gz | rg easymenu` works.
- `gzip -d -c help-mode.el.gz` does not fail.. it spits out the contents to the stdout

---

_Comment by @kaushalmodi on 2018-09-12 20:07_

![image](https://user-images.githubusercontent.com/3578197/45450364-0173e900-b6a6-11e8-97a3-c7a25f27b0c1.png)


---

_Comment by @kaushalmodi on 2018-09-12 20:08_

May be the gzip handling from inside rg is OS specific? I am on RHEL 6.8.

---

_Comment by @BurntSushi on 2018-09-12 20:13_

I don't see why it would be. The message it spits out to you should be exactly the command that ripgrep runs. I don't know what's going on, sorry. The next step I'd take is to run ripgrep under `strace` and see if you can find the exact `gzip` command that is being run and confirm that everything lines up. If not, then perhpas we've found the problem. If so, then... I'm not sure where else to look.

For giggles, do you know if this worked with ripgrep 0.9.0? If you don't know, can you download the Linux binary from github releases and try it?

---

_Comment by @kaushalmodi on 2018-09-12 20:26_

Thanks for those pointers. Turns out that the rg binary (even v 0.10.0) downloaded from releases works fine! So somehow my local build has this issue..

[Here][1] is my rg build script in bash.. the relevant part (I think) is:

```sh
# Disable debug info to reduce build size (size drops from 25MB to 5.8MB)
# https://github.com/BurntSushi/ripgrep/issues/413
sed -r -i.bkp 's/^(\s*debug\s*=\s*)true/\1false/' Cargo.toml

# Delete all the build directories here, as we need the build directory to
# contain *only one* dir at the end of each build.. so that we can blindly do:
#   cd ./target/x86_64-unknown-linux-musl/release/build/ripgrep-*/out/
# .. as you see later. That way, we don't have to have complicated script to
# figure out which of all the build directories is the latest.
rm -rf ./target/x86_64-unknown-linux-musl/release/build/ripgrep-*

# Build
# https://github.com/BurntSushi/ripgrep/issues/85#issuecomment-249428112
rustup target add x86_64-unknown-linux-musl
RUSTFLAGS="-C target-cpu=native" rustup run stable cargo build --target x86_64-unknown-linux-musl --release
```

Do you see anything wrong in that? I have been building rg using that script for few years! now. It's just that I tried out the `-z` switch and it didn't work.

[1]: https://ptpb.pw/JCSW/bash

---

_Comment by @BurntSushi on 2018-09-12 20:59_

Nothing obviously stands out at me, no. One possibility is that given you're on RHEL 6.8, you might be compiling with a much older version of musl than what is contained in the releases on github. So to rule that out, here are some thoughts:

1. Check which version of musl you're building with. How old is it? If it's old, can you upgrade? If so, does that fix it?
2. Try doing just a normal `cargo build --release` without any other shenanigans, which should build with your local copy of glibc. Does that work?

---

_Comment by @kaushalmodi on 2018-09-12 21:09_

> One possibility is that given you're on RHEL 6.8, you might be compiling with a much older version of musl than what is contained in the releases on github.

That seems to be the problem because my local build with x86_64-unknown-linux-gnu target (default) works fine.

I added the musl target using `rustup target add x86_64-unknown-linux-musl`, but looks like something else is also needed to be installed locally? musl? How do I find my musl version? (sorry for being naive)

---

As the issue is already resolved, I will close this. Thanks for your help!

---

_Closed by @kaushalmodi on 2018-09-12 21:12_

---

_Comment by @BurntSushi on 2018-09-12 21:14_

> I added the musl target using `rustup target add x86_64-unknown-linux-musl`, but looks like something else is also needed to be installed locally? musl? How do I find my musl version? (sorry for being naive)

Right. AFAIK, `rustup target add x86_64-unknown-linux-musl` just adds the Rust bits to use musl, such as Rust's standard library compiled with musl. Honestly, if you're just building this binary for yourself, I would use glibc. The only reason I use musl in the releases is because it creates a fully static binary that's nice for distributing, but it doesn't matter much if you're the only one using your build.

In terms of having musl installed... That's really a question for your Linux distro. You need to use your package manager to discover what, if any, musl package is installed. If you didn't install it with your package manager, then... good luck. :-)

Here's my package info for musl:

```
$ pacman -Qi musl
Name            : musl
Version         : 1.1.20-1
Description     : Lightweight implementation of C standard library
Architecture    : x86_64
URL             : http://www.musl-libc.org/
Licenses        : MIT
Groups          : None
Provides        : None
Depends On      : None
Optional Deps   : None
Required By     : None
Optional For    : None
Conflicts With  : None
Replaces        : None
Installed Size  : 3.58 MiB
Packager        : Sergej Pupykin <pupykin.s+arch@gmail.com>
Build Date      : Thu 06 Sep 2018 04:59:42 PM EDT
Install Date    : Sat 08 Sep 2018 01:35:53 PM EDT
Install Reason  : Explicitly installed
Install Script  : No
Validated By    : Signature
```

And the list of files in my installed musl package, in case that's helpful to you: https://gist.github.com/5c6681c37d7f00d9e2ee32cd5e5170d4

---

_Comment by @kaushalmodi on 2018-09-12 21:19_

Thank you!

I am feeling so dumb at the moment. I never had musl installed :P

I just discovered https://www.musl-libc.org/download.html

So if I reach a deadend there, I'll give up on musl. I like static binaries so I went the musl route to begin with.. I thought it was a "magic switch" that rustc used :)

---

_Comment by @kaushalmodi on 2018-09-12 21:20_

OK, installing musl was easy.. I ended up with https://ptpb.pw/hcY4.

---

_Comment by @BurntSushi on 2018-09-12 21:26_

@kaushalmodi Note that I could be wrong about needing to have musl installed. That might only be necessary if you're building with PCRE2 support.

---

_Comment by @kaushalmodi on 2018-09-12 21:27_

Hmm, `musl-gcc` is installed (musl 1.1.20), its include/ and lib/ dir are along with my other libraries' include/ and lib/ dirs. And still I am getting the same gzip error. Looks like I need to stick with the dynamic binary for now.

---
