```yaml
number: 247
title: Add Homu (?) and Highfive
type: issue
state: closed
author: kbknapp
labels: []
assignees: []
created_at: 2015-09-10T04:08:47Z
updated_at: 2018-08-02T03:29:44Z
url: https://github.com/clap-rs/clap/issues/247
synced_at: 2026-01-12T16:14:08Z
```

# Add Homu (?) and Highfive

---

_@kbknapp_

These could be good additions now that we have more collaborators.

Homu, I'm unsure about because Github protected branches essentially do the same thing? I'm not sure what Homu would give us over protected branches (i.e. no merge until all tests pass, etc.)

Highfive, does an auto assign to PRs. If someone wants to re-assign (due to someone else with more knowledge on a subject, available time, etc.) comment with `r? @<USER>`. **If a PR needs my review**, please `r? @kbknapp` me so I can check it and I will :+1: when good. (for minor doc changes, examples, this **isn't** really required).

I will also be assigning others for my own PRs so that I don't merge anything dumb into master.

Todo:
- [x] Add Homu (..._maybe_)
- [x] Add Highfive (his/her name is @yo-bot for those who care :stuck_out_tongue_winking_eye: )

Edit: bold text spelling error


---

_Label `T: RFC / question` added by @kbknapp on 2015-09-10 04:08_

---

_Label `D: easy` added by @kbknapp on 2015-09-10 04:08_

---

_Label `P3: want to have` added by @kbknapp on 2015-09-10 04:08_

---

_Comment by @kbknapp on 2015-09-10 18:46_

The only thing I can find that Homu may give us that we don't already have (with protected branches and @yo-bot ) is that it won't start CI until the PR is accepted by someone. This is a benefit, but of questionable quantity.


---

_Comment by @WildCryptoFox on 2015-09-11 02:46_

It does keep the CI with less traffic; and for my #249 this would have been desired as I _KNOW_ that it wouldn't build.


---

_Comment by @WildCryptoFox on 2015-09-11 02:54_

@homu also handles merging after checks are satisfied - while in this time, the merge may still be cancelled. And shows who approved the merge (in the git log).


---

_Referenced in [clap-rs/clap#246](../../clap-rs/clap/pulls/246.md) on 2015-09-11 07:56_

---

_Comment by @WildCryptoFox on 2015-09-11 08:00_

http://homu.io/ **MUCH** more useful than @yo-bot.


---

_Comment by @Vinatorul on 2015-09-11 08:08_

I think using highfive to assign PR adds to PR conversation much waste messages. 
It is useful when we can aprove commits or PR using bot, when it can auto-asssign lables.

Also as @james-darkfox replied it is useful for email comments, but is anyone of us usign email for now?


---

_Comment by @WildCryptoFox on 2015-09-11 08:16_

Considering the lack of quotes and "Alice said foo", would appear that none of us are using github via Email.

@homu can approve commits, wakes up travis to test builds, waits for travis to return status, and merges. @homu may get a bit verbose but well worth it. **VERY** good bot.


---

_Comment by @Vinatorul on 2015-09-11 08:41_

> Considering the lack of quotes and "Alice said foo",

@james-darkfox There is an easier way: comments from email marked with mail icon


---

_Comment by @WildCryptoFox on 2015-09-11 08:43_

@Vinatorul Nice to know. I haven't noticed.


---

_Comment by @kbknapp on 2015-09-12 01:31_

I use comments from email occasionally. Also yo-bot should auto assign PRs which is nice because it alerts you to a PR being filed.

I'll look at adding homu as well because the more I think about it, not bugging Travis until someone has accepted a PR is kind of nice. Also it should re-run Travis once aa different PR is merged to ensure there are no new conflicts. 


---

_Comment by @WildCryptoFox on 2015-09-12 03:24_

Adding homu is incredibly easy. http://homu.io/ type in "kbknapp/clap-rs", click register, allow application (github). Done. The harder step is setting up travis, which has already been done.


---

_Comment by @kbknapp on 2015-09-21 00:03_

@james-darkfox done :smile: 


---

_Closed by @kbknapp on 2015-09-21 00:03_

---

_Comment by @WildCryptoFox on 2015-09-21 01:37_

Woot!


---

_Comment by @kbknapp on 2015-09-21 03:40_

@james-darkfox @Vinatorul @sru 

Just as an update. `@yo-bot` will auto-assign a random reviewer for PRs. Once a PR is good you can use the [homu commands](http://homu.io/) to accept the PR. **If the PR passes all tests, accepting will auto-merge if no changes have been made/other PRs accepted.** If it's a major change, or something that you think I'd like to review, I'd appreciate someone `r? @kbknapp`ing me first :wink: Also, you can un-accept a PR with `@homu r-` FYI

If you're auto-random-selected by `@yo-bot` to review a PR, but don't have time or someone is more well versed in whatever the PR deals with, please assign someone else for review (either via `r? NAME` or manually via github...doesn't matter to me).

Hopefully these changes will put us a more streamlined path...not that there has been any problems :stuck_out_tongue_winking_eye: 

Also, I'm starting some intense classes soon, so my time will be somewhat limited for a bit. But I'll make a valiant effort to review everything and stay plugged into the conversations and decisions. If it appears I've missed anything, or you've been waiting a while for me to answer please don't hesitate to shoot me an e-mail (or assign the issue/PR to me which will generate an e-mail).

**EDIT:** @james-darkfox had a good comment in #261 regarding if I submit something and one of you are assigned as the reviewer. If it's something I submit, and you review it saying it's good to go you can `@homu r+` it, no need to re-assign back to me. I just want to make sure my work gets checked too :wink:


---

_Comment by @WildCryptoFox on 2015-09-23 15:43_

@kbknapp yo-bot happen to have any settings?

This is **perfectly fine behavior, and expected until 2.0 is official**

https://github.com/kbknapp/clap-rs/pull/275#issuecomment-142639074 (deleted)

```
<img src="http://www.joshmatthews.net/warning.svg" alt="warning" height=20> **Warning** <img src="http://www.joshmatthews.net/warning.svg" alt="warning" height=20>

* Pull requests are usually filed against the master branch for this repo, but this one is against redesign. Please double check that you specified the right target!
```


---
