```yaml
number: 773
title: Explain ld errors
type: pull_request
state: merged
author: konstin
labels:
  - error messages
assignees: []
merged: true
base: main
head: explain-ld-errors
created_at: 2024-01-04T12:19:13Z
updated_at: 2024-01-04T19:56:39Z
url: https://github.com/astral-sh/uv/pull/773
synced_at: 2026-01-12T16:04:11Z
```

# Explain ld errors

---

_@konstin_

One of the most common ways source dists fail to build (on linux) is when the linker fails because the shared library of a native dependency is not installed. These errors are hard to understand when you're not a c programmer:

```
       In file included from /usr/include/python3.10/unicodeobject.h:1046,
                        from /usr/include/python3.10/Python.h:83,
                        from Modules/3.x/readline.c:8:
       Modules/3.x/readline.c: In function ‘on_completion’:
       /usr/include/python3.10/cpython/unicodeobject.h:744:29: warning: initialization discards ‘const’ qualifier from pointer target type [-Wdiscarded-qualifiers]
         744 | #define _PyUnicode_AsString PyUnicode_AsUTF8
             |                             ^~~~~~~~~~~~~~~~
       Modules/3.x/readline.c:842:23: note: in expansion of macro ‘_PyUnicode_AsString’
         842 |             char *s = _PyUnicode_AsString(r);
             |                       ^~~~~~~~~~~~~~~~~~~
       Modules/3.x/readline.c: In function ‘readline_until_enter_or_signal’:
       Modules/3.x/readline.c:1044:9: warning: ‘sigrelse’ is deprecated: Use the sigprocmask function instead [-Wdeprecated-declarations]
        1044 |         sigrelse(SIGINT);
             |         ^~~~~~~~
       In file included from Modules/3.x/readline.c:10:
       /usr/include/signal.h:359:12: note: declared here
         359 | extern int sigrelse (int __sig) __THROW
             |            ^~~~~~~~
       Modules/3.x/readline.c: In function ‘PyInit_readline’:
       Modules/3.x/readline.c:1179:34: warning: assignment to ‘char * (*)(FILE *, FILE *, const char *)’ from incompatible pointer type ‘char * (*)(FILE *, FILE *, char *)’ [-Wincompatible-pointer-types]
        1179 |     PyOS_ReadlineFunctionPointer = call_readline;
             |                                  ^
       In file included from /usr/include/string.h:535,
                        from /usr/include/python3.10/Python.h:30,
                        from Modules/3.x/readline.c:8:
       In function ‘strncpy’,
           inlined from ‘call_readline’ at Modules/3.x/readline.c:1124:9:
       /usr/include/x86_64-linux-gnu/bits/string_fortified.h:95:10: warning: ‘__builtin_strncpy’ output truncated before terminating nul copying as many bytes from a string as its length [-Wstringop-truncation]
          95 |   return __builtin___strncpy_chk (__dest, __src, __len,
             |          ^~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
          96 |                                   __glibc_objsize (__dest));
             |                                   ~~~~~~~~~~~~~~~~~~~~~~~~~
       Modules/3.x/readline.c: In function ‘call_readline’:
       Modules/3.x/readline.c:1099:9: note: length computed here
        1099 |     n = strlen(p);
             |         ^~~~~~~~~
       /usr/bin/ld: cannot find -lncurses: No such file or directory
       collect2: error: ld returned 1 exit status
       error: command '/usr/bin/x86_64-linux-gnu-gcc' failed with exit code 1
       ---
```

We parse these errors out, tell the user about the missing shared library and even the most likely debian/ubuntu package name:

```
This error likely indicates that you need to install the library that provides a shared library for ncurses for pygraphviz-1.11 (e.g. libncurses-dev)
```

---

_Label `error messages` added by @konstin on 2024-01-04 12:19_

---

_@charliermarsh approved on 2024-01-04 19:34_

---

_Merged by @konstin on 2024-01-04 19:56_

---

_Closed by @konstin on 2024-01-04 19:56_

---

_Branch deleted on 2024-01-04 19:56_

---
