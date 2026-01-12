```yaml
number: 368
title: images being searched?
type: issue
state: closed
author: ryanong
labels: []
assignees: []
created_at: 2017-02-17T15:58:07Z
updated_at: 2017-02-17T16:45:54Z
url: https://github.com/BurntSushi/ripgrep/issues/368
synced_at: 2026-01-12T16:13:21Z
```

# images being searched?

---

_@ryanong_

I was reading in the documentation binary files are skipped. Are images considered binary or not?

Other filetypes being search which I would consider binary:
font files
xls files

---

_Comment by @BurntSushi on 2017-02-17 16:01_

Binary detection is, at best, a heuristic. The heuristic is similar to what GNU grep does. If the file contains a `NUL` byte, then the file is treated as a "binary" file. There is however a bug associated with detection that causes it to be too conservative. See #52.

Could you please provide more details on the problem you're having? In particular, can you provide a small reproducible example?

---

_Comment by @ryanong on 2017-02-17 16:09_

yea sure
download https://upload.wikimedia.org/wikipedia/commons/8/85/Capybara_portrait.jpg into a temp folder and run `rg --files` and it comes up as a valid searchable file.

I'm on `ripgrep 0.4.0`

---

_Comment by @BurntSushi on 2017-02-17 16:11_

> and it comes up as a valid searchable file

That's because it is. The only way to know whether a file is binary or not is... to search it.

---

_Closed by @BurntSushi on 2017-02-17 16:11_

---

_Comment by @ryanong on 2017-02-17 16:12_

fair point. I guess I was just expecting functionality similar to ag
https://github.com/ggreer/the_silver_searcher/blob/b5e750a42c28d0ac1f5acd6ac82cfdd8518c732e/src/util.c#L351 but I I understand that ripgrep is not ag

---

_Comment by @BurntSushi on 2017-02-17 16:17_

> I guess I was just expecting functionality similar to ag

It is similar. In fact, with your example file, the behavior is nearly identical. You're using the `--files` flag which say "print all files that ripgrep **will search**." Watch:

```
$ mkdir ripgrep-368
$ cd ripgrep-368
$ curl -sO 'https://upload.wikimedia.org/wikipedia/commons/8/85/Capybara_portrait.jpg'
$ rg purl
$ rg -a purl
Capybara_portrait.jpg
24:            xmlns:dc="http://purl.org/dc/elements/1.1/">
$ ag purl
$ ag -a purl
Binary file Capybara_portrait.jpg matches.
$ rg --files
Capybara_portrait.jpg
$ ag -g .
Capybara_portrait.jpg
```

Both tools emit `Capybara_portrait.jpg` as files that will be searched, but both tools ignore that file when actually executing the search.

---

_Comment by @ryanong on 2017-02-17 16:20_

Ah cool! thanks for making an awesome tool and taking the time to explain.

The reason I brought this up was I am trying to figure out why AG is faster searching my repo and I doubt that ag is faster than rg at individual files.

---

_Comment by @BurntSushi on 2017-02-17 16:25_

When you say "ag is faster," do you mean "it beat ripgrep by a few milliseconds on my tiny repo"? Or do you mean "it beat ripgrep by several seconds on a very large repo"?

If the former, then that's not too interesting. At that point, we're talking about process overhead and the cost of spinning up threads.

If the latter, then it's hard to say without a more thorough analysis. If you could reproduce your results on a repo we can both access, then I could probably tell you exactly what's going on if you're curious.

---

_Comment by @ryanong on 2017-02-17 16:31_

The later, ag does it in about a second and ripgrep takes about 5-10 seconds. I'll attempt to make a reproducible repo. I'll open a new issue with step by step replication.

---

_Comment by @BurntSushi on 2017-02-17 16:38_

@ryanong Wow that's a big difference! Thanks for the attempt. :-)

(The first thing I'd check for is whether they are searching the same files. ag's gitignore support isn't great, so if it's accidentally ignoring some large files or something, that might explain things.)

---

_Comment by @ryanong on 2017-02-17 16:42_

yep that is what I was thinking, which is why I thought images were being searched.
I compared `ag -l` to `ripgrep --files` and the only thing that stuck out were images

---

_Comment by @BurntSushi on 2017-02-17 16:43_

@ryanong You probably want to compare `ag -l` with `rg -l`. :-)

---

_Comment by @ryanong on 2017-02-17 16:44_

`ag -l` without an argument lists all files. `rg -l` requires an argument

---

_Comment by @ryanong on 2017-02-17 16:45_

I just did `rg -l ""` and that worked. there is no difference in files :/ I'll keep poking around

---

_Comment by @BurntSushi on 2017-02-17 16:45_

@ryanong Try `ag -l '^'` vs `rg -l '^'`.

---

_Comment by @BurntSushi on 2017-02-17 16:45_

ah yeah, weird. best bet is to find a reproduction we can both see i guess. thanks for your patience :-)

---
