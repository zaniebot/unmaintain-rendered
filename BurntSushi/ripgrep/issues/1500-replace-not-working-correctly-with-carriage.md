```yaml
number: 1500
title: "--replace not working correctly with carriage returns on Windows"
type: issue
state: closed
author: BurntSushi
labels:
  - invalid
assignees: []
created_at: 2020-02-27T13:07:16Z
updated_at: 2020-02-29T01:04:56Z
url: https://github.com/BurntSushi/ripgrep/issues/1500
synced_at: 2026-01-12T16:13:23Z
```

# --replace not working correctly with carriage returns on Windows

---

_@BurntSushi_

Originally [reported by @sergeevabc](https://github.com/BurntSushi/ripgrep/issues/416#issuecomment-591925122). Namely, the third command below produces unexpected output:

```
printf "1931. Arrowsmith" | rg "(\d+)..(.*)" -r "$2 $1"
Arrowsmith 1931               <--- as expected, but printf is not a part of Windows 

echo 1931. Arrowsmith | rg "(\d+)..(.*)" -r "$2 $1"
 1931smith                    <--- weird misplacement

echo 1931. Arrowsmith | rg "(\d+)..(.*)" -r "$2 $1" --crlf
Arrowsmith  1931              <--- extra space in between
```

Using `xxd` confirms the extra space:

```
echo 1931. Arrowsmith | rg "(\d+)..(.*)" -r "$2 $1" --crlf | xxd
00000000: 4172 726f 7773 6d69 7468 2020 3139 3331  Arrowsmith  1931
00000010: 0d0a 
```

I have not been able to reproduce this on Linux, even after inserting carriage returns. Below is copied from [my comment]() in #416: 

OK, so I'm assuming that `echo` on Windows probably emits a `\r\n` as a new line, so let's try that:

```
$ printf "1931. Arrowsmith\r\n" | rg '(\d+)..(.*)' -r '$2 $1'
 1931smith
```

It's quite likely here that `(.*)` is matching the `\r` so the replacement actually winds up being `Arrowsmith\r 1931`. We can confirm this by looking at the hex output:

```
$ printf "1931. Arrowsmith\r\n" | rg '(\d+)..(.*)' -r '$2 $1' | xxd
00000000: 4172 726f 7773 6d69 7468 0d20 3139 3331  Arrowsmith. 1931
00000010: 0a
```

You can see the `0d` in there, which corresponds to `\r`. The strange output is actually what one expects:

```
$ echo 'Arrowsmith\r 1931'
 1931smith
```

Because `\r` will move the cursor position back to the beginning of the line. Subsequent printing then overwrites characters that were already printed. e.g.,

```
$ echo 'Arrowsmith\r 1931X'
 1931Xmith

$ echo 'Arrowsmith\r 1931XX'
 1931XXith

$ echo 'Arrowsmith\r 1931XXX'
 1931XXXth
```

Adding the `--crlf` flag seemingly gets this right:

```
$ printf "1931. Arrowsmith\r\n" | rg '(\d+)..(.*)' -r '$2 $1' --crlf
Arrowsmith 1931
```

Confirming with `xxd` that there is a single space:

```
$ printf "1931. Arrowsmith\r\n" | rg '(\d+)..(.*)' -r '$2 $1' --crlf | xxd
00000000: 4172 726f 7773 6d69 7468 2031 3933 310d  Arrowsmith 1931.
00000010: 0a
```

Notice that there is only one `0x20` byte there. So I can't reproduce your issue, at least on Linux, and in theory, the above should be equivalent to what you're doing.

---

_Label `bug` added by @BurntSushi on 2020-02-27 13:07_

---

_Label `question` added by @BurntSushi on 2020-02-27 13:07_

---

_Comment by @BatmanAoD on 2020-02-28 19:38_

I have a Windows machine; I'll try to find time this weekend to reproduce this.

At the moment I'm at a Darwin machine, which behaves the way you've shown for Linux.

---

_Comment by @BatmanAoD on 2020-02-29 00:53_

It looks like `cmd`'s `echo` is itself producing the extra space:

```
C:\Users\batma>"\Program Files\git\usr\bin\printf.exe" "1931. Arrowsmith\r\n"  | "\Program Files\Git\usr\bin\xxd.exe"
00000000: 3139 3331 2e20 4172 726f 7773 6d69 7468  1931. Arrowsmith
00000010: 0d0a                                     ..

C:\Users\batma>echo 1931. Arrowsmith  | "\Program Files\Git\usr\bin\xxd.exe"
00000000: 3139 3331 2e20 4172 726f 7773 6d69 7468  1931. Arrowsmith
00000010: 2020 0d0a                                  ..
```

---

_Comment by @BatmanAoD on 2020-02-29 00:58_

Yep, `echo` in `cmd` consumes all spaces to the end of the command.

```
C:\Users\batma>echo 1931. Arrowsmith| "\Program Files\Git\usr\bin\xxd.exe"
00000000: 3139 3331 2e20 4172 726f 7773 6d69 7468  1931. Arrowsmith
00000010: 0d0a                                     ..

C:\Users\batma>echo 1931. Arrowsmith | "\Program Files\Git\usr\bin\xxd.exe"
00000000: 3139 3331 2e20 4172 726f 7773 6d69 7468  1931. Arrowsmith
00000010: 200d 0a                                   ..
```

And, indeed, if the space before the `|` is removed in the original example, the output matches the Linux output:

```
C:\Users\batma>echo 1931. Arrowsmith| rg "(\d+)..(.*)" -r "$2 $1" --crlf
Arrowsmith 1931
```

---

_Comment by @BurntSushi on 2020-02-29 01:04_

How delightful. Thanks for tracking that down!

---

_Closed by @BurntSushi on 2020-02-29 01:04_

---

_Label `bug` removed by @BurntSushi on 2020-02-29 01:04_

---

_Label `question` removed by @BurntSushi on 2020-02-29 01:04_

---

_Label `invalid` added by @BurntSushi on 2020-02-29 01:04_

---
