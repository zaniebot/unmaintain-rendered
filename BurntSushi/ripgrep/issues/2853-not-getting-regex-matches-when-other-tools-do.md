```yaml
number: 2853
title: Not Getting Regex Matches When Other Tools Do Match
type: issue
state: closed
author: patb-stout
labels: []
assignees: []
created_at: 2024-07-15T22:49:17Z
updated_at: 2024-07-16T13:55:27Z
url: https://github.com/BurntSushi/ripgrep/issues/2853
synced_at: 2026-01-12T16:13:25Z
```

# Not Getting Regex Matches When Other Tools Do Match

---

_@patb-stout_


### Please tick this box to confirm you have reviewed the above.

- [X] I have a different issue.

### What version of ripgrep are you using?

ripgrep 14.1.0 (rev e50df40a19); [features:-simd-accel,+pcre2; simd(compile):+SSE2,-SSSE3,-AVX2; simd(runtime):+SSE2,+SSSE3,+AVX2] from ripgrep-14.1.0-x86_64-pc-windows-msvc.zip

### How did you install ripgrep?

Downloaded zip binary and used directly in DOS.

### What operating system are you using ripgrep on?

Windows Server 2019 version 1809

### Describe your bug.

From command prompt, I get no matches with rg "(".[^"]+,.[^"]+")" ./test.txt

### What are the steps to reproduce the behavior?

test.txt is a UTF8 file in DOS (CRLF) line endings. Contents of text file is as follows.
"Mark",""123 Somewhere Ln, 90210"","Blue"
""abc,def"","",""
test

### What is the actual behavior?

I get no matches.

### What is the expected behavior?

According to regex101.com, ripgrep should have returned the following two matches.
""123 Somewhere Ln, 90210""
""abc,def""

And the capture groups should have returned the following two.
"123 Somewhere Ln, 90210"
"abc,def"

Ultimately, I was trying to use ripgrep to remove doubled up double-quotes around text qualified values. These values are double text qualifed (with double quotes) and I only want them text qualified one time per text value.

![Regex Issue](https://github.com/user-attachments/assets/7d7cc1d5-f445-471f-b877-ca67f96be35b)
The attached screen capture shows how the regex pattern properly finds between the doubled double quotes and the green shows proper matching of the capture group.


---

_Comment by @VladimirMarkelov on 2024-07-15 22:58_

What is your shell? If you use cmd.exe, it treats `^` as escape characters, so your `rg "(".[^"]+,.[^"]+")"`, likely, turns into `rg "(".["]+,.["]+")"`.

---

_Comment by @BurntSushi on 2024-07-15 23:05_

Yes, this looks like a shell issue. And regex101 isn't "another tool" comparable to ripgrep. The only way regex101 is relevant is if you've isolated every other possible explanation for the difference. Shell quoting rules is one. But there are many others.

---

_Closed by @BurntSushi on 2024-07-15 23:05_

---

_Comment by @patb-stout on 2024-07-16 13:25_

I used the same regex successfully with sed on Cygwin Linux and also with UltraEdit (with the Perl regex type selected) on Windows, but I do see where the ^ symbol is seen as an escape character on DOS/command prompt (cmd.exe). I will see what I can do to get it working from DOS.

---

_Comment by @BurntSushi on 2024-07-16 13:48_

> I used the same regex successfully 

That's just it. You very likely didn't. Because of the shell quoting rules.

---

_Comment by @garoto on 2024-07-16 13:55_

`rg (".[^^\"]+,.[^^\"]+") regex.test`

Escape both carets with an extra caret and the double-quotes inside the brackets with the C escape char, the backslash.

---
