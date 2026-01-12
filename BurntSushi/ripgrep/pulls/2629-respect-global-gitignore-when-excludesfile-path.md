```yaml
number: 2629
title: Respect global .gitignore when excludesFile path uses surrounding double quotes
type: pull_request
state: closed
author: Kentokamoto
labels:
  - rollup
assignees: []
base: master
head: kentokamoto/2392-global-gitignore-bug
created_at: 2023-10-14T06:11:11Z
updated_at: 2023-11-21T04:51:57Z
url: https://github.com/BurntSushi/ripgrep/pull/2629
synced_at: 2026-01-12T18:23:14Z
```

# Respect global .gitignore when excludesFile path uses surrounding double quotes

---

_@Kentokamoto_

Related Issue: #2392 

# Summary of problem:
In a .gitconfig file, a user may specify additional file patterns they may want to ignore when committing changes by using the `core.excludesFile` setting. RipGrep respects these file patterns as well. 

Git also handles file paths that are surrounded by double quotes like
```
[core]
    excludesFile = "~/foo/bar"
```
RipGrep's logic does not handle for double quotes. 

# Changes
The regex used to parse the `excludesFile` section of the .gitconfig adds double quotes (\x22)  as a part of the parsing pattern.

# Tests
Wrote `parse_excludes_file4`  for unit testing

Manually tested from issue's example:
```
$ mkdir rg_global_gitignore
$ cd rg_global_gitignore
$ git init

# adding the config this way will strip the quotation marks
# git config --global core.excludesFile "~/.gitignore"

# open ~/.gitconfig and add:
#
# [core]
# 	excludesFile = "~/.gitignore"

$ echo "*.sql" > ~/.gitignore

$ touch backup backup.sql backup123.sql db_backup.sql
$ <path>/ripgrep/target/release/rg --files --debug
DEBUG|grep_regex::config|crates/regex/src/config.rs:173: assembling HIR from 0 fixed string literals
DEBUG|rg::args|crates/core/args.rs:555: running with 8 threads for parallelism
DEBUG|rg::args|crates/core/args.rs:1148: hyperlink format: "file://{host}{path}"
[126, 47, 46, 103, 105, 116, 105, 103, 110, 111, 114, 101]
DEBUG|globset|crates/globset/src/lib.rs:445: built glob set; 0 literals, 0 basenames, 1 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:445: built glob set; 0 literals, 0 basenames, 1 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|ignore::walk|crates/ignore/src/walk.rs:1804: ignoring ./db_backup.sql: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("/Users/kento/.gitignore"), original: "*.sql", actual: "**/*.sql", is_whitelist: false, is_only_dir: false })))
DEBUG|ignore::walk|crates/ignore/src/walk.rs:1804: ignoring ./backup123.sql: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("/Users/kento/.gitignore"), original: "*.sql", actual: "**/*.sql", is_whitelist: false, is_only_dir: false })))
DEBUG|ignore::walk|crates/ignore/src/walk.rs:1804: ignoring ./.gitignore: Ignore(IgnoreMatch(Hidden))
DEBUG|ignore::walk|crates/ignore/src/walk.rs:1804: ignoring ./.git: Ignore(IgnoreMatch(Hidden))
DEBUG|ignore::walk|crates/ignore/src/walk.rs:1804: ignoring ./backup.sql: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("/Users/kento/.gitignore"), original: "*.sql", actual: "**/*.sql", is_whitelist: false, is_only_dir: false })))
backup
```

---

_@BurntSushi requested changes on 2023-10-14 11:38_

Thanks! But the regex here looks too permissive. It accepts any sequence of whitespace and quotes. It probably should be `\s*"?\s*` or something.

And please use a literal quote instead of its hex escape. You'll want to use `r#"..."#` for the string syntax.

---

_Comment by @Kentokamoto on 2023-10-14 13:14_

@BurntSushi , Thanks for the feedback. I've gone ahead and made the changes you suggested. I added an extra test to check if more than one pair of double quotes surrounding the path is valid or not. I can remove it if it's overkill but let me know. 

---

_Review requested from @BurntSushi by @Kentokamoto on 2023-10-14 13:14_

---

_@BurntSushi approved on 2023-10-15 12:34_

This does allow a quote on one side but not the other, but we're never going to get this exactly right with using a regex to parse this file anyway.

---

_Comment by @BurntSushi on 2023-10-15 12:36_

Also, in the future, PRs like this should just be one commit. So changes should be fixed up into one commit and then force pushed.

---

_Label `rollup` added by @BurntSushi on 2023-10-15 12:36_

---

_@BurntSushi reviewed on 2023-10-15 12:41_

---

_Review comment by @BurntSushi on `crates/ignore/src/gitignore.rs`:784 on 2023-10-15 12:41_

And also, code should follow the style of the surrounding code. In this case, there should be a blank
line separating test functions.

(You don't need to do this now. I've done it for you and this PR will get rolled up into another bigger one.)

---

_Closed by @BurntSushi on 2023-11-21 04:51_

---
