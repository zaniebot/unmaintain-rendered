```yaml
number: 462
title: machine interface
type: issue
state: closed
author: albfan
labels: []
assignees: []
created_at: 2017-04-27T09:27:57Z
updated_at: 2017-04-27T11:18:37Z
url: https://github.com/BurntSushi/ripgrep/issues/462
synced_at: 2026-01-12T16:13:22Z
```

# machine interface

---

_@albfan_

Hi, I'm the maintainer of https://github.com/albfan/ag.vim

Someone ask me to integrate with rg, and after trying it is sensibly faster than ag so want to ask if it's possible to add an option to expose results in a parseable way ready for machines.

By now I'm parsing filename, column, line and contents from `--group` option so this is not a stopper, but would be great to offer results that way to ease maintenance and complex features like hightlight code

Maybe `--porceain` could be a great name for option. Anything will work

---

_Comment by @kpp on 2017-04-27 09:39_

How about existing `--vimgrep`?

---

_Comment by @tiehuis on 2017-04-27 10:37_

#244 is related. May be worthwhile to see if what is outlined there differs from your requirements.

---

_Comment by @albfan on 2017-04-27 10:46_

I use mostly search with context:

    $ rg foo -C4

Those contexts lines are really useful, so I really work on AgGroup (not the commands related with quicklist). Parsing results with context slows the interaction and is error prone

@tiehuis That would help to highlight without reprocess regex (with the escape characters hell)

Deterministic results like:

```
file: foo
   - matches: [
        {
            line: 1,
            col: 10,
            match: foo,
            before: 3,
            after: 4,
            result: "asdfasdfasdf
asdfsadfasdf
foo
asfasdfasdf
"         
 },
...
]
```

etc etc woud be great as there's lot of info rg knows but do not expose (reflected in highlights for example)

---

_Comment by @tiehuis on 2017-04-27 10:59_

Just to add, here (#359) was a similar issue regarding structured output (which looks very much the same as what you are suggesting) and a response from BurntSushi.

---

_Comment by @albfan on 2017-04-27 11:18_

@tiehuis Thanks that's exactly what I need.

I was afraid to ask for a library, but seems there's a work in progress. That's perfect for another idea I have for rg (use on gnome builder as a global search https://github.com/albfan/gnome-builder/issues/14)

Then we need to contribute on rg library. That makes sense, thanks!



---

_Closed by @albfan on 2017-04-27 11:18_

---
