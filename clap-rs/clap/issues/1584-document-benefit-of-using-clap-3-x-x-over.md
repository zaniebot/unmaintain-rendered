```yaml
number: 1584
title: Document benefit of using Clap 3.x.x over StructOpt
type: issue
state: closed
author: danieleades
labels:
  - A-docs
assignees: []
created_at: 2019-10-25T10:14:18Z
updated_at: 2021-05-26T00:34:27Z
url: https://github.com/clap-rs/clap/issues/1584
synced_at: 2026-01-12T16:14:11Z
```

# Document benefit of using Clap 3.x.x over StructOpt

---

_@danieleades_

I've been using Clap for some time now and I'm a big fan.

I've recently switched to using StructOpt because I prefer the declarative style.

I see that clap 3 is re-exporting StructOpt, so I guess it's nice to have all the functionality under one roof.

What I'm not clear on, is where the benefit lies for me to switch from StructOpt (which re-exports Clap functionality) to Clap (which re-exports StructOpt functionality) given that I'm exclusively using the declarative syntax.

What does a user gain by having Clap as the direct dependency? I don't feel this has been documented anywhere.

---

_Comment by @CreepySkeleton on 2019-10-25 11:39_

> I see that clap 3 is re-exporting StructOpt

Keep in mind that the `structopt` in question is a year-old version, 0.2.x, while the latest version is 0.3.3. `clap_derive` is not a replacement for the latest `structopt` (for now).

> I guess it's nice to have all the functionality under one roof

That would be nice indeed, but it looks like the current maintainers [do not agree](https://github.com/clap-rs/clap/issues/1574#issuecomment-543858824).

> What I'm not clear on, is where the benefit lies for me to switch from StructOpt (which re-exports Clap functionality) to Clap (which re-exports StructOpt functionality) given that I'm exclusively using the declarative syntax.

For now effectively none since you'd be moving from `structopt 0.3` to 0.2. That's a move backward, not forward.

> What does a user gain by having Clap as the direct dependency? I don't feel this has been documented anywhere.

The intention here (AFAIK) is to have them both \[`structopt` and `clap`] under a single group of maintainers so they could be developed in sync. Some things cannot be done without tight integration.

---

_Comment by @danieleades on 2019-10-25 13:30_

Thanks @CreepySkeleton, that's a really good summary of the current state of play. So for now, I'm better off using StructOpt. But then when (if) they reach parity it gets murky again...
Is that about right?

Kind of seems like a lot of overlap and a bit of bloat. 

I'd really love to hear from one of the maintainers here. @kbknapp ?

---

_Comment by @CreepySkeleton on 2019-10-25 13:57_

>  Is that about right?

If they do reach parity (and I really hope they would) `structopt` will likely become deprecated, I recall @TeXitoi (`structopt`'s author) said something about that this has been his intention.

> Kind of seems like a lot of overlap and a bit of bloat.

A lot of overlap indeed, but bot `structopt` and `clap_derive` are proc-macros, they don't end up in the final binary, no bloat involved.

> I'd really love to hear from one of the maintainers here. @kbknapp ?

@kbknapp is unlikely to reply, he's kind of [retired](https://www.reddit.com/r/rust/comments/b575hi/is_clap_maintained/ejdawiv?utm_source=share&utm_medium=web2x). 
I believe @Dylan-DPC is maintainer here.

Also @Dylan-DPC what is your opinion on this matter? Is there any particular reason not to update to `structopt 0.3`? If it's about lack of manpower I would volunteer for the job, I'm pretty familiar with `structopt` codebase.

---

_Comment by @Dylan-DPC-zz on 2019-10-25 14:23_

Give me some time i'll respond to this and the other issue later today. But please do not pass any statements on behalf of the team. Thanks

---

_Comment by @Dylan-DPC-zz on 2019-10-25 14:24_

Also @CreepySkeleton if you want to go about updating structopt to 0.3 on clap-derive, you can go ahead and do it, we have no problem with that :)

---

_Comment by @CreepySkeleton on 2019-10-25 14:43_

> But please do not pass any statements on behalf of the team

I've never meant to, I'm terribly sorry if I left wrong impression

---

_Comment by @jamestwebber on 2019-10-25 17:54_

Maybe this is a question more appropriate for the `structopt` repo, but is it generally true that `structopt` can not do anything that can't be done in plain `clap` because it is generating the same code?

---

_Comment by @TeXitoi on 2019-10-26 09:43_

You can do almost everything. There is some exceptions as https://github.com/TeXitoi/structopt/issues/130

---

_Comment by @kbknapp on 2019-10-31 00:55_

The idea when the 3.0 branch started (seemingly forever ago) was exactly as you stated, to bring all the functionality under one roof and have a more cohesive product for users.

However, with my life events being what they've been over the past year (changed jobs, moved, had a baby) it has fallen behind. My dream goal was to move StructOpt into clap, but still have @TeXitoi as the owner of `clap_derive` with full creative and administrative control. Just with the idea that we could collaborate and present users *less* confusion. Of course the argument has been raised as to why can't that happen without clap absorbing StructOpt? It could, but the two different names alone causes confusion about relation between the projects and how to share functionality between them.

I'd still love for that dream to become a reality. But it's also totally understandable that there are valid arguments against this. The next step would be trying to update `clap_derive` to feature parity with StructOpt 0.3.x.

I guess it's TBD how this will all play out. It is open source after all :smile: 

> What I'm not clear on, is where the benefit lies for me to switch from StructOpt (which re-exports Clap functionality) to Clap (which re-exports StructOpt functionality) given that I'm exclusively using the declarative syntax.

Small nit, `clap_derive` doesn't re-export StructOpt. It used the StructOpt v0.2.x code as a base and made some changes. (which I think provide some more flexibility and are more in line with clap such as the ability to split out derivable traits in order to be able to just use parts of the derive instead of all or nothing)

---

_Label `T: RFC / question` added by @kbknapp on 2019-10-31 00:56_

---

_Comment by @CreepySkeleton on 2019-10-31 01:08_

@kbknapp It's good to have you back! 

In fact, I *am* performing the transition in https://github.com/clap-rs/clap_derive/pull/22 . Right now I'm short on time but I think I'll get clear in 3-4 days. Any specific deadline I should account for?

---

_Comment by @Dylan-DPC-zz on 2019-10-31 01:31_

@CreepySkeleton 3-4 days sounds good 

---

_Comment by @danieleades on 2019-11-02 14:14_

I didn't anticipate the amount of interest this question would stir up!

I really appreciate the in depth responses, and I think I have a clear picture of where things are at. It sounds like there's not a totally coherent story for users now, but by the sounds of things it's not too far off.

I'll continue using StructOpt, and I'll be paying close attention as things develop!

---

_Comment by @danieleades on 2021-04-08 12:48_

a year and a bit down the line, where are things at now?

---

_Comment by @pksunkara on 2021-04-08 13:18_

We have decided on the plan and the difference between clap v3 and structopt long ago, https://github.com/clap-rs/clap/issues/1574#issuecomment-575919336

---

_Added to milestone `3.0` by @pksunkara on 2021-04-08 13:18_

---

_Label `C: docs` added by @pksunkara on 2021-04-08 13:18_

---

_Comment by @danieleades on 2021-04-08 14:34_

In that case, I think this issue can probably be closed

---

_Closed by @danieleades on 2021-04-08 14:34_

---

_Reopened by @pksunkara on 2021-04-08 15:11_

---

_Comment by @pksunkara on 2021-04-08 15:16_

I am using this issue as a placeholder to write a small blurb about it in the readme/website

---

_Closed by @pksunkara on 2021-05-26 00:34_

---

_Referenced in [tikv/tikv#10837](../../tikv/tikv/pulls/10837.md) on 2021-08-27 08:36_

---
