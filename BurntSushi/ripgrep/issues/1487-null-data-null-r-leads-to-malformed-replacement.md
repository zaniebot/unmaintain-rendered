```yaml
number: 1487
title: "--null-data + --null + -r leads to malformed replacement "
type: issue
state: closed
author: gaycodegal
labels:
  - bug
  - wontfix
assignees: []
created_at: 2020-02-16T02:35:34Z
updated_at: 2020-02-16T16:59:13Z
url: https://github.com/BurntSushi/ripgrep/issues/1487
synced_at: 2026-01-12T16:13:23Z
```

# --null-data + --null + -r leads to malformed replacement 

---

_@gaycodegal_

#### What version of ripgrep are you using?

ripgrep 11.0.2
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)

#### How did you install ripgrep?

cargo install ripgrep

#### What operating system are you using ripgrep on?

Linux username 5.3.0-28-generic #30~18.04.1-Ubuntu SMP Fri Jan 17 06:14:09 UTC 2020 x86_64 x86_64 x86_64 GNU/Linux

#### Sample of reproducing the bug:

What I tried to do:

```bash
echo -e "hi.hh\0you.hh" | rg --null --null-data -e '\.hh$' -r "" | xargs -0 -I{} echo {}.first {}.second
```

##### bad output
```
hi.hh.h hi.hh.hh
you
.h you
.hh
```

#### How I expect it to work

This does work fine. (note replaced \0 with \n, and removed null command line args)

```bash
echo -e "hi.hh\nyou.hh" | rg -e '\.hh$' -r "" | xargs -I{} echo {}.first {}.second
```

##### good output

```
hi.h hi.hh
you.h you.hh
```

---

_Comment by @okdana on 2020-02-16 03:34_

I guess this is because `$` still only matches `\n` and EOS with `--null-data`. You can confirm this easily by looking at what matches are highlighted when you search for a pattern containing `$` (the `<...>` here represent the matches):

```
% printf 'hi.hh\0you.hh' | \rg --color=always --null-data '\.hh$'
hi.hhyou<.hh>

% printf 'hi.hh\0you.hh\nme.hh' | \rg --color=always --null-data '\.hh$'
hi.hhyou<.hh>
me<.hh>
```

Meanwhile, GNU `grep` does this:

```
% printf 'hi.hh\0you.hh' | \ggrep --color=always -z '\.hh$'
hi<.hh>you<.hh>

% printf 'hi.hh\0you.hh\nme.hh' | \ggrep --color=always -z '\.hh$'
hi<.hh>you.hh
me<.hh>
```

Maybe it's been addressed before and i just don't remember, but it does seem like the GNU behaviour is more intuitive/useful. That's how i assumed it worked, at least


---

_Comment by @BurntSushi on 2020-02-16 16:38_

Hmm, I don't get the same outputs with the examples given...

```
$ echo -e "hi.hh\0you.hh" | rg --null --null-data -e '\.hh$' -r "" | xargs -0 -I{} echo {}.first {}.second
hi.hh.first hi.hh.second
you
.first you
.second

$ echo -e "hi.hh\nyou.hh" | rg -e '\.hh$' -r "" | xargs -I{} echo {}.first {}.second
hi.first hi.second
you.first you.second
```

I suspect @okdana is right here. Will investigate a bit more.

---

_Comment by @BurntSushi on 2020-02-16 16:58_

I'm going to mark this at `wontfix` for now. Fixing this correctly probably means adding a configurable knob to the regex engine to permit setting the line terminator recognized by `^` and `$`. I've filed an issue for that: https://github.com/rust-lang/regex/issues/644

If that ever gets fixed, then we could have ripgrep enable it whenever `--null-data` is enabled.

(GNU grep's DFA implementation takes this route as well. It can be configured to have `\0` be a line terminator.)

---

_Closed by @BurntSushi on 2020-02-16 16:58_

---

_Label `bug` added by @BurntSushi on 2020-02-16 16:59_

---

_Label `wontfix` added by @BurntSushi on 2020-02-16 16:59_

---
