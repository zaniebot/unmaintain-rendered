```yaml
number: 2831
title: "Searching disk images for a text regex causes rg to be SIGKILL'd"
type: issue
state: closed
author: RecRanger
labels:
  - wontfix
assignees: []
created_at: 2024-06-04T16:20:17Z
updated_at: 2024-06-05T17:56:44Z
url: https://github.com/BurntSushi/ripgrep/issues/2831
synced_at: 2026-01-12T16:13:25Z
```

# Searching disk images for a text regex causes rg to be SIGKILL'd

---

_@RecRanger_

### Please tick this box to confirm you have reviewed the above.

- [X] I have a different issue.

### What version of ripgrep are you using?

ripgrep 14.1.0

features:-simd-accel,-pcre2
simd(compile):+SSE2,-SSSE3,-AVX2
simd(runtime):+SSE2,+SSSE3,-AVX2

PCRE2 is not available in this build of ripgrep.


### How did you install ripgrep?

Cargo

### What operating system are you using ripgrep on?

Linux Mint 21.3

### Describe your bug.

Running the following example on several TB of disk images fails.

### What are the steps to reproduce the behavior?

```
rg --byte-offset -uuu --text '[1-9A-H]{54}' .
fish: Job 1, 'rg --byte-offset -uuu --text '[…' terminated by signal SIGKILL (Forced quit)
```

### What is the actual behavior?

```
fish: Job 1, 'rg --byte-offset -uuu --text '[…' terminated by signal SIGKILL (Forced quit)
```

### What is the expected behavior?

It should work.

---

_Comment by @BurntSushi on 2024-06-04 16:40_

The SIGKILL is likely a symptom of your OS killing your process as a result of it using too much memory. In general, this is expected in some cases. It's documented in the man page:

```
ripgrep may use a large amount of memory depending on a few fac‐
tors.  Firstly,  if ripgrep uses parallelism for search (the de‐
fault), then the entire  output  for  each  individual  file  is
buffered into memory in order to prevent interleaving matches in
the  output. To avoid this, you can disable parallelism with the
-j1 flag. Secondly, ripgrep always needs to have at least a sin‐
gle line in memory in order to execute a search. A file  with  a
very  long  line  can thus cause ripgrep to use a lot of memory.
Generally, this only occurs when searching binary data with  the
-a/--text  flag enabled. (When the -a/--text flag isn't enabled,
ripgrep will replace all NUL bytes with line terminators,  which
typically  prevents exorbitant memory usage.) Thirdly, when rip‐
grep searches a large file using a memory map, the process  will
likely report its resident memory usage as the size of the file.
However,  this does not mean ripgrep actually needed to use that
much heap memory; the operating  system  will  generally  handle
this for you.
```

---

_Closed by @BurntSushi on 2024-06-04 16:40_

---

_Label `wontfix` added by @BurntSushi on 2024-06-04 16:40_

---

_Comment by @BurntSushi on 2024-06-04 16:43_

The key sentence there for your case is likely:

> Secondly, ripgrep always needs to have at least a single line in memory in order to execute a search.

Binary data can have very long runs of NUL bytes. Or just long runs of bytes that don't contain a line terminator.

greps are fundamentally line oriented tools. The data you are searching is likely not line oriented. From the first sentence of the README:

> ripgrep is a **line-oriented search tool**...

ripgrep makes an effort to work on data that isn't line oriented, but it is fundamentally not designed for it.

---

_Comment by @RecRanger on 2024-06-05 03:46_

`ugrep` supports searching these files.

I'm not sure how ripgrep aims to compare to ugrep, but currently it's less good at this, and there's a solution possible to fix this. I'd suggest that leaving this issue open, even if you don't intend to solve it in the near future, might be valuable. 

---

_Comment by @BurntSushi on 2024-06-05 04:21_

Without a reproduction to compare what precisely is happening, there's really not much more I can say. There are likely trade offs here that you aren't accounting for.

>  I'd suggest that leaving this issue open, even if you don't intend to solve it in the near future, might be valuable.

The issue isn't gone. It's just closed. If something changes, we can reopen it.

---

_Comment by @RecRanger on 2024-06-05 05:11_

It's exactly what you were describing with large strings of null bytes.

Here's somewhat of a repro:

#### This prepped file seems to work fine:
```bash
# 1 MiB of random data
dd if=/dev/urandom count=1024 bs=1024 >> ./binary_test_1.bin

# about 10 GB of zeros
dd status=progress if=/dev/zero count=10240000 bs=1024 >> ./binary_test_1.bin

echo "This is the text I'm searching for" >> ./binary_test_1.bin
```

#### This prepped file results in very high memory usage:

```bash
dd status=progress if=/dev/zero count=10240000 bs=1024 > ./binary_test_2.bin ; echo 'This is the text I am searching for' >> ./binary_test_2.bin ; dd if=/dev/zero count=10240000 bs=1024 status=progress >> ./binary_test_2.bin
```


#### Search the file:
```bash
rg -uuu 'searching for' ./binary_test_2.bin
rg -uuu --text --byte-offset --only-matching 'searching for' ./binary_test_2.bin
```


Use htop to see high memory usage while doing this. The usage grows steadily with time, all the way up to around 18G.

---

_Comment by @BurntSushi on 2024-06-05 12:55_

Yeah, exactly as I thought. You're looking at its behavior with respect to memory, but you aren't actually checking that the result is consistent.

I roughly copied your process but simplified things a little in places. Here's my `dd` command:

```
dd status=progress if=/dev/zero count=10240000 bs=1024 > ./binary-test.bin ; echo 'ZQZQZQZQZQ' >> ./binary-test.bin ; dd if=/dev/zero count=10240000 bs=1024 status=progress >> ./binary-test.bin
```

And now my grep commands:

```
$ time rg -a --no-mmap ZQZQZQZQZQ binary-test.bin > out.rg

real    12.564
user    2.550
sys     8.708
maxmem  11077 MB
faults  0

$ time LC_ALL=C grep -a ZQZQZQZQZQ binary-test.bin > out.grep

real    14.144
user    4.070
sys     10.062
maxmem  16647 MB
faults  0

$ time LC_ALL=C grep -a ZQZQZQZQZQ binary-test.bin > out.grep

real    15.539
user    4.040
sys     10.257
maxmem  16648 MB
faults  0
```

(I added `--no-mmap` to ripgrep and ugrep to constrain things a bit here, since using file backed memory maps will usually impact the max resident memory usage reported. This just makes sure we're comparing the same thing. We don't need to do this for GNU grep because it will never use file backed memory maps. It used to long ago, but not any more.)

Oh it must be that ugrep is doing something amazing here! No.... Let's check the output file:

```
$ l
total 40G
-rw-rw-r-- 1 andrew users  20G Jun  5 08:24 binary-test.bin
-rw-rw-r-- 1 andrew users 9.8G Jun  5 08:28 out.grep
-rw-rw-r-- 1 andrew users 9.8G Jun  5 08:26 out.rg
-rw-rw-r-- 1 andrew users 114K Jun  5 08:28 out.ugrep
```

Both GNU grep and ripgrep emit the correct result here. Namely, the input file has two ~10GB sized lines, by construction. The thing we're searching for is at the very end of the first line. Both GNU grep and ripgrep print that line as-is. ugrep... does not. It prints a truncate version of it. You still see the match, but you're missing a huge chunk of the first part of the line.

What ugrep does might be good enough for you, and that's fine and great. But silently truncating lines like this is not something ripgrep is going to do. I would rather ripgrep fail, because then at least you don't think you're getting the correct output.

With all that said, there is a flag that will help in this case, where ripgrep will treat all NUL bytes as line terminators. GNU grep supports it too:

```
$ time rg --null-data --no-mmap ZQZQZQZQZQ binary-test.bin > out.rg

real    2:00.24
user    1:57.95
sys     2.246
maxmem  19 MB
faults  0

$ time LC_ALL=C grep --null-data ZQZQZQZQZQ binary-test.bin > out.grep

real    6.202
user    4.160
sys     2.034
maxmem  19 MB
faults  0
```

There is a perf bug in ripgrep here (filed in #2832), but it does avoid the exorbitant memory usage. The search is just slow.

What about ugrep? Its README says:

> Ugrep is a true drop-in replacement for GNU grep

So the GNU grep command above should work if I swap out grep for ugrep right?

```
$ time ugrep-6.1.0 --null-data --no-mmap ZQZQZQZQZQ binary-test.bin > out.ugrep
ugrep: invalid option --null-data, did you mean --neg-regexp=, --not, --no-any-line, --no-ascii, --no-binary, --no-bool, --no-break, --no-byte-offset, --no-color, --no-config, --no-confirm, --no-decompress, --no-dereference, --no-dereference-files, --no-dotall, --no-empty, --no-filename, --no-filter, --no-glob-ignore-case, --no-group-separator, --no-heading, --no-hidden, --no-hyperlink, --no-ignore-binary, --no-ignore-case, --no-ignore-files --no-initial-tab, --no-invert-match, --no-line-number, --no-only-line-number, --no-only-matching, --no-messages, --no-mmap, --no-pager, --no-pretty, --no-smart-case, --no-sort, --no-split, --no-stats, --no-tree, --no-ungroup, --no-view or --null?
For more help on options, try `ugrep --help' or `ugrep --help WHAT'

$ ln -s ~/bin/x86_64/ugrep-6.1.0 grep

$ time ./grep --null-data --no-mmap ZQZQZQZQZQ binary-test.bin > out.ugrep
ugrep: invalid option --null-data, did you mean --neg-regexp=, --not, --no-any-line, --no-ascii, --no-binary, --no-bool, --no-break, --no-byte-offset, --no-color, --no-config, --no-confirm, --no-decompress, --no-dereference, --no-dereference-files, --no-dotall, --no-empty, --no-filename, --no-filter, --no-glob-ignore-case, --no-group-separator, --no-heading, --no-hidden, --no-hyperlink, --no-ignore-binary, --no-ignore-case, --no-ignore-files --no-initial-tab, --no-invert-match, --no-line-number, --no-only-line-number, --no-only-matching, --no-messages, --no-mmap, --no-pager, --no-pretty, --no-smart-case, --no-sort, --no-split, --no-stats, --no-tree, --no-ungroup, --no-view or --null?
For more help on options, try `ugrep --help' or `ugrep --help WHAT'
```

:thinking: 

Anyway, it looks like there's a perf bug to fix in ripgrep when `--null-data` is used, but otherwise, I don't see anything else to change here. You're fundamentally asking a grep here to print out a line that is 10GB long. You shouldn't be surprised when it uses a similar amount of memory. Your surprise seems rooted in the fact that "ugrep can do it," but actually, ugrep can't.

---

_Comment by @RecRanger on 2024-06-05 17:50_

Notice that I mention the `--only-matching`, which restricts to only the matching regex (which is, of course, just a string here, but could be an actual interesting output). I think there's a potential optimization to not load entire "lines" into memory when the `--only-matching` flag is used, and when the regex has a clear max length. Thoughts? 

---

_Comment by @BurntSushi on 2024-06-05 17:55_

A potential optimization for sure, but not one that I plan to pursue. It requires the regex engine to support streaming, and this comes with a whole host of other trade offs. It's a big effort. ripgrep's regex engine supports this to a degree, but wiring everything up is non-trivial. Stream searching is abstraction busting.

The lower hanging fruit here is to fix the perf bug in `--null-data`. Then you can use that.

---
