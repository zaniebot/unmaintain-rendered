```yaml
number: 1279
title: request to benchmark Kazahana against ripgrep
type: issue
state: closed
author: Sanmayce
labels:
  - invalid
assignees: []
created_at: 2019-05-12T11:17:57Z
updated_at: 2020-10-24T20:53:58Z
url: https://github.com/BurntSushi/ripgrep/issues/1279
synced_at: 2026-01-12T16:13:23Z
```

# request to benchmark Kazahana against ripgrep

---

_@Sanmayce_

Hi,
wanted to download your testdatafile 'OpenSubtitles2016.raw.en.gz' from the main page, could you share a link or email me the working link?

See, I am interested to make a comparison of fastest string searchers into a single file - a BIG one, either as your 9GB one or as my Wikipedia 60GB dataset.

https://www.linuxquestions.org/questions/showthread.php?p=5292720

---

_Comment by @Sanmayce on 2019-05-13 21:22_

Oh, sorry, you already answered this at "Can't run benchsuite on master #1257" issue.

Two more things.

First, are you up to making a speed benchmark with this testdatafile including my Kazahana and, let's say, patterns:
"Sherlock "
"Sherlock Holmes"
"depressed Sherlock Holmes"
The idea behind third one is to destroy the pace of order 1 and order 2 BMH variants - 'es' and 'es' being the culprit, that is.

Second, just saw your stats for "Sherlock " pattern/literal:
https://news.ycombinator.com/item?id=19535225

```
    $ time rg --no-mmap 'Sherlock ' OpenSubtitles2016.raw.en | wc -l
    6698
    
    real    3.006
    user    1.658
    sys     1.345
    maxmem  8 MB
    faults  0
    
    $ time grep 'Sherlock ' OpenSubtitles2016.raw.en | wc -l
    6698
    
    real    9.023
    user    7.921
    sys     1.092
    maxmem  8 MB
    faults  0
```

Don't get why '6698', Kazahana's and grep's reported '6722':

```
C:\grep_vs_Kazahana>dir

12/20/2014  07:40 PM           451,824 grep-2.5.4-bin.zip
12/20/2014  07:40 PM           898,241 grep-2.5.4-dep.zip
12/20/2014  07:40 PM         1,361,303 grep-2.5.4-src.zip
12/20/2014  07:40 PM            96,256 grep.exe
05/13/2019  06:54 AM               576 GREP_fast.bat
05/13/2019  03:45 PM           263,485 Kazahana.txt
12/20/2014  07:40 PM               394 Kazahana_compile_GCC.bat
12/20/2014  07:40 PM               339 Kazahana_compile_Intel15_32bit_Qparallel.bat
05/13/2019  05:42 AM           278,041 Kazahana_Hexadecad_GCC_472.exe
05/13/2019  05:42 AM           260,419 Kazahana_Monad_GCC_472.exe
12/20/2014  07:40 PM         1,005,430 Kazahana_r1-++fix+nowait_critical_nixFIX_WolfRAM+fixITER+EX+CS_fix_DEFINE.c
12/20/2014  07:40 PM         1,008,128 libiconv2.dll
12/20/2014  07:40 PM           103,424 libintl3.dll
12/20/2014  07:40 PM         1,042,360 libiomp5md.dll
02/24/2018  11:23 AM            94,540 LineWordreporter.c
02/24/2018  11:23 AM            69,120 LineWordreporter.exe
04/08/2019  04:44 PM             1,633 MokujIN Amber 224 prompt.lnk
04/25/2019  02:06 AM             1,633 MokujIN GREEN 224 prompt.lnk
11/11/2016  08:44 PM     9,968,530,111 OpenSubtitle_corpus_en_2016_(337,845,355_lines).txt
12/20/2014  07:40 PM           140,288 pcre3.dll
12/20/2014  07:40 PM            79,360 regex2.dll
05/12/2019  02:08 PM         6,923,495 ripgrep-11.0.1-i686-pc-windows-gnu.zip
05/12/2019  02:08 PM         1,595,373 ripgrep-11.0.1-i686-pc-windows-msvc.zip
05/12/2019  02:09 PM         7,084,974 ripgrep-11.0.1-x86_64-pc-windows-gnu.zip
05/12/2019  02:08 PM         1,767,433 ripgrep-11.0.1-x86_64-pc-windows-msvc.zip
12/20/2014  07:40 PM             4,096 timer32.exe

C:\grep_vs_Kazahana>GREP_fast.bat "Sherlock " "OpenSubtitle_corpus_en_2016_(337,845,355_lines).txt"

C:\grep_vs_Kazahana>timer32.exe "Kazahana_Monad_GCC_472.exe" "Sherlock " "OpenSubtitle_corpus_en_2016_(337,845,355_lines).txt" 520123
Kazahana, a superfast exact & wildcards & Levenshtein Distance (Wagner-Fischer) searcher, r. 1-++fix+nowait_critical_nixFIX_Wolfram+fixITER+EX+CS_fix_DEFINE, copyleft Kaze 2014-Dec-04.
Pattern: Sherlock
Enforcing MONAD i.e. single-thread ...
Allocating Master-Buffer 520123KB ... OK
/; Speed: 00,000,152,320 bytes/clock; Traversed: 9,586,906,622 bytes
Kazahana: Dumped xgrams: 6,722
Kazahana: Performance: 148 KB/clock
Kazahana: Performance: Total/fread() clocks: 65,736/60,047
Kazahana: Performance: I/O time, i.e. fread() time, is 91 percents
Kazahana: Done.

Kernel  Time =    14.421 =   21%
User    Time =     3.953 =    5%
Process Time =    18.375 =   27%    Virtual  Memory =    511 MB
Global  Time =    66.625 =  100%    Physical Memory =    513 MB

C:\grep_vs_Kazahana>set LC_ALL=C

C:\grep_vs_Kazahana>timer32.exe grep.exe -F -c "Sherlock " "OpenSubtitle_corpus_en_2016_(337,845,355_lines).txt"
6722

Kernel  Time =    11.296 =   11%
User    Time =    25.421 =   26%
Process Time =    36.718 =   38%    Virtual  Memory =      1 MB
Global  Time =    95.578 =  100%    Physical Memory =      2 MB

C:\grep_vs_Kazahana>
```

My wish is to tweak (and if necessary to write some manual vectorization in the weakest chain link - the splitting of buffer to 16 chunks) the 2014 edition in order to run faster.

---

_Label `duplicate` added by @BurntSushi on 2019-05-13 21:54_

---

_Comment by @BurntSushi on 2019-05-13 22:10_

Sorry, but I don't know what Kazahana is. See [here](https://github.com/search?q=Kazahana) and [here](https://www.google.com/search?q=Kazahana+site%3Agithub.com) and [here](https://www.google.com/search?q=Kazahana+grep+search+tool).

And honestly, I find a lot of your comments here and on the linux questions forum pretty incoherent. :-/ I don't know what "destroy the pace of order 1 and order 2 BMH variants" means. I don't know what "write some manual vectorization in the weakest chain link" means. And I don't know what "the 2014 edition" is.

If you want me to do something, **then please lay out clear and concise instructions on how to do it for a Linux machine.**

> Don't get why '6698', Kazahana's and grep's reported '6722':

It looks like my (old) copy of open subtitles from 2016 is different from the one that is now offered for download:

```
$ curl -LO 'https://object.pouta.csc.fi/OPUS-OpenSubtitles/v2016/mono/en.txt.gz'
$ gunzip en.txt.gz
$ grep 'Sherlock ' en.txt| wc -l
6722
$ rg 'Sherlock ' en.txt| wc -l
6722
```

So a count of 6722 is correct. This is unfortunate because it means the benchmark suite probably needs to be updated now too. Unless I host the old files somewhere.

I'm going to close this because I'm not sure what you're after here, but if there's a concrete issue with ripgrep, then we can re-open it. Fixing the benchmark suite is already tracked by #1257.

---

_Closed by @BurntSushi on 2019-05-13 22:10_

---

_Comment by @Sanmayce on 2019-05-14 00:22_

My feedback is not an issue, just me suggesting to test how fast your ripgrep is with your corpus relatively to my C tool.

>Sorry, but I don't know what Kazahana is.

It is a console tool searching with exact/fuzzy/wildcard matching. The full source:
https://gist.githubusercontent.com/Sanmayce/2e8a25eb4949163f5d82d6d511f6e3fc/raw/ccc83e4b2643fee977301213ac3d6d51d4396428/Kazahana_r1-++fix+nowait_critical_nixFIX_WolfRAM+fixITER+EX+CS_fix_DEFINE.c

>I don't know what "destroy the pace of order 1 and order 2 BMH variants" means. 
>And I don't know what "the 2014 edition" is.

Orders 1/2/"4" of Boyer-Moore-Horspool are benchmarked here:
https://www.codeproject.com/Articles/683665/Fastest-Exact-Fuzzy-Wildcard-Full-text-Searcher

>And honestly, I find a lot of your comments here and on the linux questions forum pretty incoherent. :-/  

Incoherent you say, something tells me you used an euphemism for that one, heh-heh.

>If you want me to do something, then please lay out clear and concise instructions on how to do it for a Linux machine.

Thanks, that's what is interesting - benchmarking Rust vs C code - with different approaches taken.
When you used the word 'fastest', I thought you may also consider checking another "fastest" tool.
My word is for literal patterns - exact searching, that is. My wish is to locate what unoptimized sections are there for my buffer splitting/management by seeing how Kazahana fares against the fastest grep!
As far as I see, ripgrep has a nice blog and github presentation - the only thing that got me excited is to see whether it is faster than Kazahana in exact matching, for that purpose I conducted a quick benchmark:

Five executables were compared:
```
                                                            Global Time 
- Kazahana_Monad_GCC_472_32bit (single-threaded)                 6.446s
- grep-2.5.4                                                    20.168s
- Kazahana_Hexadecad_GCC_472_32bit (16 threaded)                 4.669s
- ripgrep-11.0.1-x86_64-pc-windows-gnu                           4.776s
- Kazahana_HEXADECAD-Threads_IntelV15_64bit (16 threaded)        4.625s
```

The full console dump:
```
F:\grep_vs_Kazahana>DIR

12/20/2014  20:40           451,824 grep-2.5.4-bin.zip
12/20/2014  20:40           898,241 grep-2.5.4-dep.zip
12/20/2014  20:40         1,361,303 grep-2.5.4-src.zip
12/20/2014  20:40            96,256 grep.exe
12/20/2014  20:40         1,008,128 libiconv2.dll
12/20/2014  20:40           103,424 libintl3.dll
12/20/2014  20:40           140,288 pcre3.dll
12/20/2014  20:40            79,360 regex2.dll

05/13/2019  06:42           278,041 Kazahana_Hexadecad_GCC_472.exe
06/11/2016  15:11            94,300 pthreadGC2.dll

05/13/2019  06:42           260,419 Kazahana_Monad_GCC_472.exe

12/20/2014  20:40         1,005,430 Kazahana_r1-++fix+nowait_critical_nixFIX_WolfRAM+fixITER+EX+CS_fix_DEFINE.c
05/14/2019  03:04           219,648 Kazahana_r1-++fix+nowait_critical_nixFIX_WolfRAM+fixITER+EX+CS_fix_DEFINE_HEXADECAD-Threads_IntelV15_Qparallel_64bit.exe
07/27/2014  09:33         1,114,552 libiomp5md.dll

10/07/2017  14:33    13,113,340,782 OpenSubtitle_corpus_en_2018_(441,450,449_lines_FROM_446,612_files).txt

04/16/2019  20:20        27,519,247 ripgrep-11.0.1-x86_64-pc-windows-gnu.exe
12/20/2014  20:40             4,096 timer32.exe

F:\grep_vs_Kazahana>timer32.exe "Kazahana_Monad_GCC_472.exe" "Sherlock Holmes" "OpenSubtitle_corpus_en_2018_(441,450,449_lines_FROM_446,612_files).txt" 520123
Kazahana, a superfast exact & wildcards & Levenshtein Distance (Wagner-Fischer) searcher, r. 1-++fix+nowait_critical_nixFIX_Wolfram+fixITER+EX+CS_fix_DEFINE, copyleft Kaze 2014-Dec-04.
Pattern: Sherlock Holmes
Enforcing MONAD i.e. single-thread ...
Allocating Master-Buffer 520123KB ... OK
\; Speed: 00,002,390,153 bytes/clock; Traversed: 12,782,542,567 bytes
Kazahana: Dumped xgrams: 7,673
Kazahana: Performance: 2,303 KB/clock
Kazahana: Performance: Total/fread() clocks: 5,560/3,727
Kazahana: Performance: I/O time, i.e. fread() time, is 67 percents
Kazahana: Done.

Kernel  Time =     3.843 =   59%
User    Time =     2.640 =   40%
Process Time =     6.484 =  100%    Virtual  Memory =    512 MB
Global  Time =     6.446 =  100%    Physical Memory =    513 MB

F:\grep_vs_Kazahana>set LC_ALL=C

F:\grep_vs_Kazahana>timer32.exe grep.exe -F -c "Sherlock Holmes" "OpenSubtitle_corpus_en_2018_(441,450,449_lines_FROM_446,612_files).txt"
7673

Kernel  Time =     3.312 =   16%
User    Time =    16.859 =   83%
Process Time =    20.171 =  100%    Virtual  Memory =      2 MB
Global  Time =    20.168 =  100%    Physical Memory =      5 MB

F:\grep_vs_Kazahana>timer32.exe "Kazahana_Hexadecad_GCC_472.exe" "Sherlock Holmes" "OpenSubtitle_corpus_en_2018_(441,450,449_lines_FROM_446,612_files).txt" 520123
Kazahana, a superfast exact & wildcards & Levenshtein Distance (Wagner-Fischer) searcher, r. 1-++fix+nowait_critical_nixFIX_Wolfram+fixITER+EX+CS_fix_DEFINE, copyleft Kaze 2014-Dec-04.
Pattern: Sherlock Holmes
omp_get_num_procs() = 8
omp_get_max_threads() = 8
omp_get_thread_limit() = 2147483647
omp_get_max_active_levels() = 2147483647
Enforcing HEXADECAD i.e. hexadecuple-threads ...
Allocating Master-Buffer 520123KB ... OK
\; Speed: 00,002,986,575 bytes/clock; Traversed: 12,782,542,567 bytes
Kazahana: Dumped xgrams: 7,673
Kazahana: Performance: 2,879 KB/clock
Kazahana: Performance: Total/fread() clocks: 4,447/3,678
Kazahana: Performance: I/O time, i.e. fread() time, is 82 percents
Kazahana: Done.

Kernel  Time =     3.875 =   82%
User    Time =     4.875 =  104%
Process Time =     8.750 =  187%    Virtual  Memory =    513 MB
Global  Time =     4.669 =  100%    Physical Memory =    514 MB

F:\grep_vs_Kazahana>timer32.exe "ripgrep-11.0.1-x86_64-pc-windows-gnu.exe" -c "Sherlock Holmes" "OpenSubtitle_corpus_en_2018_(441,450,449_lines_FROM_446,612_files).txt"
7673

Kernel  Time =     2.531 =   52%
User    Time =     2.250 =   47%
Process Time =     4.781 =  100%    Virtual  Memory =     26 MB
Global  Time =     4.776 =  100%    Physical Memory =   4096 MB

F:\grep_vs_Kazahana>timer32.exe "Kazahana_r1-++fix+nowait_critical_nixFIX_WolfRAM+fixITER+EX+CS_fix_DEFINE_HEXADECAD-Threads_IntelV15_Qparallel_64bit.exe" "Sherlock Holmes" "OpenSubtitle_corpus_en_2018_(441,450,449_lines_FROM_446,612_files).txt" 520123
Kazahana, a superfast exact & wildcards & Levenshtein Distance (Wagner-Fischer) searcher, r. 1-++fix+nowait_critical_nixFIX_Wolfram+fixITER+EX+CS_fix_DEFINE, copyleft Kaze 2014-Dec-04.
Pattern: Sherlock Holmes
omp_get_num_procs() = 8
omp_get_max_threads() = 8
omp_get_thread_limit() = 32768
omp_get_max_active_levels() = 2147483647
Enforcing HEXADECAD i.e. hexadecuple-threads ...
Allocating Master-Buffer 1023KB ... OK
/; Speed: 00,003,075,271 bytes/clock; Traversed: 13,112,959,534 bytes
Kazahana: Dumped xgrams: 7,673
Kazahana: Performance: 3,002 KB/clock
Kazahana: Performance: Total/fread() clocks: 4,265/3,738
Kazahana: Performance: I/O time, i.e. fread() time, is 87 percents
Kazahana: Performance: RDTSC I/O time, i.e. fread() time, is 8,945,072,287 ticks
Kazahana: Done.

Kernel  Time =     7.890 =  170%
User    Time =    23.593 =  510%
Process Time =    31.484 =  680%    Virtual  Memory =      7 MB
Global  Time =     4.625 =  100%    Physical Memory =      7 MB

F:\grep_vs_Kazahana>
```

The testmachine was a laptop running Windows 10, CPU i7-3630QM Nominal: @2.40 GHz Turbo: @3.40 GHz, 6MB Cache, 16GB DDR3 @800MHz.

The file is 12.2 GB long, thus 16GB RAM can house it for cached traversals:
http://opus.nlpl.eu/download.php?f=OpenSubtitles/v2018/mono/OpenSubtitles.raw.en.gz

---

_Comment by @BurntSushi on 2019-05-14 00:52_

Sorry, but I'm not going anywhere near a single 22,000+ line C file with build instructions embedded in comments for Windows-only. Moreover, tools that I benchmark have to have code that I can actually read. GNU grep barely qualifies, but---and I don't want to be mean, but you came to my issue tracker asking me to do something for you---your code is one big giant ball of spaghetti. It's just not worth spending my time on. Sorry.

I'll say this one last time: I'd be happy to do some light benchmarking under the following conditions:

1. I can do it on Linux.
2. The code is readable.
3. There is end user documentation describing its feature set so that a proper apples-to-apples comparison can be done.

---

_Renamed from "Link broken" to "request to benchmark Kazahana against ripgrep" by @BurntSushi on 2019-05-28 12:14_

---

_Label `duplicate` removed by @BurntSushi on 2019-05-28 12:14_

---

_Label `invalid` added by @BurntSushi on 2019-05-28 12:14_

---

_Comment by @Sanmayce on 2020-10-16 20:24_

Hi,
feel free to use the vector variant/ideas of **Railgun** in your projects, it is 100% free.
It would be nice for us to see how **Railgun**  written in Rust with different tunings/heuristics would behave.

Fastest GREP vs NyoTengu

The latest exact-search showdown, the testmachine: laptop running Windows 10, i5-7200U @2.5GHz, 8GB DDR4 2133MHz:

```
Haystack: linux-5.8.5.tar (983,992,320 bytes)
Needle:   "Linus Torvalds"
-------------------------------------------------------------------------------------------------------------------------------------------------
| Searcher                                                                               |                   Performance; Search Time | Matches |
-------------------------------------------------------------------------------------------------------------------------------------------------
| Nyotengu_YMM_IntelV150_64bit.exe linux-5.8.5.tar needle1.txt                           | 11,714,194,285 bytes/second; 0.084 seconds |     602 |
| NyoTengu_YMM_GCC730_64bit.exe linux-5.8.5.tar needle1.txt                              | 11,056,093,483 bytes/second; N.A.          |     602 |
| "ripgrep-12.1.1-x86_64-pc-windows-gnu.exe" -c --stats "Linus Torvalds" linux-5.8.5.tar |                        N.A.; 0.261 seconds |     602 |
-------------------------------------------------------------------------------------------------------------------------------------------------

Haystack: linux-5.8.5.tar (983,992,320 bytes)
Needle:   "5.8.5"
-------------------------------------------------------------------------------------------------------------------------------------------------
| Searcher                                                                               |                   Performance; Search Time | Matches |
-------------------------------------------------------------------------------------------------------------------------------------------------
| Nyotengu_YMM_IntelV150_64bit.exe linux-5.8.5.tar needle2.txt                           |  9,742,498,217 bytes/second; 0.101 seconds |   74028 |
| NyoTengu_YMM_GCC730_64bit.exe linux-5.8.5.tar needle2.txt                              |  9,461,464,615 bytes/second; N.A.          |   74028 |
| "ripgrep-12.1.1-x86_64-pc-windows-gnu.exe" -c --stats "5.8.5" linux-5.8.5.tar          |                        N.A.; 0.418 seconds |   74586 |
-------------------------------------------------------------------------------------------------------------------------------------------------

Note: No idea why ripgrep reports more hits, Windows' FIND.EXE reports 74028 as well?!

Haystack: enwiki-20201001-all-titles (1,193,068,592 bytes)
Needle:   "grep"
-------------------------------------------------------------------------------------------------------------------------------------------------
| Searcher                                                                               |                   Performance; Search Time | Matches |
-------------------------------------------------------------------------------------------------------------------------------------------------
| Nyotengu_YMM_IntelV150_64bit.exe enwiki-20201001-all-titles grep.txt                   | 12,174,169,306 bytes/second; 0.098 seconds |     173 |
| NyoTengu_YMM_GCC730_64bit.exe enwiki-20201001-all-titles grep.txt                      | 11,471,813,384 bytes/second; N.A.          |     173 |
| "ripgrep-12.1.1-x86_64-pc-windows-gnu.exe" -c --stats "grep" enwiki-20201001-all-titles|                        N.A.; 0.381 seconds |     173 |
-------------------------------------------------------------------------------------------------------------------------------------------------

Haystack: SCB_DNA-Genome-Homo-sapiens-GCA_000001405.28-2019-02-28 (3,353,989,743 bytes)
Needle:   "ACGT"
-------------------------------------------------------------------------------------------------------------------------------------------------
| Searcher                                                                               |                   Performance; Search Time | Matches |
-------------------------------------------------------------------------------------------------------------------------------------------------
| Nyotengu_YMM_IntelV150_64bit.exe "SCB_DNA...-2019-02-28" 4.txt                         |  3,265,812,797 bytes/second; 1.027 seconds | 1607850 |
| NyoTengu_YMM_GCC730_64bit.exe "SCB_DNA...-2019-02-28" 4.txt                            |  4,245,556,636 bytes/second; N.A.          | 1607850 |
| "ripgrep-12.1.1-x86_64-pc-windows-gnu.exe" -c --stats "ACGT" "SCB_DNA...-2019-02-28"   |                        N.A.; 5.835 seconds | 1607850 |
-------------------------------------------------------------------------------------------------------------------------------------------------

Haystack: SCB_DNA-Genome-Homo-sapiens-GCA_000001405.28-2019-02-28 (3,353,989,743 bytes)
Needle:   "AACCGGTT"
-------------------------------------------------------------------------------------------------------------------------------------------------
| Searcher                                                                               |                   Performance; Search Time | Matches |
-------------------------------------------------------------------------------------------------------------------------------------------------
| Nyotengu_YMM_IntelV150_64bit.exe "SCB_DNA...-2019-02-28" 8.txt                         |  2,173,680,974 bytes/second; 1.543 seconds |    2723 |
| NyoTengu_YMM_GCC730_64bit.exe "SCB_DNA...-2019-02-28" 8.txt                            |  2,003,578,102 bytes/second; N.A.          |    2723 |
| "ripgrep-12.1.1-x86_64-pc-windows-gnu.exe" -c --stats "AACCGGTT" "SCB_...-2019-02-28"  |                        N.A.; 5.313 seconds |    2723 |
-------------------------------------------------------------------------------------------------------------------------------------------------
```
    

As always, benchmarking is instrumental in bettering functions compilerwise, bugfixingwise, needlelengthwise...

In order to reproduce the benchmark, here is the package (except the three corpora):
www.sanmayce.com/Railgun/Benchmark_SRC-(Linus-Torvalds)_DNA-(HUMAN-GENOME)_TXT-(enwiki-all-titles)_unfinished_Nyotengu.zip

The three corpora are downloadable outside the package:

https://www.kernel.org/pub/linux/kernel/v5.x/linux-5.8.5.tar.xz
ftp://ftp.ncbi.nlm.nih.gov/genomes/all/GCA/000/001/405/GCA_000001405.28_GRCh38.p13/GCA_000001405.28_GRCh38.p13_genomic.fna.gz
https://dumps.wikimedia.org/enwiki/20201001/enwiki-20201001-all-titles.gz


---

_Comment by @BurntSushi on 2020-10-17 01:41_

@Sanmayce Thanks! I appreciate the zip. But as I said above, I'm really only willing to benchmark tools that work on Linux. I do have a Windows machine I could use to try your benchmark, but I don't know when I'll get around to it. Basically, if you take a look at the other tools I've benchmarked, they all enjoy cross platform support, some form of documentation and some easy way to build the tool _from source on Linux_.

With that said, I did have a look at some things.

First and foremost, it looks like you're trying to benchmark a substring search algorithm against a grep tool. It's not completely wrong to do that, but particularly in high match count benchmarks, it's not hard to beat even the fastest grep tool straight up because of match overhead. A fully featured grep tool is likely doing extra work per match than something that is just a simple substring search. And in particular, your tool doesn't seem to provide regex searching, which means it doesn't really make much sense to compare it with ripgrep or grep tools. All grep tools vary a bit in what features they provide, but I'd say regex searching is really necessary to be worth an in depth comparison.

Secondly, your benchmark on `linux-5.8.5.tar` is a little weird for two reasons. Firstly, it's a tar file that contains NUL bytes. grep tools, including ripgrep, tend to do some kind of binary detection because 1) they're primarily made for searching plain text files and 2) it's good to avoid dumping binary data to a Unix terminal, which could otherwise produce undesirable effects. In grep tools, you can typically disable this special binary detection with the `-a/--text` flag. For example, with GNU grep:

```
$ grep 5.8.5 linux-5.8.5.tar -F -c
74028

$ grep 5.8.5 linux-5.8.5.tar -F -c -a
69296
```

And for ripgrep:

```
$ rg 5.8.5 linux-5.8.5.tar -F -c --no-mmap
74028
$ rg 5.8.5 linux-5.8.5.tar -F -c -a
69296
```

(In ripgrep's case, the `--no-mmap` flag is given in the first example because when memory map searching is used (which is the default on Linux at least), the binary detection heuristic is partially disabled.)

The reason why the match counts differ here is quite subtle. In particular, when a file is detected as binary, both ripgrep and GNU grep will ["zap" the NUL bytes](https://github.com/BurntSushi/ripgrep/blob/6301e20ee460d11e0d81e2efacd8c52d2e231c0f/crates/searcher/src/line_buffer.rs#L520-L540) in the file by [converting them to line breaks](https://docs.rs/grep-searcher/0.1.7/grep_searcher/struct.BinaryDetection.html#method.convert). The reason for this is that binary files may lack the periodic line breaks commonly found in plain text files, which can result in very long lines that use a lot of memory. In other words, this replacement is a heuristic to avoid exorbitant memory usage. _But_, all of this only occurs when memory maps aren't used. On Windows, `rg foo some-file` should use memory maps by default.

So why does replacing NUL bytes with `\n` change the count? Well, the `-c` flag in grep tools counts the number of _lines_ that match, _not_ the total number of occurrences.

Secondly, as I said above, you're comparing one tool that only does substring search with another tool that does regex searching. If you wave your hands a bit, you can consider this a useful comparison, but only if you make sure you aren't feeding a regex to the tool that supports regexes. In this case. `.` is a regex meta character that means "match any character." So you're not benchmarking equivalent work-loads _and_ you might get different results:

```
$ time rg '5.8.5' linux-5.8.5.tar -c
69813

real    0.339
user    0.285
sys     0.054
maxmem  943 MB
faults  0

$ time rg '5\.8\.5' linux-5.8.5.tar -c
69296

real    0.189
user    0.119
sys     0.069
maxmem  943 MB
faults  0
```

As you can see, both the timing and counts are different for obvious reasons. My best guess is that your tool is counting all individual matches, _not_ the number of lines that match. (So in this case, your tool is arguably doing more work than ripgrep would. _Except_, you're passing the `--stats` flag to ripgrep, which forces it to count individual matches _in addition_ to the number of matching lines.) For example:

```
$ rg '5.8.5' linux-5.8.5.tar -c -F
69296

$ rg '5.8.5' linux-5.8.5.tar -c -F --stats
69296

74028 matches
69296 matched lines
1 files contained matches
1 files searched
0 bytes printed
17 bytes searched
0.159813 seconds spent searching
0.183953 seconds

$ rg '5.8.5' linux-5.8.5.tar -c -F --only-matching
74028
```

In the second example, I gave the `--stats` flag, which shows that there are 69,296 matching lines and 74,028 individual matches. Similarly, passing the `--only-matching` flag in addition to the `-c` flag will report the same number of individual matches. (The `-o/--only-matching` flag finds each individual match.) These are the correct results, as your tool, findstr and GNU grep all report. The reason why you got different results was because `5.8.5` is a regex and the `-F` flag I used above forces ripgrep to interpret it as a literal, not a regex. If I let it do regex searching, then:

```
$ rg '5.8.5' linux-5.8.5.tar -c --stats
69813

74586 matches
69813 matched lines
1 files contained matches
1 files searched
0 bytes printed
17 bytes searched
0.312672 seconds spent searching
0.332566 seconds
```

Which reproduces your results with 74,586 total matches.

If you want to continue benchmarking against ripgrep, I'd advise choosing a corpus that isn't binary data. If you want the Linux kernel, then concatenate all of its C files for example.

-----

As should be clear, benchmarking grep tools is an incredibly subtle endeavor. _This_ is why I have what might appear to you as such stringent requirements for doing a comparison. It takes a ton of work---usually including reading the code and of course the docs as well---to ensure that an appropriate and apples-to-apples comparison is being made. Everything about your tool makes this difficult for me:

* It's not really a grep tool. It's a toy wrapper around a substring algorithm.
* I apologize since this may sound harsh, but the code is almost impossible for me to read. It's inconsistently formatted. There's mixed tabs and spaces everywhere (and not in a good way). Almost none of the functions have well documented contracts. There's magic numbers everywhere. There's commented out code everywhere. It's just a mess. Reading the code is important so that I can check that tools are doing roughly equivalent work. Otherwise, there may be important caveats to how performance is achieved. (While this doesn't apply to your case, one common differentiating factor among grep tools that impacts both correctness and performance is Unicode support in the regex engine.)
* There's zero end user documentation to explain what its intended behavior is, what its CLI flags are and so on.
* This one is partially on me, but it looks like it's a Windows-only tool. I am not a Windows user, and given the amount of analysis I need to do in order to ensure a fair comparison, it's hard to do in an environment I'm not familiar or comfortable with.

All of this combined makes it exceedingly difficult to do the analysis necessary for a proper comparison. And I'm not just picking on you. Go read my [blog post introducing ripgrep](https://blog.burntsushi.net/ripgrep/). It's long and detailed and includes in depth analysis for every benchmark. I went out and _read the source code_ of the tools I was comparing in order to write that article. It took me weeks. My requirements aren't vacuous attempts to shrug you off. This is really what I've done in the past with other tools.

Finally, I'd like to close on a note about substring algorithms, since that seems to be where your focus lies. ripgrep uses a [very simple heuristic substring search](https://github.com/rust-lang/regex/blob/a7ef5f452ec1dcb856fae99d56c6db08bf23d1ff/src/literal/imp.rs#L369-L379) that greatly improves the common cases by selling out the less common cases. It does this by trying to guess a byte in the needle that _probably_ occurs infrequently in the haystack according to background frequencies. It then feeds this byte to `memchr`, which, as you know, [is vectorized](https://github.com/BurntSushi/rust-memchr/blob/10f11fbeaaec9d1c892be30718604a09b6622e8e/src/x86/avx.rs). For example:

```
$ time rg -a -c quartz linux-5.8.5.tar
40

real    0.160
user    0.096
sys     0.063
maxmem  944 MB
faults  0

$ time rg -a -c sternness linux-5.8.5.tar

real    0.381
user    0.323
sys     0.057
maxmem  944 MB
faults  0
```

The match counts in both of these cases are low enough where the match overhead doesn't matter, yet, searching for `quartz` is over two times faster than searching for `sternness`. That's because `quartz` has some very infrequently occurring letters in it where as `sternness` only contains very common letters. In this case, that guess holds, which means searching for `quartz` is going to spend a lot more time in `memchr` than `sternness` is, and thus, it will be much faster.

So if you're going to benchmark substring algorithms, then I would advise you to do your _own_ analysis. It's not just about the timing. Educate people. Tell them what's going on. Do that by reading the code you're benchmarking. That's what I do. Because reading the code will help inform which kinds of benchmarks to run. And I'd advise you to simplify your benchmark. You don't need to compare with ripgrep. That's a grep tool. Not a thin wrapper around a substring algorithm. Instead, write a short Rust program that uses the `regex` crate to do the search.

---

_Comment by @Sanmayce on 2020-10-17 07:56_

@BurntSushi 
Many thanks for the detailed explanations, nice indeed.

Being a toymaker makes me look weird, I do know that, but that's my style, love digging in small etudes and benchmarking them.

> I'm really only willing to benchmark tools that work on Linux. 

I get it, for many reasons I stick with Windows, my *nix experience is very limited, in fact e.g. I stumbled in basic tasks even as concatenating files, took my whole day to come up with the *nix counterpart:
https://www.linuxquestions.org/questions/linux-newbie-8/equivalent-to-windows-batch-file-4175656692/#post6010931

> ... they all enjoy cross platform support, some form of documentation and some easy way to build the tool from source on Linux.

I admit, my console tools fail to provide the "existential" minimum, but after all, my focus is on performance and sharing blueprints, writing a complete (in all departments) utility is left for well experienced coders, not me.

>A fully featured grep tool is likely doing extra work per match than something that is just a simple substring search. 

Totally, comparing grep tools against my non-regex tools is partially NOT FAIR, but I have no other tools to compare against, your RIPGREP is the best match.
Also, this inequality prompts for seeing in two directions, making a sub-tool, or rather codepath, (within grep) dealing with exact searches in high-speed manner, I would like to see a CLI tool allowing preliminary (as mere matches counting) functionality utilizing AVX2 and AVX512.
It is not good looking to have powerful tool failing to do very basic tasks fast, I mean this is an overlooked matter, one superior tool in some moment will be forced to report something and will suffer from being loaded with unrelated work i.e. the overhead.
This applies to my Kazahana search tool, compared to NyoTengu. The latter being light-weight doing one only thing, but doing it brutally fast. So, the trade-off, functionality vs performance, both still not reached, nor the balance.

> And in particular, your tool doesn't seem to provide regex searching, which means it doesn't really make much sense to compare it with ripgrep or grep tools. 

Ugh, I totally forgot about that -F option, my fault, will include it next time.

>If you want to continue benchmarking against ripgrep, I'd advise choosing a corpus that isn't binary data. If you want the Linux kernel, then concatenate all of its C files for example.

I chose the kernel by accident, not on purpose, but now it became a test with its own thread, in addition I would like to show Linus that his AVX512 bashing has other side, finding his name in his work in fastest way possible is not ... a paper ring, as Johnny Cash sings:

Being where
Love's a small word
A part-time thing
A paper ring

Speed is not a small word, to me, even Linus is blinded sometimes, the Japanese vector supercomputers are dumb enough to use 16384bit vectors.

>This is why I have what might appear to you as such stringent requirements for doing a comparison.

Just now I realize the showdown is impossible, yet we need some general speed rosters, harshness doesn't concern me, as long as we walk towards enhancing all in our unrefined approaches. Therefore thank you once more, discussions and showdowns are so funny and instrumental.

>It's not really a grep tool. It's a toy wrapper around a substring algorithm.

Indeed, it serves the role of a skeleton.

>I apologize since this may sound harsh, but the code is almost impossible for me to read. It's inconsistently formatted. There's mixed tabs and spaces everywhere (and not in a good way). Almost none of the functions have well documented contracts. There's magic numbers everywhere. There's commented out code everywhere. It's just a mess. 

That's me, I like my sources to be full with unfinished/commented/abandoned fragments - to remind me the function is imperfect and most importantly on the way of artistic beauty - it is the blueprint of something beyond the code itself - touching Zen here.

I truly love the this notion (in Ancient China there was a saying that emperors considered themselves lonely, wretched, abandoned, good for nothing orphans):
侘しく 寂しく しくじり ばかりのこの私に
"For me, a wretched lonely failure"
(note, the words “wretched” and “lonely” are also used in the term 侘寂 “wabi sabi”, a Japanese artistic aesthetic that finds beauty in imperfect and broken things, so they aren’t necessarily so negative here)

From the superb videoclip "Ningen Isu - Heartless Scat" on YouTube.

This is important - the driving force behind my activity - to reach to touch/feel true things while being aware of faultiness, a constant reminder, that is.

>This one is partially on me, but it looks like it's a Windows-only tool.

This line should generate ELF:

```
gcc -O3 -mavx2 -fomit-frame-pointer NyoTengu.c -o NyoTengu_YMM_GCC_64bit.elf -DYMMtengu -D_POSIX_ENVIRONMENT_
```

>Go read my blog post introducing ripgrep. It's long and detailed and includes in depth analysis for every benchmark. 

Likey, will read it.

>It then feeds this byte to memchr, which, as you know, is vectorized. 

Since early x86 days where SCASB dominated, till our times I see this prefix searching as an unexploited enough and surely unoptimized matter, therefore I elevated order from 1 to 4 - instead of BYTE enforcing DWORD, in a way simulating the unexisting SCASDW.

>So if you're going to benchmark substring algorithms, then I would advise you to do your own analysis. It's not just about the timing. Educate people. Tell them what's going on. Do that by reading the code you're benchmarking. That's what I do. Because reading the code will help inform which kinds of benchmarks to run. And I'd advise you to simplify your benchmark. You don't need to compare with ripgrep. That's a grep tool. Not a thin wrapper around a substring algorithm. 

Right, sharing insights and the experience is something that gladdens me, everytime, I remember how impactful (for life) was the moment when I was reading a column (some author shared a TurboPascal doing 400% faster searches than Assembly) in a old computer magazine, such etudes/articles awake the need for speed/craftsmanship. Still have it somewhere photocopied.

>Instead, write a short Rust program that uses the regex crate to do the search.

I know next to nothing about Rust and regex, will leave that area to people like you having expertise, my cup of tea is exact/wildcard/fuzzy in C.

I am a chatterbox, will stop here, when I write something new, will share it. Looking forward to seeing brutally speedy TEXT traversing CLI tool...

I have a habit saluting people with musical etudes, our Greek neighbors did the masterpiece - VOIDNAUT - Control (Official Lyric Video) album NADIR:
https://www.youtube.com/watch?v=TWYmkI7bLDw

---

_Comment by @Sanmayce on 2020-10-24 17:42_

>So if you're going to benchmark substring algorithms, then I would advise you to do your own analysis. It's not just about the timing. Educate people. Tell them what's going on. 

Sure, still didn't explain so many simple things, primarily wanna give other coders a toy to experiment with, therefore shared a concise article putting NyoTengu in Public Domain:
https://www.codeproject.com/Articles/5282980/Fastest-Fulltext-Vector-Scalar-Exact-Searcher?msg=5757799#xx5757799xx

Just saw discrepancy in matches counts, can you elaborate again:

```
05/16/2018  05:41 PM            95,665 NyoTengu_YMM_GCC730_64bit.exe
10/16/2020  12:32 PM           110,592 Nyotengu_YMM_IntelV150_64bit.exe
10/24/2020  11:02 AM     1,000,000,002 pi-billion.txt

D:\>timer64 Nyotengu_YMM_IntelV150_64bit.exe pi-billion.txt 999999.txt
NyoTengu a.k.a. 'SHETENGU' - the skydogess exact searcher, written by Kaze, 2020-Oct-10, for contacts: sanmayce@sanmayce.com
Needle = 999999
Current priority class is REALTIME_PRIORITY_CLASS.
Allocating Source-Buffer 1,000,000,002 bytes ... OK
Searching into Haystack (1,000,000,002) for all occurrences of Needle (6) with fastest (known to me) SCALAR memmem() - 'Railgun_Trolldom_64' ...
Hits: 973
Search took 0.177 seconds.
Pure Search Performance: 5,649,717,525 bytes/second.
Searching into Haystack (1,000,000,002) for all occurrences of Needle (6) with fastest (known to me) VECTOR memmem() - 'Railgun_Nyotengu_YMM' ...
Hits: 973
Search took 0.098 seconds.
Pure Search Performance: 10,204,081,653 bytes/second.

Kernel  Time =     0.515 =   69%
User    Time =     0.281 =   38%
Process Time =     0.796 =  108%    Virtual  Memory =    956 MB
Global  Time =     0.736 =  100%    Physical Memory =    956 MB

D:\>timer64 NyoTengu_YMM_GCC730_64bit.exe pi-billion.txt 999999.txt
NyoTengu a.k.a. 'SHETENGU' - the skydogess exact searcher, written by Kaze, 2020-Oct-10, for contacts: sanmayce@sanmayce.com
Needle = 999999
Current priority class is REALTIME_PRIORITY_CLASS.
Allocating Source-Buffer 1,000,000,002 bytes ... OK
Searching into Haystack (1,000,000,002) for all occurrences of Needle (6) with fastest (known to me) SCALAR memmem() - 'Railgun_Trolldom_64' ...
Hits: 973
Search took 0.191 seconds.
Pure Search Performance: 5,235,602,104 bytes/second.
Searching into Haystack (1,000,000,002) for all occurrences of Needle (6) with fastest (known to me) VECTOR memmem() - 'Railgun_Nyotengu_YMM' ...
Hits: 973
Search took 0.102 seconds.
Pure Search Performance: 9,803,921,588 bytes/second.

Kernel  Time =     0.500 =   69%
User    Time =     0.296 =   41%
Process Time =     0.796 =  110%    Virtual  Memory =    957 MB
Global  Time =     0.721 =  100%    Physical Memory =    956 MB

D:\>timer64.exe "ripgrep-12.1.1-x86_64-pc-windows-gnu.exe" -c -F --stats "999999" pi-billion.txt
1

885 matches
1 matched lines
1 files contained matches
1 files searched
0 bytes printed
1000000002 bytes searched
1.207929 seconds spent searching
1.269265 seconds

Kernel  Time =     0.093 =    7%
User    Time =     1.171 =   91%
Process Time =     1.265 =   98%    Virtual  Memory =      4 MB
Global  Time =     1.279 =  100%    Physical Memory =    960 MB
```




Also, I tried -a option, but no change in count.

---

_Comment by @BurntSushi on 2020-10-24 18:22_

Try running GNU grep as well. A count of `885` looks correct. ripgrep, GNU grep and ack all agree:

```
$ rg 999999 pi-billion.txt -o | wc -l
885
$ grep 999999 pi-billion.txt -o | wc -l
885
$ ack 999999 pi-billion.txt -o | wc -l
885
```

The `-o/--only-matching` option prints each match on its own line. Since the entire input is just one huge 1GB line (another very poor case for line oriented search tools like grep), you would otherwise just get a count of `1`. `wc -l` counts the number of lines, and thus, the total number of matches.

And no, I wouldn't expect `-a` to make a difference here because `pi-billion.txt` contains no NUL bytes:

```
$ rg -a --no-unicode '\x00' pi-billion.txt
$
```

---

_Comment by @Sanmayce on 2020-10-24 18:46_

Thanks, it is strange, don't know what is the actual reason!

I have to check whether some nasty bug is not lurking in my approaches...

---

_Comment by @Sanmayce on 2020-10-24 19:19_

I think greps are counting differently:

```
D:\Benchmark_SRC-(Linus-Torvalds)_DNA-(HUMAN-GENOME)_TXT-(enwiki-all-titles)_unfinished_Nyotengu>copy con bug.txt
9999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999^Z
        1 file(s) copied.

D:\Benchmark_SRC-(Linus-Torvalds)_DNA-(HUMAN-GENOME)_TXT-(enwiki-all-titles)_unfinished_Nyotengu>dir bug.txt

05/22/2018  11:23 PM               286 bug.txt

D:\Benchmark_SRC-(Linus-Torvalds)_DNA-(HUMAN-GENOME)_TXT-(enwiki-all-titles)_unfinished_Nyotengu>timer64 NyoTengu_YMM_GCC730_64bit.exe bug.txt 999999.txt
NyoTengu a.k.a. 'SHETENGU' - the skydogess exact searcher, written by Kaze, 2020-Oct-10, for contacts: sanmayce@sanmayce.com
Needle = 999999
Current priority class is REALTIME_PRIORITY_CLASS.
Allocating Source-Buffer 286 bytes ... OK
Searching into Haystack (286) for all occurrences of Needle (6) with fastest (known to me) SCALAR memmem() - 'Railgun_Trolldom_64' ...
Hits: 281
Search took 0.001 seconds.
Pure Search Performance: 286,000 bytes/second.
Searching into Haystack (286) for all occurrences of Needle (6) with fastest (known to me) VECTOR memmem() - 'Railgun_Nyotengu_YMM' ...
Hits: 281
Search took 0.001 seconds.
Pure Search Performance: 286,000 bytes/second.

Kernel  Time =     0.000 =    0%
User    Time =     0.000 =    0%
Process Time =     0.000 =    0%    Virtual  Memory =      1 MB
Global  Time =     0.011 =  100%    Physical Memory =      3 MB

D:\Benchmark_SRC-(Linus-Torvalds)_DNA-(HUMAN-GENOME)_TXT-(enwiki-all-titles)_unfinished_Nyotengu>timer64.exe "ripgrep-12.1.1-x86_64-pc-windows-gnu.exe" -c -F --stats "999999" bug.txt
1

47 matches
1 matched lines
1 files contained matches
1 files searched
0 bytes printed
286 bytes searched
0.000024 seconds spent searching
0.002494 seconds

Kernel  Time =     0.015 =  113%
User    Time =     0.031 =  226%
Process Time =     0.046 =  339%    Virtual  Memory =      2 MB
Global  Time =     0.013 =  100%    Physical Memory =      6 MB
```

Hmm, 286 has 286 - 6 + 1 = 281 order 6 Building-Blocks, as it should.

---

_Comment by @BurntSushi on 2020-10-24 20:00_

grep is correct. From a Python interpreter:

```
>>> string = '99999999999999999999999999999999999999999999999999999999999999999
9999999999999999999999999999999999999999999999999999999999999999999999999999999
9999999999999999999999999999999999999999999999999999999999999999999999999999999
999999999999999999999999999999999999999999999999999999999999999'
>>> string.count('999999')
47
```

You're probably counting overlapping matches, which makes sense, since the length of the string is 286, the length of the needle is 6 and `(286 - 6) + 1 = 281`.

You're welcome to define your semantics this way, but most substring iterators look for non-overlapping matches.

---

_Comment by @Sanmayce on 2020-10-24 20:04_

Aha, thank you, glad that I learned something. Since I a'm not used to *nix tools and know nothing about Python, resolved.

---

_Comment by @BurntSushi on 2020-10-24 20:38_

I don't think it's specific to nix tools. I've never seen substring iteration do anything other than non-overlapping search outside of specialized circumstances. Overlapping search is weird unless that's specifically what you want.

---

_Comment by @Sanmayce on 2020-10-24 20:53_

>Overlapping search is weird unless that's specifically what you want.

Not always, preliminary searches, in my view, have to report exactly that, just after that reporting the actual fragment/line may impose reducing the hits - for clarity. But, there is another more important aspect, there is (maybe only in my tools) exhaustive-fuzzy mode of searching that demands each and every position to be examined, it is so powerful, so NyoTengu's exact mode maybe should be called exhaustive exact - in order to distinguish them and avoid such mismatching reports, will mention this weirdness in the future.

---
