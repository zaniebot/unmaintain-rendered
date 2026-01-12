```yaml
number: 1375
title: "--debug should print information about ignore files being read"
type: issue
state: open
author: blueyed
labels:
  - enhancement
assignees: []
created_at: 2019-09-12T12:42:03Z
updated_at: 2020-05-08T11:33:37Z
url: https://github.com/BurntSushi/ripgrep/issues/1375
synced_at: 2026-01-12T16:13:23Z
```

# --debug should print information about ignore files being read

---

_@blueyed_

`rg --debug` should output information about ignore files being read.

Currently it outputs something like the following, but it is not clear where the patterns are coming from:

```
% rg --no-config --debug foo
DEBUG|rg::args|src/args.rs:544: not reading config files because --no-config is present
DEBUG|grep_regex::literal|grep-regex/src/literal.rs:59: literal prefixes detected: Literals { lits: [Complete(luarocks)], limit_size: 250, limit_class: 10 }
DEBUG|globset|globset/src/lib.rs:430: glob converted to regex: Glob { glob: "**/*~", re: "(?-u)^(?:/?|.*/)[^/]*\\~$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true }, tokens: Tokens([RecursivePrefix, ZeroOrMore, Literal('~')]) }
...
```

(This information (source filename) could also be displayed with `Ignore` / `Whitelist` instances in the debug log later on - or getting assigned a number, which then gets referenced from there (to shortness).)

ripgrep 11.0.2

---

_Label `enhancement` added by @BurntSushi on 2019-09-12 12:45_

---

_Comment by @BurntSushi on 2020-05-08 11:33_

> This information (source filename) could also be displayed with Ignore / Whitelist instances in the debug log later on 

Note that this already happens today. For example:

```
|ignore::walk|crates/ignore/src/walk.rs:1689: ignoring ./crates/regex/target: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "target", actual: "**/target", is_whitelist: false, is_only_dir: false })))
```

---
