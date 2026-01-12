```yaml
number: 1751
title: Fixed string search (-F) is ignored for pattern files (-f)
type: issue
state: closed
author: jonashaag
labels:
  - invalid
assignees: []
created_at: 2020-11-29T17:32:21Z
updated_at: 2020-11-29T18:14:53Z
url: https://github.com/BurntSushi/ripgrep/issues/1751
synced_at: 2026-01-12T16:13:24Z
```

# Fixed string search (-F) is ignored for pattern files (-f)

---

_@jonashaag_

Refs #1750

#### What version of ripgrep are you using?

```
ripgrep 12.1.1
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
```

#### How did you install ripgrep?

Manjaro packages

#### What operating system are you using ripgrep on?

Manjaro Linux (x86_64, 5.8.6-1-MANJARO)

#### Describe your bug.

`-F` is ignored when using `-f`, causing ripgrep to stop because of excessive regex size.

#### What are the steps to reproduce the behavior?

```
seq 1000000 > data1
seq 1000000 | shuf > data2
rg -F -f data1 data2 >/dev/null
Compiled regex exceeds size limit of 104857600 bytes.
```

#### What is the actual behavior?

```
rg -F -f data1 data2 --debug
DEBUG|rg::config|crates/core/config.rs:40: /home/jo/.config/ripgreprc: arguments loaded from config file: ["--max-columns=150", "--max-columns-preview", "--smart-case"]
DEBUG|rg::args|crates/core/args.rs:543: final argv: ["rg", "--max-columns=150", "--max-columns-preview", "--smart-case", "--smart-case", "-F", "-f", "data1", "data2", "--debug"]
Compiled regex exceeds size limit of 104857600 bytes.
```

#### What is the expected behavior?

Should work like grep (best case: faster)

```
time grep -f data1 data2 >/dev/null

Executed in  242,64 millis
```

---

_Comment by @BurntSushi on 2020-11-29 17:44_

> `-F` is ignored when using `-f`, causing ripgrep to stop because of excessive regex size.

It's not: https://github.com/BurntSushi/ripgrep/blob/a6d05475fb353c756e88f605fd5366a67943e591/crates/core/args.rs#L691-L695

And also:

```
[andrew@krusty ripgrep-1751]$ time rg -f data1 data2 | wc -l
1000000

real    3.327
user    2.514
sys     0.809
maxmem  2943 MB
faults  0

$ time rg -F -f data1 data2 | wc -l
1000000

real    0.240
user    0.199
sys     0.040
maxmem  80 MB
faults  0
```

... which also means that I can't reproduce your problem. Ah, but you have `--smart-case` in your ripgrep config file, and that seems to be applicable here:

```
$ time rg -S -F -f data1 data2 | wc -l
Compiled regex exceeds size limit of 104857600 bytes.
0

real    5.531
user    4.540
sys     0.985
maxmem  3552 MB
faults  0
```

Remember, smart case turns only when none of the patterns provided contains an uppercase character. Which is the case in your example. You can either remove `--smart-case` from your config file or pass the `-s/--case-sensitive` flag.

---

_Label `question` added by @BurntSushi on 2020-11-29 17:44_

---

_Comment by @jonashaag on 2020-11-29 18:12_

Aha, thanks! I agree that this a user error. Probably a naive question, but could smart case be implemented without regex so it ~does not disable~ has the speed you're used to from using `-F`?

---

_Comment by @BurntSushi on 2020-11-29 18:14_

_Could_ it be? Sure. Will it be? Probably not. In terms of implementation, it's much simpler to let the regex engine handle it.

One possible corner case that could be done without the regex engine is if ASCII case insensitivity is good enough. But ripgrep enables Unicode by default, so that would require passing `--no-unicode`.

---

_Closed by @BurntSushi on 2020-11-29 18:14_

---

_Label `question` removed by @BurntSushi on 2020-11-29 18:14_

---

_Label `invalid` added by @BurntSushi on 2020-11-29 18:14_

---
