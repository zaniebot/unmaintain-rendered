```yaml
number: 3233
title: "Unable to search pattern which is terminated with `\\\\$`"
type: issue
state: closed
author: andrey-starodubtsev
labels: []
assignees: []
created_at: 2025-12-01T12:13:00Z
updated_at: 2025-12-01T12:25:01Z
url: https://github.com/BurntSushi/ripgrep/issues/3233
synced_at: 2026-01-12T16:13:25Z
```

# Unable to search pattern which is terminated with `\\$`

---

_@andrey-starodubtsev_

### Please tick this box to confirm you have reviewed the above.

- [x] I have a different issue.

### What version of ripgrep are you using?

```
C:\> rg --version
ripgrep 15.1.0 (rev af60c2de9d)

features:+pcre2
simd(compile):+SSE2,-SSSE3,-AVX2
simd(runtime):+SSE2,+SSSE3,+AVX2

PCRE2 10.45 is available (JIT is available)
```

### How did you install ripgrep?

`winget install BurntSushi.ripgrep.MSVC`

### What operating system are you using ripgrep on?

Windows 11 Home

### Describe your bug.

`ripgrep` can't find patterns which are terminated with `\\$`

### What are the steps to reproduce the behavior?

```
C:\>cat 1.h
#pragma once

#define FOO(x) \
    static int a = x; \
    ++a;

C:\>grep "static.*\\\\$" 1.h
    static int a = x; \

C:\>grep --version
grep (GNU grep) 3.0
Copyright (C) 2017 Free Software Foundation, Inc.
License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>.
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.

Written by Mike Haertel and others, see <http://git.sv.gnu.org/cgit/grep.git/tree/AUTHORS>.

C:\>rg "static.*\\\\$" 1.h

```

WSL version with the latest release of `ripgrep` works fine:

```
➜  /tmp cat 1.h
#pragma once

#define FOO(x)        \
    static int a = x; \
    ++a;
➜  /tmp grep "static.*\\\\$" 1.h
    static int a = x; \
➜  /tmp rg "static.*\\\\$" 1.h
4:    static int a = x; \
➜  grep --version
grep (GNU grep) 3.7
Copyright (C) 2021 Free Software Foundation, Inc.
License GPLv3+: GNU GPL version 3 or later <https://gnu.org/licenses/gpl.html>.
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.

Written by Mike Haertel and others; see
<https://git.sv.gnu.org/cgit/grep.git/tree/AUTHORS>.
➜  /tmp rg --version
ripgrep 15.1.0 (rev af60c2de9d)

features:+pcre2
simd(compile):+SSE2,-SSSE3,-AVX2
simd(runtime):+SSE2,+SSSE3,+AVX2

PCRE2 10.45 is available (JIT is available)
```

### What is the actual behavior?

empty match

### What is the expected behavior?

non-empty match

---

_Renamed from "Unable to search pattern which terminates with `\\$`" to "Unable to search pattern which is terminated with `\\$`" by @andrey-starodubtsev on 2025-12-01 12:13_

---

_Locked by @ghost on 2025-12-01 12:25_

---
