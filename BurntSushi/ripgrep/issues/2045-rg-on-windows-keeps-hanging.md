```yaml
number: 2045
title: rg on Windows keeps hanging
type: issue
state: closed
author: annazolkieve
labels:
  - invalid
assignees: []
created_at: 2021-10-27T14:08:29Z
updated_at: 2021-11-22T14:51:16Z
url: https://github.com/BurntSushi/ripgrep/issues/2045
synced_at: 2026-01-12T16:13:24Z
```

# rg on Windows keeps hanging

---

_@annazolkieve_

Jul 21 2020:
```
grep -r xxx .
xxx.c:extern void      xxx ( void )

ext code 0 / time elapsed 397ms

$ rg xxx
xxx.c
492: extern void      xxx ( void )

# rg hangs: have to interrupt rg via CTRL+C
exit code 127 / time elapsed 46m21s

rg version: ??
```

Oct 27 2021:
```
$ grep -r xxx .
./file1.cpp:     	xxx
./file2.cpp:     	xxx
...
./fileN.cpp:     	xxx
ext code 0 / time elapsed: 1.427s

$ rg xxx
./file1.cpp:     	xxx
./file2.cpp:     	xxx
...
# rg hangs: have to interrupt rg via CTRL+C
exit code 127 / time elapsed: 29m28s

$ rg --version
ripgrep 13.0.0 (rev af6b6c543b)
-SIMD -AVX (compiled)
+SIMD -AVX (runtime)
```
Cannot share the directory, sorry.
OS: Win10 64-bit.

---

_Comment by @BurntSushi on 2021-10-27 14:14_

Sorry, but I cannot reproduce this. Without a reproduction, it's basically impossible for me to debug. The only other approach here is for you to debug it. You'll probably need to run with the `--trace` flag, or wittle your corpus down, until you find the exact file that ripgrep is hanging on.

---

_Comment by @chuanqisun on 2021-11-11 01:24_

Not sure if it's related. I'm maintaining an app that calls the `rg` command from node.js, using [child_process.exec](https://nodejs.org/api/child_process.html#child_processexeccommand-options-callback) api. [Here](https://github.com/osmoscraft/osmosnote/blob/master/packages/server/src/routes/search-note.ts#L126) is how the rg command is being generated, and [here](https://github.com/osmoscraft/osmosnote/blob/master/packages/server/src/lib/run-shell.ts) is how the `exec` is being called. After I upgrade from rg 12.1.1 to 13.0.0, the `exec` no longer invokes the callback, as if the child process never finished. Some more observations:
1. I was able to manually execute the same command in terminal without any problems.
2. The `exec` can execute other shell command like `ls` or `git` without any problems.

So, for what it's worth, I wonder if there were some changes in 13.0.0 that affected how the process exits, and because of that, node.js stop receiving the exit signal.

For reference, this happened in WSL2. `5.10.60.1-microsoft-standard-WSL2 #1 SMP Wed Aug 25 23:20:18 UTC 2021 x86_64 x86_64 x86_64 GNU/Linux`

UPDATE

just tried on my linux machine, found the same hanging issue.
`5.13.0-7620-generic #20~1634827117~21.04~874b071-Ubuntu SMP Fri Oct 29 15:06:55 UTC  x86_64 x86_64 x86_64 GNU/Linux`

UPDATE 2

I've created a minimum repro and captured trace in https://github.com/BurntSushi/ripgrep/issues/2056. Didn't want to steal the thread from this bug.

---

_Label `invalid` added by @BurntSushi on 2021-11-11 13:35_

---

_Comment by @BurntSushi on 2021-11-11 13:35_

Closing this due to lack of reproduction.

---

_Comment by @matkoniecz on 2021-11-22 14:28_

> Closing this due to lack of reproduction.

It is still open

---

_Closed by @BurntSushi on 2021-11-22 14:51_

---
