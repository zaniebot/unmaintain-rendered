```yaml
number: 380
title: line numbers only when reading file (and tty)
type: issue
state: closed
author: Dieterbe
labels:
  - question
assignees: []
created_at: 2017-02-24T15:31:27Z
updated_at: 2017-03-17T09:37:15Z
url: https://github.com/BurntSushi/ripgrep/issues/380
synced_at: 2026-01-12T16:13:21Z
```

# line numbers only when reading file (and tty)

---

_@Dieterbe_

Hello,
first of, ripgrep is slick. thank you very much. I have one letter of the alphabet (`r`) dedicated to just ripgrep. because typing `rg` would be too much for me. that's how much i use it. my alias actually invokes `rg -j1` so that i get deterministic output (FWIW)


anyway, i know how line number printing behavior is determined by whether you're in a tty or not, and then you can still turn it on and off explicitly.

Personally, i find them a bit too annoying when i'm matching against stdin. since stdin is typically a stream, line numbers don't make sense. especially when i then want to copy paste the output to others, things can get confusing. so could we do a mode for `line-numbers-only-when-tty-and-stdin` or something?

If i'm the only one asking for this and/or it doesn't seem worth it, I'll learn how to live with it...

so to be clear what I mean is, if you're in a terminal
```
> echo -e 'a\nb\nab' | r a # i wish i had no line numbers here
1:a
3:ab
> echo -e 'a\nb\nab' > file.txt
> r a file.txt # line numbers ok here
1:a
3:ab
```

Thanks again. cheerios!

---

_Comment by @BurntSushi on 2017-02-24 15:39_

> `line-numbers-only-when-tty-and-stdin`

I think you mean `line-numbers-only-when-tty-and-not-stdin`, right?

If so, this has something I've thought about since I made ripgrep. I kind of side with you on this. I think showing line numbers when searching only stdin is a little strange. I'm not quite sure if the right answer is to remove them in that specific case, which I think is why I never bothered to change it. If you do pipe ripgrep into another program, e.g., `echo -e 'a\nb\nab' | rg a | cat`, then the line numbers disappear. But so do colors, which is a pain.

---

_Label `question` added by @BurntSushi on 2017-02-24 15:40_

---

_Comment by @Dieterbe on 2017-02-24 16:01_

oops yes. But it was a bit tongue-in-cheek, i hope we can come up with something more elegant than such a flag. In fact, it seems we both think it would be sensible to adjust default behavior for this case :) do you see any downsides to introducing this change?

---

_Comment by @BurntSushi on 2017-02-24 16:06_

> In fact, it seems we both think it would be sensible to adjust default behavior for this case :)

Errm, yes, on the same page. :-)

> do you see any downsides to introducing this change?

I think the key downside is the case analysis and behavioral changes. For example, `rg pattern myfile` will show line numbers but `echo wat | rg pattern` won't. I would guess that `rg pattern -` wouldn't either. What about `rg pattern - myfile`? I would guess that we would only special case when "ripgrep is only searching stdin."

---

_Comment by @Dieterbe on 2017-02-24 16:46_

> `echo wat | rg` pattern won't. I would guess that `rg pattern - `wouldn't either

right. agreed.

> What about rg `pattern - myfile`? 

We could turn this:
```
rg b - file.txt <<< b
<stdin>
1:b

file.txt
2:b
3:ab
```

into this:
```
rg b - file.txt <<< b
<stdin>
b

file.txt
2:b
3:ab
```

for me the same reasoning applies. stdin is a stream so line numbers are meaningless to me. for files they are meaningful. so when you do both in one run, those ideas still apply IMHO

---

_Comment by @BurntSushi on 2017-02-24 16:47_

I find the idea of having an inconsistent output format in the same set of search results a bit unnerving.

I think I'm OK with the other stuff.

---

_Comment by @Dieterbe on 2017-02-24 19:04_

> I find the idea of having an inconsistent output format in the same set of search results a bit unnerving.

actually me too, I think we can keep `rg pattern - myfile` as is and apply only the special case when "ripgrep is only searching stdin."

---

_Closed by @BurntSushi on 2017-03-13 00:22_

---

_Comment by @Dieterbe on 2017-03-14 09:10_

thank you @BurntSushi , i'm enjoying the new output already when piping things into ripgrep :)

---

_Comment by @jzinn on 2017-03-17 03:51_

> especially when i then want to copy paste the output to others

For copy paste, you can do
```
# mac
echo -e 'a\nb\nab' | rg a | pbcopy
```
or
```
# linux
echo -e 'a\nb\nab' | rg a | xclip -selection clipboard
```
In this case, stdout is not a tty so ripgrep (even before 4ef4818) does not print line numbers.


---

_Comment by @Dieterbe on 2017-03-17 09:37_

my workflow is that typically i'll run ripgrep manually, potentially a few times while tweaking the pattern to get an narrow but accurate output, and then i just use my mouse to copy the output that is already visible.

---
