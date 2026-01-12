```yaml
number: 1867
title: "ripgrep match digit use [0-9] or [[:digit:]] slow \\d"
type: issue
state: closed
author: x1e
labels:
  - question
assignees: []
created_at: 2021-05-19T06:44:58Z
updated_at: 2021-05-31T23:34:11Z
url: https://github.com/BurntSushi/ripgrep/issues/1867
synced_at: 2026-01-12T16:13:24Z
```

# ripgrep match digit use [0-9] or [[:digit:]] slow \d

---

_@x1e_

#### What version of ripgrep are you using?

ripgrep 11.0.2 (rev 3de31f7527)
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)

ripgrep 12.1.1 (rev 7cb211378a)
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)

#### How did you install ripgrep?

ripgrep 11.0.2 Github binary releases
ripgrep 12.1.1 Github binary releases

#### What operating system are you using ripgrep on?

Debian 9.12 stretch

#### Describe your bug.

[0-9] or [[:digit:]] match digit slow \d

#### What are the steps to reproduce the behavior?

11.0.2

time rg -c 996947807574403616 xxxx.log
7
real    0m24.966s
user    0m17.292s
sys     0m7.612s

time rg -c 99694780757440361[0-9] xxxx.log
40
real    2m40.820s
user    2m37.720s
sys     0m2.716s

time rg -c '99694780757440361[[:digit:]]' xxx.log
40

real    2m39.442s
user    2m37.456s
sys     0m1.604s


time rg -c 99694780757440361\\d xxxx.log
40
real    0m19.755s
user    0m18.072s
sys     0m1.632s

time rg -c 99694780757440361. xxxx.log
40
real    0m19.156s
user    0m17.504s
sys     0m1.604s

time rg -c 99694780757[0-9]40361[0-9] xxxx.log
645
real    0m51.093s
user    0m49.356s
sys     0m1.612s

time rg -c 99694780757\\d40361\\d  xxxx.log 
645
real    0m34.909s
user    0m33.080s
sys     0m1.732s

12.1.1

time rg -c 996947807574403616 xxxx.log
7
real    0m19.576s
user    0m17.924s
sys     0m1.604s

time rg -c 99694780757440361[0-9] xxxx.log
40
real    1m53.430s
user    1m51.496s
sys     0m1.664s

time rg -c '99694780757440361[[:digit:]]' xxx.log
40
real    1m53.763s
user    1m52.132s
sys     0m1.360s

time rg -c 99694780757440361\\d xxxx.log
40
real    0m19.009s
user    0m17.308s
sys     0m1.652s

time rg -c 99694780757440361. xxxx.log
40
real    0m19.673s
user    0m18.028s
sys     0m1.596s

time rg -c 99694780757[0-9]40361[0-9] xxxx.log
645
real    0m55.672s
user    0m53.968s
sys     0m1.568s

time rg -c 99694780757\\d40361\\d  xxxx.log 
645
real    0m34.300s
user    0m32.612s
sys     0m1.604s

#### What is the actual behavior?

 [0-9] or [[:digit:]] slow \d or .

#### What is the expected behavior?

[0-9] or [[:digit:]] or \d have same speed.


---

_Comment by @BurntSushi on 2021-05-19 10:12_

I'm sorry but this report isn't actionable. Please provide the input. If you can't share the input, then please find a way to anonymize it or another input that we both have access to.

Also, the search times here are quite long. To me, they suggest the file is quite large. Possibly large enough that it does not fit into your I/O cache, and that most time is being spent doing I/O. Thus, you might just be witnessing noise.

And finally, `\d` is not equivalent to `[0-9]`. `\d` is Unicode-aware. But I wouldn't really expect that to matter here. Alas, I don't know.

---

_Label `question` added by @BurntSushi on 2021-05-19 10:12_

---

_Comment by @BurntSushi on 2021-05-31 23:34_

Closing due to inactivity. Happy to re-open if given more data.

---

_Closed by @BurntSushi on 2021-05-31 23:34_

---
