```yaml
number: 2376
title: Include per-file ignore matches in debug logging
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/log
created_at: 2023-01-31T04:07:20Z
updated_at: 2023-01-31T04:11:58Z
url: https://github.com/astral-sh/ruff/pull/2376
synced_at: 2026-01-12T04:52:00Z
```

# Include per-file ignore matches in debug logging

---

_Pull request opened by @charliermarsh on 2023-01-31 04:07_

Example usage (note the `Adding per-file ignores...` line):

```console
❯ cargo run foo -n -v
   Compiling ruff v0.0.238 (/Users/crmarsh/workspace/ruff)
   Compiling ruff_cli v0.0.238 (/Users/crmarsh/workspace/ruff/ruff_cli)
    Finished dev [unoptimized + debuginfo] target(s) in 9.66s
     Running `target/debug/ruff foo -n -v`
[2023-01-30][23:04:56][globset][DEBUG] built glob set; 19 literals, 0 basenames, 0 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
[2023-01-30][23:04:56][globset][DEBUG] built glob set; 19 literals, 0 basenames, 0 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
[2023-01-30][23:04:56][globset][DEBUG] built glob set; 15 literals, 70 basenames, 0 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
[2023-01-30][23:04:56][globset][DEBUG] built glob set; 15 literals, 70 basenames, 0 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
[2023-01-30][23:04:56][globset][DEBUG] glob converted to regex: Glob { glob: "**/*.py[cod]", re: "(?-u)^(?:/?|.*/)[^/]*\\.py[cod]$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true }, tokens: Tokens([RecursivePrefix, ZeroOrMore, Literal('.'), Literal('p'), Literal('y'), Class { negated: false, ranges: [('c', 'c'), ('o', 'o'), ('d', 'd')] }]) }
[2023-01-30][23:04:56][globset][DEBUG] glob converted to regex: Glob { glob: "**/.coverage.*", re: "(?-u)^(?:/?|.*/)\\.coverage\\.[^/]*$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true }, tokens: Tokens([RecursivePrefix, Literal('.'), Literal('c'), Literal('o'), Literal('v'), Literal('e'), Literal('r'), Literal('a'), Literal('g'), Literal('e'), Literal('.'), ZeroOrMore]) }
[2023-01-30][23:04:56][globset][DEBUG] built glob set; 4 literals, 66 basenames, 11 extensions, 0 prefixes, 0 suffixes, 3 required extensions, 2 regexes
[2023-01-30][23:04:56][globset][DEBUG] built glob set; 19 literals, 0 basenames, 0 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
[2023-01-30][23:04:56][ignore::walk][DEBUG] ignoring /Users/crmarsh/workspace/ruff/foo/.ruff_cache: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("/Users/crmarsh/workspace/ruff/.gitignore"), original: ".ruff_cache", actual: "**/.ruff_cache", is_whitelist: false, is_only_dir: false })))
[2023-01-30][23:04:56][ruff::commands][DEBUG] Identified files to lint in: 13.642625ms
[2023-01-30][23:04:56][ruff::fs][DEBUG] Adding per-file ignores for "/Users/crmarsh/workspace/ruff/foo/bar.py" due to basename match on "(?-u)^bar\\.py$": {UnusedImport}
[2023-01-30][23:04:56][ruff::commands][DEBUG] Checked files in: 2.662291ms
foo/bar.py:5:5: F841 Local variable `x` is assigned to but never used
Found 1 error.
1 potentially fixable with the --fix option.
```

Closes #2373.

---

_Merged by @charliermarsh on 2023-01-31 04:11_

---

_Closed by @charliermarsh on 2023-01-31 04:11_

---

_Branch deleted on 2023-01-31 04:11_

---
