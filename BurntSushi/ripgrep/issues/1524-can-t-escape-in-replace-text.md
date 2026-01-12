```yaml
number: 1524
title: "Can't escape '$' in --replace text"
type: issue
state: closed
author: linuskr
labels:
  - doc
assignees: []
created_at: 2020-03-19T14:20:11Z
updated_at: 2020-05-09T03:24:43Z
url: https://github.com/BurntSushi/ripgrep/issues/1524
synced_at: 2026-01-12T16:13:23Z
```

# Can't escape '$' in --replace text

---

_@linuskr_

#### What version of ripgrep are you using?
ripgrep 12.0.0
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)

#### What operating system are you using ripgrep on?

Windows 10

#### Describe your question, feature request, or bug.

When using ripgrep's --replace option, '$' can't be used in the replacement text, because it is interpreted as the name of a capture group and cannot be escaped:  
`rg -iIN 'test([^:_$])' -r '$replacement_text$$1'` outputs `$1` instead of e.g. `$replacement_text$!`


Trying to escape the '$' does not work either:  
`rg -iIN 'test([^:_$])' -r '\$replacement_text\$$1'` yields `\\$1` instead.

---

_Comment by @BurntSushi on 2020-03-19 14:26_

It can be escaped. Use `$$` to write a literal `$`. It's documented here: https://docs.rs/regex/1.3.5/regex/struct.Captures.html#method.expand

It should be documented in the help text for the `--replace` flag though, so I'll mark this as a doc bug.

---

_Label `doc` added by @BurntSushi on 2020-03-19 14:26_

---

_Comment by @linuskr on 2020-03-19 14:37_

Didn't think about that, thank you for the quick response and ripgrep itself.

---

_Closed by @BurntSushi on 2020-05-09 03:24_

---
