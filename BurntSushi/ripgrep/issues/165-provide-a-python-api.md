```yaml
number: 165
title: Provide a Python API
type: issue
state: closed
author: libratiger
labels: []
assignees: []
created_at: 2016-10-11T15:07:24Z
updated_at: 2016-12-30T16:38:27Z
url: https://github.com/BurntSushi/ripgrep/issues/165
synced_at: 2026-01-12T16:13:21Z
```

# Provide a Python API

---

_@libratiger_

The ripgrep is really awesome, If we can offer a Python API, maybe we can use ripgrep in a Wider area.


---

_Renamed from "Offer the Python API" to "Provide a Python API" by @libratiger on 2016-10-11 15:08_

---

_Comment by @BurntSushi on 2016-10-11 16:34_

I agree this would be very cool. Not even that, it should be eminently possible as well. [Someone is already working on a Python API for Rust's regex library.](https://github.com/davidblewett/regex/tree/rure-python/regex-capi/src/python)

With that said, the amount of work required to do this is quite a lot. It requires splitting everything out into a library, writing C interfaces and then writing idiomatic Python bindings. It's unlikely something I'll ever do personally, although I'd be happy to support such an effort.

I'd like to close this because this comes too far down on the side of wishlist as opposed to something that's likely to get worked on within the next _year_.


---

_Closed by @BurntSushi on 2016-10-11 16:34_

---

_Comment by @h5rdly on 2016-12-25 19:55_

Hi,

I have no understanding of Rust, but does this bindings library perhaps allow for the task you described to be easier? It would be very useful to have a Python api to use rg

https://github.com/dgrunwald/rust-cpython

---

_Comment by @BurntSushi on 2016-12-26 17:26_

@h5rdly I'm aware of that work and it doesn't necessarily make it easier. It depends on how you want to build the bindings. For example, if you use `cffi`, then I don't think there's any place for using something like `rust-cpython`. You can see how the regex bindings work here: https://github.com/davidblewett/rure-python/tree/master/rure

The hard work here is in crafting the C API, writing tests for it and then building a Pythonic interface to them.

---

_Comment by @h5rdly on 2016-12-29 07:02_

That seems like a pain. I think bindings would allow a much wider use of rg, especially because it's cross platform already, but I can hardly press the matter :)

---

_Comment by @BurntSushi on 2016-12-29 14:17_

Moreover, libripgrep isn't finished yet, so creating bindings to it today would be fruitless. 

Once libripgrep is done, this is a task i would be willing to mentor.

---

_Comment by @BurntSushi on 2016-12-29 14:19_

In the mean time, consider a lower tech solution: running the rg executable and parsing the results. It wouldn't be too hard to write a library for it!

---

_Comment by @h5rdly on 2016-12-30 16:38_

I figured the same, didn't want to say anything until I actually start :)

This is for a file manager I'm writing to teach myself python, and thought it'd be nice to have a powerful cross platform search tool as a backend. I'm just figuring out the basics, so this is a little down the road. 

I did put some thought into how I want to approach it, which is why I'm trying to convince @gpshead, the maintainer of subprocess32, to backport run(), which seems like a much more convenient function to work with than Popen() -
https://docs.python.org/3/library/subprocess.html#subprocess.run

so that such code is Python 2/3 compatible. Would be awesome if you could chime in.
https://github.com/google/python-subprocess32/issues/17	

---
