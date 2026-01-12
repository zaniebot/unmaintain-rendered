```yaml
number: 1559
title: "Missing matches with ' +' pattern when using `--case-sensitive` mode"
type: issue
state: closed
author: aseure
labels: []
assignees: []
created_at: 2020-04-23T10:14:16Z
updated_at: 2020-04-23T12:39:10Z
url: https://github.com/BurntSushi/ripgrep/issues/1559
synced_at: 2026-01-12T16:13:23Z
```

# Missing matches with ' +' pattern when using `--case-sensitive` mode

---

_@aseure_

#### What version of ripgrep are you using?

```
ripgrep 12.0.1
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
```

#### How did you install ripgrep?

Installed via HomeBrew using `brew install ripgrep`.

#### What operating system are you using ripgrep on?

macOS Catalina 10.15.4

#### Describe your bug.

A regular expression is not matching all locations where the pattern can be found when searching with case sensitive mode enabled.

#### What are the steps to reproduce the behavior?

**file**
```go
type A struct {
	TaskID int `json:"taskID"`
}

type B struct {
	ObjectID string `json:"objectID"`
	TaskID   int    `json:"taskID"`
}
```

**command**
```
rg --case-sensitive 'TaskID +int'
```

#### What is the actual behavior?

```
$ rg --debug --case-sensitive 'TaskID +int'
DEBUG|grep_regex::literal|crates/regex/src/literal.rs:180: required literal found: "TaskID int"
DEBUG|grep_regex::matcher|crates/regex/src/matcher.rs:50: extracted fast line regex: (?-u:TaskID int)
DEBUG|globset|crates/globset/src/lib.rs:426: glob converted to regex: Glob { glob: "**/*~", re: "(?-u)^(?:/?|.*/)[^/]*\\~$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true }, tokens: Tokens([RecursivePrefix, ZeroOrMore, Literal('~')]) }
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 8 basenames, 5 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 1 regexes
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 11 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 11 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 11 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 11 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 11 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
file.go
2:	TaskID int `json:"taskID"`
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 11 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 11 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 11 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 11 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
```

#### What is the expected behavior?

Ripgrep should have also matched the second match on line 7 which contains `TaskID`, followed by more than one space, followed by the `int` type. For the record, this is what `grep` and `ag` are outputing in that case:

```
$ grep -E 'TaskID +int' file.go
	TaskID int `json:"taskID"`
	TaskID   int    `json:"taskID"
```

```
$ ag 'TaskID +int'
file.go
2:	TaskID int `json:"taskID"`
7:	TaskID   int    `json:"taskID"`
```

---

_Closed by @BurntSushi on 2020-04-23 12:38_

---

_Comment by @BurntSushi on 2020-04-23 12:39_

Thanks for the report! This turned out to be a duplicate of #1537 in that they share the same root cause. It was fixed on master a few weeks ago, but I've added your particular example to the regression test suite.

I'll try to get a new release out soon with this fix.

---
