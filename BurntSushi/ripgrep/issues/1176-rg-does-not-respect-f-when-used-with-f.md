```yaml
number: 1176
title: rg does not respect -F when used with -f
type: issue
state: closed
author: draggo
labels: []
assignees: []
created_at: 2019-01-26T20:42:31Z
updated_at: 2019-01-26T21:05:55Z
url: https://github.com/BurntSushi/ripgrep/issues/1176
synced_at: 2026-01-12T16:13:23Z
```

# rg does not respect -F when used with -f

---

_@draggo_

#### What version of ripgrep are you using?

$ rg --version
ripgrep 0.10.0
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)

$ ripgrep/target/release/rg --version
ripgrep 0.10.0 (rev bf842dbc7f)
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)

#### How did you install ripgrep?

2 ways:
1. brew install ripgrep
2. local build of source per "To build ripgrep" instructions, with Rust installed via 'brew install rust'

#### What operating system are you using ripgrep on?

macOS High Sierra, version 10.13.6 (17G3025)

#### Describe your question, feature request, or bug.

Even with -F option, rg attempts to interpret meta characters for patterns supplied in a file.

#### If this is a bug, what are the steps to reproduce the behavior?

Create a simple test file containing meta characters
% echo 'not a (cloud) in the sky' > test.txt

Searching the file using the file itself as a fixed pattern doesn't find a match.
% rg -F -f test.txt test.txt
[no output]

But, searching the file with the fixed pattern specified on the command line does find a match (as desired).
% rg -F 'not a (cloud) in the sky' test.txt
```
1:not a (cloud) in the sky
```

I believe that the first search should produce the same result as the second one.

#### If this is a bug, what is the actual behavior?

$ rg --debug -F -f test.txt test.txt
```
DEBUG|grep_regex::literal|grep-regex/src/literal.rs:58: literal prefixes detected: Literals { lits: [Complete(not a cloud in the sky)], limit_size: 250, limit_class: 10 }
DEBUG|globset|globset/src/lib.rs:429: built glob set; 0 literals, 0 basenames, 8 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
```
$ rg --debug -F 'not a (cloud) in the sky' test.txt
```
DEBUG|grep_regex::literal|grep-regex/src/literal.rs:58: literal prefixes detected: Literals { lits: [Complete(not a (cloud) in the sky)], limit_size: 250, limit_class: 10 }
DEBUG|globset|globset/src/lib.rs:429: built glob set; 0 literals, 0 basenames, 8 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
1:not a (cloud) in the sky
```

#### If this is a bug, what is the expected behavior?

When both -F (or --fixed-strings) and -f (or --file) are given, rg should not attempt to interpret any meta characters in the PATTERNFILE.

For reference, the macOS default grep and GNU grep (installed via homebrew) give the expected result.  (But are not nearly as fast as ripgrep for many of my tasks; **thank you for a fantastic tool!**)

$ grep -F -f test.txt test.txt
```
not a (cloud) in the sky
```
$ grep -V
```
grep (BSD grep) 2.5.1-FreeBSD
```

$ ggrep -F -f test.txt test.txt
```
not a (cloud) in the sky
```
$ ggrep -V
```
ggrep (GNU grep) 3.3
Packaged by Homebrew
Copyright (C) 2018 Free Software Foundation, Inc.
License GPLv3+: GNU GPL version 3 or later <https://gnu.org/licenses/gpl.html>.
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.

Written by Mike Haertel and others; see
<https://git.sv.gnu.org/cgit/grep.git/tree/AUTHORS>.
```

---

_Closed by @BurntSushi on 2019-01-26 21:01_

---

_Comment by @BurntSushi on 2019-01-26 21:02_

Wow, what a terrible regression! I'm surprised we've gone this long without hearing about it. This is fixed on master.

---

_Comment by @draggo on 2019-01-26 21:05_

Fix confirmed ... thank you!  And thanks again for the great tool.

---
