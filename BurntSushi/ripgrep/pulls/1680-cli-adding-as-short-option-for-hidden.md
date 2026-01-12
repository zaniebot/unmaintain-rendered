```yaml
number: 1680
title: "cli: adding -. as short option for --hidden"
type: pull_request
state: closed
author: marcospb19
labels:
  - rollup
assignees: []
base: master
head: hidden-flag-short-option
created_at: 2020-09-10T23:43:31Z
updated_at: 2021-06-01T01:51:23Z
url: https://github.com/BurntSushi/ripgrep/pull/1680
synced_at: 2026-01-12T18:23:14Z
```

# cli: adding -. as short option for --hidden

---

_@marcospb19_

There was no short option for `--hidden`, I assume because `-H` is already taken by `--with-filename` from the original `grep`, and `-h` should always be `--help`.

The fact is, a lot of times when searching recursively in a directory, `--hidden` is needed to show files that start with an `.` (dot).

So doesn't it make sense to use a dot for the short flag?
`rg -. PATTERN`.

The only software I'm aware that uses `-.` is `feh`, however there seems to be no semantic relation with the dot itself, in contrast with `ripgrep`'s.

---

_Comment by @marcospb19 on 2020-09-10 23:46_

Hey, thanks for the great software, I'll understand if think that this flag is weird, it was also weird for me at first glance, so no hard feelings if you just decline it :smiley: .

Also, I'm not sure if `-.` is portable.

---

_Comment by @BurntSushi on 2020-09-11 13:17_

I like the cleverness of this, but I'm not sure how I feel about it. It pushes my weirdness budget, personally. I am potentially in favor of having a short flag for `--hidden`, although personally, I tend to use `-uu` instead. However, this does have the side effect of also ignoring gitignore files, which I suppose may be undesirable. But usually if I want to search hidden files, I'm probably wanting to search things in gitignore too.

As far as portable, `-.` wouldn't have any technical issues with being portable. With that said, using a leading `.` to indicate that a file is hidden is a Unix convention, and I don't know how Windows users would feel about it. (On Windows, a file being hidden is I think an actual attribute of the file's metadata.)

@okdana What are your thoughts on this?

---

_Comment by @okdana on 2020-09-11 18:29_

I think `-a` is the closest thing to a standard for this, but obv that's taken. `-.` isn't a portability issue for `rg` itself (as long as you continue to use an argument-processing library that supports it), but it's possible that some external software like wrapper scripts and completion functions might not handle it well? I'm pretty sure zsh will, don't know about bash, fish, or anything Windows-related. `.` isn't special to any shell, though, afaik, so it does have that going for it.

I don't know. There's definitely a logic to it, but it's weird. And it sets a precedent.

---

_Comment by @marcospb19 on 2020-09-12 00:42_

@BurntSushi and @okdana, thanks for the responses!

> Is it possible that some external software like wrapper scripts and completion functions might not handle it well? I'm pretty sure zsh will, don't know about bash, fish, or anything Windows-related.

I assume that everything will work with `zsh`, `bash` and `fish`, I also tried searching for [feh](https://github.com/derf/feh) issues with the `-.` flag in the whole git history and issues/PRs sections, and I found nothing, but again, I know nothing about the `Windows` side of it :disappointed: .

---

> As far as portable, -. wouldn't have any technical issues with being portable. With that said, using a leading . to indicate that a file is hidden is a Unix convention, and I don't know how Windows users would feel about it.

I personally think that offering a short flag as an extra option of usage will always be better than not offering a short flag option at all, if `unix` users can use it and enjoy it, that's a great thing.

It would be even better if someone could figure out a better option that could satisfy all the user base, however I'm out of ideas (I would suggest something with `clap`'s short options aliases, but... this feature does not exist yet, weird).

---

_@BurntSushi approved on 2021-05-30 16:34_

While I'm still a little unsure, I decided to accept this. I like the connection between `-.` and hidden files, even though it is a bit Unixy.

---

_Label `rollup` added by @BurntSushi on 2021-05-30 16:34_

---

_Closed by @BurntSushi on 2021-06-01 01:51_

---
