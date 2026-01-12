```yaml
number: 2671
title: Issues with ripgrep finding files with no newline at end of text file
type: issue
state: closed
author: ksandvik
labels:
  - invalid
assignees: []
created_at: 2023-12-02T19:08:05Z
updated_at: 2024-01-11T19:11:53Z
url: https://github.com/BurntSushi/ripgrep/issues/2671
synced_at: 2026-01-12T16:13:24Z
```

# Issues with ripgrep finding files with no newline at end of text file

---

_@ksandvik_

### Please tick this box to confirm you have reviewed the above.

- [X] I have a different issue.

### What version of ripgrep are you using?

ripgrep 14.0.3

### How did you install ripgrep?

brew

### What operating system are you using ripgrep on?

MacOSX 14.1.2


### Describe your bug.

I used this syntax to find files with a missing last new line but just now I reports all files this way, whether they have a new line at the end or not:

  rg -s  -l '[^\n]\z'

Is there a new syntax? Or something else? This worked in 13.0.


### What are the steps to reproduce the behavior?

rg -s  -l '[^\n]\z' on any file with a mixture of markdown files or other files with a mixture of missing new lines at the end.

### What is the actual behavior?

```
rg -s  -l '[^\n]\z' --debug
DEBUG|rg::flags::parse|crates/core/flags/parse.rs:97: no extra arguments found from configuration file
DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1099: using heuristics to determine whether to read from stdin or search ./ (is_readable_stdin=false, stdin_consumed=false, mode=Search(FilesWithMatches))
DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1109: heuristic chose to search ./
DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1260: found hostname for hyperlink configuration: KSM1
DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1270: hyperlink format: ""
DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:174: using 10 thread(s)
DEBUG|globset|crates/globset/src/lib.rs:453: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:453: built glob set; 0 literals, 4 basenames, 0 extensions, 0 prefixes, 0 suffixes, 1 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:448: glob converted to regex: Glob { glob: "work/**/*", re: "(?-u)^work(?:/|/.*/)[^/]*$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true, empty_alternates: false }, tokens: Tokens([Literal('w'), Literal('o'), Literal('r'), Literal('k'), RecursiveZeroOrMore, ZeroOrMore]) }
DEBUG|globset|crates/globset/src/lib.rs:453: built glob set; 0 literals, 4 basenames, 0 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 1 regexes
```

### What is the expected behavior?

Only find files with a missing single new line at end of text file.

---

_Comment by @BurntSushi on 2024-01-06 19:07_

You didn't provide a reproduction. Please provide one next time. I'll do it for you this time:

```
$ printf "foo\nquux\n" > haystack-with-trailing-newline
$ printf "foo\nquux" > haystack-no-trailing-newline
$ rg-13.0.0 -s -l '[^\n]\z'
haystack-no-trailing-newline
$ rg-14.0.3 -s -l '[^\n]\z'
haystack-with-trailing-newline
haystack-no-trailing-newline
```

The fact that that worked in ripgrep 13 was a bug. Anchors line `\A` and `\z` don't make sense with how you're using them because ripgrep is a line oriented searcher. There is no notion of "end of file." The only thing available to you is "end of line." And the logical model under which ripgrep operates is to search the contents of each line, and _not_ the actual line terminators themselves.

The only way to have this work correctly is to enable multiline mode. In multiline mode, ripgrep is no longer line oriented, and treats the entire contents of each file as a single haystack. In that context, you can use anchors like `\A` and `\z` to mean "beginning of file" and "end of file," respectively.

In your case, you just need to add the `-U` flag:

```
$ rg-13.0.0 -l -U '[^\n]\z'
haystack-no-trailing-newline
$ rg-14.0.3 -l -U '[^\n]\z'
haystack-no-trailing-newline
```

---

_Closed by @BurntSushi on 2024-01-06 19:07_

---

_Label `invalid` added by @BurntSushi on 2024-01-06 19:07_

---

_Comment by @ksandvik on 2024-01-11 19:11_

Thanks for the clarification.

---
