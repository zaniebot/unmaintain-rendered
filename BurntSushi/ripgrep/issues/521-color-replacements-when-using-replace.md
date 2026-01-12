```yaml
number: 521
title: Color replacements when using --replace
type: issue
state: closed
author: lydell
labels:
  - enhancement
assignees: []
created_at: 2017-06-17T12:24:18Z
updated_at: 2017-10-08T12:01:12Z
url: https://github.com/BurntSushi/ripgrep/issues/521
synced_at: 2026-01-12T16:13:22Z
```

# Color replacements when using --replace

---

_@lydell_

ripgrep colors matches in red (by default). That's very useful.

When using the `--replace` option, I would find it very useful if the replacements were colored. Currently, nothing is colored (except file paths and line numbers of course).

Here's a screenshot showing what I mean:

![screenshot from 2017-06-17 14-21-23](https://user-images.githubusercontent.com/2142817/27252839-4bf37a2e-5368-11e7-9f66-5adf9f90a67e.png)

And here's a text sketch:

```
$ cat test
exponential notation: 123e4
another example: 42e-10
$ rg '(\d+)e' --replace '${1}E' test
1:exponential notation: <color>123E</color>4
2:another example: <color>42E</color>-10
```

What do you think?

(By the way, thank you very much for ripgrep. It's excellent.)

---

_Label `enhancement` added by @BurntSushi on 2017-06-17 13:11_

---

_Comment by @BurntSushi on 2017-06-17 13:12_

Agreed. I like it!

---

_Comment by @okdana on 2017-06-19 11:21_

Maybe a new colour spec? So you could do like `--colors 'replace:fg:red'` (with the default being identical to the default `match` spec)?

---

_Comment by @BurntSushi on 2017-06-19 11:26_

@okdana I'd rather not add a new color spec. We should just use the existing `match` spec. When `-r/--replace` is used, all matches are replaced, so there's never a need to distinguish between the replacement color and the match color.

---

_Comment by @okdana on 2017-06-19 11:27_

Fair.

I was thinking there would be some cases where you didn't want colouring, like when your replacement affects the entire line, but on balance an additional colour spec doesn't really help you there. If you ever have a situation like that where don't want the colours, you'll still have to pass `--colors` to disable them, and if you're doing that anyway you can just as well turn off `match`.

I retract my suggestion

---

_Closed by @BurntSushi on 2017-10-08 12:01_

---
