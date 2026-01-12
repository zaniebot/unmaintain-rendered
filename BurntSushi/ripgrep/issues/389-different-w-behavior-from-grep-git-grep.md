```yaml
number: 389
title: Different -w behavior from grep/git-grep
type: issue
state: closed
author: crumblingstatue
labels:
  - question
  - libripgrep
assignees: []
created_at: 2017-03-01T10:22:01Z
updated_at: 2018-08-20T11:10:21Z
url: https://github.com/BurntSushi/ripgrep/issues/389
synced_at: 2026-01-12T16:13:21Z
```

# Different -w behavior from grep/git-grep

---

_@crumblingstatue_

```
$ echo '-2' | rg -w '\-2' # rg yields no results
$ echo '-2' | grep -w '\-2' # grep (and git grep) matches
-2
```

Not sure if this is intentional or not, but it did surprise me that I didn't find what I was looking for when searching my codebase with ripgrep, and I had to resort to git-grep.

I used `-w` because I was specifically looking for the value `-2`, and not e.g. `-24`.

Using `ripgrep 0.4.0`.

---

_Comment by @BurntSushi on 2017-03-01 13:21_

This is highly interesting.

The relevant passage from `man grep` (for GNU grep) is:

```
-w, --word-regexp
       Select only those lines containing matches that form whole words. The
       test is that the matching substring must either be at the beginning of
       the line, or preceded by a non-word constituent character. Similarly,
       it must be either at the end of the line or followed by a non-word
       constituent character. Word-constituent characters are letters, digits,
       and the underscore. This option has no effect if -x is also specified.
```

The key part is "... either be **at the beginning of the line**, or preceded by a non-word constituent character." ripgrep currently implements the `-w` flag by translating the given `pattern` to `\b(?:pattern)\b`, but it looks like grep actually does `(?:^|\b)(?:pattern)(?:$|\b)`. I guess ripgrep should do that as well.

If my hypothesis is correct, then `echo ' -2' | grep -w -e '-2'` should return nothing. This is because neither ` ` nor `-` match `\w`, and therefore, `\b` shouldn't match. Interestingly, it does return a match:

```
$ echo ' -2' | grep -w -e '-2'
 -2
```

While the equivalent ripgrep command does not:

```
$ echo ' -2' | rg -e '(^|\b)-2($|\b)'
```

Interestingly, the same grep command does not either:

```
$ echo ' -2' | egrep -e '(^|\b)-2($|\b)'
```

This has to mean that my interpretation of `-w` is wrong.

Re-reading it, it now seems clearer to me that it isn't actually using word boundary assertions, since it says "preceded by a non-word constituent character" but doesn't say anything about the first letter of the match.

Looking at the source of GNU grep, I spotted this:

```
  /* In the match_words and match_lines cases, we use a different pattern
     for the DFA matcher that will quickly throw out cases that won't work.
     Then if DFA succeeds we do some hairy stuff using the regex matcher
     to decide whether the match should really count. */
  if (match_words || match_lines)
    {
      static char const line_beg_no_bk[] = "^(";
      static char const line_end_no_bk[] = ")$";
      static char const word_beg_no_bk[] = "(^|[^[:alnum:]_])(";
      static char const word_end_no_bk[] = ")([^[:alnum:]_]|$)";
      static char const line_beg_bk[] = "^\\(";
      static char const line_end_bk[] = "\\)$";
      static char const word_beg_bk[] = "\\(^\\|[^[:alnum:]_]\\)\\(";
      static char const word_end_bk[] = "\\)\\([^[:alnum:]_]\\|$\\)";
```

Which is interesting and confirms my suspicion that `\b` isn't actually being used to implement `-w`.

---

I will need to think on this more to figure out what to do. In the meantime, you could, depending on your use case, work-around this by:

```
$ echo ' -2' | rg -e '(^|\W)-2($|\W)'
1: -2
```

(This isn't a full solution though, since your colors will be messed up by including the surrounding non-word characters.)

---

_Label `question` added by @BurntSushi on 2017-03-01 13:21_

---

_Renamed from "Different word boundary behavior from grep/git-grep" to "Different -w behavior from grep/git-grep" by @crumblingstatue on 2017-03-01 13:41_

---

_Comment by @crumblingstatue on 2017-03-01 13:54_

> Re-reading it, it now seems clearer to me that it isn't actually using word boundary assertions, since it says "preceded by a non-word constituent character" *but doesn't say anything about the first letter of the match*.

I think that's the key point. The match itself can contain both word and non-word characters. It's the surrounding context that matters. That way, you can intuitively match expressions like `2 - 2`, and it will only match that, and not e.g. `42 - 24`.

---

_Label `libripgrep` added by @BurntSushi on 2017-03-12 20:53_

---

_Added to milestone `libripgrep` by @BurntSushi on 2017-03-12 20:53_

---

_Comment by @BurntSushi on 2018-06-30 13:49_

I have pretty high confidence that this will be fixed in libripgrep.

---

_Closed by @BurntSushi on 2018-08-20 11:10_

---
