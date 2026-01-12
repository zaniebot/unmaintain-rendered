```yaml
number: 738
title: Backwards-incompatible change made.
type: issue
state: closed
author: jrvanwhy
labels: []
assignees: []
created_at: 2016-11-12T04:47:59Z
updated_at: 2018-08-02T03:29:56Z
url: https://github.com/clap-rs/clap/issues/738
synced_at: 2026-01-12T16:14:09Z
```

# Backwards-incompatible change made.

---

_@jrvanwhy_

Commit 1ced2a7433ea8937a1b260ea65d708f32ca7c95e introduced a " + Clone" trait bound to many functions, including App::get_matches_with(), which is a breaking change and technically requires a major version increase.

We should see if this bound can be removed without losing functionality (which would obviously be another breaking change).

---

_Comment by @jrvanwhy on 2016-11-12 08:13_

I think I have a workable solution, but it's hanging the compiler. Unfortunately, I need to get permission from my employer before releasing my changes, so this will take a bit of time. Assuming I am correct in thinking I have a solution, I will have a patch ASAP, but that may be a few days.


---

_Comment by @kbknapp on 2016-11-12 17:05_

Thanks for filing this! And also thanks for working on this issue! Let me know if you have any questions. 


---

_Label `C: settings` added by @kbknapp on 2016-11-12 17:06_

---

_Label `D: intermediate` added by @kbknapp on 2016-11-12 17:06_

---

_Label `P1: urgent` added by @kbknapp on 2016-11-12 17:06_

---

_Label `Regression` added by @kbknapp on 2016-11-12 17:06_

---

_Label `W: 2.x` added by @kbknapp on 2016-11-12 17:06_

---

_Comment by @jrvanwhy on 2016-11-12 18:43_

Thanks for the offer of help. I've looked at my employer's (Google's) documentation, and it looks like they will own the copyright on the patch I'm creating. I notice that the current codebase is copyrighted solely by you. Are you fine accepting patches that would make you share copyright with Google in those two files? (It'd still be under the MIT license, of course).

If not, then my suggestion is to remove the Clone bound from get_matches_from(), get_matches_from_safe(), and get_matches_from_borrow() in app/mod.rs and change the iterator in get_matches_with() in parser.rs to create OsString objects with than T: Into<OsString> objects. I'm not sure if that works, however, because I'm hitting a compiler bug (currently waiting on Google's permission to release the patch so the Rust team can investigate the issue).


---

_Comment by @kbknapp on 2016-11-12 21:35_

@jrvanwhy I'm not against sharing the copyright, but being that I'm only one guy working on a side project that would be alittle more complicated than I'd want to think/worry about right now since I'm nowhere even close to being a lawyer :stuck_out_tongue_winking_eye: 

Let me see if I can get this working - if not then I'll concede and take the patch!


---

_Comment by @jrvanwhy on 2016-11-12 22:42_

I understand completely, sorry my patch-making is tied up in bureaucracy. Good luck making it work, and thank you for supporting this project so well!


---

_Comment by @tormol on 2016-11-13 13:22_

@kbknapp I'm almost certain you already share the copyright.
Copyright reassignment isn't mentioned anywhere, and that that stuff [seems to require paperwork](https://www.gnu.org/prep/maintain/maintain.html#Copyright-Papers).
The copyright notice in lib.rs says '... and `clap-rs` contributors', so either that or LICENCE-MIT is wrong.
If you were the sole copyright holder the contributor checkoff for [re-licensing](https://github.com/kbknapp/clap-rs/issues/389) would be unnecessary.


---

_Comment by @kbknapp on 2016-11-14 21:22_

@tormol you are probably correct, which is in part why I tend to steer clear of copyright paperwork unless it's required since it's a murky field.

I think what I'm most leery of is singling out a particular contributor for individual lines/files. Because what happens when those particular parts change, do the derivative works also need to added to this list of singled out contributions? That would be hard to keep up correctly. Granted this isn't to say any one of the clap contributors could request the same thing, but most are good with having their name listed in the contributors and using things like `git blame`.

With individual github contributors this is typically hand waved over, but when big corporation get involved things to be done correctly or it gets messy quick. Since I'm unfamiliar with how that whole derivative work part works, that's what I'm unsure of. The way I understand it sounds like a virus that spreads to all things derived from those original few lines? 

I'm **all for** sharing the love with the contributors, because without all of them, clap wouldn't be what it is today! I just don't want to do something totally wrong and get hung out to dry by mistake!

I could be waaay off too, so take what I say with a grain of salt :stuck_out_tongue_winking_eye: 


---

_Comment by @jrvanwhy on 2016-11-14 23:40_

@kbknapp The following might work, but I would need to clear it with Google first:
- I replace "Kevin B. Knapp" with "Kevin B. Knapp and other contributors" in LICENSE-MIT
- I add Google, Inc to CONTRIBUTORS

Would that set of changes be fine with you? Of course, don't feel like you need to wait for me to make changes -- if you find a solution on your own, go ahead and commit it. I'm just trying to save you some effort by solving it myself ;p


---

_Comment by @jrvanwhy on 2016-11-15 07:08_

The above changes are fine with Google. However, I'm not sure how to make the "Google, Inc" persistent in CONTRIBUTORS, since CONTRIBUTORS is apparently regularly regenerated from commit messages.


---

_Comment by @kbknapp on 2016-11-15 22:13_

@jrvanwhy absolutely, I was going to do that regardless (well similar, I was going to list your name :wink:). Assuming it's good with your employer I'd still like to add your name to the CONTRIBUTORS.md list. After making that change, do you know who from Google can give the thumbs up/down to #389 ? That change would allow this library to be compatible with the Rust ecosystem at large. As @tormol pointed out, that thumbs up/down may not even be totally necessary, but in case it is I'd still like to know who can give that approval.


---

_Comment by @jrvanwhy on 2016-11-15 22:32_

@kbknapp Unfortunately, Google requires that I add "Google Inc" as a contributor, which is difficult because your contributors list is auto-generated and the tool you're using would only pick up my username. The patch I posted (on Reddit) for the compiler devs changed LICENSE-MIT to mention Google Inc, which I find a bit heavy-handed.

Assuming I am able to create a working patch, I will ask Google's licensing team how to proceed. My guess is that I should release the patch under the proposed dual license.


---

_Comment by @jrvanwhy on 2016-11-17 07:27_

@kbknapp Here's an idea: What if I modified justfile to append something like:

`The following entities have contributed to clap-rs:
- Google Inc`

to CONTRIBUTORS.md so that you can maintain a list of companies/nonprofit organizations/governments, etc... that have contributed to clap-rs?

Also, I'm still blocked on [the rustc bug I found](https://github.com/rust-lang/rust/issues/37779).


---

_Comment by @kbknapp on 2016-11-21 15:12_

I'm good with that too :+1: 

---

_Referenced in [vitiral/artifact#15](../../vitiral/artifact/issues/15.md) on 2016-11-26 05:21_

---

_Comment by @vitiral on 2016-11-26 05:23_

I just hit this bug and it was a quick fix. I just want to let you know how grateful I am for this library. It is an amazing command line lib and you are awesome for maintaining it. :+1:

---

_Closed by @kbknapp on 2018-02-02 01:54_

---
