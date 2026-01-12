```yaml
number: 631
title: "report \"binary file match\" when possible"
type: issue
state: closed
author: BurntSushi
labels:
  - duplicate
  - question
assignees: []
created_at: 2017-10-09T16:51:55Z
updated_at: 2024-02-17T12:21:12Z
url: https://github.com/BurntSushi/ripgrep/issues/631
synced_at: 2026-01-12T16:13:22Z
```

# report "binary file match" when possible

---

_@BurntSushi_

[This comment](https://news.ycombinator.com/item?id=15433341) describes a use case for searching binary files, but not actually emitting the match. `grep` does this by printing `Binary file foo matches`.

The interesting interaction here is that ripgrep attempts to ignore binary files altogether by default. In particular, it will stop searching a file as soon as it think it's a binary file. I guess the intended effect here is to prevent a bunch of "binary file matches" messages from appearing when searching a large directory tree.

The linked comment suggests emitting the message only when a single `-u` is given. It feels kind of right to me, but it also couples a single `-u` flag with "disable gitignore and show binary file names that matched."

What should we do here? I guess I find the use case interesting, but I'm having difficulty coming up with a good UX for it.

---

_Label `question` added by @BurntSushi on 2017-10-09 16:51_

---

_Comment by @ibotty on 2017-10-11 14:33_

I sometimes want to honor .gitignore, but still (rip)grep the binary files that are checked in that repository (pdfs, mostly). So I would prefer using a separate flag, i.e. `--report-binary-matches`, that I would just include in my rg alias.

---

_Comment by @BurntSushi on 2017-10-22 02:03_

Whoops, I actually think this is a duplicate of #306.

---

_Closed by @BurntSushi on 2017-10-22 02:03_

---

_Label `duplicate` added by @BurntSushi on 2017-10-22 02:03_

---

_Comment by @sergeevabc on 2024-02-17 07:58_

I came here from [Hacker News thread](https://news.ycombinator.com/item?id=15433006) looking for the final solution on how to deal with binary files properly. My case is to find all console apps written in Go. So far I used an unintuitive command `-uuu` and got the following output:

``` console
$ rg.exe -uuu "runtime.go"
network\croc.exe: binary file matches (found "\0" byte around offset 3)
network\wormhole-william.exe: binary file matches (found "\0" byte around offset 3)
```

Is there a way to do it differently?

---

_Comment by @BurntSushi on 2024-02-17 11:41_

> I came here from [Hacker News thread](https://news.ycombinator.com/item?id=15433006) looking for the final solution on how to deal with binary files properly. My case is to find all console apps written in Go. So far I used an unintuitive command `-uuu` and got the following output:
> 
> ```
> $ rg.exe -uuu "runtime.go"
> network\croc.exe: binary file matches (found "\0" byte around offset 3)
> network\wormhole-william.exe: binary file matches (found "\0" byte around offset 3)
> ```
> 
> Is there a way to do it differently?

Please read the guide. It explains what `-uuu` does. In your case, it's probably the third `u` that matters, which disables filtering of binary files. But since you didn't provide a complete reproduction, I can't say for certain. It's possible the first `u` (disables gitignore filtering) is also relevant.

If you just want to disable binary filtering, then use the `--binary` flag. This is documented in the guide.

---

_Comment by @sergeevabc on 2024-02-17 12:21_

Indeed, thank you.

``` console
$ rg.exe --binary -l "runtime.go"
network\croc.exe
network\wormhole-william.exe
```

---
