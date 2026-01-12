```yaml
number: 134
title: processing a large ignore file is very slow
type: issue
state: closed
author: Deewiant
labels:
  - bug
assignees: []
created_at: 2016-09-29T17:55:17Z
updated_at: 2016-10-05T00:34:19Z
url: https://github.com/BurntSushi/ripgrep/issues/134
synced_at: 2026-01-12T18:23:11Z
```

# processing a large ignore file is very slow

---

_@Deewiant_

Testcase: pristine git clone of [mysql/mysql-server](https://github.com/mysql/mysql-server) at the current HEAD commit, 71f48ab393bce80a59e5a2e498cd1f46f6b43f9a. Quick comparison of `git grep` and `rg` on a fixed-string pattern that is not found anywhere in the repository, after warming up the filesystem cache with `git grep` a few times:

```
$ time git grep thiswillnotbefound
0.29suser 0.33skernel 0.135elapsed 459%CPU 26Mmaxmem
0inputs+0outputs (0major+8952minor)pagefaults (39406voluntary+52in)switches

$ time rg thiswillnotbefound
3845.59suser 0.78skernel 8:01.86elapsed 798%CPU 130Mmaxmem
0inputs+0outputs (0major+5543minor)pagefaults (51voluntary+133031in)switches
```

Somehow ripgrep is more than three orders of magnitude slower than `git grep` here, at 8 minutes versus 0.1 seconds. This is of course not a very good sample of results but I think with a difference that big you don't really need the details. (Feel free to ask if you can't reproduce it though.)

This is with ripgrep 0.2.1, git 2.10.0.

One-shot results from The Silver Searcher 0.33.0 and sift 0.8.0 for further comparison:

```
$ time ag thiswillnotbefound
1.64suser 0.75skernel 1.324elapsed 180%CPU 4Mmaxmem
0inputs+0outputs (0major+29085minor)pagefaults (66106voluntary+108in)switches

$ time sift --git thiswillnotbefound
Error: 4 files skipped due to very long lines (>= 262144 bytes). See options --blocksize, --err-show-line-length and --err-skip-line-length.
24.31suser 0.54skernel 7.715elapsed 322%CPU 26Mmaxmem
0inputs+0outputs (0major+4039minor)pagefaults (77269voluntary+348in)switches
```


---

_Comment by @oconnor663 on 2016-09-29 20:53_

I can repro this. It looks like the problem is something to do with `.gitignore` handling. If we disable that, `rg` is actually faster:

```
$ cd mysql-server

$ time git grep thiswillnotbefound
git grep thiswillnotbefound  0.12s user 0.15s system 257% cpu 0.107 total

$ time rg thiswillnotbefound --no-ignore
rg thiswillnotbefound --no-ignore  0.16s user 0.10s system 292% cpu 0.090 total

$ time rg thiswillnotbefound
# ... I'm still waiting for this to finish.
```


---

_Comment by @BurntSushi on 2016-09-29 20:58_

NICE FIND.

This is definitely the `.gitignore` handling. The `.gitignore` in the root repo has over 3,000 patterns in it. From looking at the profile, it's causing the regex engine to divert to a slower execution model (an NFA instead of a DFA), which is what would explain the slowness. We can fix this in a few ways (by either splitting the giant regex up or by increasing the memory limit so that the DFA can execute).


---

_Label `bug` added by @BurntSushi on 2016-09-29 20:58_

---

_Comment by @BurntSushi on 2016-09-29 20:59_

Many of the patterns also look like simple literals. We should also investigate to make sure `ripgrep`'s globber isn't using a regex for that case.


---

_Renamed from "Orders of magnitude slower than git grep in mysql/mysql-server" to "processing a large ignore file is very slow" by @BurntSushi on 2016-09-29 22:50_

---

_Comment by @BurntSushi on 2016-10-01 13:26_

See #141 for some details on this, where @lilydjwg ran into the same problem searching `percona-server`.


---

_Closed by @BurntSushi on 2016-10-05 00:29_

---

_Comment by @BurntSushi on 2016-10-05 00:32_

OK, this one took me a while because I paid back some technical debt.

While this issue can technically still arise, I've reduced the chance of it occurring substantially. There is still another path we can take to fix it for good, but I'll wait until it actually happens so I have a real test case to look at. In particular, I can now search the MySQL repo in reasonable time (three times faster than the silver searcher).

As it happens, this repo is a good example of where `git grep` can steal our lunch (on simple literals only) and there's not much we can do about it. Well... maybe a parallel directory iterator will help. :-)


---
