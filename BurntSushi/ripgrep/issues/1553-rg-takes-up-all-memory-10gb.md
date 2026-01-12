```yaml
number: 1553
title: "rg takes up all memory (>10GB)"
type: issue
state: closed
author: albertz
labels:
  - duplicate
assignees: []
created_at: 2020-04-17T10:22:16Z
updated_at: 2020-04-18T17:05:11Z
url: https://github.com/BurntSushi/ripgrep/issues/1553
synced_at: 2026-01-12T16:13:23Z
```

# rg takes up all memory (>10GB)

---

_@albertz_

#### What version of ripgrep are you using?

ripgrep 11.0.1 (rev 973de50c9e)
-SIMD -AVX (compiled)
+SIMD -AVX (runtime)

#### How did you install ripgrep?

Via VSCode, Version: 1.44.0
Commit: 2aae1f26c72891c399f860409176fe435a154b13
Date: 2020-04-08T08:23:56.137Z
Electron: 7.1.11
Chrome: 78.0.3904.130
Node.js: 12.8.1
V8: 7.8.279.23-electron.0
OS: Darwin x64 17.7.0

#### What operating system are you using ripgrep on?

Linux (via the VSCode SSH extension, where VSCode itself runs on Mac).

#### Describe your bug.

The `rg` process takes a lot of memory (>10GB) when searching through a very big directory (several 100GB, also slow NFS, also having a couple of symlinks, which also might be circular).
In the extreme case, it crashes the whole system.

#### What are the steps to reproduce the behavior?

Have a very big directory (several 100GB, also slow NFS, also having a couple of symlinks, which also might be circular).

VSCode ([via](https://github.com/microsoft/vscode-ripgrep/issues/11), [via](https://github.com/microsoft/vscode/issues/95201), [via](https://github.com/microsoft/vscode-python/issues/11173)) starts it like this:
`rg --files --hidden --case-sensitive -g **/*.ipynb -g !**/.git -g !**/.svn -g !**/.hg -g !**/CVS -g !**/.DS_Store -g !**/__pycache__ -g !**/*.pyc --no-ignore --follow --no-config --no-ignore-global`

#### What is the actual behavior?

It starts the search. However, I can see that the memory just increases, and the process never manages to finish.


See `ps axu` on the Linux SSH server, and search for the `rg` process by VSCode:
```
USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
zeyer    14685  117 53.7 8244360 8231756 ?     Sl   11:12  13:09 /u/zeyer/.vscode-server/bin/0ba0ca52957102ca3527cf479571617f0de6ed50/node_modules/vscode-ripgrep/bin/rg --files --hidden --case-sensitive -g **/*.ipynb -g !**/.git -g !**/.svn -g !**/.hg -g !**/CVS -g !**/.DS_Store -g !**/__pycache__ -g !**/*.pyc --no-ignore --follow --no-config --no-ignore-global
zeyer    20113 87.3 12.8 1978088 1965448 ?     Sl   11:30   4:42 /u/zeyer/.vscode-server/bin/2aae1f26c72891c399f860409176fe435a154b13/node_modules/vscode-ripgrep/bin/rg --files --hidden --case-sensitive -g **/*.ipynb -g !**/.git -g !**/.svn -g !**/.hg -g !**/CVS -g !**/.DS_Store -g !**/__pycache__ -g !**/*.pyc --no-ignore --follow --no-config --no-ignore-global
```
The `cwd` of that process is a VSCode setup, which is the very big project directory. Many of the files are behind some symlinks, but I partly want to follow symlinks. (I'm not sure how to selectively disable to not follow some of the symlinks.)

I think the issue is that the directory is just very big.

I happened already multiple times that the Linux server became unresponsive because all memory was occupied, and luckily the Linux OOM killer killed `rg` and the system recovered a while later. This is what I see in `dmesg -T`:

```
[Tue Apr 14 11:14:15 2020] [ pid ]   uid  tgid total_vm      rss pgtables_bytes swapents oom_score_adj name
...
[Tue Apr 14 11:14:15 2020] [27303]  2574 27303  2550868  2546981 20484096        0             0 rg
...
[Tue Apr 14 11:14:15 2020] Out of memory: Kill process 27303 (rg) score 528 or sacrifice child
[Tue Apr 14 11:14:15 2020] Killed process 27303 (rg) total-vm:10203472kB, anon-rss:10187924kB, file-rss:0kB, shmem-rss:0kB
[Tue Apr 14 11:14:15 2020] oom_reaper: reaped process 27303 (rg), now anon-rss:0kB, file-rss:0kB, shmem-rss:0kB
```

#### What is the expected behavior?

It should never happen (crashing my whole system because OOM), even if the user opens a immensely huge directory (or let's say an infinite big virtual FUSE directory).


---

_Comment by @BurntSushi on 2020-04-17 11:14_

This is, coincidentally, probably a duplicate of https://github.com/BurntSushi/ripgrep/issues/1550

There are cases where ripgrep may use a lot of memory. They are documented in the man page. Please consult those to see if they match your use case.

But unfortunately, this ticket is not actionable. Without a reproduction, there is really nothing I can do, sorry. If you can come up with a reproduction, then I'm happy to re-open.

---

_Closed by @BurntSushi on 2020-04-17 11:14_

---

_Label `duplicate` added by @BurntSushi on 2020-04-17 11:14_

---

_Comment by @BurntSushi on 2020-04-17 11:16_

(You're also using an old version of ripgrep, which is generally not supported. Although I don't expect upgrading to fix your problem.)

---

_Comment by @albertz on 2020-04-17 11:42_

Just wondering: The case if there are circular dependencies in symlinks is handled correctly though, right? It would not loop forever and always get deeper and deeper.

---

_Comment by @BurntSushi on 2020-04-17 11:52_

Yes. ripgrep has symlink loop detection.

If you want to debug this yourself, you could try removing `--follow`. You could also try using `--threads 1`.

---

_Comment by @Shnatsel on 2020-04-18 17:02_

Latest master might work for you now that #1554 is merged

---

_Comment by @albertz on 2020-04-18 17:05_

Thanks!

---
