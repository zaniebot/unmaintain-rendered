```yaml
number: 248
title: Automatically determine an appropriate default color scheme on Windows
type: issue
state: closed
author: retep998
labels:
  - question
assignees: []
created_at: 2016-11-24T23:06:01Z
updated_at: 2017-11-05T13:26:39Z
url: https://github.com/BurntSushi/ripgrep/issues/248
synced_at: 2026-01-12T16:13:21Z
```

# Automatically determine an appropriate default color scheme on Windows

---

_@retep998_

By calling `GetConsoleScreenBufferInfoEx` you can inspect the `ColorTable` field to get the current 16 colors for the console. You can then do some fancy stuff using a crate like `palette` to pick colors that have sufficient contrast with the background color (convert from sRGB to CIELAB).

---

_Comment by @BurntSushi on 2016-11-26 03:07_

I would personally rather solve this issue by making it possible to just choose your scheme. Picking a scheme automatically seems like overengineering and just asking for trouble. See #196 for persistent config.

---

_Comment by @BurntSushi on 2016-11-26 03:08_

In particular, can you point to other CLI tools that implement the desired functionality?

---

_Label `question` added by @BurntSushi on 2016-11-27 00:17_

---

_Closed by @BurntSushi on 2017-01-07 03:47_

---

_Comment by @sergeevabc on 2017-11-05 09:44_

Some color changes happened this year, they became dull.
Is it possible to make filename and match colors more saturated to make it easier to distinguish?

ripgrep 0.7.0
![](http://images.vfl.ru/ii/1509874813/a36b32e4/19286891.png)

ripgrep 0.3.2
![](http://images.vfl.ru/ii/1509875980/5b3f2edb/19287161.png)

ag 2.1.0
![](http://images.vfl.ru/ii/1509875485/6028ac8f/19287051.png)

---

_Comment by @BurntSushi on 2017-11-05 12:00_

@sergeevabc That question seems unrelated to this issue? Please just open new issues in that case.

You can change colors with the `--colors` flag. Please read the documentation. There is even a tip in the README: https://github.com/BurntSushi/ripgrep#how-do-i-make-the-output-look-like-ags

---

_Comment by @sergeevabc on 2017-11-05 13:14_

Sorry if my decision to not create a duplicate turned out to be wrong. Am not so immersed in dev to say for sure whether question is related to what was discussed earlier or not, but @retep998 dreamed about colors with sufficient contrast on Windows and that dream seemed very in line with mine.

I searched for Ripgrep’s documentation on colors in the past, but found none. Tip about AG’s scheme on the frontpage is too fresh to be noticed, a 2 days old commit. Nevertheless I’m looking for more saturated (or [accessible][1] as would my ophthalmologist, a man of science, say) Ripgrep’s defaults out of the box (it promises sane defaults after all) and not the ability to make its output look like something other grep tool with a help of poorly memorized long sequence of switches. 

You almost had it in 0.3.2 (except for dull line numbers), but it’s gone. :disappointed:

[1]: https://encrypted.google.com/search?q=contrast+tool+accessibility

---

_Comment by @BurntSushi on 2017-11-05 13:17_

> I searched for the documentation on colors in the past, but found none. 

It's right there in the output of `rg --help`. As I said before, look for the `--colors` flag. It's also in the man page.

---
