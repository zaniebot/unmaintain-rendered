---
number: 4295
title: "Unbound `max_term_width` hurts readability"
type: issue
state: closed
author: SUPERCILEX
labels:
  - C-bug
  - M-breaking-change
  - S-waiting-on-decision
  - A-help
assignees: []
created_at: 2022-09-30T03:38:41Z
updated_at: 2023-07-17T15:26:00Z
url: https://github.com/clap-rs/clap/issues/4295
synced_at: 2026-01-07T13:12:20-06:00
---

# Unbound `max_term_width` hurts readability

---

_Issue opened by @SUPERCILEX on 2022-09-30 03:38_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

nightly

### Clap Version

4.0.4

### Minimal reproducible code

```rust
#!/usr/bin/env -S rust-script --debug

//! ```cargo
//! [dependencies]
//! clap = { version = "4.0.4", features = ["derive", "wrap_help"] }
//! ```

use clap::{Parser, Subcommand};

#[derive(Parser)]
/// Generate a random directory hierarchy with some number of files
///
/// A pseudo-random directory hierarchy will be generated (seeded by this
/// command's input parameters) containing approximately the target
/// number of files. The exact configuration of files and directories in
/// the hierarchy is probabilistically determined to mostly match the
/// specified parameters.
///
/// Generated files and directories are named using monotonically increasing
/// numbers, where files are named `n` and directories are named `n.dir`
/// for a given natural number `n`.
///
/// By default, generated files are empty, but random data can be used as
/// the file contents with the `total-bytes` option.
struct Cli {
  #[clap(subcommand)]
  command: Commands,
}

#[derive(Subcommand)]
enum Commands { Foo }

fn main() {
    let mut _command = Cli::parse();
}

```


### Steps to reproduce the bug with the above code

Run

### Actual Behaviour

Help is way too long

### Expected Behaviour

Max 100 chars

### Additional Context

Workaround: manually call `#[command(max_term_width = 100)]`

### Debug Output

_No response_

---

_Comment by @SUPERCILEX on 2022-09-30 03:40_

I tried messing around with `*term_width` but those don't do anything unless `wrap_help` is enabled which I don't want.

---

_Comment by @epage on 2022-09-30 13:36_

In the [migration guide](https://github.com/clap-rs/clap/blob/master/CHANGELOG.md#migrating), there is this line:

> Run `cargo add clap -F wrap_help` unless you want to hard code line wraps

We previously had a "wrap_help" feature flag that didn't control help wrapping but instead controlled wrapping to the terminal's width.  I moved wrapping into that feature as well as there are a lot of commands that focus on keeping their help short enough that only in small terminals do they need wrapping.

For now, I left it as non-default but called it out in the migration guide.

---

_Closed by @epage on 2022-09-30 13:36_

---

_Comment by @SUPERCILEX on 2022-09-30 16:50_

I saw that, but it's odd to have `*term_width` functions that are broken unless a feature is enabled. I'd also rather not take on the terminal_size dependencies when it shouldn't be needed.

---

_Comment by @SUPERCILEX on 2022-09-30 16:56_

Eh, actually nevermind about taking on the extra deps. Though the *term_width functions should really be behind the feature flag. In general, the clap docs are in a pretty unhealthy state right now with a lot of references to things that don't exist or don't work anymore.

---

_Comment by @epage on 2022-09-30 17:33_

> Though the *term_width functions should really be behind the feature flag

They are now documented as needing `wrap_help`, see  https://github.com/clap-rs/clap/commit/c16fdbedc17a7ccac6ebc1bdfa195d8215182256

> In general, the clap docs are in a pretty unhealthy state right now with a lot of references to things that don't exist or don't work anymore.

Thats a pretty broad comment to be making especially when it hasn't been backed up with creating issues to point out what else is the matter.

---

_Comment by @SUPERCILEX on 2022-09-30 18:06_

> They are now documented as needing wrap_help

For 5.0, these should really be behind the feature unless there's some reasoning I'm not aware of.

> Thats a pretty broad comment to be making especially when it hasn't been backed up with creating issues to point out what else is the matter.

Fair enough, though the \*term_width functions are just one example. I must admit the upgrade to 4.0 was quite frustrating. Created https://github.com/clap-rs/clap/issues/4308. It might be worth picking some random OSS project and upgrading them to clap v4 to see what it's like for yourself.

---

_Comment by @epage on 2022-09-30 18:11_

> It might be worth picking some random OSS project and upgrading them to clap v4 to see what it's like for yourself.

I'm familiar with [what the upgrade is like](https://www.reddit.com/r/rust/comments/xqiqfy/clap_400_a_rust_argument_parser_is_released/iq9yrw8/)

---

_Comment by @SUPERCILEX on 2022-09-30 18:25_

That's actually pretty cool, wasn't aware. Maybe the actual issue then is that there's too little info explaining how to restore old behavior. The changelog sort of reads as a "Here's what changed, good luck trying to restore old behavior you liked lol." Of course I'm exaggerating, but adding the `wrap_help` feature doesn't make the CLI act like before, you also need to set the max_term_width. Why? I dunno. That kind of breakage would be fine if it was clearly explained as "Here's what the old thing does: example. Here's what the new thing does: example. Here's how to restore x, y, z old behavior in the new thing."

---

_Comment by @epage on 2022-09-30 18:46_

>  adding the wrap_help feature doesn't make the CLI act like before, you also need to set the max_term_width. 

That would be a bug.  Please create a minimal reproduction case and open an issue

---

_Comment by @SUPERCILEX on 2022-09-30 18:56_

> That would be a bug. Please create a minimal reproduction case and open an issue

That's what this issue was about. Run the above repro (`./foo help`) with or without the `wrap_help` feature and the help will extend through the entire terminal instead of wrapping at 100 chars. Caused by this: https://github.com/clap-rs/clap/blob/106d8f5e90952edf77f53f19ab0041712175267d/src/output/help_template.rs#L104

---

_Comment by @epage on 2022-09-30 19:31_

To summarize what is going on
- In clap 3 with `wrap_help`, we would wrap on the terminal's width
- In clap 3 without `wrap_help`, we would assume the terminal width was 100 and wrap there
- In clap 4 with `wrap_help`, the behavior is the same
- In clap 4 without `wrap_help`, we no longer wrap

Nothing is broken per se.  The behavior for `wrap_help` has remain unchanged and we intentionally changed the behavior without `wrap_help`

I can understand though of changing the behavior of `max_term_width` so that we default to a set number as having too long of lines becomes unhelpful for reading.  The question is what that default `max_term_width` should be and is this innocuous enough to not be a breaking change (I think so).

Re-opening, taking this as a request to change `max_term_width`

---

_Reopened by @epage on 2022-09-30 19:31_

---

_Renamed from "Help wrapping broken in v4" to "Unbound `max_term_width` hurts readability" by @epage on 2022-09-30 19:32_

---

_Label `S-waiting-on-decision` added by @epage on 2022-09-30 19:32_

---

_Label `A-help` added by @epage on 2022-09-30 19:32_

---

_Comment by @epage on 2022-11-16 03:13_

From #4484

> IIRC this is what it was before. I think it's better to default things to be more readable than force everyone to pick some inconsistent max_width. The people that still want infinite max width can set it to usize::MAX manually.

This was added in #654 to resolve #653.  The value does not seem to have changed in that entire time.

---

_Comment by @SUPERCILEX on 2022-11-16 04:05_

That issue appears to be confirming the behavior of the PR I just opened:

> I want to say wrap at x width **unless** it's smaller then wrap there instead

What is there to discuss? It's basically just "Should max_term_width have a default?" yes/no. If the answer is no, then this issue should be closed, otherwise my PR should be merged. Clap 3 defaulted to yes and if we take `wrap_help` as the equivalent of no special config in clap 3 (since it used to wrap stuff without wrap_help), then clap 4 should have a default too.

---

_Comment by @epage on 2022-11-16 12:55_

> What is there to discuss? It's basically just "Should max_term_width have a default?" yes/no. If the answer is no, then this issue should be closed, otherwise my PR should be merged. Clap 3 defaulted to yes and if we take wrap_help as the equivalent of no special config in clap 3 (since it used to wrap stuff without wrap_help), then clap 4 should have a default too.

- Whether we should go forward with this or not
- What the default should be
- Should we still have a term width default
- Whether this is a breaking change

---

_Comment by @SUPERCILEX on 2022-11-16 18:26_

Well again, I still consider this a regression, so:
- Clap 3 had a default, so yes. Regression aside, it's quite unpleasant to read something that requires turning your head, we're just not used to that.
- Clap 3 had it at 100.
- We don't? If you're talking about `current_width.unwrap_or(100)`, that 100 doesn't actually do anything since it gets ignored without wrap_help and won't be None unless there's an error with wrap_help enabled.
- This is a regression. Clap 4's current behavior broke clap 3 users like me. Also, you just "broke" the short command output to make it [look nicer](https://github.com/SUPERCILEX/ftzz/commit/4c1e4d97d3f4a8b48f0f4396c784beaac49e8618#diff-16c5c609cb6d69b2626e8fad281d9a3e804ec404e07b6a69183f76d2ee41e4b3), so I don't see how this is any different.

---

_Comment by @epage on 2022-11-17 01:26_

I'm stepping away from this Issue for a cool down period as you are coming across as belligerent in responses rather than collaborating in good faith.

---

_Comment by @SUPERCILEX on 2022-11-17 02:52_

I'm frustrated because a good chunk of my interactions with you have been immediately rejected without justification beyond what amounts to "I don't like this." Let's take this issue as an example. We're the only two participants and I've said what I think multiple times: we should bring back the max width (because it's a clap 3 regression, hard to read, and not a breaking change). For there to be discussion, I would need to know what your opinion is, but I don't. Furthermore, you haven't acknowledged my points so there are no counterpoints I can address. Hence there is nothing for *me* to discuss. So I guess I feel like I'm being needlessly asked to waste my time repeating myself over and over again (I've sunk ~5hrs into this issue by now for what I feel should be a trivial change).

---

I also think my language might be misconstrued, so I want to be super clear.

> What is there to discuss?

This is an honest question. Again, you've put out the questions to discuss but not provided opinions. I've answered the questions several times so what else am I supposed to say? Again, honest question. I have no idea what else you're picturing in your head when you say discuss. There's nothing more for me to say.

> Also, you just "broke" the short command output to make it [look nicer](https://github.com/SUPERCILEX/ftzz/commit/4c1e4d97d3f4a8b48f0f4396c784beaac49e8618#diff-16c5c609cb6d69b2626e8fad281d9a3e804ec404e07b6a69183f76d2ee41e4b3), so I don't see how this is any different.

Putting "broke" in quotes is supposed to mean I don't consider that a breaking change. When I said "look nicer," I meant that sincerely though I can see how it could be seen a sarcasm. Apologies if that's how you interpreted it. I'm saying that you've already made changes to the help output to improve it and I consider that to be fine so we may as well keep doing that. Basically, "eh/whatevs/don't care."

---

_Comment by @SUPERCILEX on 2022-11-17 03:07_

> I would need to know what your opinion is, but I don't

Actually, I take that back. To quote you: "having too long of lines becomes unhelpful for reading" and "is this innocuous enough to not be a breaking change (I think so)." So we're even in agreement? I just don't get it.

---

_Comment by @epage on 2022-11-17 16:26_

> I'm frustrated because a good chunk of my interactions with you have been immediately rejected without justification beyond what amounts to "I don't like this." Let's take this issue as an example. We're the only two participants and I've said what I think multiple times: we should bring back the max width (because it's a clap 3 regression, hard to read, and not a breaking change). For there to be discussion, I would need to know what your opinion is, but I don't. Furthermore, you haven't acknowledged my points so there are no counterpoints I can address. Hence there is nothing for me to discuss. So I guess I feel like I'm being needlessly asked to waste my time repeating myself over and over again (I've sunk ~5hrs into this issue by now for what I feel should be a trivial change).

The state of this Issue is marked in the labels, as I mentioned.  I've consistently asked for people to discuss things before moving on to PRs
- So we have a central place for the conversation as PR authors can come and go for an Issue and sometimes one author might do multiple PRs
- PRs put extra social pressure on maintainers to merge because of the extra commitment involved by the author.

Another important aspect is communication.  This issue would go a lot more smoothly but
- Previous comments keep reiterating the same thing rather than answering my questions, not from one person's perspective, but from the clap project's perspective which needs to take all users into account.  This requires empathy to consider how things might impact other users.
- Previous comments do not clearly articulate the different cases and the how the change fits into it, making the conversation a lot harder to have

I've also had less patience in this issue because
- My time is even more limited right now and I have to prioritize accordingly as I normally get time during work on this but I am on family leave atm
- A lot of assumed trust has already been lost due to behavior on other issues in other repos.  In a short period of time, I experienced a lot of combativeness that ate up a disproportionate amount of my time and zapped a significant amount of my emotional energy, requiring distance and recharge to avoid burn out.  The problems in those other PRs and Issues reflect the same kind of problems seeing here (narrow focus on one's own needs, not looking at wider project and user needs which includes narrative flow of documentation which can be hard to do, unclear communication, etc)

I'd recommend taking a step back for a few days, and looking at this again with fresh eyes.

---

_Comment by @SUPERCILEX on 2022-11-21 03:52_

> from the clap project's perspective which needs to take all users into account. This requires empathy to consider how things might impact other users.

That's true, but under the assumption that this isn't any more of a breaking change than the other help tweaks, there are only a few options:
- People don't care (since we're the only two people in this issue, that's my hypothesis).
  - If I'm wrong and your fear is that trust in you/clap will be lost due to this change, I'm happy to take the blame.
- People like the change.
- People don't want a max width at all.
  - I would find this very odd. No sane reading medium has unbound max line lengths. Strong rationale would need to be provided for why this should be the default.
- People disagree with the default column choice.
  - The exact max width is very subjective and I don't have a strong opinion. If other people hate 100, let them throw tomatoes until a consensus is reached and then pick that. Or you can be the BDFL and pick something.

> My time is even more limited right now

Same here, which is why it's sad that we're wasting time on this. Concerns or fears about the change *still* haven't been shared, even though it appears you are in favor the change.

> A lot of assumed trust has already been lost

Same here, this is draining. I'm not enjoying this any more than you are.

> I'd recommend taking a step back for a few days, and looking at this again with fresh eyes.

~I still don't get what you want from me.~ Hopefully the thoughts below are what you were looking for.

---

_Comment by @SUPERCILEX on 2022-11-21 04:21_

> People don't want a max width at all.

Just thought of something: those people would probably argue that a max width requires them to scroll more vertically, so maybe the max width should be 120? Or you could argue that the terminal's actual width is the prefered width, but I've found that to rarely be the case as the terminal tends to be in-editor or tiled by a window manager these days. If not, then the default starting width for a terminal width is 80 AFAIK, so this change wouldn't affect those people.

---

_Comment by @SUPERCILEX on 2022-11-21 04:44_

I'd like to also clarify why I believe this is a regression, because I don't think I'm getting through. In clap 3, the default width was 100 because the `wrap_help` feature was disabled by default. In clap 4, you *must* enabled the `wrap_help` feature to get any wrapping which means people who just want some default wrapping as before will reach for `wrap_help`. The fact that text wrapping is tied to the terminal width is actually something I'm not a huge fan of, but that's a separate discussion.

My point is that by forcing people to use their terminal width to get any wrapping at all, the distinction between devs saying "I want my users to be terminal wrapped" vs "I want wrapping of any kind" has been lost. And this issue is arguing that it's better to cater to the "I want wrapping of any kind" people because they are necessarily a superset of the "I want my users to be terminal wrapped" people (though I guess that's bad logic because the set difference could still be smaller, but I don't think it is).

---

_Comment by @SUPERCILEX on 2022-11-21 05:18_

Sigh, now I can't get this issue out of my head. Prior art: https://github.com/clap-rs/clap/issues/1891

> Side note, I think I'm just starting to hit this more personally because I have a vertical monitor and primarily use i3. So it's not uncommon for me to have a terminal window that is less than 120 characters wide (...it's 104 characters wide, I just counted...).

> The motivating factor behind this issue is to discuss if there is a better way to handle small terminals by default.

Similar to the other quote I posted, the problem solved by using the terminal width seems to be about when it's too thin.

> Linus rant

Seems to be about 80 chars specifically, so let's not do that. I'm leaning more towards 120?

> Then I believe 100 is good, especially when rust uses 100 too haha.

A point in favor of 100.

> It doesn't matter, I guess. It's just the problem is pretty much non-pressing, it generates much more discussion than it worth, and it's not like we're making some sort of hard decision here - we can always revise it in future. We're all in agreement that 100 is a good choice, so let's just get it over with.

That's a great summary of how I feel about this issue. I just sunk another 1.5 hrs into it, and for what? To me, it was self-evident that *some* max width was better than none because every modern website ever, the existence of books, newspapers, etc uses some max width. I should have been quicker to see that you don't share that axium and given you arguments to support it. But also, now I'm sad that I've lost so much time to an issue that seemed trivial to me.

---

_Comment by @kim on 2022-12-02 08:56_

So... why is it that setting `max_term_width` requires taking on another transitive dependency? :thinking: 

I would also like to point out that the documentation for [Command::max_term_width](https://docs.rs/clap/4.0.29/clap/builder/struct.Command.html#method.max_term_width) seems to be wrong. It says

> Using 0 will ignore terminal widths and use source formatting (default).

That would be my preference, but it is not how it behaves.  Instead, the help is wrapped to the dynamically determined terminal width.

---

_Referenced in [rust-lang/cargo#11895](../../rust-lang/cargo/issues/11895.md) on 2023-03-27 13:22_

---

_Label `M-breaking-change` added by @epage on 2023-07-17 14:14_

---

_Comment by @epage on 2023-07-17 14:32_

Finally getting back to this

> So... why is it that setting max_term_width requires taking on another transitive dependency? thinking

Originally, wrapping itself was an external dependency but was always forced on.  `wrap_help` didn't pull in the wrapping dependency but the term-width detection.

We've re-implemented the text wrapping code internally but whether its internal or not makes less of a difference (besides people who play dependency golf which I disagree with).

So rather than forcing a dependency to be always on, we've consolidated wrapping related features to `wrap_help`.

> I would also like to point out that the documentation for [Command::max_term_width](https://docs.rs/clap/4.0.29/clap/builder/struct.Command.html#method.max_term_width) seems to be wrong. It says

An upcoming change will fix this documentation bug.

---

_Comment by @epage on 2023-07-17 14:37_

@SUPERCILEX 

> That's true, but under the assumption that this isn't any more of a breaking change than the other help tweaks, there are only a few options:

The case not being handled is if someone is already setting one or both values.  If someone is setting the term width to a hard coded value and we set max term width to be less than that, we've violated expectations from the given API

---

_Referenced in [clap-rs/clap#5014](../../clap-rs/clap/pulls/5014.md) on 2023-07-17 14:43_

---

_Closed by @epage on 2023-07-17 15:26_

---
