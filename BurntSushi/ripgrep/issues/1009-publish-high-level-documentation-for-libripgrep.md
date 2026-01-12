```yaml
number: 1009
title: publish high level documentation for libripgrep (cookbook + guide)
type: issue
state: open
author: jessegrosjean
labels:
  - question
  - libripgrep
assignees: []
created_at: 2018-08-08T15:19:45Z
updated_at: 2023-07-04T17:17:23Z
url: https://github.com/BurntSushi/ripgrep/issues/1009
synced_at: 2026-01-12T16:13:22Z
```

# publish high level documentation for libripgrep (cookbook + guide)

---

_@jessegrosjean_

#### What version of ripgrep are you using?

https://github.com/BurntSushi/ripgrep/tree/ag/libripgrep-freeze-2

#### Describe your question, feature request, or bug.

I'm excited to see the [libripgrep-freeze-2 work](https://github.com/BurntSushi/ripgrep/tree/ag/libripgrep-freeze-2). I'd love to see a best practices example that recreates `rg --count` with the lib.

I've created a starting point here:

https://github.com/jessegrosjean/libripgrep_example

But I'm new to rust as of yesterday, and don't have a good idea of what I'm doing. I hope someone can take a look at that code and let me know how to improve it. And eventually I hope same/similar code will get included as an example with libripgrep.


---

_Comment by @BurntSushi on 2018-08-08 15:32_

I'm excited that you are already trying to jump into libripgrep, but it isn't on master yet and thus still in development. I created those branches for folks to try out some new ripgrep features but don't really intend it to be anything more than that.

It is indeed the case that a major part of the remaining work is doumentation and examples, among other things, such as additional integration tests to cover new features. There is already a fair bit of docs, but there is basically no high level documentation.

I will take a look at your code and give feedback when I get a chance, but I don't want to get too caught up with thia before I'm actually ready to publish libripgrep.

---

_Comment by @BurntSushi on 2018-08-08 17:22_

OK, this was a fun exercise. This is the first time I got to really play with the aggregate libripgrep (outside of plugging it into ripgrep, which is... a more mechanical process).

I have three revisions for you. Each one has a different flavor, depending on what you're trying to do.

The first is a mostly cleaned up version of yours: https://gist.github.com/BurntSushi/0c567907ae83594fe377d0d858133d09

The high level changes:

* I switched your `MsQueue` to `crossbeam-channel`, which has a clean API and should be the default channel implementation that folks reach for. (ripgrep is still using MsQueue. I'd like to move it to crossbeam-channel, but it brings in a number of new dependencies that seem to belong to a part of the ecosystem that doesn't prioritize supporting older Rust compilers.)
* I removed uses of `grep-*` crates and instead funneled everything through `grep` re-exports. (My intent here is to push folks toward use of the `grep` crate for most uses, and I expect that's where a lot of the high level docs will live.)
* I added a bit more error handling. Some were being ignored and others would result in panics. There isn't a ton of flexibility here for error handling during search. Either you print the error message to stderr and move on, or you quit the whole thing. I opted to do what ripgrep does: print the error and move on.
* I also limited searches to only attempt to search files. This excludes directories, which your program attempted to search. This also excludes symlinks though.
* I made two performance tweaks. First, I used `RegexMatcher::new_line_matcher` instead of `RegexMatcher::new`. The former should be used whenever you only care about matches spanning at most one line. Secondly, I disabled the use of memory maps. Memory maps (at least on Linux) are a somewhat niche optimization that really only holds its own when searching a small number of very large files. On some platforms, their worse and on others, they're better. In some work loads---like searching a whole bunch of tiny files---memory maps can be dramatically slower. For example, not enabling memory maps makes **your program run literally as fast as ripgrep does on the Linux kernel repo.** (This is a key goal I wanted to accomplish: the ability to write small programs with ripgrep-level speed by default.)

Here's revision 2: https://gist.github.com/BurntSushi/4486b6f28bc26e1700695437bb542bcd

This is pretty similar to the first revision, but I removed your custom implementation of `Sink` (nice work on figuring that out!) and replaced it with a built-in implementation of `Sink` that happens to give you what you need here. It does force you to enable line counting, which is in theory slower, but is generally not noticeable when searching across lots of small files. And even when it is noticeable, it's typically not that big of a difference.

The final revision is a bit of a cheat: https://gist.github.com/BurntSushi/fa04b2cc0b194e1be7399721f1711394

This revision is actually probably the _ideal_ answer to your query of "re-create a simple `rg --count` implementation." (I note that the first two revisions, along with your code, actually implement `rg --count-matches`. The third revision implements `rg --count` specifically, which can be faster.)

Basically, the third revision uses the `grep-printer` crate, which comes with its own `Summary` printer. This is what powers the output of flags like `--count`, `--count-matches`, `--files-with-matches` and `--files-without-match`. This revision _also_ includes automatic cross platform color support.

All revisions perform about the same for me on my system on the Linux kernel repo, and they also match ripgrep's performance. Yay!

---

_Comment by @BurntSushi on 2018-08-08 17:27_

@jessegrosjean Sorry I didn't add this in my previous comment, but I'm super impressed you were able to get a working program while also being simultaneously new to Rust and working without any high level docs or examples. I've toiled quite a bit over the various APIs and worry quite a bit whether something is more complex than it needs to be. Abstraction has steep costs...

---

_Comment by @jessegrosjean on 2018-08-08 18:04_

I'm just headed away from computer, but looking forward to digging through all this in a day or two when I get back. Thanks so much!

---

_Label `question` added by @BurntSushi on 2018-08-09 14:18_

---

_Comment by @jessegrosjean on 2018-08-09 18:15_

> This is pretty similar to the first revision, but I removed your custom implementation of Sink (nice work on figuring that out!) and replaced it with a built-in implementation of Sink that happens to give you what you need here.

Bytes simplifies things a lot.

I actually started with that approach, but decided I was in the wrong place because I was expecting some solution that would give me a list of matching ranges in a callback. Then I started digging into the other Sink implementations since I knew they generated ranges somehow. And that's where I eventually learned that I would need to run the matcher over the given content to get the ranges... but by that time I'd forgotten that I was already in that position when I started with Bytes.

Makes sense now, I wouldn't want to have to pay the cost of iterating over those matches if I didn't need them. Helper `submatches()` function on Sink would have allowed me to figure things out easier?

>  your program run literally as fast as ripgrep does on the Linux kernel repo.

Yeah, I love this! And actually it runs a bit faster since I can call it through a C API directly and don't need to worry about firing up a subprocess and parsing text results. Also I get to add filename matching logic to the same pass instead of having to perform that logic in a second pass. So for my case I'm getting more then 2x performance boost. :)

> @jessegrosjean Sorry I didn't add this in my previous comment, but I'm super impressed you were able to get a working program while also being simultaneously new to Rust and working without any high level docs or examples. I've toiled quite a bit over the various APIs and worry quite a bit whether something is more complex than it needs to be. Abstraction has steep costs...

Ha, thanks. From my copy past and modify marathon I think the basic abstractions are pretty good. I had a whole lot of pain and confusion trying to figure out how rust in general worked, but the particular ripgrep API that I needed (minus the Bytes thing I guess) was pretty strait forward.

As far as ripgrep related code the part where I got most confused was with how the parallel walker worked. The actually documentation explains it pretty well. But I was just copy/pasting code and was very confused with this basic structure:

```
parallel_walker.run(move || {
        ...
        Box::new(move |result| {
            ...
        }
}
```

I didn't realize for a long time that the outer part was called only once per thread, and the inner part was called multiple times on that same thread.

The reason this wasn't obvious is because I didn't realize the outer part was returning the inner closure until long into the process. Especially hard I think when that closure has more then a few lines of logic.

It wasn't until I got things running in debugger that I figured out what was happening. And then to really finish things off I decided to read the docs, and that made it clear. I'm not sure you can do anything about it, seems like you are just following rust style, but an explicit `return Box::new(move |result| {` would have made things obvious and saved me a bunch of time I think.

---

_Comment by @BurntSushi on 2018-08-09 18:33_

RE parallel walker: yes, I absolutely hate the API. Nested closures are just not good. I believe there's another issue tracking some ideas there, but yeah, that definitely needs rethinking but it probably won't be something I tackle for a while. I think in general the `ignore` crate needs a rethink, including its implementation. It is not easy code to maintain.

---

_Renamed from "libripgrep example" to "public high level documentation for libripgrep (cookbook + guide)" by @BurntSushi on 2018-08-19 13:52_

---

_Renamed from "public high level documentation for libripgrep (cookbook + guide)" to "publish high level documentation for libripgrep (cookbook + guide)" by @BurntSushi on 2018-08-19 13:52_

---

_Added to milestone `libripgrep` by @BurntSushi on 2019-01-27 18:16_

---

_Label `libripgrep` added by @BurntSushi on 2019-01-27 18:16_

---

_Comment by @NightMachinery on 2020-05-22 13:44_

Should I parse the json output now if I want to use ripgrep in rust? My main goal is customizing the output format so I can parse the cleaned result easily with `fzf`'s `--with-nth`.

---

_Comment by @BurntSushi on 2020-05-22 13:53_

@NightMachinary I think Discussions would be a better place for questions like that: https://github.com/BurntSushi/ripgrep/discussions/1592

---
