```yaml
number: 2778
title: .gitignore rule is matched incorrectly while in a subdir
type: issue
state: closed
author: woess
labels:
  - bug
  - rollup
assignees: []
created_at: 2024-04-10T00:15:54Z
updated_at: 2025-09-20T01:08:22Z
url: https://github.com/BurntSushi/ripgrep/issues/2778
synced_at: 2026-01-12T16:13:24Z
```

# .gitignore rule is matched incorrectly while in a subdir

---

_@woess_

### Please tick this box to confirm you have reviewed the above.

- [X] I have a different issue.

### What version of ripgrep are you using?

ripgrep 14.1.0

### How did you install ripgrep?

cargo install

### What operating system are you using ripgrep on?

Linux

### Describe your bug.

.gitignore rule to ignore `/dir/*.ext` works correctly when running rg from the repo root, but incorrectly ignores `*.ext` in all subdirs when running from `dir`.

### What are the steps to reproduce the behavior? / What is the actual behavior?

```
mkdir /tmp/repro
cd /tmp/repro
git init
mkdir parent
mkdir parent/subdir
echo "/parent/*.txt" > .gitignore
echo "please ignore me" > parent/ignore-me.txt
echo "please don't ignore me" > parent/subdir/dont-ignore-me.txt
```

while in git repo root dir (/tmp/repro), everything is working as expected:
```
$ rg ignore
parent/subdir/dont-ignore-me.txt
1:please don't ignore me
```
but once you `cd` into `./parent`, suddenly the rule is unexpectedly applied to the file in `subdir`, too, and nothing is found:
```
$ cd parent
$ rg ignore
rg: No files were searched, which means ripgrep probably applied a filter you didn't expect.
Running with --debug will show why files are being skipped.
```

Debug output (argument parsing omitted: "no extra arguments found from configuration file", "heuristic chose to search ./"):
```
/tmp/repro $ rg ignore
rg: DEBUG|grep_regex::config|/…/grep-regex-0.1.12/src/config.rs:175: assembling HIR from 1 fixed string literals
rg: DEBUG|globset|/…/globset-0.4.14/src/lib.rs:453: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
rg: DEBUG|globset|/…/globset-0.4.14/src/lib.rs:453: built glob set; 0 literals, 0 basenames, 0 extensions, 0 prefixes, 0 suffixes, 1 required extensions, 0 regexes
rg: DEBUG|ignore::walk|/…/ignore-0.4.22/src/walk.rs:1799: ignoring ./.git: Ignore(IgnoreMatch(Hidden))
rg: DEBUG|ignore::walk|/…/ignore-0.4.22/src/walk.rs:1799: ignoring ./.gitignore: Ignore(IgnoreMatch(Hidden))
rg: DEBUG|ignore::walk|/…/ignore-0.4.22/src/walk.rs:1799: ignoring ./parent/ignore-me.txt: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "/parent/*.txt", actual: "parent/*.txt", is_whitelist: false, is_only_dir: false })))
test/subdir/ignore-me.txt
1:please don't ignore me

/tmp/repro/parent $ rg ignore
rg: DEBUG|grep_regex::config|/…/grep-regex-0.1.12/src/config.rs:175: assembling HIR from 1 fixed string literals
rg: DEBUG|globset|/…/globset-0.4.14/src/lib.rs:453: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
rg: DEBUG|globset|/…/globset-0.4.14/src/lib.rs:453: built glob set; 0 literals, 0 basenames, 0 extensions, 0 prefixes, 0 suffixes, 1 required extensions, 0 regexes
rg: DEBUG|ignore::walk|/…/ignore-0.4.22/src/walk.rs:1799: ignoring ./ignore-me.txt: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("/tmp/repro/.gitignore"), original: "/parent/*.txt", actual: "parent/*.txt", is_whitelist: false, is_only_dir: false })))
rg: DEBUG|ignore::walk|/…/ignore-0.4.22/src/walk.rs:1799: ignoring ./subdir/dont-ignore-me.txt: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("/tmp/repro/.gitignore"), original: "/parent/*.txt", actual: "parent/*.txt", is_whitelist: false, is_only_dir: false })))
rg: No files were searched, which means ripgrep probably applied a filter you didn't expect.
Running with --debug will show why files are being skipped.
```

### What is the expected behavior?

```
/tmp/repro/parent $ rg ignore
subdir/dont-ignore-me.txt
1:please don't ignore me
```

---

_Comment by @h4emp3 on 2024-06-28 10:33_

I just was pretty confused about the ignore behaviour of ripgrep and I think I observed the same problem described here.

---

Minimal repro I could come up with:

```sh
mkdir .git
echo '/folder/file' > .gitignore
mkdir -p folder/sub/folder/sub
echo 'do NOT find me' > folder/file
echo 'find me' > folder/sub/file
```

```
# rg --version
ripgrep 14.1.0

features:-simd-accel,+pcre2
simd(compile):+SSE2,-SSSE3,-AVX2
simd(runtime):+SSE2,+SSSE3,+AVX2

PCRE2 10.42 is available (JIT is available)
```

```
# rg --debug 'find me' .
rg: DEBUG|rg::flags::parse|crates/core/flags/parse.rs:97: no extra arguments found from configuration file
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1260: found hostname for hyperlink configuration: vermeer
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1270: hyperlink format: ""
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:174: using 12 thread(s)
rg: DEBUG|grep_regex::config|/usr/share/cargo/registry/grep-regex-0.1.12/src/config.rs:175: assembling HIR from 1 fixed string literals
rg: DEBUG|globset|/usr/share/carg
---o/registry/globset-0.4.14/src/lib.rs:453: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
rg: DEBUG|globset|/usr/share/cargo/registry/globset-0.4.14/src/lib.rs:453: built glob set; 1 literals, 0 basenames, 0 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
rg: DEBUG|ignore::walk|/usr/share/cargo/registry/ignore-0.4.22/src/walk.rs:1799: ignoring ./.git: Ignore(IgnoreMatch(Hidden))
rg: DEBUG|ignore::walk|/usr/share/cargo/registry/ignore-0.4.22/src/walk.rs:1799: ignoring ./.gitignore: Ignore(IgnoreMatch(Hidden))
rg: DEBUG|ignore::walk|/usr/share/cargo/registry/ignore-0.4.22/src/walk.rs:1799: ignoring ./folder/file: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "/folder/file", actual: "folder/file", is_whitelist: false, is_only_dir: false })))
./folder/sub/file
1:find me
```

```
# rg --debug 'find me' folder
rg: DEBUG|rg::flags::parse|crates/core/flags/parse.rs:97: no extra arguments found from configuration file
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1260: found hostname for hyperlink configuration: vermeer
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1270: hyperlink format: ""
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:174: using 12 thread(s)
rg: DEBUG|grep_regex::config|/usr/share/cargo/registry/grep-regex-0.1.12/src/config.rs:175: assembling HIR from 1 fixed string literals
rg: DEBUG|globset|/usr/share/cargo/registry/globset-0.4.14/src/lib.rs:453: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
rg: DEBUG|globset|/usr/share/cargo/registry/globset-0.4.14/src/lib.rs:453: built glob set; 1 literals, 0 basenames, 0 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
rg: DEBUG|ignore::walk|/usr/share/cargo/registry/ignore-0.4.22/src/walk.rs:1799: ignoring folder/file: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("/tmp/rg-glob-test/.gitignore"), original: "/folder/file", actual: "folder/file", is_whitelist: false, is_only_dir: false })))
rg: DEBUG|ignore::walk|/usr/share/cargo/registry/ignore-0.4.22/src/walk.rs:1799: ignoring folder/sub/file: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("/tmp/rg-glob-test/.gitignore"), original: "/folder/file", actual: "folder/file", is_whitelist: false, is_only_dir: false })))
```

I expected the second call to rg to also find the file in the subdirectory.

Perhaps the most notable difference to the original issue description is: You don't actually need to cd into the subdirectory to trigger the error, it is enough to pass the folder as argument to rg.

---

Thanks in advance, I really appreciate your work on rg and your comments and writeups on it and other topics!

---

_Comment by @ttrei on 2024-08-29 16:11_

I encountered a similar problem with ignore behavior.

```
mkdir repro
cd repro
mkdir -p a/b/c
echo "foo" > a/b/c/foo.txt
echo "**/b/c" > .rgignore
```
```
# rg --version
ripgrep 14.1.0

features:-simd-accel,+pcre2
simd(compile):+SSE2,-SSSE3,-AVX2
simd(runtime):+SSE2,+SSSE3,+AVX2

PCRE2 10.42 is available (JIT is available)
```
```
# rg foo ./ --debug
rg: DEBUG|rg::flags::config|crates/core/flags/config.rs:41: /home/reinis/.ripgreprc: arguments loaded from config file: ["--smart-case", "--follow"]
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1260: found hostname for hyperlink configuration: mercury
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1264: found wsl_prefix for hyperlink configuration: wsl$/debian-main
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1270: hyperlink format: ""
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:174: using 12 thread(s)
rg: DEBUG|globset|crates/globset/src/lib.rs:453: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
rg: DEBUG|globset|crates/globset/src/lib.rs:453: built glob set; 0 literals, 5 basenames, 1 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
rg: DEBUG|globset|crates/globset/src/lib.rs:453: built glob set; 0 literals, 5 basenames, 1 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
rg: DEBUG|globset|crates/globset/src/lib.rs:453: built glob set; 0 literals, 5 basenames, 1 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
rg: DEBUG|globset|crates/globset/src/lib.rs:453: built glob set; 1 literals, 0 basenames, 0 extensions, 0 prefixes, 1 suffixes, 0 required extensions, 0 regexes
rg: DEBUG|ignore::walk|crates/ignore/src/walk.rs:1799: ignoring ./.rgignore: Ignore(IgnoreMatch(Hidden))
rg: DEBUG|ignore::walk|crates/ignore/src/walk.rs:1799: ignoring ./a/b/c: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.rgignore"), original: "**/b/c", actual: "**/b/c", is_whitelist: false, is_only_dir: false })))
```
```
# rg foo ./a --debug
rg: DEBUG|rg::flags::config|crates/core/flags/config.rs:41: /home/reinis/.ripgreprc: arguments loaded from config file: ["--smart-case", "--follow"]
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1260: found hostname for hyperlink configuration: mercury
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1264: found wsl_prefix for hyperlink configuration: wsl$/debian-main
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1270: hyperlink format: ""
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:174: using 12 thread(s)
rg: DEBUG|globset|crates/globset/src/lib.rs:453: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
rg: DEBUG|globset|crates/globset/src/lib.rs:453: built glob set; 0 literals, 5 basenames, 1 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
rg: DEBUG|globset|crates/globset/src/lib.rs:453: built glob set; 0 literals, 5 basenames, 1 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
rg: DEBUG|globset|crates/globset/src/lib.rs:453: built glob set; 0 literals, 5 basenames, 1 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
rg: DEBUG|globset|crates/globset/src/lib.rs:453: built glob set; 1 literals, 0 basenames, 0 extensions, 0 prefixes, 1 suffixes, 0 required extensions, 0 regexes
./a/b/c/foo.txt
1:foo
```
I expect the second rg invocation to also ignore the b/c subdirectory.

---

_Comment by @deanqx on 2024-11-02 14:21_

same here

---

_Comment by @ronjakoistinen on 2025-01-16 17:39_

I'm seeing the same behaviour:

```console
$ mkdir /tmp/repro
$ cd /tmp/repro/
$ mkdir -p foo/bar
$ echo "**/bar/*.txt" > .rgignore
$ echo "ignore me" > foo/bar/ignore.txt


$ rg ignore --debug
rg: DEBUG|rg::flags::parse|/home/ronja/.cargo/registry/src/index.crates.io-6f17d22bba15001f/ripgrep-14.1.1/crates/core/flags/parse.rs:97: no extra arguments found from configuration file
rg: DEBUG|rg::flags::hiargs|/home/ronja/.cargo/registry/src/index.crates.io-6f17d22bba15001f/ripgrep-14.1.1/crates/core/flags/hiargs.rs:1083: number of paths given to search: 0
rg: DEBUG|rg::flags::hiargs|/home/ronja/.cargo/registry/src/index.crates.io-6f17d22bba15001f/ripgrep-14.1.1/crates/core/flags/hiargs.rs:1108: using heuristics to determine whether to read from stdin or search ./ (is_readable_stdin=false, stdin_consumed=false, mode=Search(Standard))
rg: DEBUG|rg::flags::hiargs|/home/ronja/.cargo/registry/src/index.crates.io-6f17d22bba15001f/ripgrep-14.1.1/crates/core/flags/hiargs.rs:1118: heuristic chose to search ./
rg: DEBUG|rg::flags::hiargs|/home/ronja/.cargo/registry/src/index.crates.io-6f17d22bba15001f/ripgrep-14.1.1/crates/core/flags/hiargs.rs:1269: found hostname for hyperlink configuration: taxus
rg: DEBUG|rg::flags::hiargs|/home/ronja/.cargo/registry/src/index.crates.io-6f17d22bba15001f/ripgrep-14.1.1/crates/core/flags/hiargs.rs:1279: hyperlink format: ""
rg: DEBUG|rg::flags::hiargs|/home/ronja/.cargo/registry/src/index.crates.io-6f17d22bba15001f/ripgrep-14.1.1/crates/core/flags/hiargs.rs:174: using 12 thread(s)
rg: DEBUG|grep_regex::config|/home/ronja/.cargo/registry/src/index.crates.io-6f17d22bba15001f/grep-regex-0.1.13/src/config.rs:175: assembling HIR from 1 fixed string literals
rg: DEBUG|globset|/home/ronja/.cargo/registry/src/index.crates.io-6f17d22bba15001f/globset-0.4.15/src/lib.rs:453: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
rg: DEBUG|ignore::gitignore|/home/ronja/.cargo/registry/src/index.crates.io-6f17d22bba15001f/ignore-0.4.23/src/gitignore.rs:393: opened gitignore file: ./.rgignore
rg: DEBUG|globset|/home/ronja/.cargo/registry/src/index.crates.io-6f17d22bba15001f/globset-0.4.15/src/lib.rs:453: built glob set; 0 literals, 0 basenames, 0 extensions, 0 prefixes, 0 suffixes, 1 required extensions, 0 regexes
rg: DEBUG|ignore::walk|/home/ronja/.cargo/registry/src/index.crates.io-6f17d22bba15001f/ignore-0.4.23/src/walk.rs:1799: ignoring ./.rgignore: Ignore(IgnoreMatch(Hidden))
rg: DEBUG|ignore::walk|/home/ronja/.cargo/registry/src/index.crates.io-6f17d22bba15001f/ignore-0.4.23/src/walk.rs:1799: ignoring ./foo/bar/ignore.txt: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.rgignore"), original: "**/bar/*.txt", actual: "**/bar/*.txt", is_whitelist: false, is_only_dir: false })))
rg: No files were searched, which means ripgrep probably applied a filter you didn't expect.
Running with --debug will show why files are being skipped.


$ rg ignore foo/ --debug
rg: DEBUG|rg::flags::parse|/home/ronja/.cargo/registry/src/index.crates.io-6f17d22bba15001f/ripgrep-14.1.1/crates/core/flags/parse.rs:97: no extra arguments found from configuration file
rg: DEBUG|rg::flags::hiargs|/home/ronja/.cargo/registry/src/index.crates.io-6f17d22bba15001f/ripgrep-14.1.1/crates/core/flags/hiargs.rs:1083: number of paths given to search: 1
rg: DEBUG|rg::flags::hiargs|/home/ronja/.cargo/registry/src/index.crates.io-6f17d22bba15001f/ripgrep-14.1.1/crates/core/flags/hiargs.rs:1094: is_one_file? false
rg: DEBUG|rg::flags::hiargs|/home/ronja/.cargo/registry/src/index.crates.io-6f17d22bba15001f/ripgrep-14.1.1/crates/core/flags/hiargs.rs:1269: found hostname for hyperlink configuration: taxus
rg: DEBUG|rg::flags::hiargs|/home/ronja/.cargo/registry/src/index.crates.io-6f17d22bba15001f/ripgrep-14.1.1/crates/core/flags/hiargs.rs:1279: hyperlink format: ""
rg: DEBUG|rg::flags::hiargs|/home/ronja/.cargo/registry/src/index.crates.io-6f17d22bba15001f/ripgrep-14.1.1/crates/core/flags/hiargs.rs:174: using 12 thread(s)
rg: DEBUG|grep_regex::config|/home/ronja/.cargo/registry/src/index.crates.io-6f17d22bba15001f/grep-regex-0.1.13/src/config.rs:175: assembling HIR from 1 fixed string literals
rg: DEBUG|globset|/home/ronja/.cargo/registry/src/index.crates.io-6f17d22bba15001f/globset-0.4.15/src/lib.rs:453: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
rg: DEBUG|ignore::gitignore|/home/ronja/.cargo/registry/src/index.crates.io-6f17d22bba15001f/ignore-0.4.23/src/gitignore.rs:393: opened gitignore file: /tmp/repro/.rgignore
rg: DEBUG|globset|/home/ronja/.cargo/registry/src/index.crates.io-6f17d22bba15001f/globset-0.4.15/src/lib.rs:453: built glob set; 0 literals, 0 basenames, 0 extensions, 0 prefixes, 0 suffixes, 1 required extensions, 0 regexes
foo/bar/ignore.txt
1:ignore me


$ rg --version
ripgrep 14.1.1

features:-pcre2
simd(compile):+SSE2,-SSSE3,-AVX2
simd(runtime):+SSE2,+SSSE3,+AVX2

PCRE2 is not available in this build of ripgrep.
```

---

_Comment by @woess on 2025-02-12 00:47_

I've done some digging and it seems this code is responsible for this issue:
https://github.com/BurntSushi/ripgrep/blob/e2362d4d5185d02fa857bf381e7bd52e66fafc73/crates/ignore/src/dir.rs#L457-L478

What's happening here is that when we are in cwd `/tmp/repro/parent` and descend into `./subdir`:
`abs_parent_path="/tmp/repro/parent"`, `dir_path="./subdir"`, and `path="subdir/dont-ignore-me.txt"`.
1. `strip_prefix` strips `"./"` from `dir_path`, so `path_prefix="subdir"`.
2. `strip_prefix` strips `path_prefix` from `path`, i.e. `"subdir/dont-ignore-me.txt"` => `"/dont-ignore-me.txt"`.
3. `strip_prefix` strips the leading `"/"` from that, leaving only `p="dont-ignore-me.txt"`
4. `abs_parent_path.join(p)` results in `"/tmp/repro/parent/dont-ignore-me.txt"`.

Note how `subdir` is missing in the final path.

Replacing the above code snippet with just:
```rust
    let path = abs_parent_path.join(path);
```
fixes the problem for me, but is probably not the correct fix.

**Edit**: Found an open PR that fixes this issue as well: https://github.com/BurntSushi/ripgrep/pull/2933


---

_Label `bug` added by @BurntSushi on 2025-07-11 16:44_

---

_Label `rollup` added by @BurntSushi on 2025-07-11 16:44_

---

_Closed by @BurntSushi on 2025-09-20 01:08_

---
