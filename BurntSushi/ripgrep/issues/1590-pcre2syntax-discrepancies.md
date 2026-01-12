```yaml
number: 1590
title: "PCRE2SYNTAX discrepancies:"
type: issue
state: closed
author: StacktraceException
labels:
  - invalid
assignees: []
created_at: 2020-05-21T10:33:51Z
updated_at: 2021-10-26T19:43:23Z
url: https://github.com/BurntSushi/ripgrep/issues/1590
synced_at: 2026-01-12T16:13:23Z
```

# PCRE2SYNTAX discrepancies:

---

_@StacktraceException_

#### What version of ripgrep are you using?

ripgrep 12.1.0
-SIMD -AVX (compiled)
+SIMD -AVX (runtime)

#### How did you install ripgrep?
pacman -S ripgrep

#### What operating system are you using ripgrep on?

Arch Linux (up-to-date)

#### Describe your bug.

ripgrep -P does not support returning capture groups:

rg -P '(el)' --replace '\1'  replaces all el occurences with \1 instead of the capture group.
rg -P '(el)' --replace '\\1' is the it should (and it does) happen.

Alslo tried named capturing groups per the pcre2syntax documentation, they don't work neither.

Maybe this is related to the shell interpreting some chars as command arguements for the entire command instead of the expression, which is another bug: for example a dash - should not have to be escaped by default.


PCRE2 is the best: https://en.wikipedia.org/wiki/Comparison_of_regular-expression_engines

---

_Comment by @BurntSushi on 2020-05-21 11:29_

Could you please advise me on how to improve the docs for the `--replace` flag? Currently, they say:

> Capture group indices (e.g., $5) and names (e.g., $foo) are supported in the replacement string.

-----

> for example a dash - should not have to be escaped by default.

The docs for the PATTERN positional argument say:

> To match a pattern beginning with a dash, use the `-e/--regexp` option

-----

> PCRE2 is the best: https://en.wikipedia.org/wiki/Comparison_of_regular-expression_engines

I don't understand what you're trying to say. There is no "best" regex engine. And that table doesn't even have ripgrep's default regex engine.

---

_Comment by @BurntSushi on 2021-10-26 19:43_

Closing due to inactivity.

---

_Closed by @BurntSushi on 2021-10-26 19:43_

---

_Label `invalid` added by @BurntSushi on 2021-10-26 19:43_

---
