```yaml
number: 1263
title: "ripgrep is slower than \"grep -E\" in a simple or search."
type: issue
state: closed
author: NanXiao
labels:
  - question
assignees: []
created_at: 2019-04-22T09:10:33Z
updated_at: 2019-04-22T11:46:20Z
url: https://github.com/BurntSushi/ripgrep/issues/1263
synced_at: 2026-01-12T16:13:23Z
```

# ripgrep is slower than "grep -E" in a simple or search.

---

_@NanXiao_

#### What version of ripgrep are you using?

```
# rg --version
ripgrep 11.0.1
-SIMD -AVX (compiled)
+SIMD -AVX (runtime)

```

#### How did you install ripgrep?

`# pacman -Syu ripgrep`

#### What operating system are you using ripgrep on?

Arch Linux

#### Describe your question, feature request, or bug.

Not sure it is a general behavior or just a special case on my machine. The test file is very simple:  

```
# cat test.txt
11
22
33
44
```

Use following script to run "`grep -E`" 10 times:  

```
# cat loop.sh
#!/bin/bash
for i in {1..10}
do
   { time grep -E "22|33" test.txt > /dev/null ; } |& grep real | sed -E 's/[^0-9\.]+//g' | tr -d '\n'| (cat && echo " * 1000") | bc
done
```
```
# ./loop.sh
2.000
4.000
2.000
2.000
2.000
1.000
1.000
1.000
1.000
1.000
```

Use `rg` instead of "`grep -E`":  
```
# cat loop_rg.sh
#!/bin/bash
for i in {1..10}
do
   { time rg "22|33" test.txt > /dev/null ; } |& grep real | sed -E 's/[^0-9\.]+//g' | tr -d '\n'| (cat && echo " * 1000") | bc
done
```
```
# ./loop_rg.sh
8.000
6.000
7.000
7.000
7.000
7.000
6.000
6.000
7.000
5.000
```

You can see in average `rg` is `6` ~`7` ms slower compared to "`grep -E`".




---

_Comment by @BurntSushi on 2019-04-22 10:57_

This isn't really actionable. A difference of a few milliseconds is not something I'm particularly concerned about. This is well within the range of startup overhead. Re-running your loop script, for example, shows quite a bit of variance. `egrep` does tend to be a bit faster on average, but a better tool for this kind of benchmarking is hyperfine. And even with hyperfine, there is an overall variance of about 1ms:

```
$ hyperfine "rg --no-config '22|33' test.txt > /dev/null"
Benchmark #1: rg --no-config '22|33' test.txt > /dev/null
  Time (mean ± σ):       8.1 ms ±   2.4 ms    [User: 4.6 ms, System: 3.7 ms]
  Range (min … max):     2.0 ms …  14.7 ms    258 runs

  Warning: Command took less than 5 ms to complete. Results might be inaccurate.

$ hyperfine "rg --no-config '22|33' test.txt > /dev/null"
Benchmark #1: rg --no-config '22|33' test.txt > /dev/null
  Time (mean ± σ):       7.1 ms ±   2.4 ms    [User: 4.5 ms, System: 3.0 ms]
  Range (min … max):     1.3 ms …  12.6 ms    242 runs

  Warning: Command took less than 5 ms to complete. Results might be inaccurate.

$ hyperfine "rg --no-config '22|33' test.txt > /dev/null"
Benchmark #1: rg --no-config '22|33' test.txt > /dev/null
  Time (mean ± σ):       8.3 ms ±   2.5 ms    [User: 5.2 ms, System: 3.2 ms]
  Range (min … max):     1.2 ms …  13.7 ms    236 runs

  Warning: Command took less than 5 ms to complete. Results might be inaccurate.

$ hyperfine "rg --no-config '22|33' test.txt > /dev/null"
Benchmark #1: rg --no-config '22|33' test.txt > /dev/null
  Time (mean ± σ):       8.2 ms ±   2.4 ms    [User: 5.0 ms, System: 3.4 ms]
  Range (min … max):     1.7 ms …  13.4 ms    270 runs

  Warning: Command took less than 5 ms to complete. Results might be inaccurate.

$ hyperfine "rg --no-config '22|33' test.txt > /dev/null"
Benchmark #1: rg --no-config '22|33' test.txt > /dev/null
  Time (mean ± σ):       7.9 ms ±   2.3 ms    [User: 5.0 ms, System: 3.1 ms]
  Range (min … max):     2.5 ms …  12.6 ms    220 runs

  Warning: Command took less than 5 ms to complete. Results might be inaccurate.
```

And the same for `grep -E`:

```
$ hyperfine "grep -E '22|33' test.txt > /dev/null"
Benchmark #1: grep -E '22|33' test.txt > /dev/null
  Time (mean ± σ):       4.4 ms ±   1.5 ms    [User: 3.4 ms, System: 1.5 ms]
  Range (min … max):     0.0 ms …   8.0 ms    416 runs

  Warning: Command took less than 5 ms to complete. Results might be inaccurate.

$ hyperfine "grep -E '22|33' test.txt > /dev/null"
Benchmark #1: grep -E '22|33' test.txt > /dev/null
  Time (mean ± σ):       5.1 ms ±   1.4 ms    [User: 4.0 ms, System: 1.5 ms]
  Range (min … max):     1.2 ms …   8.5 ms    376 runs

  Warning: Command took less than 5 ms to complete. Results might be inaccurate.

$ hyperfine "grep -E '22|33' test.txt > /dev/null"
Benchmark #1: grep -E '22|33' test.txt > /dev/null
  Time (mean ± σ):       4.9 ms ±   1.3 ms    [User: 4.1 ms, System: 1.4 ms]
  Range (min … max):     0.2 ms …   7.3 ms    263 runs

  Warning: Command took less than 5 ms to complete. Results might be inaccurate.

$ hyperfine "grep -E '22|33' test.txt > /dev/null"
Benchmark #1: grep -E '22|33' test.txt > /dev/null
  Time (mean ± σ):       5.4 ms ±   1.4 ms    [User: 4.2 ms, System: 1.6 ms]
  Range (min … max):     1.8 ms …   9.8 ms    286 runs

  Warning: Command took less than 5 ms to complete. Results might be inaccurate.

$ hyperfine "grep -E '22|33' test.txt > /dev/null"
Benchmark #1: grep -E '22|33' test.txt > /dev/null
  Time (mean ± σ):       5.2 ms ±   1.3 ms    [User: 4.0 ms, System: 1.7 ms]
  Range (min … max):     1.2 ms …   9.1 ms    260 runs

  Warning: Command took less than 5 ms to complete. Results might be inaccurate.
```

So according to hyperfine, there's an approximate difference of about 3ms, on average. (Notice that I used `--no-config` with ripgrep, which prevents it from trying to read any config file.)

This isn't something I'm going to look into, but others are welcome to debug the performance difference.

---

_Closed by @BurntSushi on 2019-04-22 10:57_

---

_Label `question` added by @BurntSushi on 2019-04-22 10:57_

---
