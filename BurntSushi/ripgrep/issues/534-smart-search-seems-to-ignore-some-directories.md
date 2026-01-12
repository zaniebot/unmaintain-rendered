```yaml
number: 534
title: Smart search seems to ignore some directories
type: issue
state: closed
author: ltratt
labels: []
assignees: []
created_at: 2017-06-30T09:53:56Z
updated_at: 2017-06-30T13:29:23Z
url: https://github.com/BurntSushi/ripgrep/issues/534
synced_at: 2026-01-12T16:13:22Z
```

# Smart search seems to ignore some directories

---

_@ltratt_

I get confusing behaviour (at least to me!) with the default settings, where a directory doesn't appear to be searched for non-obvious reasons:

```
$ pwd
/home/ltratt/w/pliss/llvm
$ rg IndexVec src
src/lib/Analysis/LoopAccessAnalysis.cpp
1498:  auto &IndexVector = Accesses.find(Access)->second;
1501:  transform(IndexVector,
$ rg IndexVec .
$ 
```

Note that I haven't changed directory between those two searches: I've simply specified either a sub-dir or the current dir. There doesn't appear to be anything about the current directory that would suggest that things shouldn't be searched:

```
$ ls -la
total 32
drwxr-xr-x   4 ltratt  ltratt   512 May 19 07:22 .
drwxr-xr-x   4 ltratt  ltratt   512 May 19 08:25 ..
drwxr-xr-x  15 ltratt  ltratt   512 May 19 07:23 release
drwxr-xr-x  16 ltratt  ltratt  1024 May 19 07:21 src
$
```

There is a `.gitignore` file in `src/` but a) it doesn't mention anything in the `lib/` subdir b) I'm not sure why specifying `.` vs `src` would make any difference.

It's possible that I'm simply being an idiot with respect to my expectations for smart search, but I must admit that I'm a bit baffled! Happens with both ripgrep 0.5.2 and ripgrep b6f1e5db. [FWIW, `ag` doesn't seem to suffer from the same issue, but it may have different smart searching rules.]

---

_Comment by @BurntSushi on 2017-06-30 10:54_

Given what you've told me, this also seems weird, but there actually isn't enough information here to know for sure:

1. Is the code your searching publicly accessible so that others can try to reproduce it?
2. Can you show the output of both of your `rg` commands, but with the `--debug` flag given?
3. Do you have any global gitignore settings? e.g., In `$HOME/.config/git/ignore`? Or do you have any repo wide gitignore settings? e.g., In `./.git/info/exclude`? (ripgrep will read both of these.)

Invariably, the `--debug` flag will spew a bunch of output, but it should tell you why specific things are being ignored.

---

_Comment by @ltratt on 2017-06-30 11:53_

Thanks for the quick reply, especially as it's probably something stupid that I'm doing!

1) It's the LLVM repository (or a minor fork of it IIRC), although if I clone it in `/tmp/` I don't get this behaviour. If it's useful I can probably work out how to get you a copy.

2) Here's both commands with `--debug`:
```
$ pwd
/home/ltratt/w/pliss/llvm
$ rg --debug IndexVec .
DEBUG:grep::search: regex ast:
Literal {
    chars: [
        'I',
        'n',
        'd',
        'e',
        'x',
        'V',
        'e',
        'c'
    ],
    casei: false
}
DEBUG:grep::literals: literal prefixes detected: Literals { lits: [Complete(IndexVec)], limit_size: 250, limit_class: 10 }
DEBUG:globset: glob converted to regex: Glob { glob: "**/*.sw[klmnop]", re: "(?-u)^(?:/?|.*/).*\\.sw[klmnop]$", opts: GlobOptions { case_insensitive: false, literal_separator: false }, tokens: Tokens([RecursivePrefix, ZeroOrMore, Literal('.'), Literal('s'), Literal('w'), Class { negated: false, ranges: [('k', 'k'), ('l', 'l'), ('m', 'm'), ('n', 'n'), ('o', 'o'), ('p', 'p')] }]) }
DEBUG:globset: built glob set; 0 literals, 0 basenames, 1 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 1 regexes
DEBUG:globset: glob converted to regex: Glob { glob: "**/.DCOPserver*", re: "(?-u)^(?:/?|.*/)\\.DCOPserver.*$", opts: GlobOptions { case_insensitive: false, literal_separator: false }, tokens: Tokens([RecursivePrefix, Literal('.'), Literal('D'), Literal('C'), Literal('O'), Literal('P'), Literal('s'), Literal('e'), Literal('r'), Literal('v'), Literal('e'), Literal('r'), ZeroOrMore]) }
DEBUG:globset: glob converted to regex: Glob { glob: "**/.zsh_history*", re: "(?-u)^(?:/?|.*/)\\.zsh_history.*$", opts: GlobOptions { case_insensitive: false, literal_separator: false }, tokens: Tokens([RecursivePrefix, Literal('.'), Literal('z'), Literal('s'), Literal('h'), Literal('_'), Literal('h'), Literal('i'), Literal('s'), Literal('t'), Literal('o'), Literal('r'), Literal('y'), ZeroOrMore]) }
DEBUG:globset: glob converted to regex: Glob { glob: "**/.recently-used*", re: "(?-u)^(?:/?|.*/)\\.recently\\-used.*$", opts: GlobOptions { case_insensitive: false, literal_separator: false }, tokens: Tokens([RecursivePrefix, Literal('.'), Literal('r'), Literal('e'), Literal('c'), Literal('e'), Literal('n'), Literal('t'), Literal('l'), Literal('y'), Literal('-'), Literal('u'), Literal('s'), Literal('e'), Literal('d'), ZeroOrMore]) }
DEBUG:globset: glob converted to regex: Glob { glob: "**/.serverauth.*", re: "(?-u)^(?:/?|.*/)\\.serverauth\\..*$", opts: GlobOptions { case_insensitive: false, literal_separator: false }, tokens: Tokens([RecursivePrefix, Literal('.'), Literal('s'), Literal('e'), Literal('r'), Literal('v'), Literal('e'), Literal('r'), Literal('a'), Literal('u'), Literal('t'), Literal('h'), Literal('.'), ZeroOrMore]) }
DEBUG:globset: glob converted to regex: Glob { glob: "**/.texlive_201*", re: "(?-u)^(?:/?|.*/)\\.texlive_201.*$", opts: GlobOptions { case_insensitive: false, literal_separator: false }, tokens: Tokens([RecursivePrefix, Literal('.'), Literal('t'), Literal('e'), Literal('x'), Literal('l'), Literal('i'), Literal('v'), Literal('e'), Literal('_'), Literal('2'), Literal('0'), Literal('1'), ZeroOrMore]) }
DEBUG:globset: built glob set; 0 literals, 166 basenames, 1 extensions, 0 prefixes, 1 suffixes, 0 required extensions, 5 regexes
DEBUG:ignore::walk: ignoring ./src: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("/home/ltratt/.gitignore"), original: "src", actual: "**/src", is_whitelist: false, is_only_dir: false })))
DEBUG:ignore::walk: ignoring ./release/bin: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("/home/ltratt/.gitignore"), original: "bin", actual: "**/bin", is_whitelist: false, is_only_dir: false })))
DEBUG:ignore::walk: ignoring ./release/utils/unittest/CMakeFiles/gtest.dir/googletest/src: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("/home/ltratt/.gitignore"), original: "src", actual: "**/src", is_whitelist: false, is_only_dir: false })))
DEBUG:ignore::walk: ignoring ./release/utils/unittest/CMakeFiles/gtest.dir/googlemock/src: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("/home/ltratt/.gitignore"), original: "src", actual: "**/src", is_whitelist: false, is_only_dir: false })))
$ rg --debug IndexVec src
DEBUG:grep::search: regex ast:
Literal {
    chars: [
        'I',
        'n',
        'd',
        'e',
        'x',
        'V',
        'e',
        'c'
    ],
    casei: false
}
DEBUG:grep::literals: literal prefixes detected: Literals { lits: [Complete(IndexVec)], limit_size: 250, limit_class: 10 }
DEBUG:globset: glob converted to regex: Glob { glob: "**/*.sw[klmnop]", re: "(?-u)^(?:/?|.*/).*\\.sw[klmnop]$", opts: GlobOptions { case_insensitive: false, literal_separator: false }, tokens: Tokens([RecursivePrefix, ZeroOrMore, Literal('.'), Literal('s'), Literal('w'), Class { negated: false, ranges: [('k', 'k'), ('l', 'l'), ('m', 'm'), ('n', 'n'), ('o', 'o'), ('p', 'p')] }]) }
DEBUG:globset: built glob set; 0 literals, 0 basenames, 1 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 1 regexes
DEBUG:globset: glob converted to regex: Glob { glob: "**/.DCOPserver*", re: "(?-u)^(?:/?|.*/)\\.DCOPserver.*$", opts: GlobOptions { case_insensitive: false, literal_separator: false }, tokens: Tokens([RecursivePrefix, Literal('.'), Literal('D'), Literal('C'), Literal('O'), Literal('P'), Literal('s'), Literal('e'), Literal('r'), Literal('v'), Literal('e'), Literal('r'), ZeroOrMore]) }
DEBUG:globset: glob converted to regex: Glob { glob: "**/.zsh_history*", re: "(?-u)^(?:/?|.*/)\\.zsh_history.*$", opts: GlobOptions { case_insensitive: false, literal_separator: false }, tokens: Tokens([RecursivePrefix, Literal('.'), Literal('z'), Literal('s'), Literal('h'), Literal('_'), Literal('h'), Literal('i'), Literal('s'), Literal('t'), Literal('o'), Literal('r'), Literal('y'), ZeroOrMore]) }
DEBUG:globset: glob converted to regex: Glob { glob: "**/.recently-used*", re: "(?-u)^(?:/?|.*/)\\.recently\\-used.*$", opts: GlobOptions { case_insensitive: false, literal_separator: false }, tokens: Tokens([RecursivePrefix, Literal('.'), Literal('r'), Literal('e'), Literal('c'), Literal('e'), Literal('n'), Literal('t'), Literal('l'), Literal('y'), Literal('-'), Literal('u'), Literal('s'), Literal('e'), Literal('d'), ZeroOrMore]) }
DEBUG:globset: glob converted to regex: Glob { glob: "**/.serverauth.*", re: "(?-u)^(?:/?|.*/)\\.serverauth\\..*$", opts: GlobOptions { case_insensitive: false, literal_separator: false }, tokens: Tokens([RecursivePrefix, Literal('.'), Literal('s'), Literal('e'), Literal('r'), Literal('v'), Literal('e'), Literal('r'), Literal('a'), Literal('u'), Literal('t'), Literal('h'), Literal('.'), ZeroOrMore]) }
DEBUG:globset: glob converted to regex: Glob { glob: "**/.texlive_201*", re: "(?-u)^(?:/?|.*/)\\.texlive_201.*$", opts: GlobOptions { case_insensitive: false, literal_separator: false }, tokens: Tokens([RecursivePrefix, Literal('.'), Literal('t'), Literal('e'), Literal('x'), Literal('l'), Literal('i'), Literal('v'), Literal('e'), Literal('_'), Literal('2'), Literal('0'), Literal('1'), ZeroOrMore]) }
DEBUG:globset: built glob set; 0 literals, 166 basenames, 1 extensions, 0 prefixes, 1 suffixes, 0 required extensions, 5 regexes
DEBUG:globset: glob converted to regex: Glob { glob: "**/.*.sw?", re: "(?-u)^(?:/?|.*/)\\..*\\.sw.$", opts: GlobOptions { case_insensitive: false, literal_separator: false }, tokens: Tokens([RecursivePrefix, Literal('.'), ZeroOrMore, Literal('.'), Literal('s'), Literal('w'), Any]) }
DEBUG:globset: glob converted to regex: Glob { glob: "**/.sw?", re: "(?-u)^(?:/?|.*/)\\.sw.$", opts: GlobOptions { case_insensitive: false, literal_separator: false }, tokens: Tokens([RecursivePrefix, Literal('.'), Literal('s'), Literal('w'), Any]) }
DEBUG:globset: glob converted to regex: Glob { glob: "**/projects/*", re: "(?-u)^(?:/?|.*/)projects/[^/]*$", opts: GlobOptions { case_insensitive: false, literal_separator: true }, tokens: Tokens([RecursivePrefix, Literal('p'), Literal('r'), Literal('o'), Literal('j'), Literal('e'), Literal('c'), Literal('t'), Literal('s'), Literal('/'), ZeroOrMore]) }
DEBUG:globset: glob converted to regex: Glob { glob: "**/projects/*.*", re: "(?-u)^(?:/?|.*/)projects/[^/]*\\.[^/]*$", opts: GlobOptions { case_insensitive: false, literal_separator: true }, tokens: Tokens([RecursivePrefix, Literal('p'), Literal('r'), Literal('o'), Literal('j'), Literal('e'), Literal('c'), Literal('t'), Literal('s'), Literal('/'), ZeroOrMore, Literal('.'), ZeroOrMore]) }
DEBUG:globset: glob converted to regex: Glob { glob: "**/runtimes/*", re: "(?-u)^(?:/?|.*/)runtimes/[^/]*$", opts: GlobOptions { case_insensitive: false, literal_separator: true }, tokens: Tokens([RecursivePrefix, Literal('r'), Literal('u'), Literal('n'), Literal('t'), Literal('i'), Literal('m'), Literal('e'), Literal('s'), Literal('/'), ZeroOrMore]) }
DEBUG:globset: glob converted to regex: Glob { glob: "**/runtimes/*.*", re: "(?-u)^(?:/?|.*/)runtimes/[^/]*\\.[^/]*$", opts: GlobOptions { case_insensitive: false, literal_separator: true }, tokens: Tokens([RecursivePrefix, Literal('r'), Literal('u'), Literal('n'), Literal('t'), Literal('i'), Literal('m'), Literal('e'), Literal('s'), Literal('/'), ZeroOrMore, Literal('.'), ZeroOrMore]) }
DEBUG:globset: built glob set; 19 literals, 6 basenames, 2 extensions, 0 prefixes, 13 suffixes, 0 required extensions, 6 regexes
DEBUG:ignore::walk: ignoring src/.svn: Ignore(IgnoreMatch(Hidden))
DEBUG:ignore::walk: ignoring src/.clang-format: Ignore(IgnoreMatch(Hidden))
DEBUG:ignore::walk: ignoring src/.gitignore: Ignore(IgnoreMatch(Hidden))
DEBUG:ignore::walk: ignoring src/.clang-tidy: Ignore(IgnoreMatch(Hidden))
DEBUG:ignore::walk: ignoring src/.arcconfig: Ignore(IgnoreMatch(Hidden))
DEBUG:ignore::walk: ignoring src/test/.clang-format: Ignore(IgnoreMatch(Hidden))
DEBUG:ignore::walk: whitelisting src/projects/LLVMBuild.txt: Whitelist(IgnoreMatch(Gitignore(Glob { from: Some("src/.gitignore"), original: "!projects/*.*", actual: "**/projects/*.*", is_whitelist: true, is_only_dir: false })))
DEBUG:ignore::walk: whitelisting src/projects/CMakeLists.txt: Whitelist(IgnoreMatch(Gitignore(Glob { from: Some("src/.gitignore"), original: "!projects/*.*", actual: "**/projects/*.*", is_whitelist: true, is_only_dir: false })))
DEBUG:ignore::walk: whitelisting src/runtimes/Components.cmake.in: Whitelist(IgnoreMatch(Gitignore(Glob { from: Some("src/.gitignore"), original: "!runtimes/*.*", actual: "**/runtimes/*.*", is_whitelist: true, is_only_dir: false })))
DEBUG:ignore::walk: whitelisting src/runtimes/CMakeLists.txt: Whitelist(IgnoreMatch(Gitignore(Glob { from: Some("src/.gitignore"), original: "!runtimes/*.*", actual: "**/runtimes/*.*", is_whitelist: true, is_only_dir: false })))
DEBUG:ignore::walk: ignoring src/utils/unittest/googlemock/src: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("/home/ltratt/.gitignore"), original: "src", actual: "**/src", is_whitelist: false, is_only_dir: false })))
DEBUG:ignore::walk: ignoring src/utils/unittest/googletest/src: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("/home/ltratt/.gitignore"), original: "src", actual: "**/src", is_whitelist: false, is_only_dir: false })))
DEBUG:ignore::walk: ignoring src/utils/llvm-build/llvmbuild/__init__.pyc: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("src/.gitignore"), original: "*.pyc", actual: "**/*.pyc", is_whitelist: false, is_only_dir: false })))
DEBUG:ignore::walk: ignoring src/utils/llvm-build/llvmbuild/main.pyc: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("src/.gitignore"), original: "*.pyc", actual: "**/*.pyc", is_whitelist: false, is_only_dir: false })))
DEBUG:ignore::walk: ignoring src/utils/llvm-build/llvmbuild/componentinfo.pyc: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("src/.gitignore"), original: "*.pyc", actual: "**/*.pyc", is_whitelist: false, is_only_dir: false })))
DEBUG:ignore::walk: ignoring src/utils/llvm-build/llvmbuild/util.pyc: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("src/.gitignore"), original: "*.pyc", actual: "**/*.pyc", is_whitelist: false, is_only_dir: false })))
DEBUG:ignore::walk: ignoring src/utils/llvm-build/llvmbuild/configutil.pyc: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("src/.gitignore"), original: "*.pyc", actual: "**/*.pyc", is_whitelist: false, is_only_dir: false })))
DEBUG:ignore::walk: ignoring src/utils/lit/tests/.coveragerc: Ignore(IgnoreMatch(Hidden))
src/lib/Analysis/LoopAccessAnalysis.cpp
1498:  auto &IndexVector = Accesses.find(Access)->second;
1501:  transform(IndexVector,
$
```

3) I do have a `.gitignore_global`:
```
$ cat /home/ltratt/.gitignore_global 
*.sw[klmnop]
*.bak
$
```

---

_Comment by @BurntSushi on 2017-06-30 12:35_

Right, OK, so if you carefully scrutinize the debug output of the first command, you'll see this:

```
DEBUG:ignore::walk: ignoring ./src: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("/home/ltratt/.gitignore"), original: "src", actual: "**/src", is_whitelist: false, is_only_dir: false })))
```

Basically, that says `./src` is being ignored because your `/home/ltratt/.gitignore` file contains an entry called `src`. The main problem with "smart search" in my experience is that if you're in a directory that has an ancestor with a `.gitignore`, then that `.gitignore` file is applied to all children directories. Note though, that this only applies to child directories that are themselves not `.git` checkouts. Is your llvm source a git clone with its own `.git` directory?

---

_Comment by @BurntSushi on 2017-06-30 12:36_

This also explains why a checkout in `/tmp` doesn't exhibit this behavior, since `/home/ltratt` isn't an ancestor of `/tmp`.

Also, the silver searcher doesn't exhibit this behavior because it doesn't actually implement the `gitignore` rules correctly.

---

_Comment by @ltratt on 2017-06-30 13:16_

Ug, I should have guessed! No, the LLVM dir is a download not a clone (so no `.git` dir).

So one final question before I close this issue and hang my head in shame. The gitignore check only ignores files that are children of things I pass on the command-line? i.e. if I do `rg b searchstring` then `b` is searched regardless of whether it's referenced in an upper `.gitignore` file or not. But if my directory structure is `a/b` and I do `rg a searchstring` then `b` won't be searched if it's referenced in an upper `.gitignore` file? [I think this is perfectly reasonable behaviour, but it's not something that has a direct analogy in git itself, so I thought it might be worth checking!]

---

_Comment by @BurntSushi on 2017-06-30 13:22_

> hang my head in shame

You really shouldn't! You aren't the first person to struggle with this. There is even a special cased error message in ripgrep that gets triggered when you run `rg pattern` and no files get searched. You didn't happen to trip it in this case, but if you did, the error message suggests running with `--debug`, which usually clears up the problem if you look at the output closely enough. This particular failure mode occurs because lots of folks---myself included---put their `$HOME` in git, and use `.gitignore` to ignore everything by default and un-ignore certain things. But if you search in a non-git sub-directory of `$HOME` (like you are), then `$HOME/.gitignore` is still in play, and it's probably the case that everything gets ignored. (The first line of my `$HOME/.gitignore` is `*`.)

> The gitignore check only ignores files that are children of things I pass on the command-line?

Correct. The idea here is that if you explicitly give a file path, then you are expressing knowledge that should supersede any other "automatic" smarts saying that something should be ignored.

But yeah, smart searching is nice until it's *too* smart, and then it just becomes opaque and frustrating. It's a tough balance. Ideas for making failure modes more explicit would be most welcome. :-)

---

_Comment by @ltratt on 2017-06-30 13:29_

Thanks for the detailed explanation! I must admit, I have absolutely no idea what a good failure message is. In an ideal world, I suppose it would be preferable if `.gitignore` anchored patterns to the directory the `.gitignore` file is in by default, but that ship has long sailed. If I think of anything, I'll shout. But, until then, I'll close this issue and thank you again for your time!

---

_Closed by @ltratt on 2017-06-30 13:29_

---
