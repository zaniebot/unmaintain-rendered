```yaml
number: 1311
title: Unexpected (or incorrect?) multiline replace behavior
type: issue
state: closed
author: mqudsi
labels:
  - bug
  - rollup
assignees: []
created_at: 2019-06-23T18:21:14Z
updated_at: 2021-06-16T18:33:22Z
url: https://github.com/BurntSushi/ripgrep/issues/1311
synced_at: 2026-01-12T16:13:23Z
```

# Unexpected (or incorrect?) multiline replace behavior

---

_@mqudsi_

#### What version of ripgrep are you using?

ripgrep 11.0.1 (rev 34677d2622)
+SIMD -AVX (compiled)
+SIMD +AVX (runtime)

#### How did you install ripgrep?

cargo from git

#### What operating system are you using ripgrep on?

Distributor ID:	Debian
Description:	Debian GNU/Linux 10 (buster)
Release:	10
Codename:	buster

#### Describe your question, feature request, or bug.

I was trying to replace some of my usages of other commands (`sed`, `tr`, `ag`) with ripgrep now that it has proper multiline and pcre2 support features, and ran into the following odd behavior:

```
> printf "hello\nworld\n" | rg -U "\n" --color always 
hello
world 
```
This works fine, everything matches, nothing is highlighted because the newline can't be colored.

But when I try to replace, I get the following behavior:

```
> printf "hello\nworld\n" | rg -U "\n" -r "?" 
hello?
world?
```

which isn't correct because the newline has been correctly matched but incorrectly retained in the output.

Whereas the following does work:

```
> printf "hello\nworld\n" | rg --null-data "\n" -r "?"
hello?world?
```

But I don't see any documented reason why the former shouldn't give the same output as well. I can understand technically why this is happening (rg internally preserves the concept of lines but merely glosses over them for search purposes with `-U`), but the confluence of both `-U` and `-r` results in incorrect behavior: the output should either include the replacement character and no new lines (desirable) or the newlines and no replacement character (unfortunate but understandable limitation). I don't think including both can be considered correct, however.

---

_Comment by @mqudsi on 2019-06-23 18:39_

Actually, this has to be a mere oversight because the following *does* work:

```
> printf "hello\nworld\n" | rg -U o\nw -r '?'
hell?orld
```

and I can even trick rg into giving me the behavior I'm looking for with this:

```
> printf "hello\nworld\n" | rg -U '(.?)\n(.?)' -r '$1?$2'
hello?world
```

(although that fails when there are empty lines in the input)

---

_Comment by @learnbyexample on 2019-06-24 05:23_

My understanding is that `-U` doesn't strictly behave as if entire input is single input string. Rather, it allows the match to cross multiple lines when necessary, like the `printf "hello\nworld\n" | rg -U 'o\nw' -r '?'` example.

You might feel `--null-data` is working, but that is simply adding NUL character for each block of output, instead of newline. It so happens you have only one block in your example as there is no NUL character in input. `printf "hello\nworld\n" | perl -0777 -pe 's/\n/?/g'` is an example for slurping entire input as single string.


---

_Comment by @mqudsi on 2019-06-24 05:24_

> You might feel --null-data is working, but that is simply adding NUL character for each block of output, instead of newline. 

Yes, and that's why I would like `-U` to work instead ;)

---

_Label `bug` added by @BurntSushi on 2019-06-24 10:52_

---

_Comment by @BurntSushi on 2019-06-24 10:53_

This is likely a bug in how new lines are handled by the printer. I don't know when I'll get around to it.

---

_Comment by @mqudsi on 2019-06-25 22:46_

Thanks for the confirmation anyway, @BurntSushi!

---

_Comment by @dkasak on 2020-06-04 14:12_

Just found this bug myself. Here's some more weirdness in case it points to the source of the bug.

```
➜ printf 'foo\n bar' | rg --multiline --passthru --replace '?' '\s+'            
foo?bar

➜ printf 'foo \nbar' | rg --multiline --passthru --replace '?' '\s+' 
foo?
bar

➜ printf 'foo \n bar' | rg --multiline --passthru --replace '?' '\s+'
foo?bar
```

---

_Label `rollup` added by @BurntSushi on 2021-05-31 10:59_

---

_Closed by @BurntSushi on 2021-06-01 01:51_

---

_Comment by @krtab on 2021-06-16 18:33_

Hi @BurntSushi!

Just wanted to thank you for fixing this. I have just encountered this very bug, and it is very pleasing to see "closed 16 days ago"! :+1: 

---
