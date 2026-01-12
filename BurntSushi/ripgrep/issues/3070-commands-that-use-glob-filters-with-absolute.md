```yaml
number: 3070
title: Commands that use glob filters with absolute paths behave differently depending on cwd
type: issue
state: open
author: mattalxndr
labels:
  - bug
assignees: []
created_at: 2025-06-20T00:46:57Z
updated_at: 2025-10-16T01:38:51Z
url: https://github.com/BurntSushi/ripgrep/issues/3070
synced_at: 2026-01-12T16:13:25Z
```

# Commands that use glob filters with absolute paths behave differently depending on cwd

---

_@mattalxndr_

### Please tick this box to confirm you have reviewed the above.

- [x] I have a different issue.

### What version of ripgrep are you using?

```
ripgrep 14.1.1

features:+pcre2
simd(compile):+SSE2,-SSSE3,-AVX2
simd(runtime):+SSE2,+SSSE3,+AVX2

PCRE2 10.43 is available (JIT is available)
```

### How did you install ripgrep?

pacman

### What operating system are you using ripgrep on?

Arch Linux, rolling release

### Describe your bug.

Commands that use glob filters with absolute paths behave differently depending on current working directory.

### What are the steps to reproduce the behavior?

```shell
 ~ % mkdir /tmp/rg
 ~ % echo 1 > /tmp/rg/one.txt
 ~ % echo 2 > /tmp/rg/two.txt
 ~ % unset RIPGREP_CONFIG_PATH
 ~ % cd /
 / % rg --glob='!/tmp/rg/two.txt' . /tmp/rg/
/tmp/rg/one.txt
1:1
 / % cd /tmp/rg
 /tmp/rg % rg --glob='!/tmp/rg/two.txt' . /tmp/rg/
/tmp/rg/one.txt
1:1

/tmp/rg/two.txt
1:2
```


### What is the actual behavior?

Here're those last two commands again, with `--debug`:

```shell
 / % rg --debug --glob='!/tmp/rg/two.txt' . /tmp/rg/
rg: DEBUG|rg::flags::parse|crates/core/flags/parse.rs:97: no extra arguments found from configuration file
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1083: number of paths given to search: 1
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1094: is_one_file? false
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1269: found hostname for hyperlink configuration: m
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1279: hyperlink format: ""
rg: DEBUG|globset|crates/globset/src/lib.rs:453: built glob set; 1 literals, 0 basenames, 0 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:174: using 8 thread(s)
rg: DEBUG|globset|crates/globset/src/lib.rs:453: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
rg: DEBUG|ignore::gitignore|crates/ignore/src/gitignore.rs:393: opened gitignore file: /home/matt/.config/git/ignore
rg: DEBUG|globset|crates/globset/src/lib.rs:448: glob converted to regex: Glob { glob: "**/[._]*.s[a-v][a-z]", re: "(?-u)^(?:/?|.*/)[\\._][^/]*\\.s[a-v][a-z]$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true, empty_alternates: false }, tokens: Tokens([RecursivePrefix, Class { negated: false, ranges: [('.', '.'), ('_', '_')] }, ZeroOrMore, Literal('.'), Literal('s'), Class { negated: false, ranges: [('a', 'v')] }, Class { negated: false, ranges: [('a', 'z')] }]) }
rg: DEBUG|globset|crates/globset/src/lib.rs:448: glob converted to regex: Glob { glob: "**/[._]*.sw[a-p]", re: "(?-u)^(?:/?|.*/)[\\._][^/]*\\.sw[a-p]$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true, empty_alternates: false }, tokens: Tokens([RecursivePrefix, Class { negated: false, ranges: [('.', '.'), ('_', '_')] }, ZeroOrMore, Literal('.'), Literal('s'), Literal('w'), Class { negated: false, ranges: [('a', 'p')] }]) }
rg: DEBUG|globset|crates/globset/src/lib.rs:448: glob converted to regex: Glob { glob: "**/[._]s[a-rt-v][a-z]", re: "(?-u)^(?:/?|.*/)[\\._]s[a-rt-v][a-z]$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true, empty_alternates: false }, tokens: Tokens([RecursivePrefix, Class { negated: false, ranges: [('.', '.'), ('_', '_')] }, Literal('s'), Class { negated: false, ranges: [('a', 'r'), ('t', 'v')] }, Class { negated: false, ranges: [('a', 'z')] }]) }
rg: DEBUG|globset|crates/globset/src/lib.rs:448: glob converted to regex: Glob { glob: "**/[._]ss[a-gi-z]", re: "(?-u)^(?:/?|.*/)[\\._]ss[a-gi-z]$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true, empty_alternates: false }, tokens: Tokens([RecursivePrefix, Class { negated: false, ranges: [('.', '.'), ('_', '_')] }, Literal('s'), Literal('s'), Class { negated: false, ranges: [('a', 'g'), ('i', 'z')] }]) }
rg: DEBUG|globset|crates/globset/src/lib.rs:448: glob converted to regex: Glob { glob: "**/[._]sw[a-p]", re: "(?-u)^(?:/?|.*/)[\\._]sw[a-p]$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true, empty_alternates: false }, tokens: Tokens([RecursivePrefix, Class { negated: false, ranges: [('.', '.'), ('_', '_')] }, Literal('s'), Literal('w'), Class { negated: false, ranges: [('a', 'p')] }]) }
rg: DEBUG|globset|crates/globset/src/lib.rs:448: glob converted to regex: Glob { glob: "**/*~", re: "(?-u)^(?:/?|.*/)[^/]*\\~$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true, empty_alternates: false }, tokens: Tokens([RecursivePrefix, ZeroOrMore, Literal('~')]) }
rg: DEBUG|globset|crates/globset/src/lib.rs:448: glob converted to regex: Glob { glob: ".idea/**/dictionaries", re: "(?-u)^\\.idea(?:/|/.*/)dictionaries$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true, empty_alternates: false }, tokens: Tokens([Literal('.'), Literal('i'), Literal('d'), Literal('e'), Literal('a'), RecursiveZeroOrMore, Literal('d'), Literal('i'), Literal('c'), Literal('t'), Literal('i'), Literal('o'), Literal('n'), Literal('a'), Literal('r'), Literal('i'), Literal('e'), Literal('s')]) }
rg: DEBUG|globset|crates/globset/src/lib.rs:448: glob converted to regex: Glob { glob: ".idea/**/shelf", re: "(?-u)^\\.idea(?:/|/.*/)shelf$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true, empty_alternates: false }, tokens: Tokens([Literal('.'), Literal('i'), Literal('d'), Literal('e'), Literal('a'), RecursiveZeroOrMore, Literal('s'), Literal('h'), Literal('e'), Literal('l'), Literal('f')]) }
rg: DEBUG|globset|crates/globset/src/lib.rs:448: glob converted to regex: Glob { glob: ".idea/**/dataSources", re: "(?-u)^\\.idea(?:/|/.*/)dataSources$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true, empty_alternates: false }, tokens: Tokens([Literal('.'), Literal('i'), Literal('d'), Literal('e'), Literal('a'), RecursiveZeroOrMore, Literal('d'), Literal('a'), Literal('t'), Literal('a'), Literal('S'), Literal('o'), Literal('u'), Literal('r'), Literal('c'), Literal('e'), Literal('s')]) }
rg: DEBUG|globset|crates/globset/src/lib.rs:448: glob converted to regex: Glob { glob: ".idea/**/libraries", re: "(?-u)^\\.idea(?:/|/.*/)libraries$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true, empty_alternates: false }, tokens: Tokens([Literal('.'), Literal('i'), Literal('d'), Literal('e'), Literal('a'), RecursiveZeroOrMore, Literal('l'), Literal('i'), Literal('b'), Literal('r'), Literal('a'), Literal('r'), Literal('i'), Literal('e'), Literal('s')]) }
rg: DEBUG|globset|crates/globset/src/lib.rs:448: glob converted to regex: Glob { glob: "**/cmake-build-*", re: "(?-u)^(?:/?|.*/)cmake\\-build\\-[^/]*$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true, empty_alternates: false }, tokens: Tokens([RecursivePrefix, Literal('c'), Literal('m'), Literal('a'), Literal('k'), Literal('e'), Literal('-'), Literal('b'), Literal('u'), Literal('i'), Literal('l'), Literal('d'), Literal('-'), ZeroOrMore]) }
rg: DEBUG|globset|crates/globset/src/lib.rs:448: glob converted to regex: Glob { glob: ".vscode/*", re: "(?-u)^\\.vscode/[^/]*$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true, empty_alternates: false }, tokens: Tokens([Literal('.'), Literal('v'), Literal('s'), Literal('c'), Literal('o'), Literal('d'), Literal('e'), Literal('/'), ZeroOrMore]) }
rg: DEBUG|globset|crates/globset/src/lib.rs:448: glob converted to regex: Glob { glob: "**/*~", re: "(?-u)^(?:/?|.*/)[^/]*\\~$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true, empty_alternates: false }, tokens: Tokens([RecursivePrefix, ZeroOrMore, Literal('~')]) }
rg: DEBUG|globset|crates/globset/src/lib.rs:448: glob converted to regex: Glob { glob: "**/.fuse_hidden*", re: "(?-u)^(?:/?|.*/)\\.fuse_hidden[^/]*$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true, empty_alternates: false }, tokens: Tokens([RecursivePrefix, Literal('.'), Literal('f'), Literal('u'), Literal('s'), Literal('e'), Literal('_'), Literal('h'), Literal('i'), Literal('d'), Literal('d'), Literal('e'), Literal('n'), ZeroOrMore]) }
rg: DEBUG|globset|crates/globset/src/lib.rs:448: glob converted to regex: Glob { glob: "**/.Trash-*", re: "(?-u)^(?:/?|.*/)\\.Trash\\-[^/]*$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true, empty_alternates: false }, tokens: Tokens([RecursivePrefix, Literal('.'), Literal('T'), Literal('r'), Literal('a'), Literal('s'), Literal('h'), Literal('-'), ZeroOrMore]) }
rg: DEBUG|globset|crates/globset/src/lib.rs:448: glob converted to regex: Glob { glob: "**/.nfs*", re: "(?-u)^(?:/?|.*/)\\.nfs[^/]*$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true, empty_alternates: false }, tokens: Tokens([RecursivePrefix, Literal('.'), Literal('n'), Literal('f'), Literal('s'), ZeroOrMore]) }
rg: DEBUG|globset|crates/globset/src/lib.rs:448: glob converted to regex: Glob { glob: "**/._*", re: "(?-u)^(?:/?|.*/)\\._[^/]*$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true, empty_alternates: false }, tokens: Tokens([RecursivePrefix, Literal('.'), Literal('_'), ZeroOrMore]) }
rg: DEBUG|globset|crates/globset/src/lib.rs:448: glob converted to regex: Glob { glob: "**/*zcompdump*", re: "(?-u)^(?:/?|.*/)[^/]*zcompdump[^/]*$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true, empty_alternates: false }, tokens: Tokens([RecursivePrefix, ZeroOrMore, Literal('z'), Literal('c'), Literal('o'), Literal('m'), Literal('p'), Literal('d'), Literal('u'), Literal('m'), Literal('p'), ZeroOrMore]) }
rg: DEBUG|globset|crates/globset/src/lib.rs:448: glob converted to regex: Glob { glob: "tests/_output/*", re: "(?-u)^tests/_output/[^/]*$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true, empty_alternates: false }, tokens: Tokens([Literal('t'), Literal('e'), Literal('s'), Literal('t'), Literal('s'), Literal('/'), Literal('_'), Literal('o'), Literal('u'), Literal('t'), Literal('p'), Literal('u'), Literal('t'), Literal('/'), ZeroOrMore]) }
rg: DEBUG|globset|crates/globset/src/lib.rs:453: built glob set; 10 literals, 41 basenames, 11 extensions, 0 prefixes, 0 suffixes, 6 required extensions, 19 regexes
rg: DEBUG|ignore::walk|crates/ignore/src/walk.rs:1799: ignoring /tmp/rg/two.txt: Ignore(IgnoreMatch(Override(Glob(Matched(Glob { from: None, original: "!/tmp/rg/two.txt", actual: "tmp/rg/two.txt", is_whitelist: true, is_only_dir: false })))))
/tmp/rg/one.txt
1:1
 / % cd /tmp/rg
 /tmp/rg % rg --debug --glob='!/tmp/rg/two.txt' . /tmp/rg/
rg: DEBUG|rg::flags::parse|crates/core/flags/parse.rs:97: no extra arguments found from configuration file
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1083: number of paths given to search: 1
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1094: is_one_file? false
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1269: found hostname for hyperlink configuration: m
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1279: hyperlink format: ""
rg: DEBUG|globset|crates/globset/src/lib.rs:453: built glob set; 1 literals, 0 basenames, 0 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:174: using 8 thread(s)
rg: DEBUG|globset|crates/globset/src/lib.rs:453: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
rg: DEBUG|ignore::gitignore|crates/ignore/src/gitignore.rs:393: opened gitignore file: /home/matt/.config/git/ignore
rg: DEBUG|globset|crates/globset/src/lib.rs:448: glob converted to regex: Glob { glob: "**/[._]*.s[a-v][a-z]", re: "(?-u)^(?:/?|.*/)[\\._][^/]*\\.s[a-v][a-z]$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true, empty_alternates: false }, tokens: Tokens([RecursivePrefix, Class { negated: false, ranges: [('.', '.'), ('_', '_')] }, ZeroOrMore, Literal('.'), Literal('s'), Class { negated: false, ranges: [('a', 'v')] }, Class { negated: false, ranges: [('a', 'z')] }]) }
rg: DEBUG|globset|crates/globset/src/lib.rs:448: glob converted to regex: Glob { glob: "**/[._]*.sw[a-p]", re: "(?-u)^(?:/?|.*/)[\\._][^/]*\\.sw[a-p]$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true, empty_alternates: false }, tokens: Tokens([RecursivePrefix, Class { negated: false, ranges: [('.', '.'), ('_', '_')] }, ZeroOrMore, Literal('.'), Literal('s'), Literal('w'), Class { negated: false, ranges: [('a', 'p')] }]) }
rg: DEBUG|globset|crates/globset/src/lib.rs:448: glob converted to regex: Glob { glob: "**/[._]s[a-rt-v][a-z]", re: "(?-u)^(?:/?|.*/)[\\._]s[a-rt-v][a-z]$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true, empty_alternates: false }, tokens: Tokens([RecursivePrefix, Class { negated: false, ranges: [('.', '.'), ('_', '_')] }, Literal('s'), Class { negated: false, ranges: [('a', 'r'), ('t', 'v')] }, Class { negated: false, ranges: [('a', 'z')] }]) }
rg: DEBUG|globset|crates/globset/src/lib.rs:448: glob converted to regex: Glob { glob: "**/[._]ss[a-gi-z]", re: "(?-u)^(?:/?|.*/)[\\._]ss[a-gi-z]$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true, empty_alternates: false }, tokens: Tokens([RecursivePrefix, Class { negated: false, ranges: [('.', '.'), ('_', '_')] }, Literal('s'), Literal('s'), Class { negated: false, ranges: [('a', 'g'), ('i', 'z')] }]) }
rg: DEBUG|globset|crates/globset/src/lib.rs:448: glob converted to regex: Glob { glob: "**/[._]sw[a-p]", re: "(?-u)^(?:/?|.*/)[\\._]sw[a-p]$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true, empty_alternates: false }, tokens: Tokens([RecursivePrefix, Class { negated: false, ranges: [('.', '.'), ('_', '_')] }, Literal('s'), Literal('w'), Class { negated: false, ranges: [('a', 'p')] }]) }
rg: DEBUG|globset|crates/globset/src/lib.rs:448: glob converted to regex: Glob { glob: "**/*~", re: "(?-u)^(?:/?|.*/)[^/]*\\~$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true, empty_alternates: false }, tokens: Tokens([RecursivePrefix, ZeroOrMore, Literal('~')]) }
rg: DEBUG|globset|crates/globset/src/lib.rs:448: glob converted to regex: Glob { glob: ".idea/**/dictionaries", re: "(?-u)^\\.idea(?:/|/.*/)dictionaries$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true, empty_alternates: false }, tokens: Tokens([Literal('.'), Literal('i'), Literal('d'), Literal('e'), Literal('a'), RecursiveZeroOrMore, Literal('d'), Literal('i'), Literal('c'), Literal('t'), Literal('i'), Literal('o'), Literal('n'), Literal('a'), Literal('r'), Literal('i'), Literal('e'), Literal('s')]) }
rg: DEBUG|globset|crates/globset/src/lib.rs:448: glob converted to regex: Glob { glob: ".idea/**/shelf", re: "(?-u)^\\.idea(?:/|/.*/)shelf$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true, empty_alternates: false }, tokens: Tokens([Literal('.'), Literal('i'), Literal('d'), Literal('e'), Literal('a'), RecursiveZeroOrMore, Literal('s'), Literal('h'), Literal('e'), Literal('l'), Literal('f')]) }
rg: DEBUG|globset|crates/globset/src/lib.rs:448: glob converted to regex: Glob { glob: ".idea/**/dataSources", re: "(?-u)^\\.idea(?:/|/.*/)dataSources$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true, empty_alternates: false }, tokens: Tokens([Literal('.'), Literal('i'), Literal('d'), Literal('e'), Literal('a'), RecursiveZeroOrMore, Literal('d'), Literal('a'), Literal('t'), Literal('a'), Literal('S'), Literal('o'), Literal('u'), Literal('r'), Literal('c'), Literal('e'), Literal('s')]) }
rg: DEBUG|globset|crates/globset/src/lib.rs:448: glob converted to regex: Glob { glob: ".idea/**/libraries", re: "(?-u)^\\.idea(?:/|/.*/)libraries$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true, empty_alternates: false }, tokens: Tokens([Literal('.'), Literal('i'), Literal('d'), Literal('e'), Literal('a'), RecursiveZeroOrMore, Literal('l'), Literal('i'), Literal('b'), Literal('r'), Literal('a'), Literal('r'), Literal('i'), Literal('e'), Literal('s')]) }
rg: DEBUG|globset|crates/globset/src/lib.rs:448: glob converted to regex: Glob { glob: "**/cmake-build-*", re: "(?-u)^(?:/?|.*/)cmake\\-build\\-[^/]*$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true, empty_alternates: false }, tokens: Tokens([RecursivePrefix, Literal('c'), Literal('m'), Literal('a'), Literal('k'), Literal('e'), Literal('-'), Literal('b'), Literal('u'), Literal('i'), Literal('l'), Literal('d'), Literal('-'), ZeroOrMore]) }
rg: DEBUG|globset|crates/globset/src/lib.rs:448: glob converted to regex: Glob { glob: ".vscode/*", re: "(?-u)^\\.vscode/[^/]*$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true, empty_alternates: false }, tokens: Tokens([Literal('.'), Literal('v'), Literal('s'), Literal('c'), Literal('o'), Literal('d'), Literal('e'), Literal('/'), ZeroOrMore]) }
rg: DEBUG|globset|crates/globset/src/lib.rs:448: glob converted to regex: Glob { glob: "**/*~", re: "(?-u)^(?:/?|.*/)[^/]*\\~$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true, empty_alternates: false }, tokens: Tokens([RecursivePrefix, ZeroOrMore, Literal('~')]) }
rg: DEBUG|globset|crates/globset/src/lib.rs:448: glob converted to regex: Glob { glob: "**/.fuse_hidden*", re: "(?-u)^(?:/?|.*/)\\.fuse_hidden[^/]*$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true, empty_alternates: false }, tokens: Tokens([RecursivePrefix, Literal('.'), Literal('f'), Literal('u'), Literal('s'), Literal('e'), Literal('_'), Literal('h'), Literal('i'), Literal('d'), Literal('d'), Literal('e'), Literal('n'), ZeroOrMore]) }
rg: DEBUG|globset|crates/globset/src/lib.rs:448: glob converted to regex: Glob { glob: "**/.Trash-*", re: "(?-u)^(?:/?|.*/)\\.Trash\\-[^/]*$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true, empty_alternates: false }, tokens: Tokens([RecursivePrefix, Literal('.'), Literal('T'), Literal('r'), Literal('a'), Literal('s'), Literal('h'), Literal('-'), ZeroOrMore]) }
rg: DEBUG|globset|crates/globset/src/lib.rs:448: glob converted to regex: Glob { glob: "**/.nfs*", re: "(?-u)^(?:/?|.*/)\\.nfs[^/]*$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true, empty_alternates: false }, tokens: Tokens([RecursivePrefix, Literal('.'), Literal('n'), Literal('f'), Literal('s'), ZeroOrMore]) }
rg: DEBUG|globset|crates/globset/src/lib.rs:448: glob converted to regex: Glob { glob: "**/._*", re: "(?-u)^(?:/?|.*/)\\._[^/]*$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true, empty_alternates: false }, tokens: Tokens([RecursivePrefix, Literal('.'), Literal('_'), ZeroOrMore]) }
rg: DEBUG|globset|crates/globset/src/lib.rs:448: glob converted to regex: Glob { glob: "**/*zcompdump*", re: "(?-u)^(?:/?|.*/)[^/]*zcompdump[^/]*$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true, empty_alternates: false }, tokens: Tokens([RecursivePrefix, ZeroOrMore, Literal('z'), Literal('c'), Literal('o'), Literal('m'), Literal('p'), Literal('d'), Literal('u'), Literal('m'), Literal('p'), ZeroOrMore]) }
rg: DEBUG|globset|crates/globset/src/lib.rs:448: glob converted to regex: Glob { glob: "tests/_output/*", re: "(?-u)^tests/_output/[^/]*$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true, empty_alternates: false }, tokens: Tokens([Literal('t'), Literal('e'), Literal('s'), Literal('t'), Literal('s'), Literal('/'), Literal('_'), Literal('o'), Literal('u'), Literal('t'), Literal('p'), Literal('u'), Literal('t'), Literal('/'), ZeroOrMore]) }
rg: DEBUG|globset|crates/globset/src/lib.rs:453: built glob set; 10 literals, 41 basenames, 11 extensions, 0 prefixes, 0 suffixes, 6 required extensions, 19 regexes
/tmp/rg/one.txt
1:1

/tmp/rg/two.txt
1:2

```

### What is the expected behavior?

I would expect the output of the second `rg` command to match the first, since all that's different is the current working directory. 

---

_Comment by @BurntSushi on 2025-06-20 00:51_

ripgrep doesn't really have a way to filter based on absolute paths. It's a limitation of the gitignore paradigm, and the `-g/--glob` is specifically documented to behave like gitignore.

So this is working as intended, although I agree it is counter-intuitive and ripgrep probably should have a better way to approach the problem you're trying to solve.

---

_Comment by @mattalxndr on 2025-06-20 00:54_

Perhaps a wrapper script to absolute-ize the paths, pushd to /, run `rg`, then popd? Nah.. 😆 

---

_Label `bug` added by @BurntSushi on 2025-10-16 01:28_

---

_Comment by @BurntSushi on 2025-10-16 01:38_

I looked into this to see if there was an easy fix in ripgrep. I figured out the problem, but the fix is hard.

Basically, the `ignore` crate handles most-to-all of ripgrep's filtering. When I built it, I didn't carefully consider how filtering was impacted based on how gitignore patterns were matched relative to:

* The current working directory.
* The actual path to search as written by a user. e.g., `./` vs `/tmp/rg`.

The `ignore` crate generally considers gitignore patterns to match relative to where the gitignore _file_ that defines those patterns lives. To that end, before matching a file path against a pattern, the file path has a prefix equivalent to the directory containing that gitignore file stripped. That permits things like `/foo` to match correctly regardless of where ripgrep is searching or how you get there. That part of ripgrep works decently well.

But some gitignore patterns don't come from gitignore files in a directory tree like that. Some come from a global git configuration, or ripgrep's `--ignore-file` flag, or indeed, ripgrep's `-g/--glob` flags. In that case, I had mistakenly designed `ignore` around the assumption that those globs should be matched relative to _ripgrep's current working directory_. (And even then, I didn't get that completely right until recently. See the fix for #3179.) But that's actually very wrong, even though it works in a lot of cases.

After fixing #3179, I had believed that the correct approach here is that globs should be matched _relative to the directory paths given to ripgrep_. So if you search `/tmp/rg`, then `/tmp/rg` should be stripped from the file path before matching. But that actually won't fix this particular bug. Because here, the stripping is the problem. When you `cd` into `/tmp/rg`, that becomes ripgrep's CWD and that's the prefix that gets stripped when matching on `/tmp/rg/two.txt`. So you end up matching the glob `/tmp/rg/two.txt` against `two.txt`, which doesn't match.

In any case, pretty much all of `ignore` assumes that the "root" of a gitignore pattern is invariant throughout its lifetime. I suspect the "root" needs to be able to change, and that is a massive refactoring.

So unfortunately, this bug likely will remain unfixed until I have a chance to rebuild the `ignore` crate from first principles. (I've been talking about doing that for years already, so it probably won't happen any time soon.)

---
