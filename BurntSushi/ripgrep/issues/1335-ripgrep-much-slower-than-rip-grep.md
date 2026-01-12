```yaml
number: 1335
title: ripgrep much slower than (rip)grep
type: issue
state: closed
author: zsugabubus
labels:
  - question
assignees: []
created_at: 2019-07-31T01:28:31Z
updated_at: 2020-09-13T12:52:16Z
url: https://github.com/BurntSushi/ripgrep/issues/1335
synced_at: 2026-01-12T16:13:23Z
```

# ripgrep much slower than (rip)grep

---

_@zsugabubus_

#### What version of ripgrep are you using?

ripgrep 11.0.1
-SIMD -AVX (compiled)
+SIMD -AVX (runtime)

#### How did you install ripgrep?

pacman -S ripgrep

#### What operating system are you using ripgrep on?

Arch Linux

#### Describe your question, feature request, or bug.

ripgrep is 3-4-5 times slower than GNU grep on simple string pattern.

#### If this is a bug, what is the actual behavior?

```
grep file\.php -r .  0.19s user 0.10s system 98% cpu 0.298 total
rg (--no-ignore) file\.php  3.68s user 0.45s system 365% cpu 1.129 total
rg --debug --hidden --no-ignore file\.php  6.12s user 0.42s system 383% cpu 1.706 total
DEBUG|grep_regex::literal|grep-regex/src/literal.rs:115: required literal found: "file"
DEBUG|globset|globset/src/lib.rs:435: built glob set; 0 literals, 0 basenames, 11 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|globset/src/lib.rs:435: built glob set; 0 literals, 0 basenames, 11 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|globset/src/lib.rs:435: built glob set; 0 literals, 0 basenames, 11 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|globset/src/lib.rs:435: built glob set; 0 literals, 0 basenames, 11 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|globset/src/lib.rs:435: built glob set; 0 literals, 0 basenames, 11 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
```

As I tried to find _one-by-one_ what top-level file (or directory) causes the slowdown, I came to the conclusion: none. Ripgrep should be faster... So I did this table:
(Sorry, for hiding the names, I'm not sure if I'm allowed to show them.)
```
#!/bin/sh
$ echo GREPWHAT=$GREPWHAT RGFLAGS=$RGFLAGS GREPFLAGS=$GREPFLAGS; find -maxdepth 1 | xargs -I{} sh -c 'echo $(echo {} | tr a-z x):$(du -sh {} | cut -f1):$(find {} | wc -l):$( ( time rg "$GREPWHAT" $RGFLAGS {} &>/dev/null ) 2>&1 | tr \\t\\n \  ):$( ( time find -mindepth 1 -maxdepth 1 -not -path {} | xargs rg "$GREPWHAT" $RGFLAGS  &>/dev/null ) 2>&1 | tr \\t\\n \  ):$( ( time grep "$GREPWHAT" $GREPFLAGS -r {} &>/dev/null ) 2>&1 | tr \\t\\n \ ):$( ( time find -mindepth 1 -maxdepth 1 -not -path {} | xargs grep "$GREPWHAT" $GREPFLAGS -r &>/dev/null ) 2>&1 | tr \\t\\n \  )' | column -ts: -Rsize,files,rgtime,rgwothistime,greptime,grepwothistime -Nname,size,files,rgtime,rgwothistime,greptime,grepwothistime

GREPWHAT=xxxxxxxxxxxxxxxxxxx.xxx RGFLAGS=-j1 GREPFLAGS=
name                  size  files                                      rgtime                                rgwothistime                                    greptime                             grepwothistime
.                     127M  12961   real 0m0.964s user 0m3.295s sys 0m0.348s    real 0m0.971s user 0m3.285s sys 0m0.388s    real 0m0.146s user 0m0.039s sys 0m0.100s    real 0m0.148s user 0m0.069s sys 0m0.078s
./xx                  4.0K      1   real 0m0.009s user 0m0.006s sys 0m0.003s    real 0m0.919s user 0m3.097s sys 0m0.406s    real 0m0.003s user 0m0.000s sys 0m0.003s    real 0m0.146s user 0m0.055s sys 0m0.089s
./_xxx_xxxxxx.xxx     428K      1   real 0m0.011s user 0m0.008s sys 0m0.004s    real 0m0.936s user 0m3.123s sys 0m0.388s    real 0m0.004s user 0m0.004s sys 0m0.000s    real 0m0.144s user 0m0.043s sys 0m0.100s
./xxxxxx               89M  11741   real 0m0.217s user 0m0.576s sys 0m0.211s    real 0m0.221s user 0m0.688s sys 0m0.109s    real 0m0.106s user 0m0.036s sys 0m0.064s    real 0m0.051s user 0m0.023s sys 0m0.029s
./xxxxx                16K      7   real 0m0.010s user 0m0.009s sys 0m0.004s    real 0m0.942s user 0m3.101s sys 0m0.418s    real 0m0.003s user 0m0.003s sys 0m0.000s    real 0m0.147s user 0m0.049s sys 0m0.098s
./xxxxxxx              52K     28   real 0m0.014s user 0m0.015s sys 0m0.004s    real 0m0.946s user 0m3.148s sys 0m0.418s    real 0m0.003s user 0m0.003s sys 0m0.000s    real 0m0.152s user 0m0.048s sys 0m0.099s
./xxxxxx.xxx          4.0K      1   real 0m0.008s user 0m0.006s sys 0m0.003s    real 0m0.929s user 0m3.113s sys 0m0.400s    real 0m0.003s user 0m0.000s sys 0m0.003s    real 0m0.143s user 0m0.051s sys 0m0.091s
./xxxxxx               20K      5   real 0m0.012s user 0m0.015s sys 0m0.000s    real 0m0.926s user 0m3.114s sys 0m0.421s    real 0m0.003s user 0m0.000s sys 0m0.003s    real 0m0.144s user 0m0.038s sys 0m0.105s
./xxxxxxxxx           248K     60   real 0m0.013s user 0m0.015s sys 0m0.005s    real 0m0.939s user 0m3.139s sys 0m0.384s    real 0m0.004s user 0m0.000s sys 0m0.004s    real 0m0.151s user 0m0.056s sys 0m0.095s
./xxxxxx.xx           4.0K      1   real 0m0.006s user 0m0.000s sys 0m0.006s    real 0m0.937s user 0m3.176s sys 0m0.413s    real 0m0.004s user 0m0.004s sys 0m0.000s    real 0m0.145s user 0m0.055s sys 0m0.089s
./xxxxxx               35M    522   real 0m0.068s user 0m0.138s sys 0m0.077s    real 0m0.227s user 0m0.639s sys 0m0.211s    real 0m0.036s user 0m0.025s sys 0m0.011s    real 0m0.114s user 0m0.031s sys 0m0.082s
./xxxx.xxx            4.0K      1   real 0m0.006s user 0m0.006s sys 0m0.001s    real 0m0.933s user 0m3.123s sys 0m0.408s    real 0m0.003s user 0m0.000s sys 0m0.003s    real 0m0.150s user 0m0.045s sys 0m0.104s
./xxxxxxx.xxx         4.0K      1   real 0m0.007s user 0m0.001s sys 0m0.006s    real 0m0.941s user 0m3.139s sys 0m0.409s    real 0m0.003s user 0m0.003s sys 0m0.000s    real 0m0.145s user 0m0.072s sys 0m0.072s
./xxxxx.xxx           4.0K      1   real 0m0.008s user 0m0.004s sys 0m0.004s    real 0m0.957s user 0m3.127s sys 0m0.419s    real 0m0.004s user 0m0.004s sys 0m0.000s    real 0m0.154s user 0m0.048s sys 0m0.101s
./xxxxxxxx            276K     52   real 0m0.013s user 0m0.016s sys 0m0.000s    real 0m1.037s user 0m2.956s sys 0m0.385s    real 0m0.004s user 0m0.004s sys 0m0.000s    real 0m0.147s user 0m0.036s sys 0m0.109s
./xxxxxx              128K     34   real 0m0.012s user 0m0.010s sys 0m0.005s    real 0m0.934s user 0m3.142s sys 0m0.398s    real 0m0.003s user 0m0.000s sys 0m0.003s    real 0m0.146s user 0m0.062s sys 0m0.083s
./xxxxxxxx.xxxx       212K      1   real 0m0.009s user 0m0.005s sys 0m0.004s    real 0m0.924s user 0m3.123s sys 0m0.362s    real 0m0.004s user 0m0.000s sys 0m0.003s    real 0m0.147s user 0m0.063s sys 0m0.083s
./xxxxxxxx.xxxx       4.0K      1   real 0m0.008s user 0m0.003s sys 0m0.005s    real 0m0.928s user 0m3.067s sys 0m0.422s    real 0m0.004s user 0m0.004s sys 0m0.000s    real 0m0.151s user 0m0.057s sys 0m0.088s
./xxxxxx              1.2M    294   real 0m0.018s user 0m0.027s sys 0m0.008s    real 0m0.852s user 0m2.761s sys 0m0.476s    real 0m0.007s user 0m0.000s sys 0m0.007s    real 0m0.141s user 0m0.033s sys 0m0.108s
./xxxxxxxxx            12K      5   real 0m0.012s user 0m0.016s sys 0m0.000s    real 0m0.987s user 0m2.797s sys 0m0.365s    real 0m0.003s user 0m0.000s sys 0m0.003s    real 0m0.148s user 0m0.038s sys 0m0.109s
./xxxxxxx             4.0K      1   real 0m0.005s user 0m0.000s sys 0m0.005s    real 0m1.074s user 0m2.989s sys 0m0.386s    real 0m0.005s user 0m0.004s sys 0m0.000s    real 0m0.143s user 0m0.057s sys 0m0.085s
./xxx                 764K    198   real 0m0.014s user 0m0.011s sys 0m0.012s    real 0m0.970s user 0m2.792s sys 0m0.481s    real 0m0.005s user 0m0.005s sys 0m0.000s    real 0m0.201s user 0m0.065s sys 0m0.133s
./.xxxxxxxx.xxxx.xxx   44K      1   real 0m0.007s user 0m0.003s sys 0m0.003s    real 0m1.393s user 0m2.965s sys 0m0.569s    real 0m0.004s user 0m0.000s sys 0m0.003s    real 0m0.205s user 0m0.079s sys 0m0.119s
./.xxxxxxxxx          4.0K      1   real 0m0.007s user 0m0.001s sys 0m0.006s    real 0m1.140s user 0m2.975s sys 0m0.556s    real 0m0.004s user 0m0.000s sys 0m0.004s    real 0m0.156s user 0m0.051s sys 0m0.099s
./.xxxxxxxxxxxxx      4.0K      1   real 0m0.006s user 0m0.003s sys 0m0.003s    real 0m0.954s user 0m3.055s sys 0m0.484s    real 0m0.003s user 0m0.003s sys 0m0.000s    real 0m0.149s user 0m0.070s sys 0m0.078s
./.xxx.xxxxxxx        4.0K      1   real 0m0.007s user 0m0.004s sys 0m0.003s    real 0m0.956s user 0m3.130s sys 0m0.402s    real 0m0.003s user 0m0.003s sys 0m0.000s    real 0m0.149s user 0m0.068s sys 0m0.076s
$ # Note that the sum of ripgrep times doesn't match with . time.
$ # wothis means: grepping in the directory with that file excluded.
$ time tokei
-------------------------------------------------------------------------------
 Language            Files        Lines         Code     Comments       Blanks
-------------------------------------------------------------------------------
...
-------------------------------------------------------------------------------
 Total                9256      1221713       827992       262436       131285
-------------------------------------------------------------------------------

real    0m1.061s
user    0m3.384s
sys     0m0.295s
$ # Note that tokei has similar running time.
$ stat -f . | head -3
  File: "."
    ID: 0        Namelen: 255     Type: tmpfs
Block size: 4096       Fundamental block size: 4096
$ # Fast as hell tmpfs. Does it count?
```

Tomorrow I will make a line length statistics if it counts, but I think it should be an issue around directory listing. Anyway, I would like to help, so tell me what to do/share. `--debug` didn't help too much. 

---

_Comment by @BurntSushi on 2019-07-31 01:39_

Unfortunately, there is really nothing I can do unless I can reproduce it. I'd suggest trying to reproduce the performance difference on a corpus that we both have access to.

I generally find it difficult to read the data you've given to me. I'm not really sure what I'm looking at or the nature of the query or the corpus. For example, how many matches are printed? Do ripgrep and grep print the same output? Do your timings really suggest that multi-threaded ripgrep is slower than single threaded grep? That's pretty crazy to me if true, and suggests something wonky is going on. Is there anything special about your system's configuration?

---

_Label `question` added by @BurntSushi on 2019-07-31 01:39_

---

_Comment by @BurntSushi on 2019-07-31 01:40_

I also find it interesting that you don't have AVX enabled in your CPU. To me, that suggests your CPU is quite old. What kind of CPU is it and how many cores does it have?

---

_Comment by @zsugabubus on 2019-07-31 07:40_

> I generally find it difficult to read the data you've given to me.

Sorry, I tried to make it readable. So that's a directory listing. To find out what causes the slowdown:
* Printed some statistics about individual files (`size`, `files`),
* I ran rg/grep on every entry (`{rg,grep}time`),
* I ran rg/grep on every file in the directory, but excluding the current entry (`{rg,grep}wothistime`)
    For example, if you have `a, b, c` files in your directory, you will see the result of `time rg a c` at row `./b` and column `rgwothistime`.

1) This two (`rg b` + `rg a c`) should be "equal" with the running time of rg/grep on the whole directory (`rg .` -> lists the directories internally, instead of getting filenames from arguments).
2) Sum of  rg/grep times on individual files (`{rg,grep}time`) also should be "equal"/comparable with rg/grep times on the whole directory. `rg a` + `rg b` + `rg c` =?= `rg .`

I hope it helps a bit.

> For example, how many matches are printed? Do ripgrep and grep print the same output?

Yes, sorry, both prints two matches.

> Do your timings really suggest that multi-threaded ripgrep is slower than single threaded grep?

Yes, for example with `-j2` it wasn't ~1 second but 6.

> Is there anything special about your system's configuration?

I wouldn't say. I'm just working on tmpfs, but it just should make things faster.

> What kind of CPU is it and how many cores does it have?

[Intel(R) Core(TM) i5-2410M CPU @ 2.30GHz](https://ark.intel.com/content/www/us/en/ark/products/52224/intel-core-i5-2410m-processor-3m-cache-up-to-2-90-ghz.html)

> I'd suggest trying to reproduce the performance difference on a corpus that we both have access to.

Tried on linux codebase now, but no luck. 

> Unfortunately, there is really nothing I can do unless I can reproduce it.

Yeah, I know. Later in the day I can run profiler tools on rg.

---

_Comment by @zsugabubus on 2019-07-31 12:18_

I tried obfuscating the files but timings became distorted in a great extent. Instead, I quickly made a little syscall statistics.

##### grep
```
$ time grep -o file\\.php -r . | wc -l
2
grep --color=auto -o file\\.php -r .  0.18s user 0.10s system 98% cpu 0.290 total
wc -l  0.00s user 0.00s system 0% cpu 0.288 total

$ time strace -fc grep -o file\\.php -r . | wc -l
2
% time     seconds  usecs/call     calls    errors syscall
------ ----------- ----------- --------- --------- ----------------
 27.47    0.132252           5     23744           read
 19.48    0.093756           6     14792         7 openat
 17.93    0.086329           4     17569           close
 13.95    0.067153           4     14787           fstat
  7.82    0.037661           4      8346           fcntl
  6.71    0.032310           5      5562           newfstatat
  6.47    0.031156           5      5564           getdents64
  0.11    0.000534          66         8           munmap
  0.02    0.000095           6        14           lseek
  0.02    0.000093           3        26           mmap
  0.00    0.000022          22         1           write
  0.00    0.000000           0         2           stat
  0.00    0.000000           0         6           mprotect
  0.00    0.000000           0         6           brk
  0.00    0.000000           0         3           rt_sigaction
  0.00    0.000000           0         1           rt_sigprocmask
  0.00    0.000000           0         1           access
  0.00    0.000000           0         1           execve
  0.00    0.000000           0         1           sigaltstack
  0.00    0.000000           0         1           fstatfs
  0.00    0.000000           0         2         1 arch_prctl
  0.00    0.000000           0         1           futex
  0.00    0.000000           0         1           set_tid_address
  0.00    0.000000           0         1           set_robust_list
  0.00    0.000000           0         1           prlimit64
------ ----------- ----------- --------- --------- ----------------
100.00    0.481361                 90441         8 total
strace -fc grep -o file\\.php -r .  0.78s user 2.47s system 119% cpu 2.722 total
wc -l  0.00s user 0.00s system 0% cpu 2.709 total

```

##### ripgrep
```
$ time rg -o file\\.php | wc -l
2
rg -o file\\.php  3.93s user 0.44s system 379% cpu 1.153 total
wc -l  0.00s user 0.00s system 0% cpu 1.153 total

$ time strace -cf rg -o file\\.php | wc -l
strace: Process 31403 attached
strace: Process 31404 attached
strace: Process 31405 attached
strace: Process 31406 attached
2
% time     seconds  usecs/call     calls    errors syscall
------ ----------- ----------- --------- --------- ----------------
 38.56    0.960239        1574       610       149 futex
 25.61    0.637726          17     35550           read
 19.73    0.491392          19     25185     11059 openat
  7.87    0.196092          13     14126           close
  3.61    0.089929          16      5548           getdents64
  1.81    0.045032          16      2785           fstat
  1.80    0.044863          15      2805      2787 stat
  0.42    0.010535          22       460           mprotect
  0.26    0.006442         429        15           munmap
  0.20    0.005027         418        12           mremap
  0.05    0.001256         125        10           nanosleep
  0.05    0.001123          23        48           mmap
  0.01    0.000222          20        11           brk
  0.01    0.000158          39         4           madvise
  0.00    0.000065          13         5           getrandom
  0.00    0.000045           3        15           sigaltstack
  0.00    0.000033          16         2           write
  0.00    0.000000           0         8           lseek
  0.00    0.000000           0         5           rt_sigaction
  0.00    0.000000           0         1           rt_sigprocmask
  0.00    0.000000           0        13        12 ioctl
  0.00    0.000000           0         1           access
  0.00    0.000000           0         4           clone
  0.00    0.000000           0         1           execve
  0.00    0.000000           0         1           fcntl
  0.00    0.000000           0         7           getcwd
  0.00    0.000000           0         2         1 arch_prctl
  0.00    0.000000           0        13           sched_getaffinity
  0.00    0.000000           0         1           set_tid_address
  0.00    0.000000           0         5           set_robust_list
  0.00    0.000000           0         2           prlimit64
------ ----------- ----------- --------- --------- ----------------
100.00    2.490179                 87255     14008 total
strace -cf rg -o file\\.php  3.65s user 2.05s system 249% cpu 2.286 total
wc -l  0.00s user 0.00s system 0% cpu 2.285 total


---

_Comment by @BurntSushi on 2019-07-31 13:39_

The syscall frequency table is quite interesting. ripgrep not only appears to be opening more files than grep, but it's also getting errors for a lot of them. A few more thoughts:

* Are you _sure_ there isn't anything weird about your system?
* Does your directory hierarchy contain a lot of `.ignore` or `.gitignore` files?
* How many total directories are in your tree?
* Can you try running ripgrep with `rg -uuu` and `rg -uuu -j1`? (And show the syscall frequency table for each, as well as timings.) Effectively, `rg -uuu -j1` should do roughly the same work that `grep -r` does, since the `-uuu` disables automatic filtering (ignore files, hidden files, binary files), while the `-j1` flag forces single threaded search, just like `grep`. So it's the closest "apples to apples" comparison you can get.
* Sanity check: do you have a ripgrep config file enabled that is setting additional flags?
* Can you run ripgrep with the `--debug` flag and look at the output for anything unusal? You won't be able to share this directly since it will contain file names. Running with `--trace` might also be good, but it will contain a lot more output.

The `time` output you've shared is also interesting, in that most of the additional time is spent in `user`, not `sys`, so the syscall frequency table may wind up being a red herring.

Is it possible for you to narrow down the corpus? For example, if you run ripgrep (and grep) on the each top-level directory in your tree, do they all have similar timings? Hopefully, one of those directories will take much longer to search with ripgrep than grep, which should then let you repeat the process recursively.

I think it would be nice if ripgrep grew a flag that allowed it to print the total search time for each individual file. You can _almost_ get it with the `--json` output, but it will only show elapsed search times for files that contain matches. You can get it show elapsed time for _every_ file it searches by setting [`always_begin_end` to `true`](https://github.com/BurntSushi/ripgrep/blob/bc37c32717301abf26e5aecc942688d2ddd63692/src/args.rs#L749). If you do make that change, then this should output a table how much time it takes to search each file. If a particular file (or small number of files) stand out as taking a long time to search, then that will perhaps narrow things down. But this does require making aforementioned change to ripgrep and re-compiling:

```
$ rg 'fn is_' src/ --json | rg '"type":"end"' | jq -j -r '.data.path.text, " ", .data.stats.elapsed.human, "\n"'
src/subject.rs 0.000024s
src/args.rs 0.000058s
```

---

_Comment by @zsugabubus on 2019-07-31 15:35_

> The syscall frequency table is quite interesting. ripgrep not only appears to be opening more files than grep, but it's also getting errors for a lot of them.

My naïve first thought about it was that (before I got the statistics) maybe ripgrep always chooses next paths to explore in a very unfortunate way. I know nothing about internal working of ripgrep, but an unbalanced directory structure can cause it, can it be possible?

> Are you sure there isn't anything weird about your system?

At first I ran it in a custom `systemd-nspawn(1)` container, where I first experienced the issue, but then I run several tests on my real machine too and got the _exact same results_. I can only think `tmpfs` that could be a little bit "weird".

> Does your directory hierarchy contain a lot of .ignore or .gitignore files?

```
$ find -name '.*ignore' -print0 | wc -l --files0-from=/dev/stdin | tail -1
328 total
```

These files contain mostly things that don't exist, so `--no-ignore` gives 60-80ms speedup, but sometimes not even that much. In my initial comment I put `--no-ignore` in parenthesis, because it meant only marginal difference.

> How many total directories are in your tree?

```
$ find -type d | wc -l
2473
```

> Sanity check: do you have a ripgrep config file enabled that is setting additional flags?

No.

> Can you try running ripgrep with rg -uuu and rg -uuu -j1?

```
$ time rg -uuu -o file\\.php | wc -l
2
rg -uuu -o file\\.php  7.00s user 0.50s system 350% cpu 2.140 total
wc -l  0.00s user 0.00s system 0% cpu 2.140 total
```
[`strace -CrTfo ../strace -s0 rg -uuu -o file\\.php | wc -l && sed -i '100,$s/".\+"/"string"/' ../strace`](https://gist.github.com/zsugabubus/562714382920713c9ebea08a6813cf1e/raw/d0a5524c0a0d12e60297c365e771d1b12338ffdf/rg.strace)

```
$ time rg -uuu -j1 -o file\\.php | wc -l
2
rg -uuu -j1 -o file\\.php  6.26s user 0.18s system 99% cpu 6.485 total
wc -l  0.00s user 0.00s system 0% cpu 6.484 total
```

[`strace -CrTfo ../strace -s0 rg -uuu -j1 -o file\\.php | wc -l && sed -i '100,$s/".\+"/"string"/' ../strace`](https://gist.github.com/zsugabubus/c20d1d23bdb801e427b5ee67eb8a4dac/raw/b8bbf0f5a94a72ef0252f228782770c79a5b7b61/rg-j1.strace)

> Can you run ripgrep with the --debug flag

Ran with `--debug --trace`:
```
g/searching using generic reader/d
g/will use fast line searcher/d
g/ignoring/d
g/searching via roll buffer strategy/d
g/glob converted to regex/d
g/built glob set/d

DEBUG|grep_regex::literal|grep-regex/src/literal.rs:59: literal prefixes detected: Literals { lits: [Complete(file.php)], limit_size: 250, limit_class: 10 }
TRACE|grep_regex::matcher|grep-regex/src/matcher.rs:56: final regex: "file\\.php"
```

> Is it possible for you to narrow down the corpus? For example, if you run ripgrep (and grep) on the each top-level directory in your tree, do they all have similar timings? Hopefully, one of those directories will take much longer to search with ripgrep than grep, which should then let you repeat the process recursively.

This is what I did and would like to illustrate with my table above. ripgrepping the top level directory takes 964ms, but total search time of individual children files and directories is not more than ~300ms. Take the 4th entry for example. It takes 217ms, but if I try to ripgrep its children one-by-one, then their total search time will be (more) less. So there is no one big file.

There are no binary files, just text files with very looong lines (7.5 millon characters).

> setting always_begin_end to true

Hmm. I got _very very weird_ [results](https://gist.github.com/zsugabubus/79f82a623d2d1804c96814875dfe7b87).

---

_Comment by @BurntSushi on 2019-07-31 16:19_

> This is what I did and would like to illustrate with my table above. ripgrepping the top level directory takes 964ms, but total search time of individual children files and directories is not more than ~300ms.

I mean, _compared_ to grep. Your second entry has `real 0m0.919s` for ripgrep, but `real 0m0.009s` for grep. But as I said before, the table is hard to understand because it's not totally clear how it was produced. (The command you've shown is quite complex.)

> There are no binary files, just text files with very looong lines (7.5 millon).

How long are the lines? And do you mean to say there are 7.5 million _files_? Quite frankly, I'm pretty surprised that grep is searching that entire directory in under a second (if I am to understand your timings correctly). For example, on my checkout of the entire Chromium repository, `grep -r Openbox` takes about 7.3 seconds to complete, and there are only ~230,000 files. In contrast, ripgrep takes 1.7 seconds when multi-threaded and 7.9 seconds single threaded (which is actually an interesting slowdown compared to grep here which I'll also investigate, but not nearly as much as yours).

So maybe the other idea I have is whether grep is actually searching all of your data or not. Could you compare `rg -uuua` with `grep -ra`? Another thing to try is `rg -uuu . ./ > output.rg` vs `grep -r . ./ > output.grep`, and then compare the output. Are they roughly similar size?

---

_Comment by @zsugabubus on 2019-07-31 16:37_

> 7.5 million files

I meant 7.5 million character long lines.

---

_Comment by @zsugabubus on 2019-07-31 16:52_

###### Break the command along ':'s and you will see how values were computed.

> Your second entry has real 0m0.919s for ripgrep, but real 0m0.009s for grep.

I guess 0.919s is at `rgwothistime` and 0.009s is at `rgtime` columns. Both times are for ripgrep. For matching grep times scroll right.

* `rgtime` means: simply `time rg ./xx`. It took only 0.009s because it was a small directory.
* `rg w/o this time` means: `time rg {every other ./file you see there except ./xx}`.

I know `w/o this` columns seem bullshit, but it just a sanity check. One column shows times for `rg ./xx` and the column next to it shows times for `rg {every other ./file you see there except ./xx}`. If we add up this two, we should get time for `rg ./xx   +  {every other ./file except ./xx}` == `rg {every children of .}` == `rg .`.



---

_Comment by @zsugabubus on 2019-07-31 17:20_

##### `rg -uuua` vs `grep -ra`

```
rg -uuua file\\.php               6.97s user 0.43s system 383% cpu 1.928 total
grep --color=auto -ra file\\.php  0.15s user 0.11s system  98% cpu 0.265 total
```

`rg -aj4` is around 1.3.

##### `rg -uuu . ./` vs `grep -r . ./`

```
rg -uuu . ./              > /tmp/out.rg    4.84s user 0.68s system 346% cpu 1.594 total
grep --color=auto -r . ./ > /tmp/out.grep  0.37s user 0.20s system  96% cpu 0.591 total
```

Sizes: 173M±0.6M.

---

_Comment by @BurntSushi on 2019-07-31 17:45_

My next step here would be to keep comparing directories. In your table, there are lots of directories that have ripgrep being much slower than grep. I would just keep digging into those directories recursively until you widdle the corpus down. It might also be interesting to try your search in a way where both tools exclude files with very long lines, if that's possible.

My guess is that the 7.5 million character lines has something to do with this. That is extremely unusual for a plain text file, and it's plausible it's exposing pathological behavior in either tool.

---

_Comment by @zsugabubus on 2019-07-31 18:03_

> My guess is that the 7.5 million character lines has something to do with this.

Maybe, but on the other hand '\n' is just like any other character, and these lines don't have to be printed. And as an important note:
* either time info of JSON output is buggy and really the long lines cause the slowdown,
* or it has nothing to do with file sizes and line length.

Why? Look at my `always_begin_end = true` [results from above](https://gist.github.com/zsugabubus/79f82a623d2d1804c96814875dfe7b87):
```
(top 1000 slowest)
      time          size
 993: 0.024438s      587
...
1001: 0.046391s  7528513
(slowest)
```

That 7.5M file is one long line. It takes the longest time to scan: 0.05s. 8 lines above a 0.0006M file took 0.02s to scan. ~Impossible~. My best guess would be it's waiting for something. Maybe a spinlock, if not syscalls?

(I checked three times the results and it's not an individual case... but the most shocking.)

---

_Comment by @BurntSushi on 2019-07-31 18:17_

> but on the other hand '\n' is just like any other character, and these lines don't have to be printed

Nope, that's very wrong. Both grep (AFAIK) and ripgrep _require_ an entire line to be in memory in order to search it. Therefore, when reading from a file, they both (should) buffer until it sees EOF or the next `\n`. This is related to binary files, which very frequently have long sequences of bytes without a `\n` in them. The way the `-a/--text` flag works in both grep and ripgrep is to replace all `NUL` bytes with a line terminator, which prevents exorbitant memory usage in most cases. This isn't done by default since both grep and ripgrep will stop searching when they see a NUL byte. Thus, plain text files with very long lines could be provoking something pathological here. (Note that this is super subtle, and has taken me a long time to work out completely.)

Remember, I am **just guessing** based on my mental model of how these programs work and my interpretation of the data you've presented so far. Both things could be wrong in subtle ways. ;-)

I agree that your JSON results are a bit interesting. I'm not sure what to make of them. I think I'm at a loss at this point and would require a reproduction to move further.

---

_Comment by @zsugabubus on 2019-07-31 18:29_

> Nope, that's very wrong

Oops, sorry. What was in my mind is that they don't have to be printed, so they don't have to be stored. Silly thing.


---

_Comment by @zsugabubus on 2019-07-31 18:45_

I can't believe it, but hurray ripgrep is faster:
```
$ time rg file\\.php ./.../thathugefile
rg file\\.php                  0.01s user 0.01s system 96% cpu 0.015 total
$ time grep file\\.php ./.../thathugefile
grep --color=auto file.php     0.02s user 0.02s system 98% cpu 0.038 total
```

Notes:
1) times differ (!): not 0.046391s as above, just 0.015s,
2) ripgrep handles extreme line length excellently,
3) this huge file isn't in that folder that takes 200ms to scan (4th entry),
4) my conclusion would be that the key to the solution isn't in long lines, but something really different.

----

So... what if it isn't an issue with ripgrep but an issue with `walkdir` (or whatever it uses).

> Note that tokei has similar running time.

```
$ time strace -fc tokei
% time     seconds  usecs/call     calls    errors syscall
------ ----------- ----------- --------- --------- ----------------
 44.92    1.346454        2259       596       138 futex
 31.02    0.929767           9     97796           read
 14.12    0.423211          17     24584     11063 openat
  4.52    0.135603          10     13521           close
  2.38    0.071207          12      5548           getdents64
  1.33    0.039944          14      2789      2787 stat
  1.05    0.031580          11      2782           fstat
  0.32    0.009592          17       543           mprotect
  0.17    0.005214           8       590           sched_yield
  0.05    0.001363          80        17           sigaltstack
  0.03    0.000822         137         6           clone
  0.03    0.000767          17        43           mmap
  0.02    0.000691         345         2           nanosleep
  0.01    0.000338          48         7           set_robust_list
  0.01    0.000282          25        11           munmap
  0.01    0.000188          23         8           sched_getaffinity
  0.00    0.000133          19         7           mremap
  0.00    0.000090          30         3           getrandom
  0.00    0.000052           6         8           lseek
  0.00    0.000036           7         5           rt_sigaction
  0.00    0.000032           4         7           brk
  0.00    0.000025          25         1           access
  0.00    0.000022          11         2         1 arch_prctl
  0.00    0.000021          10         2           getcwd
  0.00    0.000017           8         2           prlimit64
  0.00    0.000013          13         1           ioctl
  0.00    0.000012          12         1           execve
  0.00    0.000009           9         1           set_tid_address
  0.00    0.000008           8         1           rt_sigprocmask
  0.00    0.000008           8         1           fcntl
  0.00    0.000000           0         1           write
  0.00    0.000000           0         2           madvise
------ ----------- ----------- --------- --------- ----------------
100.00    2.997501                148888     13989 total
strace -fc tokei  4.90s user 3.29s system 251% cpu 3.259 total
```

Seems similar, don't you think?


---

_Comment by @BurntSushi on 2019-07-31 22:20_

I poked at the long lines because a 7.5 million character long line is highly unusual, so it's just a shot in the dark with respect to it being an anomaly.

I cannot come up with any hypothesis that would explain slowness in ripgrep's directory traversal. Multithreaded and single threaded directory traversal use completely different code, so the issue would need to be present in  both implementations. I wrote both of them myself, so it's possible.

While the syscall traces are interesting, your `time` output suggests the difference is in `user` time and not `sys` time, which points away from the specific syscalls invoked.

I'm afraid the only path forward here is to either recursively widdle the corpus down so that a proper analysis can be done, or by giving me a reproduction so that I can investigate myself. Sorry. :-/

---

_Comment by @BurntSushi on 2019-07-31 22:21_

Tokei uses the same directory traversal code as ripgrep, so that is an interesting point in favor of that being the slow down. But I still can't think of a hypothesis to explain it.

---

_Comment by @BurntSushi on 2019-08-01 02:21_

Have you tried different queries? e.g., `filephp` with no period? Or other queries in general?

---

_Comment by @zsugabubus on 2019-08-01 08:40_

> Have you tried different queries?

Yes, of course, but results have to taken with care, because with "complex" regexp queries ripgrep can easily beat grep even if it's slowed down by this issue. But generally speaking yes, if I grep for regexp-less or queries with moderated amount of regexps I get similar times.

---

In the mean time, I created some simple projects to test `ignore` and `walkdir` crates. Probably You do, but I didn't think of it before, that `openat` and `stat` errors come from nonexistent `.ignore` and `.git`... etc files.

**UPDATE**: And futex errors come from `ignore` crate parallel walking.

**UPDATE2**: Walking the directory: 0.150s; parallel: highly varies between 0.125-0.170s. I will try making some changes in the parallel walk code of `ignore` to see if some changes.

---

_Comment by @BurntSushi on 2019-08-01 10:54_

> But generally speaking yes, if I grep for regexp-less or queries with moderated amount of regexps I get similar times

For grins, can you try searching for `qqqqqqqqqq`? (I am trying to rule out pathological behavior in the literal searcher. ripgrep uses a different one from grep. Searching for a pattern with the same rare letter repeated should hopefully put these on a level playing field. Again, just a shot in the dark.)

>  Probably You do, but I didn't think of it before, that openat and stat errors come from nonexistent .ignore and .git... etc files.

Right yeah. Those shouldn't be happening though when running with `-uuu`. If they are, then that's a bug.

Thanks for really digging into this! I'll be quite curious to see what you find!

---

_Comment by @zsugabubus on 2019-08-01 12:11_

> can you try searching for qqqqqqqqqq?

Searched for every such pattern using `{0..9} {a..z} {A..Z}` letters, but it stays around 1.2s (grep 0.3s). (There were about 5 cases when it dropped slightly below 1s.) 

ripgrep had higher variance in times (0.92-1.23s) compared to grep (0.29-0.36s). Aside form the working of the algorithm, it suggests me that it's the effect of the bug.


---

_Comment by @zsugabubus on 2019-08-01 12:22_

I think I got it. Look at the times.

```                                                                                                                                                                
~/ripgrep/target/release/rg 'abrakadabra'  4.07s user 0.52s system 350% cpu 1.308 total                                                                                                                                                             
~/ripgrep/target/release/rg 'abrakadabra'  2.03s user 0.49s system 357% cpu 0.706 total                                                                                                                                                             
~/ripgrep/target/release/rg 'abrakadabra'  4.09s user 0.44s system 349% cpu 1.298 total                                                                                                                                                               
~/ripgrep/target/release/rg 'abrakadabra'  3.87s user 0.45s system 361% cpu 1.194 total                                                                                                                                                             
~/ripgrep/target/release/rg 'abrakadabra'  1.24s user 0.35s system 349% cpu 0.455 total

# Later:
~/ripgrep/target/release/rg 'abrakadabra'  3.08s user 0.42s system 360% cpu 0.972 total
~/ripgrep/target/release/rg 'abrakadabra'  2.27s user 0.38s system 349% cpu 0.757 total
~/ripgrep/target/release/rg 'abrakadabra'  2.14s user 0.40s system 360% cpu 0.706 total
~/ripgrep/target/release/rg 'abrakadabra'  3.99s user 0.48s system 360% cpu 1.240 total
~/ripgrep/target/release/rg 'abrakadabra'  3.78s user 0.46s system 353% cpu 1.200 total
~/ripgrep/target/release/rg 'abrakadabra'  2.54s user 0.38s system 362% cpu 0.807 total
~/ripgrep/target/release/rg 'abrakadabra'  3.68s user 0.45s system 361% cpu 1.144 total
~/ripgrep/target/release/rg 'abrakadabra'  3.58s user 0.52s system 351% cpu 1.163 total
~/ripgrep/target/release/rg 'abrakadabra'  2.14s user 0.38s system 364% cpu 0.691 total
~/ripgrep/target/release/rg 'abrakadabra'  2.83s user 0.41s system 356% cpu 0.908 total
~/ripgrep/target/release/rg 'abrakadabra'  1.35s user 0.35s system 348% cpu 0.489 total
```

Used a little shitty hack, that tells some threads to refuse to the work if it is a directory, so basically one threads does the directory traversal and others taking care of files. It can have huge performance penalty though, but... it works sometimes.

---

(Contains probable fix for removing unnecessary `sleep`.)

```diff
diff --git a/ignore/src/walk.rs b/ignore/src/walk.rs
index df796d4..ecced49 100644
--- a/ignore/src/walk.rs
+++ b/ignore/src/walk.rs
@@ -1161,7 +1161,7 @@ impl WalkParallel {
         let num_quitting = Arc::new(AtomicUsize::new(0));
         let quit_now = Arc::new(AtomicBool::new(false));
         let mut handles = vec![];
-        for _ in 0..threads {
+        for i in 0..threads {
             let worker = Worker {
                 f: mkf(),
                 tx: tx.clone(),
@@ -1177,7 +1177,7 @@ impl WalkParallel {
                 follow_links: self.follow_links,
                 skip: self.skip.clone(),
             };
-            handles.push(thread::spawn(|| worker.run()));
+            handles.push(thread::spawn(move || worker.run(i > 0)));
         }
         drop(tx);
         drop(rx);
@@ -1314,7 +1314,7 @@ impl Worker {
     ///
     /// The worker will call the caller's callback for all entries that aren't
     /// skipped by the ignore matcher.
-    fn run(mut self) {
+    fn run(mut self, files_only: bool) {
         while let Some(mut work) = self.get_work() {
             // If the work is not a directory, then we can just execute the
             // caller's callback immediately and move on.
@@ -1331,6 +1331,12 @@ impl Worker {
                     return;
                 }
             }
+
+            if files_only {
+                self.tx.send(Message::Work(work)).unwrap();
+                continue;
+            }
+
             let readdir = match work.read_dir() {
                 Ok(readdir) => readdir,
                 Err(err) => {
@@ -1470,11 +1476,12 @@ impl Worker {
     /// If all work has been exhausted, then this returns None. The worker
     /// should then subsequently quit.
     fn get_work(&mut self) -> Option<Work> {
+        let mut value = self.rx.try_recv().map_err(|_| ());
         loop {
             if self.is_quit_now() {
                 return None;
             }
-            match self.rx.try_recv() {
+            match value {
                 Ok(Message::Work(work)) => {
                     self.waiting(false);
                     self.quitting(false);
@@ -1514,22 +1521,15 @@ impl Worker {
                 Err(_) => {
                     self.waiting(true);
                     self.quitting(false);
+                    // If all threads are waiting, then quit.
                     if self.num_waiting() == self.threads {
                         for _ in 0..self.threads {
                             self.tx.send(Message::Quit).unwrap();
                         }
-                    } else {
-                        // You're right to consider this suspicious, but it's
-                        // a useful heuristic to permit producers to catch up
-                        // to consumers without burning the CPU. It is also
-                        // useful as a means to prevent burning the CPU if only
-                        // one worker is left doing actual work. It's not
-                        // perfect and it doesn't leave the CPU completely
-                        // idle, but it's not clear what else we can do. :-/
-                        thread::sleep(Duration::from_millis(1));
                     }
                 }
             }
+            value = self.rx.recv().map_err(|_| ());
         }
     }
```

---

_Comment by @BurntSushi on 2019-08-01 12:35_

Hmmm but that doesn't explain why single-threaded ripgrep is so much slower than single-threaded grep?

---

_Comment by @zsugabubus on 2019-08-01 12:42_

Ehhh.

---

_Comment by @zsugabubus on 2019-08-01 14:37_

* Where does ripgrep can do lot's of memcpy?
* Why does ripgrep read 3 bytes at first from files?

---

_Comment by @BurntSushi on 2019-08-01 14:39_

> Where does ripgrep can do lot's of memcpy?

Sorry, don't know what you mean. Probably when reading files.

> Why does ripgrep read 3 bytes at first from files?

To check the BOM.

---

_Comment by @zsugabubus on 2019-08-01 15:04_

> Sorry, don't know what you mean. Probably when reading files.

From perf report it seems like ripgrep does nothing just copies memory all the time.

> To check the BOM.

Just because, it shows up as a separate syscall. It means a bunch of syscalls and 0.1s (estimated) in total in my case. Can't a bigger chunk be read?

---

_Comment by @BurntSushi on 2019-08-01 15:11_

I'd have to see it myself.

> Just because, it shows up as a separate syscall. It means a bunch of syscalls and 0.1s (estimated) in total in my case. Can't a bigger chunk be read?

The answers to these types of questions is almost always "yes." But I don't know what the complexities are off the top of my head, nor do I know what the trade-offs are. It would require someone making the change and measuring it.

I think looking at these micro-optimizations is probably a distraction, and I really just do not have the time or patience to go back and forth about them all the time. I realize you're being super helpful here by trying to diagnose a performance problem, but all I have are guesses and bad theories. The most likely explanation, given that you can reproduce this on other corpora, is that there is something special about your corpus that is provoking some kind of bad behavior in ripgrep.

---

_Comment by @zsugabubus on 2019-08-01 18:25_

I also know that not this causes the slowdown for me, I just thought you would be interested in it. 

Anyway, I'm not sure I understand why 10000 system calls (linear in searched files), that takes up 10% of running time is labeled as micro-optimization.

---

I flattened the directories, to exclude my slow directory traversal theory.
* ripgrep still reproduced the issue, with the same times,
* ran ripgrep on files less then `x` size, then files greater than `x` size; the sum of the two running times didn't match (was less) the needed time for ripgrepping all files,
* grep always overperformed ripgrep in the previous tests, so I thought that really there are files that hard for ripgrep,
* so I run ripgrep and grep on each file in a for loop; result: nothing curious.

The last thing I can think is:
```
    88.76%    88.76%  rg       libc-2.29.so        [.] __memmove_sse2_unaligned_erms
+   88.76%     0.00%  rg       libc-2.29.so        [.] __memcpy_sse2_unaligned_erms (inlined)
+   88.38%     0.00%  rg       rg                  [.] 0x00005557c69de13c
     0.45%     0.00%  rg       libc-2.29.so        [.] __GI___libc_realloc (inlined)
```

---

_Comment by @zsugabubus on 2019-08-02 02:21_

Two difference between the two is a file with a 7.5m-character-long line.

##### perf stats
```
Performance counter stats for 'sh -c rg -uuuj1 abrakadabra $files':

            144.27 msec task-clock:u              #    1.003 CPUs utilized
                 0      context-switches:u        #    0.000 K/sec
                 0      cpu-migrations:u          #    0.000 K/sec
             5,320      page-faults:u             #    0.037 M/sec
       302,890,228      cycles:u                  #    2.099 GHz
       298,341,425      stalled-cycles-frontend:u #   98.50% frontend cycles idle
       191,548,028      stalled-cycles-backend:u  #   63.24% backend cycles idle
       204,282,200      instructions:u            #    0.67  insn per cycle
                                                  #    1.46  stalled cycles per insn
        22,090,443      branches:u                #  153.118 M/sec
           246,221      branch-misses:u           #    1.11% of all branches

       0.143805265 seconds time elapsed

       0.128016000 seconds user
       0.016430000 seconds sys

 Performance counter stats for 'sh -c rg -uuuj1 abrakadabra $files':

             33.07 msec task-clock:u              #    1.023 CPUs utilized
                 0      context-switches:u        #    0.000 K/sec
                 0      cpu-migrations:u          #    0.000 K/sec
             1,100      page-faults:u             #    0.033 M/sec
        26,158,964      cycles:u                  #    0.791 GHz
        31,883,728      stalled-cycles-frontend:u #  121.88% frontend cycles idle
        26,446,107      stalled-cycles-backend:u  #  101.10% backend cycles idle
        37,740,516      instructions:u            #    1.44  insn per cycle
                                                  #    0.84  stalled cycles per insn
         7,038,734      branches:u                #  212.868 M/sec
           188,825      branch-misses:u           #    2.68% of all branches

       0.032336605 seconds time elapsed

       0.024202000 seconds user
       0.009026000 seconds sys
```

(`$files` is a worst-case subset of all files.)

##### perf reports
```
+   26.38%    26.38%  rg       [unknown]           [k] 0xffffffff83a00b07
+   23.66%     1.33%  rg       libc-2.29.so        [.] __memset_sse2_unaligned_erms
+   19.06%    18.41%  rg       libc-2.29.so        [.] __memmove_sse2_unaligned_erms
+   18.46%     0.00%  rg       rg                  [.] 0x0000563090fa5ac0
+   10.85%    10.85%  rg       [unknown]           [k] 0xffffffff83a00163
+    5.21%     5.21%  sh       [unknown]           [k] 0xffffffff83a00b07
+    4.88%     0.00%  rg       libpthread-2.29.so  [.] __libc_read
+    3.46%     2.89%  sh       libc-2.29.so        [.] __strcmp_sse2_unaligned
+    2.84%     2.84%  sh       [unknown]           [k] 0xffffffff83a00163
+    2.10%     0.00%  rg       libpthread-2.29.so  [.] __open64

+   77.67%    76.91%  rg       libc-2.29.so        [.] __memmove_sse2_unaligned_erms
+   77.57%     0.00%  rg       rg                  [.] 0x0000563181086ac0
+    8.05%     8.05%  rg       [unknown]           [k] 0xffffffff83a00b07
+    6.42%     0.30%  rg       libc-2.29.so        [.] __memset_sse2_unaligned_erms
+    3.18%     3.18%  rg       [unknown]           [k] 0xffffffff83a00163
+    1.66%     0.00%  rg       libpthread-2.29.so  [.] __libc_read
+    0.99%     0.00%  rg       [unknown]           [k] 0x0000000000000003
     0.98%     0.98%  sh       [unknown]           [k] 0xffffffff83a00b07
+    0.92%     0.92%  rg       rg                  [.] 0x000000000019fb07
+    0.92%     0.00%  rg       rg                  [.] 0x0000563181135b00
```

##### shuffle $files then ripgrep
```
0.01s user 0.02s system 98% cpu 0.029 total
0.03s user 0.02s system 98% cpu 0.052 total
0.11s user 0.02s system 98% cpu 0.126 total
0.08s user 0.02s system 98% cpu 0.101 total
0.05s user 0.01s system 98% cpu 0.063 total
0.09s user 0.02s system 99% cpu 0.107 total
0.02s user 0.01s system 97% cpu 0.032 total
0.11s user 0.02s system 99% cpu 0.131 total
0.07s user 0.01s system 98% cpu 0.081 total
0.09s user 0.01s system 98% cpu 0.101 total
0.13s user 0.02s system 95% cpu 0.150 total
0.01s user 0.01s system 98% cpu 0.027 total
0.11s user 0.02s system 99% cpu 0.127 total
```

I hope you can reproduce the issue: [clone this gist and run the shell script](https://gist.github.com/zsugabubus/79f82a623d2d1804c96814875dfe7b87). (Sorry, for the lot's of files, it surely can be reproduced from much less, but I have no time to experiment with it.)

#### My new theory

A huge memory reallocation could explain all these symptoms.

---

_Closed by @BurntSushi on 2019-08-02 11:36_

---

_Comment by @BurntSushi on 2019-08-02 11:36_

Wonderful, thank you. I can finally repro it. This is a very interesting bug.
To exaggerate it a bit, I copied your long-line file so that there was 120MB on
a single line:

```
$ wc longline
        1         1 120365121 longline
```

To establish a baseline and get my bearings, I just tried poking at that single
file to see how ripgrep behaves compared to grep:

```
$ time rg -a zzzzz longline

real    0.031
user    0.023
sys     0.007
maxmem  117 MB
faults  0

$ time rg -a zzzzz longline --no-mmap

real    0.154
user    0.067
sys     0.087
maxmem  159 MB
faults  0

$ time grep -a zzzzz longline

real    0.345
user    0.124
sys     0.221
maxmem  231 MB
faults  0
```

The first command is a bit deceptive, because ripgrep will open the file using
memory maps, which avoids the whole buffering strategy completely. So ripgrep
zips through it without needing to care about line terminators and such. My
guess is that this probably added to some of your confusion when trying to get
a smaller reproduction, but I forgot about it. Once we disable memory maps, we
can see that ripgrep slows down a bit, but it's still 2x faster than grep.

Another curiosity here is that the maximum memory used by grep is ~2x more than
ripgrep. My guess here is that this is because grep is using a larger buffer
size, but I'm not 100% on this.

However, things start to change once we try to search multiple files at once.
Notably, this does _not_ do directory traversal or use parallelism, so we rule
that out as the cause:

```
$ time rg -a -j1 zzzzz longline 2*

real    0.549
user    0.464
sys     0.083
maxmem  159 MB
faults  0

$ time rg -a -j1 zzzzz longline 2* --mmap

real    0.030
user    0.020
sys     0.010
maxmem  117 MB
faults  0

$ time grep -a zzzzz longline 2*

real    0.331
user    0.150
sys     0.180
maxmem  231 MB
faults  0
```

The first two commands confirm that using memory maps "fixes" this problem, so
it's really starting to look like very long lines are the issue. Despite the
fact that grep was 2x slower searching just `longline`, it winds up being
noticeably faster in this particular search. For completeness, we verify that
`2*` isn't causing any issues on its own:

```
$ time rg -a -j1 zzzzz 2*

real    0.010
user    0.006
sys     0.003
maxmem  10 MB
faults  0

$ time grep -a zzzzz 2*

real    0.008
user    0.002
sys     0.005
maxmem  10 MB
faults  0
```

So that seems fine. Digging into this a bit more, running `perf record` with
the `--call-graph` option enabled lets us track more precisely why things are
being slow. Running

```
$ perf record -g --call-graph rg -a -j1 --no-mmap zzzzz longline
```

looks fairly uninteresting and about what I'd expect. Most time is being spent
in resizing the buffer allocation to accommodate the large line, zeroing the
memory and running `memchr` on it for the actual search.

However, changing the command to

```
$ perf record -g --call-graph rg -a -j1 --no-mmap zzzzz longline 2*
```

reveals that 90% of the time is being spent in `__memmove_avx_unaligned_erms`,
and moreover, all of that time is coming from within ripgrep's
`LineBuffer::roll` method. Now that's strange, but I think it's also consistent
with everything you've observed so far. In other words, we have two pieces
that, on their own, are fine, but when combined, something perverse is
happening.

My instinct tells me that something is going wrong with reusing the
allocation. In particular, if a file like `longline` grows the allocation for
the line buffer to be very large, and that same buffer is reused for much
smaller files, then perhaps whenever those files need to roll the buffer, they
wind up paying a much larger cost for shifting things around.

Hmmm... yes... this is starting to make sense. In the `roll` implementation, we
have this call:

```
self.buf.copy_within_str(self.pos.., 0);
```

which effectively moves everything at `self.pos` and after it back to the
beginning of the buffer. If this line buffer had grown very large---due to a
file with very long lines---then this is going to do a ton of copying.

But copying _everything_ after `self.pos` is not necessary. All we need to do
is copy anything that has been _written_ after `self.pos` back to the
beginning. This is exactly the range `self.pos..self.end`, where any bytes
after `self.end` are just treated as free space that the buffer can use on the
next read. So, let's try changing the code to

```
self.buf.copy_within_str(self.pos..self.end, 0);
```

And re-running the command

```
$ time rg -a -j1 zzzzz longline 2*

real    0.159
user    0.066
sys     0.092
maxmem  159 MB
faults  0
```

Voilà! To pop back and make sure everything works across the entire directory:

```
$ time rg -auuuj1 zzzzz

real    0.158
user    0.053
sys     0.104
maxmem  159 MB
faults  0

$ time rg -auuu zzzzz

real    0.167
user    0.057
sys     0.153
maxmem  177 MB
faults  0

$ time grep -ar zzzzz

real    0.338
user    0.120
sys     0.217
maxmem  231 MB
faults  0
```

So I think that fixes it. :-) I think this explains everything. Even your
changes to directory traversal, and the otherwise inconsistent results you were
seeing. Namely, if the file with a very long line is searched _after_ most
other files, then the performance impact will be small, since the large buffer
will be used for a comparatively small number of files. So the only way to
observe this is to be in a situation where searching the file with a long line
occurs earlier in the search.

This was an excellent bug. Thank you so much for sticking with it and providing
a reproduction that I could dig into. This is now fixed on master!

---

_Comment by @zsugabubus on 2019-08-02 12:41_

Nice. I lost all my hairs... and seeing that the fix is only 8 chars... :grinning: 

I just wondered a little about Rust. Wasn't it a bit too implicit in this case? If you should have to explicitly type the end of the range, maybe you could spot out this issue more sooner.

---

_Comment by @BurntSushi on 2019-08-02 12:49_

I don't think so. I've been using `foo..` for years. It was just a mental/transcription error. The initial version of the roll buffer didn't have this bug: https://github.com/BurntSushi/ripgrep/blob/b1c52b52d6eed5b2436e7cdb92617c06c43696c3/src/search_stream.rs#L517

---

_Comment by @gleventhal on 2020-08-01 18:58_

I haven't read through everything here, but is he using ZFS on Linux by any chance?  I have some evidence that (at least in some cases) ZFS locking is leading to multithreaded ripgrep getting slower with each added core ( -j1 is faster than -j4, etc). 

@BurntSushi have you ever heard anything related to that issue before?   (update) sorry, I see that this was a bug and was resolved with a code change, but I am still interested to hear whether you've seen and ZFS-related issues.  

---

_Comment by @BurntSushi on 2020-08-01 21:32_

I haven't heard anything about that, no. I don't think I've ever tried ripgrep on zfs.

---

_Comment by @gleventhal on 2020-09-11 20:49_

I was looking at the behavior trying to better understand what is happening.  I noticed the crossbeam-channel threads during get_work and run_one were yielding the CPU a lot when a certain ZFS feature was enabled.  However they weren't done, there was more work to do (just perhaps not available at that moment, presumably because of directory locks).

When running rg --files on a large directory structure with ZFS, the runs has low CPU utilization due to the threads yielding repeatedly much of the time.  Can you explain what conditions the crossbeam-channel consumer threads would yield during a run?

---

_Comment by @BurntSushi on 2020-09-13 12:51_

You might be using an older version of ripgrep. The latest version doesn't use crossbeam at all. It just uses a stack guarded by a mutex: https://github.com/BurntSushi/ripgrep/blob/11c7b2ae17c0c2547eaffc92ac80ff03bd9950d7/crates/ignore/src/walk.rs#L1390-L1396

See #1554 for more details on that change.

---

_Comment by @BurntSushi on 2020-09-13 12:52_

Otherwise, I would expect a yield to occur if there's nothing in the queue maybe?

---
