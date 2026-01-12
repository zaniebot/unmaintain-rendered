```yaml
number: 1551
title: "Tiny bug in document for `-P, --pcre2` option"
type: issue
state: closed
author: zufuliu
labels:
  - question
assignees: []
created_at: 2020-04-13T06:39:11Z
updated_at: 2020-04-13T12:50:26Z
url: https://github.com/BurntSushi/ripgrep/issues/1551
synced_at: 2026-01-12T16:13:23Z
```

# Tiny bug in document for `-P, --pcre2` option

---

_@zufuliu_

https://github.com/BurntSushi/ripgrep/blob/master/crates/core/app.rs#L2377

in `fn flag_pcre2(args: &mut Vec<RGArg>)`, I think `\n` is `\\n`. 
the output for `rg --help` now shows:
```
...
some cases, since it has fewer introspection APIs than ripgrep's default regex
engine. For example, if you use a '
' in a PCRE2 regex without the
'-U/--multiline' flag, then ripgrep will silently fail to match anything
...
```


---

_Comment by @BurntSushi on 2020-04-13 12:26_

Could you please provide more details? There is an issue template. Please fill it out.

In particular, I cannot reproduce this problem:
![rgman](https://user-images.githubusercontent.com/456674/79120232-6f8c0080-7d60-11ea-9f1e-ff689a199834.png)


---

_Label `question` added by @BurntSushi on 2020-04-13 12:26_

---

_Comment by @zufuliu on 2020-04-13 12:31_

https://github.com/BurntSushi/ripgrep/releases/download/12.0.1/ripgrep-12.0.1-x86_64-pc-windows-msvc.zip
```
rg --version
ripgrep 12.0.1 (rev 1d5b1011e5)
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
```

on Win10 1909 x64 Windows Command Prompt.
![rg-help](https://user-images.githubusercontent.com/2289926/79120557-be1fb600-7dc5-11ea-86dc-f1b2ee8fc249.png)


---

_Comment by @BurntSushi on 2020-04-13 12:35_

What command are you running to render the man page? Is it `man`? If so, what version of `man`?

---

_Comment by @zufuliu on 2020-04-13 12:35_

look at your own screenshot, I think it should be `\n` but is empty inside single quotes.
> For example, if you use a ' '  in a PCRE2 regex without the

---

_Comment by @zufuliu on 2020-04-13 12:36_

the `rg --help` command on cmd.exe (Windows Command Prompt).

---

_Comment by @zufuliu on 2020-04-13 12:40_

Same result when running in MSYS2's mintty (MINGW64)
![rg-help-mintty](https://user-images.githubusercontent.com/2289926/79121006-dcd27c80-7dc6-11ea-8d53-b568e9d739be.png)


---

_Closed by @BurntSushi on 2020-04-13 12:50_

---

_Comment by @BurntSushi on 2020-04-13 12:50_

Ah great, thanks. For some reason I thought you were looking at the man page. This is now fixed on master.

---
