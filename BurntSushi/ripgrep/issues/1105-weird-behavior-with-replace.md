```yaml
number: 1105
title: Weird behavior with --replace
type: issue
state: closed
author: Boddlnagg
labels:
  - bug
assignees: []
created_at: 2018-11-13T07:39:03Z
updated_at: 2019-03-09T04:47:46Z
url: https://github.com/BurntSushi/ripgrep/issues/1105
synced_at: 2026-01-12T16:13:22Z
```

# Weird behavior with --replace

---

_@Boddlnagg_

#### What version of ripgrep are you using?

```
ripgrep 0.10.0 (rev 31adff6f3c)
+SIMD +AVX (compiled)
+SIMD +AVX (runtime)
```

#### How did you install ripgrep?

Cloned and compiled current master.

#### What operating system are you using ripgrep on?

Windows 10 1803

#### Problem description

Given the following input file `test.txt`:
```
1 x A
2 x B
3 x C
```

Compare the following commands with their outputs:
(1)
```
> rg -e "(\d+) x (.)" test.txt
1:1 x A
2:2 x B
3:3 x C
```
(2)
```
> rg -e "(\d+) x (.*)" --replace '$1 y $2' test.txt
1:1 y A
2:2 y B
3:3 y C
```
(3)
```
> rg -e "(\d+) x (.)" --replace '$2 y $1' test.txt
1:A y 1
2:B y 2
3:C y 3
```
(4)
```
> rg -e "(\d+) x (.*)" --replace '$2 y $1' test.txt
 y 1
 y 2
3:C y 3
```

Everything except (4) is as expected, but in (4) suddenly the capture groups seem to behave differently compared to (2) although the only difference is that they are used in a different order in `--replace` ... is this a bug? It also seems to be related to the `*` in the regex, because in (3) it works as expected.

---

_Comment by @BurntSushi on 2018-11-13 13:11_

I can reproduce this, but only when `test.txt` uses CRLF for line endings, which is common on Windows. The issue here is that your second capture group, with `.*`, matches the `\r` in the `\r\n` line ending. When you do a replacement, that `\r` is captured as part of the `$2` value. When you move it to the beginning of the line, `\r` (I believe) has the effect of deleting everything that came before it on the current line, which explains the weird output. If I remove CRLF line endings (and just use LF), then this problem goes away.

I'm not sure whether it's practical to fix this or not as-is, but I'll call this a bug for now and look into it.

For now, it's possible to fix this bug by passing the `--crlf` flag, which explicitly uses CRLF line endings. It would be OK for you to put `--crlf` in your ripgrep config file and then forget about it.

---

_Label `bug` added by @BurntSushi on 2018-11-13 13:11_

---

_Comment by @Boddlnagg on 2018-11-14 07:21_

I see, thanks for the explanation!
Maybe there could be a way to detect CRLF line endings in the input and warn about it (mentioning the existence of `--crlf`)? But I'm not sure where/when that warning should be emitted.

---

_Comment by @BurntSushi on 2019-01-26 17:39_

I've given this some thought, and I don't see any real way to fix this other than perhaps enabling `--crlf` automatically on Windows, which I'd prefer not to do.

---

_Closed by @BurntSushi on 2019-01-26 17:39_

---

_Comment by @sergeevabc on 2019-03-09 01:58_

Being a Windows user, I face mentioned annoying behavior every now and then. ```sed``` (ported from Unix) works as expected when ```sed -r "s/(.*)= (.*)/\2 \1/"``` is used to swap values, so [does][1] Regex101, but ```rg``` produces the output even battle-hardened fellas from irc.freenode.net/#regex called weird.

Example:
```(AUTHORS)= 870ee40c43041a5e0e0d6d55a077b776d2d7a091a3b25e48f27a92cebc2b8152```

Expected after ```rg "(.*)= (.*)" -r "$2 $1"```:
```870ee40c43041a5e0e0d6d55a077b776d2d7a091a3b25e48f27a92cebc2b8152 (AUTHORS)```

Actually (omg, wtf, aaggrrhh):
``` (AUTHORS)041a5e0e0d6d55a077b776d2d7a091a3b25e48f27a92cebc2b8152```

Another workaround apart from using ```--crlf``` seems to be adding ```\b```: ```rg "(.*)= (.*)\b" -r "$2 $1"```

[1]: https://regex101.com/r/wAi09b/1

---

_Comment by @BurntSushi on 2019-03-09 04:47_

@sergeevabc Yes, the problem is known, and it is as I described above. There is no debate that it is "weird," so your battle hardened friends can rest easy. Add `--crlf` to your ripgrep config until this is fixed in the regex engine.

---
