```yaml
number: 325
title: Enable LTO (Link Time Optimization)
type: pull_request
state: closed
author: wdv4758h
labels: []
assignees: []
base: master
head: lto
created_at: 2017-01-16T03:56:14Z
updated_at: 2017-03-08T15:19:20Z
url: https://github.com/BurntSushi/ripgrep/pull/325
synced_at: 2026-01-12T18:23:12Z
```

# Enable LTO (Link Time Optimization)

---

_@wdv4758h_

With this optimization, the size of stripped ripgrep's binary can reduce from `2137936` to `2117424` bytes on my machine 😃 

---

_Comment by @BurntSushi on 2017-01-16 03:59_

We should not do this just to reduce binary size. A few bytes really should not matter.

What does matter however is compile times and runtime performance. How are those two things impacted by this?

---

_Comment by @kbknapp on 2017-01-16 05:05_

My unscientific run looks to be somewhat significant (first normal run, then compiled with lto=true):

```
kevin@pc: ~ 
➜ find -type f | wc -l
146992

kevin@pc: ~ 
➜ /usr/bin/time -v rg "unsafe" -uuu 2>&1 > /dev/null
        Command being timed: "rg unsafe -uuu"
        User time (seconds): 15.83
        System time (seconds): 6.63
        Percent of CPU this job got: 760%
        Elapsed (wall clock) time (h:mm:ss or m:ss): 0:02.93
        Average shared text size (kbytes): 0
        Average unshared data size (kbytes): 0
        Average stack size (kbytes): 0
        Average total size (kbytes): 0
        Maximum resident set size (kbytes): 414300
        Average resident set size (kbytes): 0
        Major (requiring I/O) page faults: 0
        Minor (reclaiming a frame) page faults: 2498
        Voluntary context switches: 124
        Involuntary context switches: 6473
        Swaps: 0
        File system inputs: 0
        File system outputs: 0
        Socket messages sent: 0
        Socket messages received: 0
        Signals delivered: 0
        Page size (bytes): 4096
        Exit status: 0

kevin@pc: ~ 
➜ /usr/bin/time -v rg-lto "unsafe" -uuu 2>&1 > /dev/null
        Command being timed: "rg-dbg-lto unsafe -uuu"
        User time (seconds): 6.78
        System time (seconds): 7.56
        Percent of CPU this job got: 582%
        Elapsed (wall clock) time (h:mm:ss or m:ss): 0:02.46
        Average shared text size (kbytes): 0
        Average unshared data size (kbytes): 0
        Average stack size (kbytes): 0
        Average total size (kbytes): 0
        Maximum resident set size (kbytes): 482424
        Average resident set size (kbytes): 0
        Major (requiring I/O) page faults: 0
        Minor (reclaiming a frame) page faults: 4479
        Voluntary context switches: 4323
        Involuntary context switches: 3773
        Swaps: 0
        File system inputs: 0
        File system outputs: 0
        Socket messages sent: 0
        Socket messages received: 0
        Signals delivered: 0
        Page size (bytes): 4096
        Exit status: 0
```

Both were run multiple times to try and reduce any variance from file caching.

---

_Comment by @wdv4758h on 2017-01-16 05:14_

The effect of enabling `lto` is:

* longer compile time
* smaller binary size
* faster runtime

The compile time of release mode will goes from `223.90 secs` to `355.56 secs` on my machine, using `rustc 1.16.0-nightly (bf6d7b665 2017-01-15)`.

I'll try to run some benchsuite later.

---

_Comment by @wdv4758h on 2017-01-16 08:41_

Here is the result of benchsuite on my machine.

without LTO:
```
linux_alternates (pattern: ERR_SYS|PME_TURN_OFF|LINK_REQ_RST|CFG_BME_EVT)
-------------------------------------------------------------------------
rg (ignore)         0.554 +/- 0.010 (lines: 68)
rg (whitelist)*     0.497 +/- 0.023 (lines: 68)*

linux_alternates_casei (pattern: ERR_SYS|PME_TURN_OFF|LINK_REQ_RST|CFG_BME_EVT)
-------------------------------------------------------------------------------
rg (ignore)         0.565 +/- 0.006 (lines: 160)
rg (whitelist)*     0.529 +/- 0.017 (lines: 160)*

linux_literal (pattern: PM_RESUME)
----------------------------------
rg (ignore)          0.286 +/- 0.004 (lines: 16)
rg (ignore) (mmap)   3.406 +/- 0.205 (lines: 16)
rg (whitelist)*      0.236 +/- 0.016 (lines: 16)*

linux_literal_casei (pattern: PM_RESUME)
----------------------------------------
rg (ignore)          0.412 +/- 0.012 (lines: 370)
rg (ignore) (mmap)   3.470 +/- 0.127 (lines: 370)
rg (whitelist)*      0.380 +/- 0.006 (lines: 370)*

linux_literal_default (pattern: PM_RESUME)
------------------------------------------
rg*        0.247 +/- 0.005 (lines: 16)*

linux_no_literal (pattern: \w{5}\s+\w{5}\s+\w{5}\s+\w{5}\s+\w{5})
-----------------------------------------------------------------
rg (ignore)                 0.903 +/- 0.018 (lines: 490)
rg (ignore) (ASCII)         0.521 +/- 0.029 (lines: 490)
rg (whitelist)              0.759 +/- 0.022 (lines: 419)
rg (whitelist) (ASCII)*     0.451 +/- 0.004 (lines: 419)*

linux_re_literal_suffix (pattern: [A-Z]+_RESUME)
------------------------------------------------
rg (ignore)         0.276 +/- 0.009 (lines: 1652)
rg (whitelist)*     0.235 +/- 0.013 (lines: 1630)*

linux_unicode_greek (pattern: \p{Greek})
----------------------------------------
rg*    0.517 +/- 0.036 (lines: 23)*

linux_unicode_greek_casei (pattern: \p{Greek})
----------------------------------------------
rg*    0.490 +/- 0.021 (lines: 103)*

linux_unicode_word (pattern: \wAh)
----------------------------------
rg (ignore)                 0.275 +/- 0.020 (lines: 186)
rg (ignore) (ASCII)         0.288 +/- 0.019 (lines: 174)
rg (whitelist)              0.243 +/- 0.011 (lines: 180)
rg (whitelist) (ASCII)*     0.240 +/- 0.011 (lines: 168)*

linux_word (pattern: PM_RESUME)
-------------------------------
rg (ignore)         0.259 +/- 0.004 (lines: 6)
rg (whitelist)*     0.224 +/- 0.006 (lines: 6)*

subtitles_en_alternate (pattern: Sherlock Holmes|John Watson|Irene Adler|Inspector Lestrade|Professor Moriarty)
---------------------------------------------------------------------------------------------------------------
rg (lines)     3.349 +/- 0.018 (lines: 848)
rg*            3.112 +/- 0.012 (lines: 848)*

subtitles_en_alternate_casei (pattern: Sherlock Holmes|John Watson|Irene Adler|Inspector Lestrade|Professor Moriarty)
---------------------------------------------------------------------------------------------------------------------
rg*            3.498 +/- 0.012 (lines: 862)*

subtitles_en_literal (pattern: Sherlock Holmes)
-----------------------------------------------
rg*            0.379 +/- 0.024 (lines: 629)*
rg (no mmap)   0.490 +/- 0.009 (lines: 629)
rg (lines)     0.608 +/- 0.005 (lines: 629)

subtitles_en_literal_casei (pattern: Sherlock Holmes)
-----------------------------------------------------
rg*                   3.101 +/- 0.004 (lines: 642)*
rg (lines)            3.342 +/- 0.004 (lines: 642)

subtitles_en_literal_word (pattern: Sherlock Holmes)
----------------------------------------------------
rg (ASCII)*    0.606 +/- 0.001 (lines: 629)*
rg             0.610 +/- 0.002 (lines: 629)

subtitles_en_no_literal (pattern: \w{5}\s+\w{5}\s+\w{5}\s+\w{5}\s+\w{5}\s+\w{5}\s+\w{5})
----------------------------------------------------------------------------------------
rg             3.420 +/- 0.005 (lines: 13)
rg (ASCII)*    2.969 +/- 0.052 (lines: 13)*

subtitles_en_surrounding_words (pattern: \w+\s+Holmes\s+\w+)
------------------------------------------------------------
rg             0.624 +/- 0.001 (lines: 317)
rg (ASCII)*    0.613 +/- 0.001 (lines: 317)*

subtitles_ru_alternate (pattern: Шерлок Холмс|Джон Уотсон|Ирен Адлер|инспектор Лестрейд|профессор Мориарти)
-----------------------------------------------------------------------------------------------------------
rg (lines)     5.967 +/- 0.044 (lines: 691)
rg*            5.500 +/- 0.015 (lines: 691)*

subtitles_ru_alternate_casei (pattern: Шерлок Холмс|Джон Уотсон|Ирен Адлер|инспектор Лестрейд|профессор Мориарти)
-----------------------------------------------------------------------------------------------------------------
rg*            6.154 +/- 0.012 (lines: 735)*

subtitles_ru_literal (pattern: Шерлок Холмс)
--------------------------------------------
rg*            0.494 +/- 0.001 (lines: 583)*
rg (no mmap)   0.670 +/- 0.005 (lines: 583)
rg (lines)     0.930 +/- 0.004 (lines: 583)

subtitles_ru_literal_casei (pattern: Шерлок Холмс)
--------------------------------------------------
rg*                   5.496 +/- 0.007 (lines: 604)*
rg (lines)            5.954 +/- 0.023 (lines: 604)

subtitles_ru_literal_word (pattern: Шерлок Холмс)
-------------------------------------------------
rg (ASCII)*    0.507 +/- 0.022 (lines: 0)*
rg             0.942 +/- 0.006 (lines: 579)

subtitles_ru_no_literal (pattern: \w{5}\s+\w{5}\s+\w{5}\s+\w{5}\s+\w{5}\s+\w{5}\s+\w{5})
----------------------------------------------------------------------------------------
rg             5.980 +/- 0.012 (lines: 41)
rg (ASCII)*    4.786 +/- 0.025 (lines: 0)*

subtitles_ru_surrounding_words (pattern: \w+\s+Холмс\s+\w+)
-----------------------------------------------------------
rg*            1.003 +/- 0.037 (lines: 278)*
```

with LTO:
```
linux_alternates (pattern: ERR_SYS|PME_TURN_OFF|LINK_REQ_RST|CFG_BME_EVT)
-------------------------------------------------------------------------
rg (ignore)         0.504 +/- 0.007 (lines: 68)
rg (whitelist)*     0.480 +/- 0.006 (lines: 68)*

linux_alternates_casei (pattern: ERR_SYS|PME_TURN_OFF|LINK_REQ_RST|CFG_BME_EVT)
-------------------------------------------------------------------------------
rg (ignore)         0.552 +/- 0.014 (lines: 160)
rg (whitelist)*     0.510 +/- 0.007 (lines: 160)*

linux_literal (pattern: PM_RESUME)
----------------------------------
rg (ignore)          0.248 +/- 0.006 (lines: 16)
rg (ignore) (mmap)   3.029 +/- 0.080 (lines: 16)
rg (whitelist)*      0.213 +/- 0.004 (lines: 16)*

linux_literal_casei (pattern: PM_RESUME)
----------------------------------------
rg (ignore)          0.404 +/- 0.021 (lines: 370)
rg (ignore) (mmap)   3.327 +/- 0.087 (lines: 370)
rg (whitelist)*      0.400 +/- 0.029 (lines: 370)*

linux_literal_default (pattern: PM_RESUME)
------------------------------------------
rg*        0.234 +/- 0.006 (lines: 16)*

linux_no_literal (pattern: \w{5}\s+\w{5}\s+\w{5}\s+\w{5}\s+\w{5})
-----------------------------------------------------------------
rg (ignore)                 0.855 +/- 0.009 (lines: 490)
rg (ignore) (ASCII)         0.464 +/- 0.005 (lines: 490)
rg (whitelist)              0.761 +/- 0.015 (lines: 419)
rg (whitelist) (ASCII)*     0.441 +/- 0.007 (lines: 419)*

linux_re_literal_suffix (pattern: [A-Z]+_RESUME)
------------------------------------------------
rg (ignore)         0.251 +/- 0.009 (lines: 1652)
rg (whitelist)*     0.224 +/- 0.009 (lines: 1630)*

linux_unicode_greek (pattern: \p{Greek})
----------------------------------------
rg*    0.467 +/- 0.008 (lines: 23)*

linux_unicode_greek_casei (pattern: \p{Greek})
----------------------------------------------
rg*    0.468 +/- 0.020 (lines: 103)*

linux_unicode_word (pattern: \wAh)
----------------------------------
rg (ignore)                 0.263 +/- 0.002 (lines: 186)
rg (ignore) (ASCII)         0.258 +/- 0.008 (lines: 174)
rg (whitelist)*             0.248 +/- 0.028 (lines: 180)
rg (whitelist) (ASCII)      0.241 +/- 0.007 (lines: 168)*

linux_word (pattern: PM_RESUME)
-------------------------------
rg (ignore)         0.268 +/- 0.027 (lines: 6)
rg (whitelist)*     0.223 +/- 0.011 (lines: 6)*

subtitles_en_alternate (pattern: Sherlock Holmes|John Watson|Irene Adler|Inspector Lestrade|Professor Moriarty)
---------------------------------------------------------------------------------------------------------------
rg (lines)     3.344 +/- 0.019 (lines: 848)
rg*            3.107 +/- 0.014 (lines: 848)*

subtitles_en_alternate_casei (pattern: Sherlock Holmes|John Watson|Irene Adler|Inspector Lestrade|Professor Moriarty)
---------------------------------------------------------------------------------------------------------------------
rg*            3.490 +/- 0.007 (lines: 862)*

subtitles_en_literal (pattern: Sherlock Holmes)
-----------------------------------------------
rg*            0.365 +/- 0.002 (lines: 629)*
rg (no mmap)   0.485 +/- 0.002 (lines: 629)
rg (lines)     0.609 +/- 0.004 (lines: 629)

subtitles_en_literal_casei (pattern: Sherlock Holmes)
-----------------------------------------------------
rg*                   3.097 +/- 0.006 (lines: 642)*
rg (lines)            3.345 +/- 0.012 (lines: 642)

subtitles_en_literal_word (pattern: Sherlock Holmes)
----------------------------------------------------
rg (ASCII)     0.612 +/- 0.001 (lines: 629)
rg*            0.609 +/- 0.002 (lines: 629)*

subtitles_en_no_literal (pattern: \w{5}\s+\w{5}\s+\w{5}\s+\w{5}\s+\w{5}\s+\w{5}\s+\w{5})
----------------------------------------------------------------------------------------
rg             3.459 +/- 0.068 (lines: 13)
rg (ASCII)*    2.921 +/- 0.003 (lines: 13)*

subtitles_en_surrounding_words (pattern: \w+\s+Holmes\s+\w+)
------------------------------------------------------------
rg             0.623 +/- 0.002 (lines: 317)
rg (ASCII)*    0.621 +/- 0.012 (lines: 317)*

subtitles_ru_alternate (pattern: Шерлок Холмс|Джон Уотсон|Ирен Адлер|инспектор Лестрейд|профессор Мориарти)
-----------------------------------------------------------------------------------------------------------
rg (lines)     5.949 +/- 0.018 (lines: 691)
rg*            5.506 +/- 0.016 (lines: 691)*

subtitles_ru_alternate_casei (pattern: Шерлок Холмс|Джон Уотсон|Ирен Адлер|инспектор Лестрейд|профессор Мориарти)
-----------------------------------------------------------------------------------------------------------------
rg*            6.148 +/- 0.008 (lines: 735)*

subtitles_ru_literal (pattern: Шерлок Холмс)
--------------------------------------------
rg*            0.496 +/- 0.004 (lines: 583)*
rg (no mmap)   0.662 +/- 0.003 (lines: 583)
rg (lines)     0.941 +/- 0.008 (lines: 583)

subtitles_ru_literal_casei (pattern: Шерлок Холмс)
--------------------------------------------------
rg*                   5.526 +/- 0.051 (lines: 604)*
rg (lines)            5.940 +/- 0.044 (lines: 604)

subtitles_ru_literal_word (pattern: Шерлок Холмс)
-------------------------------------------------
rg (ASCII)*    0.492 +/- 0.001 (lines: 0)*
rg             0.935 +/- 0.004 (lines: 579)

subtitles_ru_no_literal (pattern: \w{5}\s+\w{5}\s+\w{5}\s+\w{5}\s+\w{5}\s+\w{5}\s+\w{5})
----------------------------------------------------------------------------------------
rg             6.034 +/- 0.008 (lines: 41)
rg (ASCII)*    4.775 +/- 0.012 (lines: 0)*

subtitles_ru_surrounding_words (pattern: \w+\s+Холмс\s+\w+)
-----------------------------------------------------------
rg*            0.978 +/- 0.002 (lines: 278)*
```

---

_Comment by @wdv4758h on 2017-01-16 08:48_

[lto.txt](https://github.com/BurntSushi/ripgrep/files/707662/lto.txt)
[non-lto.txt](https://github.com/BurntSushi/ripgrep/files/707663/non-lto.txt)


---

_Comment by @wdv4758h on 2017-01-16 09:46_

I've also tried what @kbknapp have done in the previous comment.

```
$ find -type f | wc -l
64126
$ /usr/bin/time -v rg.non-lto "unsafe" -uuu 2>&1 > /dev/null
        Command being timed: "rg.non-lto unsafe -uuu"
        User time (seconds): 9.52
        System time (seconds): 22.23
        Percent of CPU this job got: 20%
        Elapsed (wall clock) time (h:mm:ss or m:ss): 2:37.79
        Average shared text size (kbytes): 0
        Average unshared data size (kbytes): 0
        Average stack size (kbytes): 0
        Average total size (kbytes): 0
        Maximum resident set size (kbytes): 63864
        Average resident set size (kbytes): 0
        Major (requiring I/O) page faults: 2
        Minor (reclaiming a frame) page faults: 5492
        Voluntary context switches: 755812
        Involuntary context switches: 16987
        Swaps: 0
        File system inputs: 26042384
        File system outputs: 0
        Socket messages sent: 0
        Socket messages received: 0
        Signals delivered: 0
        Page size (bytes): 4096
        Exit status: 0
$ /usr/bin/time -v rg.lto "unsafe" -uuu 2>&1 > /dev/null
        Command being timed: "rg.lto unsafe -uuu"
        User time (seconds): 8.97
        System time (seconds): 18.19
        Percent of CPU this job got: 27%
        Elapsed (wall clock) time (h:mm:ss or m:ss): 1:40.63
        Average shared text size (kbytes): 0
        Average unshared data size (kbytes): 0
        Average stack size (kbytes): 0
        Average total size (kbytes): 0
        Maximum resident set size (kbytes): 75676
        Average resident set size (kbytes): 0
        Major (requiring I/O) page faults: 0
        Minor (reclaiming a frame) page faults: 11578
        Voluntary context switches: 625919
        Involuntary context switches: 14503
        Swaps: 0
        File system inputs: 20414496
        File system outputs: 0
        Socket messages sent: 0
        Socket messages received: 0
        Signals delivered: 0
        Page size (bytes): 4096
        Exit status: 0
```


---

_Comment by @BurntSushi on 2017-01-16 12:08_

From glancing at the output of the bench suite, there doesn't appear to be much of a difference. Certainly not enough to justify almost doubling compile time IMO. I might be able to be convinced to enable it when producing binaries for release.

@kbknapp @wdv4758h Is the corpus you're searching available? At least for @wdv4758h, it looks like your first search hit disk.

---

_Comment by @wdv4758h on 2017-01-16 13:15_

@BurntSushi I didn't notice I pick a run with additional 2 major page faults.

Here is another run:

```
$ /usr/bin/time -v rg.non-lto "unsafe" -uuu 2>&1 > /dev/null
        Command being timed: "rg.non-lto unsafe -uuu"
        User time (seconds): 9.49
        System time (seconds): 21.09
        Percent of CPU this job got: 24%
        Elapsed (wall clock) time (h:mm:ss or m:ss): 2:07.20
        Average shared text size (kbytes): 0
        Average unshared data size (kbytes): 0
        Average stack size (kbytes): 0
        Average total size (kbytes): 0
        Maximum resident set size (kbytes): 68288
        Average resident set size (kbytes): 0
        Major (requiring I/O) page faults: 0
        Minor (reclaiming a frame) page faults: 8875
        Voluntary context switches: 690747
        Involuntary context switches: 16041
        Swaps: 0
        File system inputs: 23068152
        File system outputs: 0
        Socket messages sent: 0
        Socket messages received: 0
        Signals delivered: 0
        Page size (bytes): 4096
        Exit status: 0
```

I ran it in `ripgrep` folder with files produced by `cargo build --release` and `benchsuite --download all`.

---

_@BurntSushi requested changes on 2017-02-18 17:10_

Sorry, but after thinking about this more, I can't merge this. I tried it out myself locally, and I can see a very small runtime difference. But the added compile time isn't even close to worth it. With LTO enabled, a re-compile of ripgrep takes >70 seconds, but without LTO, a re-compile takes <25 seconds. Huge difference. This isn't a reasonable increase because I frequently need to do cargo build --release during development.

If someone wanted this then I'd be open to adding it only in CI when the final release artifact is produced.

---

_Comment by @BurntSushi on 2017-03-08 15:19_

I'm just going to close this, but I do remain open to:

> If someone wanted this then I'd be open to adding it only in CI when the final release artifact is produced.

---

_Closed by @BurntSushi on 2017-03-08 15:19_

---
