```yaml
number: 2199
title: Please allow a list of files and directories to search via file or pipe
type: issue
state: closed
author: setharnold
labels:
  - duplicate
assignees: []
created_at: 2022-05-02T19:55:13Z
updated_at: 2022-05-03T02:00:57Z
url: https://github.com/BurntSushi/ripgrep/issues/2199
synced_at: 2026-01-12T16:13:24Z
```

# Please allow a list of files and directories to search via file or pipe

---

_@setharnold_

Hello, I often use ripgrep to search a few hundred million files that rarely change. Getting the list of files to search is a very expensive operation in my environment and the `plocate` tool lets me amortize that cost to overnight index generation.

The problem is that I want to run a command like the following:

`rg foo $(locate srv pool ubuntu dsc)`

This builds a command line that is far too long for my operating system or execution environment:

```
$ rg 'debian.*changelog' $(locate  .dsc srv mirror ubuntu pool)
-bash: /home/sarnold/bin/rg: Argument list too long
```

This is a small number of files and the `locate` command executes very quickly:

```
$ time locate .dsc srv mirror ubuntu pool | wc -l
142375

real	0m0.578s
user	0m0.322s
sys	0m0.278s
```

Running a very similar ripgrep command, it's been executing for ten minutes now and has only reported a hundred or so candidate files to search:

```
 $ ps auxw | head -1 ; ps auwx | grep "rg -uu"
USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
sarnold   1695  0.0  0.0  14436  1092 pts/1    S+   12:26   0:00 grep --color=auto rg -uu
sarnold   4185  125  1.6 2467468 2151536 pts/0 Sl+  12:18   9:55 rg -uu -g **/*.dsc --files /fst/trees/ubuntu/ /srv/mirror/ubuntu/
```

So, I'd like some way to avoid this costly part of my search directly in ripgrep. I want to give a path to a file or a socket that will provide the filenames or directory names to search. (Accepting an fd with the filenames might also work, and could save mandatory trips to the filesystem, but working with non-stdin filedescriptors at the shell is a bit annoying. I'm only familiar with `gpg` using this for password inputs.) (Accepting a command to run to generate the list of filenames might also work, but it's an odd inversion of typical Unix control flow.)

My current work-around is to use xargs, which feels non-optimal:
- I can't start searching until the entire list is generated, which leaves some of my disks idle longer than necessary
- Repeated execution of rg with very large argvs is going to be less efficient than one execution of rg with a very short argv
- At the end of each xargs-spawned rg run, there's going to be some moments when the process will be searching exactly one file
- Manual management of the list of files or directories to search is ever so slightly tedious

I don't have good suggestions for a command line parameter name. I've thought of the following:
- `--paths-to-search <filename>`
- `--files-to-search <filename>`
- `--files-and-directories-to-search <filename>`
- `--files-to-search-from-file <filename>`
- `--files-and-directories-to-search-from-file <filename>`
- `--files--to-search-from-fd <fd>`
- `--files-to-search-from-command <command>`
- ...

Thanks

---

_Comment by @BurntSushi on 2022-05-02 21:58_

Duplicate of #273.

Note also that this comment seems relevant to your use case: https://github.com/BurntSushi/ripgrep/issues/273#issuecomment-724140485

Namely, I'm not sure how difficult it is to refactor ripgrep to start searching before it knows the full set of paths. It could be very gnarly.

---

_Closed by @BurntSushi on 2022-05-02 21:58_

---

_Label `duplicate` added by @BurntSushi on 2022-05-02 21:58_

---

_Comment by @setharnold on 2022-05-03 02:00_

Aha, sorry I didn't spot #273. Bummer it's not straightforward. Thanks! :D

---
