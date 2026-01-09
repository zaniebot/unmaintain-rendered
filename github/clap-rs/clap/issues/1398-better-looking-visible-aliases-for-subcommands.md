---
number: 1398
title: Better looking visible aliases for subcommands
type: issue
state: open
author: sondr3
labels:
  - C-enhancement
  - S-waiting-on-decision
  - A-help
assignees: []
created_at: 2018-12-18T23:15:27Z
updated_at: 2021-12-10T16:33:27Z
url: https://github.com/clap-rs/clap/issues/1398
synced_at: 2026-01-07T13:12:19-06:00
---

# Better looking visible aliases for subcommands

---

_Issue opened by @sondr3 on 2018-12-18 23:15_

Right now when you create a visible alias for a subcommand it appears in brackets after the help message, so when I first added some visible aliases I thought it didn't work. I like how Jekyll for example does it:

```text
❯ jekyll --help
jekyll 3.8.4 -- Jekyll is a blog-aware, static site generator in Ruby

[snip]

Subcommands:
  build, b              Build your site
  serve, server, s      Serve your site locally
```

as opposed to Clap:

```text
❯ ./target/debug/blug --help

[snip]

SUBCOMMANDS:
    build     Build your blug without serving it. [aliases: b]
    create    Create a post, page, draft etc [aliases: c]
    init      Initialize a new blug [aliases: i]
```

---

_Label `C: alias` added by @CreepySkeleton on 2020-02-01 15:19_

---

_Label `C: help message` added by @CreepySkeleton on 2020-02-01 15:19_

---

_Label `C: subcommands` added by @CreepySkeleton on 2020-02-01 15:19_

---

_Label `P3: want to have` added by @CreepySkeleton on 2020-02-01 15:19_

---

_Label `T: enhancement` added by @CreepySkeleton on 2020-02-01 15:19_

---

_Label `W: 3.x` added by @CreepySkeleton on 2020-02-01 15:19_

---

_Added to milestone `3.1` by @pksunkara on 2020-04-09 07:46_

---

_Comment by @sondr3 on 2020-05-04 20:01_

After having used Rust for a while now I think this is something I might be able to implement myself, are there any thoughts on how this should work? Should I create a `AppSetting::MergedAliases` for example or just outright change how aliases are displayed? @pksunkara @CreepySkeleton 

---

_Comment by @pksunkara on 2020-05-04 21:59_

Outright change is probably not a good idea. A setting sounds good but I am wondering if we are taking this too far. What if people want it in a different kind of way?

---

_Comment by @sondr3 on 2020-05-05 16:11_

Any suggestions for how you'd approach this in that case?

---

_Comment by @pksunkara on 2020-05-05 17:37_

I am honestly stumped regarding the design here. @kbknapp What do you think?

---

_Comment by @CreepySkeleton on 2020-05-05 18:08_

I personally like this one more

```
  build, b              Build your site
  serve, server, s      Serve your site locally
```

---

_Comment by @pksunkara on 2020-05-05 18:17_

I don't think we should be defaulting to that. But I too like it, which is why I am saying that we should come up with a way to allow users to format the help messages.

---

_Comment by @kbknapp on 2020-05-05 19:24_

The difference between the two versions is aesthetic only and not structural, so I'd prefer to have one way to display it and stick to that. I don't think it's worth the overhead to provide an option to switch between whichever way is the default and the other way.

The same could be argued for options/flags, however the average `long` flag (and especially options) take up far more room than subcommand names, hence placing them at the *back* of the help message where they can easily be wrapped if required without throwing off the aligned of all other options/flags. Of course this doesn't apply as much to multi-line help messages (`long_help`), but those aren't the default.

The only aesthetic `AppSetting` we have is similar to this proposal is `UnifiedHelpMessage`, but I'd argue that's a whole different category and provides a common format used by a lot of CLIs.

As for which version I prefer? I don't have super strong feelings about either. I could be convinced that the new way looks a little better. But it also suffers from the alignment issue described above. Since this change only affects subcommands, I'd be fine accepting it. My only other concern is then the request to do the same for flags/options would become a request which I would be more against for the alignment issues.

Maybe it'd be worth a look through github and grep.app to see average length of subcommands/aliases because when combined with #1891 if we lowered the default term width to 100, I'd want to make sure we didn't end up with help messages like this (exaggerated just for example):

```
    build, b            build your
                        site
    serve, server, s    serve your
                        site
                        locally
```

---

_Comment by @pksunkara on 2020-05-05 20:25_

> But it also suffers from the alignment issue described above.

It is the main reason I prefer not to change the current way of showing aliases.

But, we can allow a `help_generate` hook which could be filled by individual clap programs if they want a different aesthetic. This could work similar to `clap_generate`.

---

_Label `W: 3.x` removed by @pksunkara on 2021-08-13 10:40_

---

_Referenced in [epage/clapng#110](../../epage/clapng/issues/110.md) on 2021-12-06 17:39_

---

_Label `C: alias` removed by @epage on 2021-12-08 19:51_

---

_Label `A-builder` added by @epage on 2021-12-08 19:51_

---

_Label `A-builder` removed by @epage on 2021-12-08 20:09_

---

_Label `C: subcommands` removed by @epage on 2021-12-08 20:10_

---

_Comment by @epage on 2021-12-09 19:29_

I think https://github.com/clap-rs/clap/issues/962 could be useful which might come at odds with showing the aliases first.

Thoughts on how these two might interact?

---

_Removed from milestone `3.1` by @epage on 2021-12-09 19:29_

---

_Label `P3: want to have` removed by @epage on 2021-12-09 19:30_

---

_Label `S-waiting-on-decision` added by @epage on 2021-12-09 19:30_

---

_Comment by @pksunkara on 2021-12-10 14:22_

How do we show the aliases in args? I think we print them as `[alias: s]` etc at the end of the help, right? Maybe we could follow a similar pattern in order to not conflict with #962.

But I think #2914 should actually solve this and #962 

---

_Comment by @epage on 2021-12-10 16:33_

Yes, I believe subcommands are currently rendered like args, with a trailing `[alias: s]`, so "doing nothing" makes it so we don't conflict with #962.

The big question is how much logic should be built-in and how much should we punt to people doing it themselves with #2914.  I lean towards closing this in favor of #2914 but keeping #962 open. I'd like to try it out on some commands to see how well it works to give an idea for how universally useful it would be which could bump its prioritization for being built-in.

One idea I had for #2963 was to experiment with making all parenthetical information dimmed, giving it a different visual than the `about`, making it easier to scan for one vs the other.  In terms of scanning, that would also help improve things here.

---
