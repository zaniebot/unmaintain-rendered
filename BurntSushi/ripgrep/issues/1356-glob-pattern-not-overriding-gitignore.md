```yaml
number: 1356
title: glob pattern not overriding .gitignore
type: issue
state: closed
author: wdesmet
labels:
  - bug
  - duplicate
  - wontfix
assignees: []
created_at: 2019-08-28T08:55:16Z
updated_at: 2019-08-28T11:10:09Z
url: https://github.com/BurntSushi/ripgrep/issues/1356
synced_at: 2026-01-12T16:13:23Z
```

# glob pattern not overriding .gitignore

---

_@wdesmet_

#### What version of ripgrep are you using?
ripgrep 11.0.1
-SIMD -AVX (compiled)
+SIMD -AVX (runtime)

#### How did you install ripgrep?

rpm

#### What operating system are you using ripgrep on?

CentOS Linux release 7.6.1810 (Core)

#### Describe your question, feature request, or bug.

rg --iglob does not seem to override gitignore when the gitignore pattern matches an entire directory.

#### If this is a bug, what are the steps to reproduce the behavior?

````
mkdir test
cd test/
git init
mkdir gen
echo 'string' >gen/test.h
rg --iglob 'gen/**' string # finds string
mkdir other
rg --iglob 'other/**' string # does not find string, as expected
echo '/gen*/' >.gitignore
rg --iglob 'gen/**' string # string is no longer found
````

The documentation appears to say that glob should always override the patterns from .gitignore, but in this case that override is not working, even though my iglob pattern seems correct.

#### If this is a bug, what is the actual behavior?

No match found when there is a .gitignore.

```
DEBUG|grep_regex::literal|grep-regex/src/literal.rs:59: literal prefixes detected: Literals { lits: [Complete(string)], limit_size: 250, limit_class: 10 }
DEBUG|globset|globset/src/lib.rs:430: glob converted to regex: Glob { glob: "gen/**/*", re: "(?-u)^gen(?:/|/.*/)[^/]*$", opts: GlobOptions { case_insensitive: false, literal_
separator: true, backslash_escape: true }, tokens: Tokens([Literal('g'), Literal('e'), Literal('n'), RecursiveZeroOrMore, ZeroOrMore]) }
DEBUG|globset|globset/src/lib.rs:435: built glob set; 0 literals, 0 basenames, 0 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 1 regexes
DEBUG|globset|globset/src/lib.rs:430: glob converted to regex: Glob { glob: ".ycm_extra_conf.py*", re: "(?-u)^\\.ycm_extra_conf\\.py[^/]*$", opts: GlobOptions { case_insensit
ive: false, literal_separator: true, backslash_escape: true }, tokens: Tokens([Literal('.'), Literal('y'), Literal('c'), Literal('m'), Literal('_'), Literal('e'), Literal('x'
), Literal('t'), Literal('r'), Literal('a'), Literal('_'), Literal('c'), Literal('o'), Literal('n'), Literal('f'), Literal('.'), Literal('p'), Literal('y'), ZeroOrMore]) }
DEBUG|globset|globset/src/lib.rs:435: built glob set; 4 literals, 1 basenames, 0 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 1 regexes
DEBUG|globset|globset/src/lib.rs:435: built glob set; 0 literals, 0 basenames, 11 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|globset/src/lib.rs:435: built glob set; 0 literals, 0 basenames, 11 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|globset/src/lib.rs:435: built glob set; 0 literals, 0 basenames, 11 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|globset/src/lib.rs:430: glob converted to regex: Glob { glob: "gen*", re: "(?-u)^gen[^/]*$", opts: GlobOptions { case_insensitive: false, literal_separator: tru
e, backslash_escape: true }, tokens: Tokens([Literal('g'), Literal('e'), Literal('n'), ZeroOrMore]) }
DEBUG|globset|globset/src/lib.rs:435: built glob set; 0 literals, 0 basenames, 0 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 1 regexes
DEBUG|globset|globset/src/lib.rs:435: built glob set; 0 literals, 0 basenames, 11 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|ignore::walk|ignore/src/walk.rs:1617: ignoring ./.git: Ignore(IgnoreMatch(Hidden))
DEBUG|globset|globset/src/lib.rs:435: built glob set; 0 literals, 0 basenames, 11 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|ignore::walk|ignore/src/walk.rs:1617: ignoring ./gen: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "/gen*/", actual: "gen*", is_whitelist:
false, is_only_dir: true })))
DEBUG|globset|globset/src/lib.rs:435: built glob set; 0 literals, 0 basenames, 11 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|ignore::walk|ignore/src/walk.rs:1617: ignoring ./other: Ignore(IgnoreMatch(Override(Glob(UnmatchedIgnore))))
DEBUG|ignore::walk|ignore/src/walk.rs:1617: ignoring ./.gitignore: Ignore(IgnoreMatch(Override(Glob(UnmatchedIgnore))))
DEBUG|globset|globset/src/lib.rs:435: built glob set; 0 literals, 0 basenames, 11 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|globset/src/lib.rs:435: built glob set; 0 literals, 0 basenames, 11 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|globset/src/lib.rs:435: built glob set; 0 literals, 0 basenames, 11 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
```

#### If this is a bug, what is the expected behavior?

I think the --iglob pattern should override gitignore.

---

_Comment by @BurntSushi on 2019-08-28 11:09_

Duplicate of https://github.com/BurntSushi/ripgrep/issues/1349

---

_Closed by @BurntSushi on 2019-08-28 11:09_

---

_Label `bug` added by @BurntSushi on 2019-08-28 11:10_

---

_Label `duplicate` added by @BurntSushi on 2019-08-28 11:10_

---

_Label `wontfix` added by @BurntSushi on 2019-08-28 11:10_

---
