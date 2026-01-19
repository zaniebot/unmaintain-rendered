```yaml
number: 478
title: improve error messages when -f flag fails
type: issue
state: open
author: BurntSushi
labels:
  - enhancement
  - help wanted
assignees: []
created_at: 2017-05-09T17:10:51Z
updated_at: 2026-01-19T15:03:03Z
url: https://github.com/BurntSushi/ripgrep/issues/478
synced_at: 2026-01-19T15:37:26Z
```

# improve error messages when -f flag fails

---

_@BurntSushi_

For example:

```
$ rg -e foo -f src/search_stream.rs 
Error parsing regex near 'fer.|*/|z{' at character offset 213: Invalid application of repetition operator to: '(?u:f)(?u:o)(?u:o)|(?u:/)*(?u:!)|(?u:T)(?u:h)(?u:e)
```

Aside from the badly formatted regex error message (see #395), the error message should show the file path and probably also the line number at which the error occurred.

---

_Label `enhancement` added by @BurntSushi on 2017-05-09 17:10_

---

_Label `help wanted` added by @BurntSushi on 2017-05-09 17:10_

---

_Comment by @nateozem on 2017-05-17 10:40_

let me see what I can do.

---

_Comment by @andrea-prearo on 2017-08-30 20:20_

@BurntSushi I submitted a [PR](https://github.com/BurntSushi/ripgrep/pull/590) to address this issue. It also tries to avoid the problem with the AST dump (issue https://github.com/BurntSushi/ripgrep/issues/395).

I have one question. Were you thinking about reporting the file path and line number where the error occurred in [regex](https://github.com/rust-lang/regex)?

---

_Comment by @BurntSushi on 2017-08-30 20:59_

> I have one question. Were you thinking about reporting the file path and line number where the error occurred in regex?

Probably not. I could maybe see a case for pushing the line number reporting into the regex crate, but certainly not the file path.

---

_Comment by @andrea-prearo on 2017-08-30 21:18_

Got it. Thanks!

---

_Comment by @petr-tik on 2019-02-04 15:45_

looks like you can close this issue due to rust-lang/regex#396

I am lurking around the repo as a happy user of ripgrep - thanks for the awesome project

---

_Comment by @BurntSushi on 2019-02-04 16:04_

@petr-tik Thanks for the kind words, but this is definitely not fixed yet. For example:

```
$ cat /tmp/patterns
foo
bar(wat
quux

$ rg -f /tmp/patterns
regex parse error:
    foo|bar(wat|quux
           ^
error: unclosed group
```

There are a few problems here:

1. The file path of the pattern file isn't shown in the error message. This is important if you use `-f` multiple times.
2. There is no line number corresponding to the pattern that failed to parse.
3. The error shows *all* of the patterns in the file given joined with a `|` instead of just the pattern that is invalid.

---

_Comment by @petr-tik on 2019-02-04 16:23_

I must have misunderstood the PRs. Only wanted to help you declutter your issues, in case it had been forgotten. 

---

_Comment by @J-Kappes on 2023-09-05 12:04_

Which files in the repo could be relevant to this?

---

_Comment by @c0d3l0v3r-HeHe on 2026-01-19 15:03_

Hi @BurntSushi , just checking whether this issue is still open.
If it hasn’t been addressed yet, I’d be happy to take a look and work on it.

---
