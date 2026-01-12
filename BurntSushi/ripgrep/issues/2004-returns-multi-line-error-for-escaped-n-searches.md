```yaml
number: 2004
title: Returns multi-line error for escaped \\n searches
type: issue
state: closed
author: Mattsi-Jansky
labels:
  - invalid
assignees: []
created_at: 2021-09-24T10:13:44Z
updated_at: 2021-09-24T14:12:54Z
url: https://github.com/BurntSushi/ripgrep/issues/2004
synced_at: 2026-01-12T16:13:24Z
```

# Returns multi-line error for escaped \\n searches

---

_@Mattsi-Jansky_

#### What version of ripgrep are you using?

```
ripgrep 12.1.1
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
```

#### How did you install ripgrep?

apt-get

#### What operating system are you using ripgrep on?

PopOS (a flavour of Ubuntu)

#### Describe your bug.


The error "the literal '"\n"' is not allowed in a regex" returns an error for the regex `\\n` because RG thinks it should be a multi-line search. Yet the `\n` is actually escaped so it doesn't match a literal newline character, it matches `\n` literally. The error message should only appear for `\n`, not `\\n`.

#### What are the steps to reproduce the behavior?

Run `rg \\n`

#### What is the actual behavior?

```
$ rg \\n
the literal '"\n"' is not allowed in a regex

Consider enabling multiline mode with the --multiline flag (or -U for short).
When multiline mode is enabled, new line characters can be matched.
```

#### What is the expected behavior?

Not return an error for regexes containing `\\n`


---

_Comment by @BurntSushi on 2021-09-24 11:56_

The behavior here is correct. It's your escaping that is not right. Try, for example, `rg \n`. That doesn't search for a newline. It searches for `n`. It's `rg \\n` that searches for a newline. To do it your way, I believe you would need `rg \\\\n`.

To be honest, I confess that I do not quite grok the full unquoted escaping rules of shell, so i can't give you a full explanation here. Instead, I can offer you some wisdom: when using escape sequences or special characters, consider putting them in single quotes. For example, `rg '\\n'` behaves how you want here.

Note that ripgrep does not detect newline characters in regexes with a simple search. Its detection is precisely done via the regex parser itself. So if the regex parser gets it wrong, then either something is very very very wrong (unlikely) or your input isn't the input you think it is (much more likely).

---

_Closed by @BurntSushi on 2021-09-24 11:56_

---

_Label `invalid` added by @BurntSushi on 2021-09-24 11:56_

---

_Comment by @Mattsi-Jansky on 2021-09-24 14:12_

You're totally right, I forgot about my terminal's own escaping. Thanks

---
