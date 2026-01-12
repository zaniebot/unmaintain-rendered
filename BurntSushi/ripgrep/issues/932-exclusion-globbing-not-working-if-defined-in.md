```yaml
number: 932
title: Exclusion globbing not working if defined in config file
type: issue
state: closed
author: eljulians
labels:
  - invalid
assignees: []
created_at: 2018-05-29T16:29:52Z
updated_at: 2018-05-30T08:56:09Z
url: https://github.com/BurntSushi/ripgrep/issues/932
synced_at: 2026-01-12T16:13:22Z
```

# Exclusion globbing not working if defined in config file

---

_@eljulians_

#### What version of ripgrep are you using?

```
ripgrep 0.8.1 (rev c8e9f25b85)
-SIMD -AVX
```

#### How did you install ripgrep?

From `.deb` binary.

#### What operating system are you using ripgrep on?

Linux Mint 18.1 Serena.

#### Describe your question, feature request, or bug.

The exclusion globbing doesn't behave properly when defined in the config file, but it does if specified as parameter.

#### If this is a bug, what are the steps to reproduce the behavior?

- Create the sandbox

```
mkdir /tmp/ripgrep-test
cd /tmp/ripgrep-test
git init
echo a > a.txt
echo b > b.txt
```

- Create a ripgrep config file with the following option:
```
--glob='!.git/*' 
```
- Export the ripgrep config path:
```
export RIPGREP_CONFIG_PATH=/path/to/ripgrep/config
```
- Execute ripgrep:
```
rg a
```
The output is:
```
No files were searched, which means ripgrep probably applied a filter you didn't expect. Try running again with --debug.                                                                                           
```

#### If this is a bug, what is the actual behavior?

```
DEBUG/rg::config/src/config.rs:42: /home/jpardo/.ripgreprc: arguments loaded from config file: ["--glob=\'!.git/*\'"]
DEBUG/rg::args/src/args.rs:143: final argv: ["rg", "--glob=\'!.git/*\'", "--debug", "a"]
DEBUG/globset/globset/src/lib.rs:396: glob converted to regex: Glob { glob: "\'!.git/*\'", re: "(?-u)^\'!\\.git/[^/]*\'$", opts: GlobOptions { case_insensitive: false, literal_separator: true }, tokens: Tokens([Literal('\''), Literal('!'), Literal('.'), Literal('g'), Literal('i'), Literal('t'), Literal('/'), ZeroOrMore, Literal('\'')]) }
DEBUG/globset/globset/src/lib.rs:401: built glob set; 0 literals, 0 basenames, 0 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 1 regexes
DEBUG/grep::search/grep/src/search.rs:195: regex ast:
Literal {
    chars: [
        'a'
    ],
    casei: false
}
DEBUG/grep::literals/grep/src/literals.rs:38: literal prefixes detected: Literals { lits: [Complete(a)], limit_size: 250, limit_class: 10 }
DEBUG/globset/globset/src/lib.rs:396: glob converted to regex: Glob { glob: "**/.sw[a-p]", re: "(?-u)^(?:/?|.*/)\\.sw[a-p]$", opts: GlobOptions { case_insensitive: false, literal_separator: false }, tokens: Tokens([RecursivePrefix, Literal('.'), Literal('s'), Literal('w'), Class { negated: false, ranges: [('a', 'p')] }]) }
DEBUG/globset/globset/src/lib.rs:396: glob converted to regex: Glob { glob: "**/.*.sw[a-p]", re: "(?-u)^(?:/?|.*/)\\..*\\.sw[a-p]$", opts: GlobOptions { case_insensitive: false, literal_separator: false }, tokens: Tokens([RecursivePrefix, Literal('.'), ZeroOrMore, Literal('.'), Literal('s'), Literal('w'), Class { negated: false, ranges: [('a', 'p')] }]) }
DEBUG/globset/globset/src/lib.rs:401: built glob set; 0 literals, 2 basenames, 0 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 2 regexes
DEBUG/ignore::walk/ignore/src/walk.rs:1428: ignoring ./a.txt: Ignore(IgnoreMatch(Override(Glob(UnmatchedIgnore))))
DEBUG/ignore::walk/ignore/src/walk.rs:1428: ignoring ./.git: Ignore(IgnoreMatch(Hidden))
DEBUG/ignore::walk/ignore/src/walk.rs:1428: ignoring ./b.txt: Ignore(IgnoreMatch(Override(Glob(UnmatchedIgnore))))
No files were searched, which means ripgrep probably applied a filter you didn't expect. Try running again with --debug.

```

#### If this is a bug, what is the expected behavior?

The expected behavior should be the one that actually gives ripgrep with the same option, but when set as option, not in the config file:

```
unset RIPGREP_CONFIG_PATH
rg --hidden --glob='!.git/*' a
```

The output then is:

```
a.txt
1:a
```

---

_Comment by @okdana on 2018-05-29 16:31_

https://github.com/BurntSushi/ripgrep/issues/927#issuecomment-391875291

---

_Comment by @BurntSushi on 2018-05-29 16:39_

The debug output says exactly how your glob is being misinterpreted:

```
DEBUG/globset/globset/src/lib.rs:396: glob converted to regex: Glob { glob: "\'!.git/*\'"
```

---

_Closed by @BurntSushi on 2018-05-29 16:39_

---

_Label `invalid` added by @BurntSushi on 2018-05-29 16:39_

---

_Comment by @eljulians on 2018-05-30 08:56_

I searched for other issues but didn't find it. Thanks for the heads up!

---
