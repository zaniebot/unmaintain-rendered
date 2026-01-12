```yaml
number: 1083
title: Add a flag to disable regex in the search pattern.
type: issue
state: closed
author: Addisonbean
labels: []
assignees: []
created_at: 2018-10-16T03:06:49Z
updated_at: 2018-10-16T03:19:50Z
url: https://github.com/BurntSushi/ripgrep/issues/1083
synced_at: 2026-01-12T16:13:22Z
```

# Add a flag to disable regex in the search pattern.

---

_@Addisonbean_

#### What version of ripgrep are you using?

```
ripgrep 0.10.0 (rev 8a7db1a918)
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
```

#### How did you install ripgrep?

`brew install ripgrep-bin`

#### What operating system are you using ripgrep on?

macOS 10.12.6

#### Describe your question, feature request, or bug.

This is a feature request for a flag to disable regex and do a normal search, which will treat special regex characters like a normal characters. The main benefit behind this is that it makes searching for strings which contain special regex characters much cleaner. Searching for the string `(${a_value})`  would require typing `\(\$\{a_value\}\)` without this feature.

---

_Comment by @okdana on 2018-10-16 03:17_

```
-F, --fixed-strings
    Treat the pattern as a literal string instead of a regular expression. When
    this flag is used, special regular expression meta characters such as .(){}*+
    do not need to be escaped.
```

---

_Comment by @Addisonbean on 2018-10-16 03:19_

@okdana Thanks for the help! I must have missed that in my search, sorry!

---

_Closed by @Addisonbean on 2018-10-16 03:19_

---
