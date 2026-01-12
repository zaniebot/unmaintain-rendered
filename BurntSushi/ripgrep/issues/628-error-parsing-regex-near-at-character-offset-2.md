```yaml
number: 628
title: "Error parsing regex near '(?<=\\{\\' at character offset 2: Unrecognized flag: '<'. (Allowed flags: i, m, s, U, u, x.)"
type: issue
state: closed
author: nbari
labels: []
assignees: []
created_at: 2017-10-09T08:29:45Z
updated_at: 2017-10-09T10:46:58Z
url: https://github.com/BurntSushi/ripgrep/issues/628
synced_at: 2026-01-12T16:13:22Z
```

# Error parsing regex near '(?<=\{\' at character offset 2: Unrecognized flag: '<'. (Allowed flags: i, m, s, U, u, x.)

---

_@nbari_

Having this input:

    {{{
          How 
          R 
          U
    }}}

    {{{ Hi }}}

And Wanting the output:

     How
     R
     U
    
     Hi

I tried to use this regex: `[^{\}]+(?=})`, the one after testing in python:

```python
>>> import re
>>> p = re.compile('[^{\}]+(?=})')
>>> foo = '''
... {{{
...       How 
...       R 
...       U
... }}}
... 
... {{{ Hi }}}'''
>>> p.findall(foo)
['\n      How \n      R \n      U\n', ' Hi ']
```

I tested also here: https://regex101.com/r/ASFIFa/1

Next, I wanted to give a try to `rg` using something like this:

      rg '[^{\}]+(?=})' input.txt

or 

    rg -e '[^{\}]+(?=})' input.txt

But always get this:

    Error parsing regex near '}]+(?=})' at character offset 9: Unrecognized flag: '='. (Allowed flags: i, m, s, U, u, x.)

With `ag` seems to be working:

```
$ ag -o '[^{\}]+(?=})' input.txt

      How 
      R 
      U

 Hi 
```

Any idea about how could I use `rg` to get the desired output?

Thanks in advance.

---

_Comment by @okdana on 2017-10-09 09:04_

A few problems:

1. `rg` returns whole lines by default rather than just the matches.

2. The regex engine `rg` uses doesn't support look-around assertions.

3. `rg` is line-based — essentially it breaks the input up into lines and searches each one separately. It can not match across lines.

You can use `--replace` in many cases to simulate the effect of look-around (in this case by deleting the closing brace)... but there is no way to get around the third issue given that exact input. You might be able to massage it a little with `perl` or `sed`, but that would only work on a per-file basis obv.

Another option would be to use `ag`, which does support look-around and multi-line matching:

```
% cat test.txt
{{{
      How 
      R 
      U
}}}

{{{ Hi }}}
% ag -o '[^{\}]+(?=})' test.txt

      How 
      R 
      U

 Hi 
```

edit: Oops, i missed that you already tried `ag`. Disregard that bit i guess.

---

_Comment by @BurntSushi on 2017-10-09 10:46_

Thanks @okdana!

@nbari This behavior is documented in the README: https://github.com/BurntSushi/ripgrep#why-shouldnt-i-use-ripgrep

---

_Closed by @BurntSushi on 2017-10-09 10:46_

---
