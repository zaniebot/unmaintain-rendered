```yaml
number: 459
title: "Allow fetching matched data in `globset`"
type: issue
state: closed
author: hauleth
labels: []
assignees: []
created_at: 2017-04-26T00:22:35Z
updated_at: 2017-05-08T21:31:22Z
url: https://github.com/BurntSushi/ripgrep/issues/459
synced_at: 2026-01-12T16:13:22Z
```

# Allow fetching matched data in `globset`

---

_@hauleth_

I want to implement [Projectionist](https://github.com/tpope/vim-projectionist) in Rust however there is no functionality that allow me to load `*` part of `src/*.rs` glob match. It would be great addition to this library.

---

_Comment by @BurntSushi on 2017-04-26 01:58_

I don't understand this feature request, sorry. Could you please provide more detail, examples and the specific problem you're trying to solve?

---

_Comment by @hauleth on 2017-04-26 08:11_

Currently `globset::GlobMatcher` only have method to match against the path. However what I would also need is to have method that would return something like `Option<Match>` where I could extract the thing that was matched by `*` ONLY. That functionality is unavailabe in neither `glob` nor `globset`. I am currently on my mobile, so I will provide exact example later. 

--
Łukasz Jan Niemier

Dnia 26.04.2017 o godz. 03:58 Andrew Gallant <notifications@github.com> napisał(a):

> I don't understand this feature request, sorry. Could you please provide more detail, examples and the specific problem you're trying to solve?
> 
> —
> You are receiving this because you authored the thread.
> Reply to this email directly, view it on GitHub, or mute the thread.
> 


---

_Comment by @BurntSushi on 2017-04-26 11:42_

@hauleth Can you please describe the problem you're trying to solve? This description should not mention the `glob` or `globset` crates at all. In other words, *what is your specification*?

Can you also please provide an example of a globbing library that does what you want?

---

_Comment by @hauleth on 2017-04-26 14:18_

I do not know any glowing library that support such feature, but basically what I need is something to match path like `a/b/c/d.rs` to glob `a/*.rs` and be able to extract part `b/c/d` which is the part matched by `*` in glob pattern. I need this to provide functionality of Projectionist that allow me to provide "alternate" files for given file which simply are path substitution based on glob.

---

_Comment by @BurntSushi on 2017-04-26 14:23_

I don't know what Projectionist is, so mentioning it doesn't help me understand your feature request.

> I do not know any glowing library that support such feature

I also don't know of any globbing library that supports it. Probably because "capturing groups" aren't a thing in globs, so I don't see how you expect this to work at all.

---

_Closed by @BurntSushi on 2017-05-08 21:31_

---
