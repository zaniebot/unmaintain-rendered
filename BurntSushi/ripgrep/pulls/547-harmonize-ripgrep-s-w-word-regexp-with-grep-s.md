```yaml
number: 547
title: "harmonize ripgrep's -w|--word-regexp with grep's"
type: pull_request
state: closed
author: kevinr
labels: []
assignees: []
base: master
head: kevinr/harmonize-grep-word-match-behavior
created_at: 2017-07-08T02:23:41Z
updated_at: 2018-01-29T21:10:11Z
url: https://github.com/BurntSushi/ripgrep/pull/547
synced_at: 2026-01-12T18:23:13Z
```

# harmonize ripgrep's -w|--word-regexp with grep's

---

_@kevinr_

This fixes https://github.com/BurntSushi/ripgrep/issues/389

grep (and git-grep) allow a match using --word-regexp to begin and end with
non-word characters, by surrounding the match with `(^|[^[:alnum:]_])` and
`([^[:alnum:]_]|$)`.

Before this patch ripgrep, by surrounding the match with `\b`, implicitly
required the match to start with a `\w` character, and would fail to match eg.
the ` - 2` in `1 - 2`, which intuitively should be allowed.

This fixes this behavior by surrounding the match with `(?:^|\W)` and
`(?:\W|$)`, using the Unicode non-word character class `\W` rather than the
ASCII character class `[:alnum:]`

I have tested that this combines correctly with `--only-matching`, that is, leading non-word characters are not printed along with the match when that option is used.

I have *not* tested that this combines correctly with `--replace`; however, use of non-capturing matches should ensure that references are numbered correctly.

---

_Comment by @kevinr on 2017-07-08 02:30_

Ah. Let me fix the tests and I'll give it another shot.

---

_Comment by @okdana on 2017-07-08 17:44_

I *think* that in order for this method to work correctly you'd have to change the printer so that when it prints the match it checks to see if `-w` is active, and if the match start/end is not equal to the line start/end it contracts the match by one character on each applicable side.

That would account for the standard behaviour as well as the `-o` behaviour, anyway. Not sure about `--replace`, might be more complicated.

---

_Comment by @BurntSushi on 2017-07-09 00:52_

Unfortunately, I think @okdana is right. The key difference is that the `\b(pat)\b` formulation uses zero length assertions, so the surrounding `\b` assertions don't actually consume any input in the match. But when you start using `\W`, that might consume at most one Unicode codepoint.

---

_Comment by @kevinr on 2017-07-10 20:54_

Oh crud. I can reproduce, I wasn't seeing the issue before because of an unrelated issue.

To check my understanding, it looks like the code around https://github.com/BurntSushi/ripgrep/blob/master/src/printer.rs#L252 is going to need to either pull the start and end of the target submatch from the match object in the presence of --word-regexp, rather than using the whole match, or increment and decrement the start and end of the whole match by the length of the beginning and ending submatches?

I was trying to avoid capturing matches, but perhaps that can't be helped (and perhaps that's why GNU grep uses capturing matches too).

---

_Comment by @okdana on 2017-07-11 06:52_

If you were going to change the way it's printed, i think you'd have to change it in two different places (`write_match()` and `write_matched_line()`), because non-line-per-match matches (i.e. the default behaviour) have a second search performed on them to figure out where to place the colours. But in each case it's just something like:

```rust
let m_start = m.start() + (m.start() != 0) as usize;
let m_end   = m.end() - (m.end() != buf.len() - 1) as usize;
```

I think probably a bigger issue though is the fact that `\W` not being zero-width means that you can't match two 'words' that are next to each other. For example:

```
given: rg -w bar <<< foo-bar-bar-baz

current method ([...] == match):
  final pattern: \b(?:bar)\b
  actual match:  foo-[bar]-[bar]-baz
  printed match: foo-[bar]-[bar]-baz

proposed method ([...] == match):
  final pattern: (?:^|\W)(?:bar)(?:\W|$)
  actual match:  foo[-bar-]bar-baz
  printed match: foo-[bar]-bar-baz
```

So you'd have to deal with that somehow too. GNU `grep` seems to have some [special handling](http://git.savannah.gnu.org/cgit/grep.git/tree/src/dfasearch.c#n376) for this.

---

_Comment by @BurntSushi on 2018-01-29 21:09_

@kevinr This PR seems to have gotten stalled, and that's mostly my fault for not staying on top of it. Based on the comments above, this issue appears to be a bit more complex than initially thought. For now, I'd like to consider fixing this issue as part of the libripgrep effort, so I'm going to close this PR. I will surely reference back to it for the tests though! Thanks!

---

_Closed by @BurntSushi on 2018-01-29 21:09_

---
