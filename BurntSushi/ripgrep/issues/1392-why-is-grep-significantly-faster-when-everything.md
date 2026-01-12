```yaml
number: 1392
title: Why is grep significantly faster when everything is already in (disk) cache? in this case
type: issue
state: closed
author: ghost
labels: []
assignees: []
created_at: 2019-09-29T16:50:46Z
updated_at: 2019-09-29T19:48:35Z
url: https://github.com/BurntSushi/ripgrep/issues/1392
synced_at: 2026-01-12T16:13:23Z
```

# Why is grep significantly faster when everything is already in (disk) cache? in this case

---

_@ghost_

#### What version of ripgrep are you using?

```
$ rg --version
ripgrep 11.0.2
-SIMD -AVX (compiled)
-SIMD -AVX (runtime)

$ grep -vV
grep (GNU grep) 3.3

```

#### How did you install ripgrep?

`sudo pacman -S ripgrep`

#### What operating system are you using ripgrep on?

Arch Linux 64 bit (it's a rolling release, so it's up to date, more or less)

#### Describe your question, feature request, or bug.

I'm running `rg` and `grep` as follows, inside the chromium checked-out source dir and while at first `rg` is way faster than `grep`, the subsequent runs for each, due to disk caching/buffering (16G RAM), both are faster than before but `rg` is now slower than `grep`:

First time for `rg`, ran this before `grep`:
```
$ time rg CHROMIUM_ZLIB_NO_CHROMECONF
third_party/zlib/zconf.h
17:#if !defined(CHROMIUM_ZLIB_NO_CHROMECONF)

third_party/zlib/patches/0000-build.patch
158:+#if !defined(CHROMIUM_ZLIB_NO_CHROMECONF)

real	0m46.012s
user	1m14.654s
sys	0m27.345s
```

First time for `grep`, ran this after the above `rg` (important due to disk caching):
```
$ time grep -nrIF CHROMIUM_ZLIB_NO_CHROMECONF
third_party/zlib/patches/0000-build.patch:158:+#if !defined(CHROMIUM_ZLIB_NO_CHROMECONF)
third_party/zlib/zconf.h:17:#if !defined(CHROMIUM_ZLIB_NO_CHROMECONF)

real	1m3.176s
user	0m5.378s
sys	0m20.152s
```
(this is possibly a bit longer by 4 seconds, than seen here)

Second and subsequent times for `rg`:
```
$ time rg CHROMIUM_ZLIB_NO_CHROMECONF
third_party/zlib/zconf.h
17:#if !defined(CHROMIUM_ZLIB_NO_CHROMECONF)

third_party/zlib/patches/0000-build.patch
158:+#if !defined(CHROMIUM_ZLIB_NO_CHROMECONF)

real	0m32.322s
user	1m50.825s
sys	0m7.670s
```
This is consistently 32-33 seconds!

Second and subsequent times for `grep`:
```
$ time grep -nrIF CHROMIUM_ZLIB_NO_CHROMECONF
third_party/zlib/patches/0000-build.patch:158:+#if !defined(CHROMIUM_ZLIB_NO_CHROMECONF)
third_party/zlib/zconf.h:17:#if !defined(CHROMIUM_ZLIB_NO_CHROMECONF)

real	0m10.007s
user	0m4.332s
sys	0m5.604s
```
This is consistently 10 seconds as long as everything remains (disk)cached.

Oh i just realized something:
```
$ type grep
grep is aliased to `grep --color=tty -d skip --exclude-dir=.git --exclude-dir=.hg --exclude-dir=.svn'
```

Not exactly sure how to use this with `rg`:
```
       --iglob GLOB ...
           Include or exclude files and directories for searching that match the given glob. This always
           overrides any other ignore logic. Multiple glob flags may be used. Globbing rules match
           .gitignore globs. Precede a glob with a ! to exclude it. Globs are matched case insensitively.
```
```
$ time rg --no-ignore --iglob '!.git','!.hg','!.svn' -- CHROMIUM_ZLIB_NO_CHROMECONF
```
these don't work:
```
$ time rg --no-ignore --iglob '!.git' '!.hg' '!.svn' -- CHROMIUM_ZLIB_NO_CHROMECONF
!.svn: No such file or directory (os error 2)
CHROMIUM_ZLIB_NO_CHROMECONF: No such file or directory (os error 2)

$ time rg --no-ignore --iglob '.git' '.hg' '.svn' -- CHROMIUM_ZLIB_NO_CHROMECONF
.svn: No such file or directory (os error 2)
CHROMIUM_ZLIB_NO_CHROMECONF: No such file or directory (os error 2)

$ time rg --no-ignore --iglob '.git' -iglob '.hg' -iglob '.svn' -- CHROMIUM_ZLIB_NO_CHROMECONF
error: The argument '--ignore-case' was provided more than once, but cannot be used multiple times

USAGE:
    
    rg [OPTIONS] PATTERN [PATH ...]
    rg [OPTIONS] [-e PATTERN ...] [-f PATTERNFILE ...] [PATH ...]
    rg [OPTIONS] --files [PATH ...]
    rg [OPTIONS] --type-list
    command | rg [OPTIONS] PATTERN

For more information try --help


```

I tried this, no effect?:
```
$ time rg --glob '!.git/*' -- CHROMIUM_ZLIB_NO_CHROMECONF
third_party/zlib/zconf.h
17:#if !defined(CHROMIUM_ZLIB_NO_CHROMECONF)

third_party/zlib/patches/0000-build.patch
158:+#if !defined(CHROMIUM_ZLIB_NO_CHROMECONF)

real	0m33.919s
user	1m56.727s
sys	0m7.472s

$ time rg --glob '!.git*' -- CHROMIUM_ZLIB_NO_CHROMECONF
third_party/zlib/zconf.h
17:#if !defined(CHROMIUM_ZLIB_NO_CHROMECONF)

third_party/zlib/patches/0000-build.patch
158:+#if !defined(CHROMIUM_ZLIB_NO_CHROMECONF)

real	0m34.017s
user	1m52.186s
sys	0m7.522s
```

I'm not sure if `.git` is ignored, or maybe it is already ignored by default? (it's not in `/.gitignore` already though)

So anyway, why would `rg` take 22-24 seconds longer than `grep` for the same stuff?

```
$ sudo compsize .
Processed 527753 files, 238469 regular extents (242009 refs), 372983 inline.
Type       Perc     Disk Usage   Uncompressed Referenced  
TOTAL       84%       26G          31G          31G       
none       100%       23G          23G          23G       
zstd        37%      2.7G         7.5G         7.9G       
```


---

_Comment by @BurntSushi on 2019-09-29 17:11_

Chromium is a big repository. It would not be surprising to me if it wasn't actually entirely in disk cache after running one search.

I don't understand why you are trying to use iglobs. Firstly, it is case insensitive globbing, which isn't necessary. Secondly, you need to specify `--iglob` for each glob, just like `--exclude`, which is pretty standard. Thirdly, ripgrep ignores all hidden directories by default.

Anyway, I am not at a computer right now. I will try your examples when I get to one. In the mean time, could you please say which revision of chromium you have checked out?

---

_Comment by @BurntSushi on 2019-09-29 17:16_

You might also consider compiling master and rerunning your benchmark.

---

_Comment by @ghost on 2019-09-29 18:19_

> could you please say which revision of chromium you have checked out?

commit f0627799ff032631e41e805fc967a48575d84312 (HEAD, tag: 77.0.3865.106)
Date:   Thu Sep 26 17:14:05 2019 -0700

Here's my reflog too(it shows that I've a relatively new `master` before having switched to `tag: 77.0.3865.106`):
```
f0627799ff03 (HEAD, tag: 77.0.3865.106) HEAD@{0}: checkout: moving from 629502a860f49e305187e05b45e92683f04320e0 to 77.0.3865.106
629502a860f4 (tag: 77.0.3865.98) HEAD@{1}: checkout: moving from f44afce39e922d6a43ec25fd7b1eba7c5c51ae8a to 77.0.3865.98
f44afce39e92 (tag: 77.0.3865.90) HEAD@{2}: checkout: moving from 58c425ba843df2918d9d4b409331972646c393dd to 77.0.3865.90
58c425ba843d HEAD@{3}: checkout: moving from master to 77.0.3865.90~1
2cbb9cb86577 (master) HEAD@{4}: clone: from https://chromium.googlesource.com/chromium/src.git
```

> Secondly, you need to specify --iglob for each glob, just like --exclude, which is pretty standard.

I see I typoed `--iglob` after the first occurence:
`$ time rg --no-ignore --iglob '.git' -iglob '.hg' -iglob '.svn'`

which is why I avoided trying to use it multiple times, I thought some bug was disallowing me to use it.

> I don't understand why you are trying to use iglobs. 

I was looking though the help/man page and I thought iglob means ignore glob :) later I found it's like --glob but with ignore case.

First time trying ripgrep, I believe.



---

_Comment by @ghost on 2019-09-29 18:37_

I forgot to mention, the checked out chromium source is a bit modified with some ungoogled-chromium-archlinux stuff. (I don't think this should matter, you'd just get one result instead of two - I think, or maybe not in this case, that patch exists anyway - , repo size is basically the same)

With master `rg` and without specifying any features(notably, not specifying [`--features 'pcre2'`](https://git.archlinux.org/svntogit/community.git/tree/trunk/PKGBUILD?h=packages/ripgrep&id=a36df81d0007d0e5d002227d6d08690d10072407#n18)) it's very fast, 3 seconds:
```
$ time /home/user/build/2nonpkgs/rust.stuff/ripgrep/target/release/rg --glob '!.git/*' -- CHROMIUM_ZLIB_NO_CHROMECONF
third_party/zlib/zconf.h
17:#if !defined(CHROMIUM_ZLIB_NO_CHROMECONF)

third_party/zlib/patches/0000-build.patch
158:+#if !defined(CHROMIUM_ZLIB_NO_CHROMECONF)

real	0m3.446s
user	0m7.816s
sys	0m4.372s
```


I just added `pcre2` feature too and see no difference, what teh ? :)
```
$ cargo build --release --features 'pcre2'
...
$ time /home/user/build/2nonpkgs/rust.stuff/ripgrep/target/release/rg --glob '!.git/*' -- CHROMIUM_ZLIB_NO_CHROMECONF
third_party/zlib/zconf.h
17:#if !defined(CHROMIUM_ZLIB_NO_CHROMECONF)

third_party/zlib/patches/0000-build.patch
158:+#if !defined(CHROMIUM_ZLIB_NO_CHROMECONF)

real	0m3.429s
user	0m7.626s
sys	0m4.322s
```

(I even `cargo clean` -ed afterwards and retried the above)
The only thing that could've "fixed" it is: `-C target-cpu=native`, the fact that I'm using `master` `rg`, and, compiled with my rust version `rustc 1.40.0-dev (f3c8eba64 2019-09-28)`
Actually, I'm determined to find out what teh... :)  'cause curious.

---

_Comment by @BurntSushi on 2019-09-29 18:48_

> I forgot to mention, the checked out chromium source is a bit modified with some ungoogled-chromium-archlinux stuff. (I don't think this should matter, you'd just get one result instead of two - I think, or maybe not in this case, that patch exists anyway - , repo size is basically the same)

It should be easy to try and reproduce this issue on a standard checkout. Could you please do that so that it makes reproduction easier?

> With master rg and without specifying any features(notably, not specifying --features 'pcre2') it's very fast, 3 seconds:

If master does indeed fix this issue, then you might be impacted by #1335, which was fixed in https://github.com/BurntSushi/ripgrep/commit/813c676ecac053559a3b1b72034e0c34c8ed7e06

---

_Comment by @ghost on 2019-09-29 18:56_

> If master does indeed fix this issue, then you might be impacted by #1335, which was fixed in 813c676

Wow! Reverting that brings back the slowness:
```
$ time /home/user/build/2nonpkgs/rust.stuff/ripgrep/target/release/rg --glob '!.git/*' -- CHROMIUM_ZLIB_NO_CHROMECONF
third_party/zlib/zconf.h
17:#if !defined(CHROMIUM_ZLIB_NO_CHROMECONF)

third_party/zlib/patches/0000-build.patch
158:+#if !defined(CHROMIUM_ZLIB_NO_CHROMECONF)

real	0m27.794s
user	1m24.397s
sys	0m6.577s
```

<details>

So on master `rg` (not reverted):

```
$ echo 3 | sudo tee /proc/sys/vm/drop_caches
3
$ time /home/user/build/2nonpkgs/rust.stuff/ripgrep/target/release/rg --glob '!.git/*' -- CHROMIUM_ZLIB_NO_CHROMECONF
third_party/zlib/zconf.h
17:#if !defined(CHROMIUM_ZLIB_NO_CHROMECONF)

third_party/zlib/patches/0000-build.patch
158:+#if !defined(CHROMIUM_ZLIB_NO_CHROMECONF)

real	0m26.790s
user	0m9.532s
sys	0m21.181s

$ echo 3 | sudo tee /proc/sys/vm/drop_caches
3
$ time grep -nrIF CHROMIUM_ZLIB_NO_CHROMECONF
third_party/zlib/patches/0000-build.patch:158:+#if !defined(CHROMIUM_ZLIB_NO_CHROMECONF)
third_party/zlib/zconf.h:17:#if !defined(CHROMIUM_ZLIB_NO_CHROMECONF)

real	2m3.342s
user	0m6.663s
sys	0m37.492s
```

So `rg` is like 4.5 times faster than `grep` when not cached?

and cached:
```
$ time grep -nrIF CHROMIUM_ZLIB_NO_CHROMECONF
third_party/zlib/patches/0000-build.patch:158:+#if !defined(CHROMIUM_ZLIB_NO_CHROMECONF)
third_party/zlib/zconf.h:17:#if !defined(CHROMIUM_ZLIB_NO_CHROMECONF)

real	0m9.470s
user	0m3.770s
sys	0m5.427s

$ time /home/user/build/2nonpkgs/rust.stuff/ripgrep/target/release/rg --glob '!.git/*' -- CHROMIUM_ZLIB_NO_CHROMECONF
third_party/zlib/zconf.h
17:#if !defined(CHROMIUM_ZLIB_NO_CHROMECONF)

third_party/zlib/patches/0000-build.patch
158:+#if !defined(CHROMIUM_ZLIB_NO_CHROMECONF)

real	0m3.297s
user	0m7.250s
sys	0m4.197s
```

2.8 times faster.

</details>


---

_Closed by @ghost on 2019-09-29 18:56_

---
