```yaml
number: 96
title: Adjust default Colour Scheme to suit both black and white terminal backgrounds
type: issue
state: closed
author: cetra3
labels:
  - question
assignees: []
created_at: 2016-09-26T02:34:35Z
updated_at: 2016-09-27T22:14:29Z
url: https://github.com/BurntSushi/ripgrep/issues/96
synced_at: 2026-01-12T18:23:11Z
```

# Adjust default Colour Scheme to suit both black and white terminal backgrounds

---

_@cetra3_

Not sure whether this can be counted as an _actual_ issue, but it looks like the default colours are more geared towards having a black background in your terminal, which can incur some readability when using a different terminal background colour.

Compare the two outputs between `ag` and `rg`:

![screen shot 2016-09-26 at 11 58 45 am](https://cloud.githubusercontent.com/assets/6415435/18820898/a80922d0-83e0-11e6-9c81-04b3d354f31a.png)

As you can see `ag` is a bit more readable here, especially on file names.

I'm not advocating to copy silver searcher's colour scheme exactly, but it would be nice if the defaults worked nicely with both.


---

_Comment by @BurntSushi on 2016-09-26 02:40_

I actually very much dislike `ag`'s color scheme (the background color on the match makes it very hard for me to read), and I use a terminal with a white background. The screenshot in the README is my environment:

[![A screenshot of a sample search with ripgrep](http://burntsushi.net/stuff/ripgrep1.png)](http://burntsushi.net/stuff/ripgrep1.png)

I see that your `green` is very very bright. Is that the problem for you? If so, you might want to tweak your terminal so that your green isn't so... fluorescent.


---

_Label `question` added by @BurntSushi on 2016-09-26 02:45_

---

_Comment by @BurntSushi on 2016-09-26 02:47_

One thing I missed is that `ag`'s green is actually darker for you. That's interesting. I wonder if it's because [I'm using `BRIGHT_GREEN`](https://github.com/BurntSushi/ripgrep/blob/master/src/printer.rs#L280), which I did because that makes it bold on Windows. It looks like I should probably use `GREEN` instead on Unix and only use the `BRIGHT_*` variants on Windows?


---

_Comment by @cetra3 on 2016-09-26 02:47_

Yeah, I am not exactly married to ag's colour scheme either.

The `Bright Green` I have is the OSX Default in the Terminal Menu (0, 255, 0).  It does look a lot better in your screenshots though, so maybe I have some colour profile set somewhere else.

I've adjusted it in my [fork](https://github.com/cetra3/ripgrep/commit/b48914fa84f9c591e8f2739b4e2e86267402dbde) to use the normal colours rather than bright (i.e, `Green` is (0, 166,0)) and it appears to look a bit nicer for my setup, but I am now questioning whether my setup is standard or not:

![screen shot 2016-09-26 at 12 17 09 pm](https://cloud.githubusercontent.com/assets/6415435/18821206/30ca2e1e-83e3-11e6-8abe-874159c7eda6.png)


---

_Comment by @BurntSushi on 2016-09-26 02:48_

@cetra3 Yup, we just discovered the same thing at the same time. :-) `rg` should switch to `GREEN` but keep `BRIGHT_GREEN` on Windows. I misdiagnosed: I think your setup is fine.


---

_Comment by @cetra3 on 2016-09-26 03:06_

At, It would be easy to add some `#[cfg(windows)]` where needed if you'd want a PR for this.

Otherwise I think maybe including it in as part of [this issue](https://github.com/BurntSushi/ripgrep/issues/51) more properly is the way to go


---

_Comment by @BurntSushi on 2016-09-26 03:25_

I'd be fine with some judicious use of cfg directives if you don't want to wait.


---

_Closed by @cetra3 on 2016-09-27 22:14_

---
