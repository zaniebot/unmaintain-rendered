---
number: 978
title: "`ArgMatches::value_of` should include arguments to subcommands"
type: issue
state: closed
author: sgrif
labels:
  - C-bug
assignees: []
created_at: 2017-06-03T13:38:45Z
updated_at: 2018-08-02T03:30:08Z
url: https://github.com/clap-rs/clap/issues/978
synced_at: 2026-01-10T01:26:40Z
---

# `ArgMatches::value_of` should include arguments to subcommands

---

_Issue opened by @sgrif on 2017-06-03 13:38_

In Diesel CLI we are using a global argument in combination with subcommands. I'd expect `matches.value_of("ARG")` to return the argument regardless of where it appeared. However, in Diesel CLI (which is using clap 2.24.2), running `diesel migration run --database-url something` will complain that the argument isn't present. Running `diesel migration --database-url something run` works.

---

_Comment by @kbknapp on 2017-06-16 14:41_

@sgrif apologies for the late reply, I've been out of town. Looking at the diesel source, this looks like a bug on clap's end. It should be a quick fix though. Let me see if I can get it put together today.

---

_Label `C: args` added by @kbknapp on 2017-06-16 14:41_

---

_Label `D: easy` added by @kbknapp on 2017-06-16 14:41_

---

_Label `P2: need to have` added by @kbknapp on 2017-06-16 14:41_

---

_Label `T: bug` added by @kbknapp on 2017-06-16 14:41_

---

_Label `W: 2.x` added by @kbknapp on 2017-06-16 14:41_

---

_Comment by @kbknapp on 2017-06-16 15:20_

@sgrif can you add [`AppSettings::PropagateGlobalValuesDown`](https://docs.rs/clap/2.24.2/clap/enum.AppSettings.html#variant.PropagateGlobalValuesDown) and see if it still gives you an error?

Why isn't this the default you ask? It wasn't possible a long time ago, so it was added after the fact 😜

---

_Comment by @sgrif on 2017-06-17 20:32_

I won't get to it for a few days, but yes I will see if that works. :)

---

_Referenced in [diesel-rs/diesel#1201](../../diesel-rs/diesel/pulls/1201.md) on 2017-09-25 18:29_

---

_Comment by @sgrif on 2017-09-25 21:24_

It's been a lot of days, but I finally got around to trying this. The setting improves things, but still doesn't fix the problem.

| Command | `diesel --db-url="" db drop` | `diesel db --db-url="" drop` | `diesel db drop --db-url=""` |
| --- | --- | --- | --- |
| Without Setting | Arg not found | Arg not found | Fine |
| With Setting | Fine | Arg not found | Fine |

| Command | `diesel --db-url="" migration run` | `diesel migration --db-url="" run` | `diesel migration run --db-url=""` |
| --- | --- | --- | --- |
| Without Setting | Arg not found | Fine | Arg not found |
| With Setting | Fine | Fine | Arg not found |


---

_Comment by @kbknapp on 2017-09-25 22:22_

No worries, I've been incredibly busy too. Thanks for all the details, once I'm back at a computer (I'm on mobile) I should be able to figure this one out quickly

---

_Comment by @willmurphyscode on 2017-09-28 11:34_

Do you know whether anyone is already working on this? If not, I'd like to try to fix it over the next few days. 

---

_Comment by @kbknapp on 2017-09-28 14:02_

@willmurphyscode

I get home from my travel tomorrow and was planning on checking it out then, but if you'd like to take a swing at it I'd be more than happy to mentor it and assist where needed. :+1:

---

_Comment by @willmurphyscode on 2017-09-29 11:49_

@kbknapp I'll take a swing at it. I confirmed the issue locally with diesel, so I was going to write some failing tests to start with. What's the best place to reach out if I need hand or have a question?

---

_Referenced in [rust-lang/crates.io#1058](../../rust-lang/crates.io/issues/1058.md) on 2017-09-29 22:17_

---

_Comment by @willmurphyscode on 2017-10-02 12:12_

@kbknapp I've written some failing tests that show the issue: https://github.com/willmurphyscode/clap-rs/blob/propagate-values-down/tests/propagate_globals_down.rs#L52 but I'm a little bit lost trying to figure out exactly what to change needs to be made to make them pass. I've commented the tests in that link. Would you mind providing a little help when you have a chance?

---

_Comment by @kbknapp on 2017-10-04 02:23_

@willmurphyscode 

> first_subcommand_can_access_global_arg_if_global_arg_is_last

[Your hypothesis](https://github.com/willmurphyscode/clap-rs/blob/9872a82ff9618f45a54907e8961ba29ddd383896/tests/propagate_globals_down.rs#L66) is correct which was initially by design. 

To fix it, you'll have to bubble all these values back up just [prior to validation occurring](https://github.com/kbknapp/clap-rs/blob/3224e2e1cd4927935f44081c1ca686d95f267790/src/app/parser.rs#L1074).

Some edge cases to watch out for would be `program --global-arg some_value outer --global-arg other_value` where multiple values aren't true. But this *should* just work because validation will occur *after* propagation.

> second_subcommand_can_access_global_arg_if_global_arg_is_in_the_middle

[You're probably right](https://github.com/willmurphyscode/clap-rs/blob/propagate-values-down/tests/propagate_globals_down.rs#L78) here too.

To solve it, you'll probably just need to add some recursive calls to [`ArgMatcher::propagate`](https://github.com/kbknapp/clap-rs/blob/3224e2e1cd4927935f44081c1ca686d95f267790/src/args/arg_matcher.rs#L23) down through all subcommands.

---

_Comment by @kbknapp on 2017-10-04 02:24_

I believe one or both of these cases are actually already fixed in 3.x just due to being a more clean code base...but I'll add this to the 3.x issue tracker just in case.

---

_Referenced in [clap-rs/clap#1010](../../clap-rs/clap/issues/1010.md) on 2017-10-04 02:45_

---

_Comment by @willmurphyscode on 2017-10-06 21:27_

@kbknapp You say "one or both of these is fixed in 3.x." Does that mean that there's nothing to do here and people should just wait for 3.x? I've opened a PR with the tests (https://github.com/kbknapp/clap-rs/pull/1060) in case someone else wants to follow up, but beyond that I'm probably out of time to work on this issue.

---

_Referenced in [clap-rs/clap#1060](../../clap-rs/clap/pulls/1060.md) on 2017-10-06 21:34_

---

_Comment by @kbknapp on 2017-10-07 17:01_

> You say "one or both of these is fixed in 3.x." Does that mean that there's nothing to do here and people should just wait for 3.x?

Since I'm not sure when I'll be able to finalize 3.x with my crazy work schedule right now, I'd rather fix them in 2.x in the mean time. I saw your comment in the PR about lack of time as well, so I can pick up where you left off. Thanks for starting this though 👍 

---

_Referenced in [clap-rs/clap#1061](../../clap-rs/clap/issues/1061.md) on 2017-10-07 17:03_

---

_Comment by @Freyskeyd on 2017-10-10 17:11_

@kbknapp I also can contribute to this issue.

I need some input to get started.

---

_Comment by @kbknapp on 2017-10-10 17:42_

@Freyskeyd thanks! 👍 

I would take the failing tests in #1060 and start with `second_subcommand_can_access_global_arg_if_global_arg_is_in_the_middle` as it will probably be easier to solve. It *may* be as easy as looping through all *matched* subcommands and recursively calling `propagate`.

If you run into any questions, feel free to ping me on Gitter since I'll get those notifications on my cell phone as well.

---

_Comment by @kbknapp on 2017-10-10 17:43_

@willmurphyscode off topic, I saw your name on the upcoming Meetup this week I didn't know you were in the area! Looking forward to meeting you guys. 😄 

---

_Added to milestone `2.27.0` by @kbknapp on 2017-10-12 21:42_

---

_Referenced in [clap-rs/clap#1070](../../clap-rs/clap/pulls/1070.md) on 2017-10-18 11:40_

---

_Comment by @kbknapp on 2017-10-18 14:45_

#1070 fixes this. @sgrif could you test your use case against the current master branch? If it's good to go I can put out another version here soon.

---

_Comment by @sgrif on 2017-10-22 19:05_

Yes, this resolves my issue

---

_Closed by @sgrif on 2017-10-22 19:05_

---

_Comment by @sgrif on 2017-10-22 19:05_

Would love a ping when a new version is released. :)

---

_Comment by @kbknapp on 2017-10-24 01:47_

@sgrif will do! The goal is to release before I leave for Rust Belt Rust :wink: 

---

_Comment by @kbknapp on 2017-10-25 01:57_

@sgrif v2.27.0 is out on crates.io and contains these fixes :)

---

_Referenced in [clap-rs/clap#1037](../../clap-rs/clap/issues/1037.md) on 2017-11-12 17:53_

---
