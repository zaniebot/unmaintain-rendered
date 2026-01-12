```yaml
number: 1262
title: Multiline mode cannot be enabled or disabled using inline modifiers
type: issue
state: closed
author: dprobinson
labels: []
assignees: []
created_at: 2019-04-21T23:09:35Z
updated_at: 2019-04-21T23:28:15Z
url: https://github.com/BurntSushi/ripgrep/issues/1262
synced_at: 2026-01-12T16:13:23Z
```

# Multiline mode cannot be enabled or disabled using inline modifiers

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

It's possible to turn case sensitivity (and presumably other modes) on and off using inline modifiers, but this doesn't extend to multiline mode "(?m)".

#### If this is a bug, what are the steps to reproduce the behavior?

Create the following corpus.txt:
```
testing
RIP
GREP
again
```
With PCRE2 engine:
```
rg -P "(?m)RIP\nGREP" corpus.txt
```
Output (blank - see bug #1261):
```
```

With Rust's engine:
```
rg "(?m)RIP\nGREP" corpus.txt
```
Output:
```
the literal '"\n"' is not allowed in a regex

Consider enabling multiline mode with the --multiline flag (or -U for short).
When multiline mode is enabled, new line characters can be matched.
```

N.B. It's not possible to disable multiline mode via inline modifiers either. Eg:

```
rg -U "(?-m)RIP\nGREP" corpus.txt
```
Output:
```
2:RIP
3:GREP
```
When you wouldn't expect a match.

#### If this is a bug, what is the actual behavior?

See above for output. Multiline mode cannot be enabled or disabled using inline modifiers.

#### If this is a bug, what is the expected behavior?

ripgrep to read the inline modifiers and enable or disable multiline mode for the relevant parts of the provided pattern as directed.

---

_Comment by @BurntSushi on 2019-04-21 23:16_

ripgrep's multiline mode is not the same as regex multiline mode. The former just allows patterns to match over multiple lines. The latter just makes anchors match at line boundaries (which is enabled by default for all searches, regardless of whether ripgrep's multiline mode is enabled or not). They are entirely orthogonal options.

---

_Closed by @BurntSushi on 2019-04-21 23:16_

---

_Comment by @dprobinson on 2019-04-21 23:28_

That makes sense. I'd wondered if this was actually a bug or not, thanks for clarifying!

---
