```yaml
number: 689
title: glob + respect ~/.ignore?
type: issue
state: closed
author: ptzz
labels: []
assignees: []
created_at: 2017-11-21T22:20:29Z
updated_at: 2017-12-11T18:43:25Z
url: https://github.com/BurntSushi/ripgrep/issues/689
synced_at: 2026-01-12T16:13:22Z
```

# glob + respect ~/.ignore?

---

_@ptzz_

With `*.png` in my `.ignore` file, the output of `ag -g foo` will not contain any png files. Is there a way to achieve the same with `rg`? `rg --iglob '*foo*' --files` lists png matches (version 0.7.1).

On a sidenote: https://github.com/sharkdp/fd is an rg-inspired tool aimed as a replacement for `find`. Is there any fundamental reason `rg` could not be as fast when used for the same purpose (using the `-g/--iglob` and `--files` options)?

---

_Comment by @okdana on 2017-11-21 23:00_

Per the man page:

>-g, --glob GLOB ...  
>Include or exclude files for searching that match the given glob. **This always overrides any other ignore logic if there is a conflict**, but is otherwise applied in addition to ignore files (e.g., .gitignore or .ignore). Multiple glob flags may be used. Globbing rules match .gitignore globs. Precede a glob with a '!' to exclude it.

So in your case you would need to do something like `rg -g '*foo*' -g '!*.png'` (the order is significant)

---

_Comment by @BurntSushi on 2017-11-21 23:53_

@okdana has it right. Example:

```
$ mkdir /tmp/ripgrep-689
$ cd /tmp/ripgrep-689
$ echo '*.png' > .ignore
$ touch foo.png bar.png
$ rg --files
$ rg --files -g '*foo*'
foo.png
$ rg --files -g '*foo*' -g '!*.png'
$ rg --files -g '!*.png' -g '*foo*'
foo.png
```

In the future, please provide reproducible examples like this.

Otherwise, this seems to be working as intended.

> On a sidenote: https://github.com/sharkdp/fd is an rg-inspired tool aimed as a replacement for find. Is there any fundamental reason rg could not be as fast when used for the same purpose (using the -g/--iglob and --files options)?

`fd` literally uses the same code to traverse a directory as ripgrep does, so I imagine their speed is comparable. Your question however implies you believe the speed is not comparable, but you didn't explain why. If you're noticing a substantial speed difference, please open a new issue and **provide a reproducible example on a corpus we both have access to.**

---

_Closed by @BurntSushi on 2017-11-21 23:53_

---

_Comment by @ptzz on 2017-11-22 04:46_

Got it, thanks for the quick reply.

I'm looking for using `rg` instead of `ag -g foo` or `fd foo`, i.e. excluding all files matched by `.ignore`. If I understand you correctly, the only way to achieve this with `rg` is to pass multiple `-g` options that negates each entry in the `.ignore` file.

Would simplifying this use case be a useful addition to `rg`?

> -g, --glob GLOB ...
> Include or exclude files for searching that match the given glob. This always overrides any other ignore logic if there is a conflict,

Perhaps an option to not override other ignore logic in case of conflict? That would be functionally equivalent (I think) to applying each glob (whether from `.ignore` or command line) sequentially in a pipeline, each glob reducing the output of the previous glob. Order would not matter.

---

_Comment by @BurntSushi on 2017-11-22 12:08_

No, sorry, I don't buy it. The whole point of the `-g` flag is that it overrides everything else. If you really need this feature, then stick to `ag` or `fd`.

> If I understand you correctly, the only way to achieve this with rg is to pass multiple -g options that negates each entry in the .ignore file.

That's the only way to guarantee it. But `-g` only overrides everything else when there is a conflict. Otherwise, your ignore rules are still respected.

A workaround is to use pipes if you only care about listing files. e.g., `rg --files | rg pattern`.

> Perhaps an option to not override other ignore logic in case of conflict? That would be functionally equivalent (I think) to applying each glob (whether from .ignore or command line) sequentially in a pipeline, each glob reducing the output of the previous glob. Order would not matter.

There is no sensible specification I can think of where order does not matter. Please see the rules here: https://docs.rs/ignore/0.3.1/ignore/struct.WalkBuilder.html#ignore-rules --- The simplest way to accomplish what you want is to change the precedence logic documented in that link such that glob overrides are checked after everything else. I do not want to add an option that enables that behavior. The rules already too complex.

---

_Comment by @ChengCat on 2017-11-27 04:45_

When would you want to override .gitignore with a command line argument? This is a surprising design. More frequently, one would want to add additional filter conditions on the command line in addition to the default output, just like the issue creator.

I really like the performance of this program, and that it is written in Rust. But I am disappointed that it doesn't provide sane defaults when I know more about it. Other points I feel surprising are:

* Not following symlinks by default (#692)
* Enabling .gitignore and .ignore by default is surprising. I expect a simple tool like `ripgrep` to be dumb
* As pointed out by many people on Hacker News(https://news.ycombinator.com/item?id=12564442), the name .ignore is too generic
* The filetype feature introduces too much ambiguity for a simple tool like this. It also makes the program stateful, so that output on different machines may differ, and cannot be depended upon.
* Search case sensitively by default. I would expect a modern tool to use smart case by default
* The output is not sorted by default
* The command line option name `--text` is confusing

---

_Comment by @BurntSushi on 2017-11-27 05:08_

I don't have the time or motivation to respond to all of your complaints. Every single one of them has been covered in the issue tracker, and I see no point in relitigating them.
Most of them can be fixed by setting your own defaults. (One exception is the `--text` flag. You are the first person to complain about that, but the flag name is taken directly from grep.)

Perhaps it is worth explicitly saying this: the default behavior of ripgrep matches a best guess at what is most convenient for some subset of users. For example, file type matching is a much loved feature, and no amount of whining about the program being "stateful" is going to change that. It is also worth pointing out that we have no hard data on what people find surprising or not intuitive, so it will always come down to subjective assessment. My preferred method of resolving such things is somewhat arbitrary, but prioritizes maintenance convenience. That is, I listen to end user feedback and adjust my mental model of what I think works best for users. I've reversed plenty of closed tickets in the past as a result of this, even for things that I said were never going to happen, like multiline search.

The bottom line is that you complaining about almost every default behavior isn't going to get us anywhere. In particular, when you complain about ripgrep defaulting to using gitignore by default, it IMO shows that you have fundamentally missed the point of ripgrep. Maybe you want a dumb search tool. That's fine if you do. But that is **decisively** not what ripgrep is (although you can make it dumb by disabling much of its default behavior). Arguing to make ripgrep dumb by default is on par with asking me to fundamentally change the goals of the project. **I'm begging you, please understand that this is a waste of time for everyone involved.**

Finally, if ripgrep's defaults bother you this much and explicitly disabling them isn't an option for whatever reason, then one thing is very clear: ripgrep is not the right tool for the job you have in mind **and that is okay.**

---

_Comment by @ChengCat on 2017-11-27 05:47_

Well, so most of my comments were agreed by others, and therefore they are valid to some extent. Perhaps explaining why I and others may expect ripgrep to be dumb helps. When I first heard about it, it is intended as a modern replacement for `grep` with better user friendliness and better performance. `grep` is a dumb tool, and its output is often depended upon in scripts. To replace such a tool like `grep`, reliability must be seriously considered. If it was advertised as another code search tool to replace `ag`, I wouldn't be so picky.

Don't get upset with my comments. I actually much agree with the two most fundamental defaults: search recursively and ignore binary files. Despite the disagreements, you developed and open sourced this program in the first place, we need to say thank you.

---

_Comment by @BurntSushi on 2017-11-27 12:52_

> If it was advertised as another code search tool to replace ag, I wouldn't be so picky.

It is! Literally the first two sentences of the README:

> ripgrep is a line-oriented search tool that recursively searches your current directory for a regex pattern while respecting your gitignore rules. To a first approximation, ripgrep combines the usability of The Silver Searcher (similar to ack) with the raw speed of GNU grep. ripgrep has first class support on Windows, macOS and Linux, with binary downloads available for every release.

I can't control how people advertised ripgrep to you. Also, to some people, it is a grep replacement. Reality is shades of grey, and advertising is an especially grey area. Advertising often sacrifices accuracy for brevity. However, if you had read the README or the blog post introducing ripgrep, then it should have been very clear that respecting gitignore by default is a fundamental design decision.

> Don't get upset with my comments. 

First of all, don't tell me how to feel. Second of all, I'm not upset, I'm frustrated that you're continuing to waste my time. Allow me to be super clear: I consider any further complaints or discussion about changing the fundamental design goals of ripgrep to be in extremely poor taste. (Not all of your complaints fit in this bucket, but some do.)

---

_Comment by @ChengCat on 2017-11-28 02:12_

I acknowledge that I could be wrong somewhere when I learned about ripgrep. However, I believe the name ripgrep is a bit misleading, it sounds like a variant of grep.

Sorry, but that meant no offense. Also my intention is not to waste your time, or my time. My original intention was to contribute useful discussions that may help ripgrep to be a better alternative of grep. Now I learned that I should just give up on that.

---

_Comment by @BurntSushi on 2017-11-28 02:24_

@ChengCat I understand. I would strongly recommend that you try to step in my shoes. I have an issue tracker to maintain and triage, with tons of users with various desires. If they all started clogging up the issue tracker with comments like yours, we'd get nothing to done. It's great that you want to help improve ripgrep, but you need to be more respectful of peoples' time and understand that **reasonable people can disagree**. An incremental step toward satisfying your use case, for example, is not to change the ripgrep defaults (which, by now, has a ton of inertia behind it making them unlikely to change) but to make it easier for you to change the defaults for your own uses of it. There are definitely problems with the latter as of today that I'd like to fix!

---

_Comment by @flip111 on 2017-12-11 18:43_

Would be nice to have a new flag for this purpose

---
