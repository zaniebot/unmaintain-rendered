```yaml
number: 1261
title: Attempting to use a newline character in pattern with the PCRE2 engine, outside multiline mode fails silently
type: issue
state: closed
author: dprobinson
labels:
  - bug
assignees: []
created_at: 2019-04-21T22:59:39Z
updated_at: 2019-08-01T21:46:32Z
url: https://github.com/BurntSushi/ripgrep/issues/1261
synced_at: 2026-01-12T16:13:23Z
```

# Attempting to use a newline character in pattern with the PCRE2 engine, outside multiline mode fails silently

---

_@dprobinson_

#### What version of ripgrep are you using?

ripgrep 11.0.1 (rev e7829c05d3)
-SIMD -AVX (compiled)
+SIMD -AVX (runtime)

#### How did you install ripgrep?

Compiled from source

#### What operating system are you using ripgrep on?

Arch Linux

#### Describe your question, feature request, or bug.

Attempting to use a newline character ("\n") in pattern with the PCRE2 engine (-P), outside multiline mode fails silently.

#### If this is a bug, what are the steps to reproduce the behavior?

Create the following corpus.txt:
```
testing
RIP
GREP
again
```

First, try the search with Rust's engine:
```
rg "RIP\nGREP" corpus.txt
```
Output:
```
the literal '"\n"' is not allowed in a regex

Consider enabling multiline mode with the --multiline flag (or -U for short).
When multiline mode is enabled, new line characters can be matched.
```
As expected (from the documentation), we see an error.


Now try searching using the PCRE2 engine:
```
rg -P "RIP\nGREP" corpus.txt
```
Output:
```
```
As we see, we've tried to use a newline char without multiline mode, but receive no error. The search fails silently.

N.B. Searching using the PCRE2 engine, with multiline mode (-U) works as expected:
```
rg -PU "RIP\nGREP" corpus.txt
```
Output:
```
2:RIP
3:GREP
```

#### If this is a bug, what is the actual behavior?

See output above

#### If this is a bug, what is the expected behavior?

I would expect ripgrep to throw an error, warning the user that newlines cannot be matched outside multiline mode (-U) when using PCRE2. This would yield the same behaviour as when Rust's engine is used (#1055)

---

_Comment by @BurntSushi on 2019-04-21 23:14_

This can't be fixed because PCRE doesn't expose anything to parse its syntax to detect the newline characters in the pattern. It might be possible to detect some simple cases without parsing the regex, but I don't know how far down that road I want to go.

---

_Comment by @dprobinson on 2019-04-21 23:28_

That's a shame. I thought the reason might be something like that.

Maybe it would be worth us tweaking the documentation, to make to clear an error won't be thrown when using PCRE2?

Current text:
> For example, when multiline mode is not enabled (the default), then the regex \p{any} will match any Unicode codepoint other than \n. Similarly, the regex \n is explicitly forbidden, and if you try to use it, ripgrep will return an error.

---

_Label `bug` added by @BurntSushi on 2019-04-22 11:42_

---

_Closed by @BurntSushi on 2019-08-01 21:46_

---
