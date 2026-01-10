---
number: 3234
title: Allow good/warning/error/hint color to be customized
type: issue
state: closed
author: milesj
labels:
  - C-enhancement
  - A-help
  - S-blocked
assignees: []
created_at: 2021-12-31T03:17:20Z
updated_at: 2024-08-09T02:23:37Z
url: https://github.com/clap-rs/clap/issues/3234
synced_at: 2026-01-10T01:27:36Z
---

# Allow good/warning/error/hint color to be customized

---

_Issue opened by @milesj on 2021-12-31 03:17_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Clap Version

3.0.0-rc.9

### Describe your use case

I would like to use colored help/errors/etc in our CLI, but would like to change the colors to match our "brand".

### Describe the solution you'd like

Provide a setting for good/warning/error/hint that allows the ANSI color code to be provided, based on the 256 table (or maybe 16m if you're crazy). https://upload.wikimedia.org/wikipedia/commons/1/15/Xterm_256color_chart.svg

It may be better to name the colors based on their actual usage, like arg/bin/label, etc.

### Alternatives, if applicable

_No response_

### Additional Context

_No response_

---

_Label `C-enhancement` added by @milesj on 2021-12-31 03:17_

---

_Label `A-help` added by @epage on 2021-12-31 12:35_

---

_Label `S-triage` added by @epage on 2021-12-31 12:35_

---

_Comment by @epage on 2021-12-31 12:46_

One challenge with this is the API.  We can't accept ANSI escape codes because clap3 intentionally added support for non-ANSI Windows terminals.  We could copy the `enum`s for color selections but we'd also need styling and the ability to compose them together.  We end up starting to duplicate a decent chunk of a terminal styling API which we are hesitant to do (#1790 is a bit more extreme of an example).

We've been instead preferring for feedback on #2963 for improving our defaults and https://github.com/clap-rs/clap/issues/2914 for allowing people to more thoroughly customize things when those defaults don't work.

> It may be better to name the colors based on their actual usage, like arg/bin/label, etc.

We've been moving in that direction.  Originally, it was named after the color but the color also implied some other styling with it.  The next step is for us to split off `--help` parts from abusing the existing semantics and giving them their own.  Since this is internal, this isn't too much of a priority though will be addressed as part of #2963 and #2389.

>  would like to change the colors to match our "brand".

Mind elaborating what is "ok" and what runs counter to your "brand"?  In what way does it run counter?

---

_Comment by @milesj on 2022-01-08 23:08_

@epage That all makes sense. I've had enough experience with ANSI and CLI tools so I totally get it.

As for the branding piece, the colors are shades of purple/blue/teal. Here's an example of the log output.

<img width="1033" alt="Screen Shot 2022-01-08 at 3 07 30 PM" src="https://user-images.githubusercontent.com/143744/148662922-4dfe0418-232c-4be9-8952-76ff41910006.png">

And this is where the colors are configured if you're curious: https://github.com/milesj/moon/blob/master/crates/logger/src/color.rs

---

_Comment by @epage on 2022-01-11 17:19_

If you have any feedback or proposals for https://github.com/clap-rs/clap/issues/2963, could you post it there?

Also, how important are errors or is the main concern the help?

Also, for anyone finding this issue, something that can help for re-evaluating is examples of precedence for this (like [dialoguer](https://docs.rs/dialoguer/latest/dialoguer/)) and proposals for how this would work either with older Windows support or why we can justify only supporting ANSI escape codes.

Otherwise, at this time I am closing out this issue, since there is no plan to grow the clap API to support this.

---

_Closed by @epage on 2022-01-11 17:19_

---

_Label `S-triage` removed by @epage on 2022-01-11 18:20_

---

_Label `S-wont-fix` added by @epage on 2022-01-11 18:20_

---

_Referenced in [clap-rs/clap#3407](../../clap-rs/clap/pulls/3407.md) on 2022-02-05 15:42_

---

_Comment by @epage on 2022-08-18 13:30_

Since this was closed, I've started work on https://github.com/epage/anstyle which aims to allow color definitions to be used in an API independent of the implementation which will allow users to define their stylesheet.

Blocked on https://github.com/epage/anstyle/issues/14

---

_Reopened by @epage on 2022-08-18 13:30_

---

_Label `S-wont-fix` removed by @epage on 2022-08-18 13:30_

---

_Label `S-blocked` added by @epage on 2022-08-18 13:30_

---

_Referenced in [clap-rs/clap#4110](../../clap-rs/clap/pulls/4110.md) on 2022-08-24 15:27_

---

_Referenced in [clap-rs/clap#4114](../../clap-rs/clap/pulls/4114.md) on 2022-08-25 18:44_

---

_Referenced in [clap-rs/clap#2963](../../clap-rs/clap/issues/2963.md) on 2022-08-26 12:23_

---

_Referenced in [clap-rs/clap#4132](../../clap-rs/clap/issues/4132.md) on 2022-08-27 02:18_

---

_Added to milestone `4.x` by @epage on 2022-08-30 21:21_

---

_Comment by @epage on 2022-09-21 14:12_

From [reddit](https://www.reddit.com/r/rust/comments/xjlie4/preannouncing_clap_40_a_rust_cli_argument_parser/ipa403b/)
> My preferred solution would be that, when customization is figured out, have the existing color behavior as some sort of easy to select "preset".

We could include presets for color schemes to help get people going.

---

_Comment by @nyurik on 2022-10-07 15:19_

I recently switch to v4, and the lack of help colors is a bit disheartening - I grew fond of seeing colors in all the "cool new Rust-based Linux tools", and that color is what instantly made these tools stand apart from the old GNU tools.  Now Clap only uses bold/underline, while continuing to call it "color". I tried to re-enable the legacy colors somehow, but despite searching all the release notes that only mention it in passing, and skimming through a very lengthy discussion on removing colors, I am beginning to suspect it is not (yet) possible.

To avoid breaking existing functionality, I feel v4 should have included the support for existing color styling from the beginning, even if it would require something like `#[clap(legacy_color_scheme = true)]`.

Is this what is planned for the next release?  Regardless of the minor grief above, thank you for all the hard work on Clap - it's one of the best crates we have in Rust! :)

---

_Comment by @epage on 2022-10-07 15:36_

@nyurik 

> I tried to re-enable the legacy colors somehow, but despite searching all the release notes that only mention it in passing, and skimming through a very lengthy discussion on removing colors, I am beginning to suspect it is not (yet) possible.

#4132 summarizes all of the help changes and includes workarounds, where possible.

For this case, it says

> Users can't rollback until theming support is in (https://github.com/clap-rs/clap/issues/3234) though they can just disable the coloring with Command::disable_colored_help(true)

@nyurik 
> legacy_color_scheme 

Such a flag is not going to be supported.  Instead, our focus will be to theming

@nyurik 
> Is this what is planned for the next release

We do not commit to specific features for a "next release" as we we release on almost every PR made. 

Addressing color support, in general, is a priority though.   The main thing blocking theming support is releasing a 1.0 of https://docs.rs/anstyle/latest/anstyle/ which is blocked on getting more feedback on the API / more runtime to make sure the API is good for a 1.0 release, see https://github.com/epage/anstyle/issues/14, see also [my earlier comment](https://github.com/clap-rs/clap/issues/3234#issuecomment-1219495963)

As it becomes a chicken and egg problem, my plan has been to implement styling support in #3108 / #1433 as that will at least give me some experience with `anstyle` to decide when its ready to go 1.0 and be safe to use in other crate's APIs.

---

_Comment by @nyurik on 2022-10-07 15:46_

Thanks @epage for such a great in-depth reply!   I think we should modify the CHANGELOG file (and possibly the release v4 comment) -- to make it clearer.  I searched for the word "color", and nothing came up. I found this

> We've moved to a more neutral palette for highlighting elements (not highlighted above)

which is not a good explanation when a highly visible breaking change is introduced. Perhaps something like this text?  I will be happy to make a pull request.

> We have removed colored help in favor of bold and underlined text. There is currently no way to re-enable the old color scheme until the styling (#3108 / #1433) is implemented.

---

_Comment by @epage on 2022-10-07 15:53_

As I'm trying to focus to get other things done so I can get back to that work, I do not have time to litigate a discussion about how to handle this (sorry, you are partly getting the short end of the stick from some other individuals taking up too much time / attention).  As that is the case, we'll leave it as-is for now.

---

_Referenced in [sharkdp/bat#2327](../../sharkdp/bat/pulls/2327.md) on 2022-10-07 17:00_

---

_Referenced in [clap-rs/clap#4429](../../clap-rs/clap/issues/4429.md) on 2022-10-29 12:49_

---

_Referenced in [clap-rs/clap#2389](../../clap-rs/clap/issues/2389.md) on 2022-11-03 02:07_

---

_Referenced in [clap-rs/clap#4444](../../clap-rs/clap/pulls/4444.md) on 2022-11-03 02:08_

---

_Referenced in [ProvableHQ/snarkOS#2113](../../ProvableHQ/snarkOS/pulls/2113.md) on 2022-11-29 16:36_

---

_Referenced in [lu-zero/cargo-c#299](../../lu-zero/cargo-c/pulls/299.md) on 2022-12-18 17:26_

---

_Comment by @casey on 2022-12-25 22:54_

Is this the issue to follow for color support? I'd like to upgrade to clap v4, but the lack of colored help is a blocker. I found this issue, but the title is a little ambiguous.

---

_Comment by @epage on 2022-12-25 23:21_

Yes, this is the issue in that it will let you color the help as you wish

---

_Referenced in [clap-rs/clap#4674](../../clap-rs/clap/issues/4674.md) on 2023-01-25 04:06_

---

_Referenced in [NNPDF/pineappl#145](../../NNPDF/pineappl/issues/145.md) on 2023-02-01 10:30_

---

_Comment by @dream-dasher on 2023-02-04 07:24_

Same as @casey.
A sea of almost identical white text adds a lot of parse work to the user.  And drains a lot of joy.  A hard no for any app with an ounce of kindness, imo.

I'm glad to see that v3 documentation still exists though!  

Is the plan to make color default again with specification possible or only allow color via manual specification?  (If the later then that's a level of friction not consistent with my use case [I'm just looking to for very basic, readable user interfaces] and would just like to have a sense of trajectory before I invest in this system.  [new to rust and trying to find cli app frameworks that let me focus on non-interface elements -- thanks in advance for any answer] )  

---

_Comment by @epage on 2023-02-04 15:04_

The plan is to add customization of whats already there. If someone wants to come up with a counter proposal for colors that meets the stated goals, we'll evaluate adopting it. So far, no one has taken up that offer. Once we allow customization, we're hoping people will do that implicitly and we'll watch to see what develops in the community.

As for other Rust CLI parsers, I believe they all have less text styling than clap.

---

_Comment by @dream-dasher on 2023-02-04 16:14_

Thanks for the response @epage.
I'm not at a place in my journey where I can fruitfully offer help on that issue. (maybe eventually!)
I'll look into front-ending my rust code with another language and use one of their frameworks. (which seems funny for a cli, but different eco-systems are in different places)

Thanks for all the work you are doing!
I can only imagine being the de facto face of argument parsing for the language is a lot of responsibility and makes decisions that leave any given set-up less supported feel very weighty.

---

_Comment by @dream-dasher on 2023-02-04 20:00_

I've noticed that piping the output of clap into [bat](https://github.com/sharkdp/bat) 🦈 🦇  gives _excellent_ default highlighting.  (i.e. easy to scan and parse)

Having only just begun looking at these crates (clap & bat's library) it's not immediately clear how to run the clap output through [bat library](https://docs.rs/bat/latest/bat/) and gain syntax advantages.  (much less apply syntax processing during compilation vs runtime)

But a crate extension that enabled coloring that works on __many_/_most_ terminals, but, being an extension, needn't support the same breadth as clap seems to me to be a possible workaround.

Having an extension that deals with colorschemes and plugs into output syntax also seems like it might be a nice decoupling for the project.

Not sure how many suggestions you want from the peanut gallery. 🥜 🎪 
But as far as the core code existing (though possibly not cleanly interfaceable ways 🤷) -- this does look like a promising approach from the outside.

Any thoughts?
Has this been considered and reject previously?  (e.g. due to code being so runtime focused that it couldn't easily get worked into a compile time extension)

_______________

Below, an example of _runtime_ syntax parsing and highlighting of clap output, using the [QuickStart Derive example](https://docs.rs/clap/4.1.4/clap/_derive/_tutorial/index.html) from current docs (v4.1)
<img width="645" alt="clap_v4 with bathelp" src="https://user-images.githubusercontent.com/33399972/216786888-fe2ceb47-7270-4a52-9822-270c8f22bf51.png">



---

_Comment by @epage on 2023-02-04 21:53_

> Not sure how many suggestions you want from the peanut gallery. 🥜 🎪
> But as far as the core code existing (though possibly not cleanly interfaceable ways 🤷) -- this does look like a promising approach from the outside.
>
> Any thoughts?
> Has this been considered and reject previously? (e.g. due to code being so runtime focused that it couldn't easily get worked into a compile time extension)

This is a fairly complex solution with a large binary size and build-time impact.  The proposed solution in this PR is relatively simple and mall; I just need to wrap up another project first.

---

_Referenced in [clap-rs/clap#4786](../../clap-rs/clap/issues/4786.md) on 2023-03-24 21:04_

---

_Referenced in [mozilla/grcov#1000](../../mozilla/grcov/pulls/1000.md) on 2023-03-26 06:34_

---

_Referenced in [clap-rs/clap#4843](../../clap-rs/clap/pulls/4843.md) on 2023-04-18 20:21_

---

_Comment by @epage on 2023-04-18 20:54_

Support for this is now available as `Command::styles` with v4.2.3 when the `unstable-styles` feature is enabled. 

Note: we are not guaranteeing semver compatibility on this feature until the feature flag is removed.

---

_Comment by @dhruvkb on 2023-04-19 15:48_

Thank you @epage! Are there any examples/docs we can refer to? The docs for `Command::styles` seem to be incorrectly copied from `Command::color`. 

<img width="1012" alt="Screenshot 2023-04-19 at 7 47 57 PM" src="https://user-images.githubusercontent.com/16580576/233130245-381d2121-b550-4e94-b806-c226021fbe05.png">


---

_Comment by @epage on 2023-04-19 16:03_

clap-rs/clap#4845 is correcting it

---

_Comment by @epage on 2023-04-19 16:16_

I guess one question is whether we should re-export anstyle (and how) or require people to depend on it directly.

Re-exports are more convenient (no need for extra dep) and keep things in sync (though anstyle will hopefully never go 2.0 or if it does, we;'ll have a long time inbetween).

My guess is we could expose it as `clap::builder::style`.

Thoughts?

---

_Comment by @dhruvkb on 2023-04-19 19:11_

I like the idea of re-exporting as it prevents the developer from needing to worry about compatibility 👍.

On a side note, I use the, derive notation but have to import `Styles` from `clap::builder`.

```rust
#[command(styles=clap::builder::Styles::plain())]
pub struct Arguments {
}
```

---

_Comment by @DianaNites on 2023-04-19 20:06_

As a user, I always prefer when a libraries public dependencies are re-exported, the way I see it, its not *my* dependency, its the libraries, and I don't want to have to manage it in my `Cargo.toml`

As for how, I think re-exporting from the crate root is the best when its an entire dependency rather than just a type or few.

---

_Comment by @epage on 2023-04-19 20:32_

> On a side note, I use the, derive notation but have to import Styles from clap::builder.

That is true of a lot of functionality.  We onlyh re-export the core types in the root.

---

_Comment by @epage on 2023-04-19 20:35_

> As for how, I think re-exporting from the **crate root** is the best when its an entire dependency rather than just a type or few.

*(Emphasis mine)*

If `anstyle` wasn't an independent crate, I'd most likely expose it in `clap::builder` or `clap::builder::style`.

I feel like a similar approach should be taken with re-exports.  I feel like dumping it in the root without taking into account any other considerations can lead to a bloated root, making it harder to find things, and a less cohesive experience.

But getting opinions out like this is why I didn't immediately re-export but brought it up here so I can see what different people's thoughts were.

---

_Comment by @bryantbiggs on 2023-04-19 21:21_

any pointers on how to enabled styled output - I've followed @dhruvkb snippet above but the output looks the same as before (and I enabled the `colors` and `unstable-styles` features as well) 

---

_Comment by @epage on 2023-04-19 21:40_

@bryantbiggs can you provide a minimal, complete reproduction case for whats not working for you, with a description of what you'd expect?

---

_Comment by @dhruvkb on 2023-04-20 07:50_

I was able to color the section headers and argument names using this snippet (building on the [`styled_str.rs` source file](https://docs.rs/clap_builder/4.2.3/src/clap_builder/builder/styled_str.rs.html#229).

```rust
pub fn get_styles() -> clap::builder::Styles {
	clap::builder::Styles::styled()
		.header(
			anstyle::Style::new()
				.bold()
				.underline()
				.fg_color(Some(anstyle::Color::Ansi(anstyle::AnsiColor::Blue))),
		)
		.literal(
			anstyle::Style::new()
				.bold()
				.fg_color(Some(anstyle::Color::Ansi(anstyle::AnsiColor::Cyan))),
		)
}

#[command(styles=get_styles())]
pub struct Arguments {
	// ...
}
```

---

_Comment by @eatradish on 2023-04-20 11:33_

Doesn't seem to be available in the builder api? (clap 4.2.4 with "cargo", "wrap_help", "unstable-styles", "color" features, rustc 1.68.2)

```
error[E0603]: struct `Styles` is private
  --> src/args.rs:1:37
   |
1  | use clap::{builder::{PossibleValue, Styles}, command, Arg, ArgAction, Command};
   |                                     ^^^^^^ private struct
   |
note: the struct `Styles` is defined here
  --> /home/saki/.cargo/registry/src/github.com-1ecc6299db9ec823/clap_builder-4.2.4/src/builder/mod.rs:66:16
   |
66 | pub(crate) use styled_str::Styles;
   |                ^^^^^^^^^^^^^^^^^^

error[E0599]: no method named `styles` found for struct `Command` in the current scope
  --> src/args.rs:49:10
   |
49 |         .styles(Styles::styled().usage(Default::default()))
   |          ^^^^^^ help: there is a method with a similar name: `get_styles`

Some errors have detailed explanations: E0599, E0603.
For more information about an error, try `rustc --explain E0599`.
```

---

_Comment by @epage on 2023-04-20 13:42_

@eatradish did you run `cargo add clap -F unstable-styles`?

I wish rustc would partially parse disabled `cfg`s so it could tell suggest the feature to you

---

_Referenced in [build-trust/ockam#4652](../../build-trust/ockam/pulls/4652.md) on 2023-04-20 14:24_

---

_Comment by @eatradish on 2023-04-20 18:02_

> @eatradish did you run `cargo add clap -F unstable-styles`?
> 
> I wish rustc would partially parse disabled `cfg`s so it could tell suggest the feature to you

My fault, I have set this version in Cargo.toml:

```
clap = { version = "4.2", features = ["cargo", "wrap_help", "color", "unstable-styles"] }
```

But the right thing to do is:

```
clap = { version = "4.2.4", features = ["cargo", "wrap_help", "color", "unstable-styles"] }
```

---

_Comment by @dhruvkb on 2023-04-26 09:51_

I cannot find the style to use to change the appearance of the default and possible values. They always appear white. Is that not currently supported?

<img width="973" alt="Screenshot 2023-04-26 at 1 51 01 PM" src="https://user-images.githubusercontent.com/16580576/234539329-81d4ab72-858c-4991-82db-8cfe02cb740e.png">

I understand that this might not be directly related to this issue, so I can open a new one if that's more appropriate.


---

_Comment by @epage on 2023-04-26 11:59_

We do not yet have a style set apart for those.  Any thoughts on a name?  I was originally thinking "hint" though that might be confused the the "tip"s in error messages.

---

_Comment by @dhruvkb on 2023-04-26 12:26_

"default" could be a good name for the default value, and "possibilities" for the possible values. That seems the most intuitive to me.

---

_Comment by @epage on 2023-04-26 12:42_

I don't want to create a style per one of these bracketed items as that doesn't scale well for us to add more in the future.

Note: there is also one for env variables

---

_Comment by @epage on 2023-05-02 14:04_

Besides naming the styling element for defaults, possible values, and env variables, I'm curious on how people think we should style it.

Some options I thought of:
- `{spec}[default: true]{reset}` (all styled the same)
- `{spec}[default: {literal}true{reset}{spec}]{reset}` (outer-styled, value stuled)
- `[{spec}default:{reset} {literal}true{reset}]` (key-styled, values-styled)


---

_Comment by @nyurik on 2023-05-02 17:38_

I might be misunderstanding the last post - if `{spec}`, `{literal}`, and `{reset}` are ANSI color escape sequences, why does it say "all styled the same"?

I think the 1st variant is a subset of the 2nd (i.e. if literal style is the same as the spec) - so I would think the 2nd variant is preferred as it offers more flexibility. I also think that by default literal and spec should be the same color (thus producing the 1st variant) - as less color-disruptive (I wouldn't want my help screen to look like the early days of MS Word with word-art, with everyone using every single font available to print a flier).  I am not a big fan of the 3rd.

---

_Comment by @epage on 2023-05-02 17:45_

> I might be misunderstanding the last post - if {spec}, {literal}, and {reset} are ANSI color escape sequences, why does it say "all styled the same"?

Sorry, forgot to go back and fix those

> I think the 1st variant is a subset of the 2nd (i.e. if literal style is the same as the spec) - so I would think the 2nd variant is preferred as it offers more flexibility. I also think that by default literal and spec should be the same color (thus producing the 1st variant) - as less color-disruptive (I wouldn't want my help screen to look like the early days of MS Word with word-art, with everyone using every single font available to print a flier). I am not a big fan of the 3rd.

The difference between (1) and (2) is `literal` is an existing style, used by `--flags` and acts like backticks in markdown.

---

_Comment by @nyurik on 2023-05-02 17:50_

thx, in that case yes, I think the 2nd is the best because it would allow this help screen (from above):

<img alt="Screenshot 2023-04-26 at 1 51 01 PM" width="973" src="https://user-images.githubusercontent.com/16580576/234539329-81d4ab72-858c-4991-82db-8cfe02cb740e.png">

to be consistent for both the default and possible values:

> display icons next to node names {spec}[default: `true`] [possible values: `true`, `false`]{reset}

---

_Comment by @epage on 2023-05-02 17:56_

To be clear, (2) would cause `true`, `true`, and `false` to be teal like `--icons`, `--align`, and `--multi-cols`.

I hesitate about (2) because we want to draw attention to literals in the first column and in usage but it feels weird to in the help description, it might be too bus.

btw this is the current way we highlight something similar in errors
```
                        let _ = write!(styled, "\n{TAB}[possible values: ");
                        if let Some((last, elements)) = possible_values.split_last() {
                            for v in elements {
                                let _ = write!(
                                    styled,
                                    "{}{}{}, ",
                                    valid.render(),
                                    Escape(v),
                                    valid.render_reset()
                                );
                            }
                            let _ = write!(
                                styled,
                                "{}{}{}",
                                valid.render(),
                                Escape(last),
                                valid.render_reset()
                            );
                        }
                        styled.push_str("]");
```

---

_Comment by @nyurik on 2023-05-02 19:18_

I think the most ideal for discussion is some real rendering image of all variants, perhaps in a few default variants (console vs markdown)?  Kinda tricky to discuss it in a plain text format.

P.S. your code example made me realize it could use some DRY, so I made a https://github.com/clap-rs/clap/pull/4876 PR - hope its ok - deletes 23 lines :)

---

_Comment by @AndreasBackx on 2023-05-25 14:21_

I was working on some tooling that makes it easier for people to switch over from x to Rust and came across this Python Click library to add colours to `--help`. I noticed it allows overriding colours per command as well. I could see it being useful where you colour some subcommands red to showcase they might be destructive or might require you being an admin or something (not referring to OS level root/admin but for whatever tool is being built). https://github.com/click-contrib/click-help-colors

Food for thought, could be something that needs thinking about for this particular issue as well.

---

_Comment by @epage on 2023-05-25 14:43_

I think that would be a useful feature and, with the plugin system I've been adding to clap, it should minimize some of the negative effects of adding it.

The question for this thread is if there is anything about that that would require breaking changes to the existing feature before we stabilize it.  The rest can be deferred out into its own issue.

Unfortunately, not seeing a quick reference, so from what I gather they allow
- help_headers_color
- help_options_color
- help_options_custom_colors (maps arg or command to color)
- prog_name_color (for `--version`)
- version_color (for `--version`)
- message_color (for `--version`)

All of that can be set on a per-command basis

One problem I see with what we currently do is we copy out styles to subcommand but we overwrite the subcommand, rather than allowing the subcommand to have special behavior.

Our style sheet is more fine grained in areas but doesn't allow controlling prog name and version separately.  Our plugin system would offer a better way for coloring most things (color set directly on arg rather than in a map) but it won't let us easily hit things like version **but** we allow setting the version as styled./

For if/when we support per-arg and per-command styling, we'll need to make sure `StyledStr` can be sorted agnostic of styling.

---

_Referenced in [clap-rs/clap#5016](../../clap-rs/clap/pulls/5016.md) on 2023-07-17 22:23_

---

_Comment by @epage on 2023-07-18 01:27_

A new release is out that re-exports `anstyle` and includes some more examples

---

_Comment by @epage on 2023-07-18 01:28_

As for styling of defaults, https://github.com/Canop/clap-help is somewhat similar to one of the options being considered.

---

_Comment by @gibfahn on 2023-07-27 22:37_

The default example in https://github.com/Canop/clap-help#with-clap-help seems quite hard to read to me (compared to the current output), it's hard to see which option in the left column maps to which description on the right column, and it's hard to see where one option's help ends and the next one begins in the right column.

This is even more true when you have more detailed help text with examples for the options.

---

_Comment by @epage on 2023-07-28 00:32_

I was more wanting to highlight how it styling the text (which ties into this issue) and  not the wider formatting change

---

_Comment by @epage on 2023-08-28 14:37_

At this point, the remaining open questions are
- How should `[default: value]` be styled / support styling
  - I'm leaning towards a uniform style for the whole block of text which means we won't use an existing style and can add this later. I'll create an issue for tracking this
  - EDIT: #5093
- Any future extensibility for customization
  - I'm thinking we shouldn't further block on this and create a separaet issue.  @AndreasBackx mind creating an issue?

So at this point, I think I'm going to move towards stabilization

---

_Referenced in [clap-rs/clap#5093](../../clap-rs/clap/issues/5093.md) on 2023-08-28 14:40_

---

_Referenced in [clap-rs/clap#5094](../../clap-rs/clap/pulls/5094.md) on 2023-08-28 14:50_

---

_Closed by @epage on 2023-08-28 15:10_

---

_Referenced in [clap-rs/clap#5095](../../clap-rs/clap/issues/5095.md) on 2023-08-28 15:12_

---

_Referenced in [clap-rs/clap#5098](../../clap-rs/clap/issues/5098.md) on 2023-08-28 17:44_

---

_Comment by @donovanglover on 2023-10-28 13:59_

It took me some time to figure this out, but [here's an example](https://github.com/donovanglover/hyprdim/commit/c9be9b037616c5b929d177c8c5dfb82f34242d8d) of adding color to `--help`.

```rust
use clap::builder::styling::{AnsiColor, Effects, Styles};
use clap::Parser;

fn styles() -> Styles {
    Styles::styled()
        .header(AnsiColor::Red.on_default() | Effects::BOLD)
        .usage(AnsiColor::Red.on_default() | Effects::BOLD)
        .literal(AnsiColor::Blue.on_default() | Effects::BOLD)
        .placeholder(AnsiColor::Green.on_default())
}

#[derive(Parser)]
#[command(author, version, about, styles = styles())]
pub struct Cli {
    // ...
}
```

---

_Referenced in [chrissimpkins/siz#14](../../chrissimpkins/siz/issues/14.md) on 2023-12-22 05:14_

---

_Referenced in [rustic-rs/rustic#1003](../../rustic-rs/rustic/issues/1003.md) on 2024-01-13 21:44_

---

_Comment by @murlakatamenka on 2024-03-12 23:54_

> It took me some time to figure this out, but [here's an example](https://github.com/donovanglover/hyprdim/commit/c9be9b037616c5b929d177c8c5dfb82f34242d8d) of adding color to `--help`.
> 
> ```rust
> use clap::builder::styling::{AnsiColor, Effects, Styles};
> use clap::Parser;
> 
> fn styles() -> Styles {
>     Styles::styled()
>         .header(AnsiColor::Red.on_default() | Effects::BOLD)
>         .usage(AnsiColor::Red.on_default() | Effects::BOLD)
>         .literal(AnsiColor::Blue.on_default() | Effects::BOLD)
>         .placeholder(AnsiColor::Green.on_default())
> }
> 
> #[derive(Parser)]
> #[command(author, version, about, styles = styles())]
> pub struct Cli {
>     // ...
> }
> ```

Piggybacking on this, since `Styles::styled` is [const](https://docs.rs/clap/4.5.2/clap/builder/struct.Styles.html):

```rs
use clap::{
    builder::styling::{AnsiColor as Ansi, Styles},
    Parser,
};

const MY_AWESOME_STYLE: Styles = Styles::styled()
    .header(Ansi::Red.on_default().bold())
    .usage(Ansi::Red.on_default().bold())
    .literal(Ansi::Blue.on_default().bold())
    .placeholder(Ansi::Green.on_default());

#[derive(Parser)]
#[command(styles = MY_AWESOME_STYLE)]
pub struct Cli {
    // ...
}

fn main() {
    let _cli = Cli::parse();
}
```

Clap v3 Style:
- https://docs.rs/clap/4.5.2/clap/builder/struct.Styles.html#example

`rustic`'s style:
- https://github.com/rustic-rs/rustic/blob/6fffaad1cc1aeebf874063fc62125f4ce3b490b5/src/commands.rs#L143-L149

---

_Comment by @MilesCranmer on 2024-04-15 11:09_

Here's an example of manually customizing the colors in the help template:

https://github.com/MilesCranmer/rip2/blob/29b46efbff046515360cf5a16bdbae678db90b37/src/args.rs#L8-L83

Both in the text (with anstyle render) and by letting clap do it.

On white background:

![Screenshot 2024-04-15 at 12 12 41](https://github.com/clap-rs/clap/assets/7593028/d48e1704-1eab-40bc-b49c-c0cc798c7cb1)

And dark:

![Screenshot 2024-04-15 at 12 13 15](https://github.com/clap-rs/clap/assets/7593028/eb3df6e2-e83f-4df8-b82e-9583875792f8)

(Same color scheme as `cargo`)


---

_Referenced in [astral-sh/uv#3473](../../astral-sh/uv/issues/3473.md) on 2024-07-07 18:34_

---

_Referenced in [robiot/rustcat#69](../../robiot/rustcat/pulls/69.md) on 2024-07-17 18:49_

---

_Comment by @heaths on 2024-08-07 08:33_

> We can't accept ANSI escape codes because clap3 intentionally added support for non-ANSI Windows terminals.

I've started digging into this problem and came across this. As someone who's worked a lot with the Windows Terminal team who added conpty support to both wt.exe and conhost.exe circa Windows 10, as well as helped the GitHub CLI team with a bunch of ANSI color issues, this is solvable. There are a lot of linked issues so I'm not sure if this has been discussed, but effectively, if you call `GetConsoleMode()` to get the current console mode, OR that with `ENABLE_VIRTUAL_TERMINAL_PROCESSING`, and call `SetConsoleMode()`. If that returns non-zero, conpty was enabled and you can safely use ANSI colors.

See <https://github.com/microsoft/vswhere/blob/9fd6f2c6df9ccff186412121f2016b81ff4d9e4c/src/vswhere.lib/Console.cpp#L116> or <https://github.com/cli/go-gh/blob/25db6b99518c88e03f71dbe9e58397c4cfb62caf/pkg/term/console_windows.go#L12>. Any terminal from Windows 10 TH2 (IIRC) in the OS will support this, though third-party solutions may not; however, they should fail that call as well, so you know not to write ANSI escape sequences (or filter them out on write - whichever is easier since parsing them out is fairly trivial e.g., <https://github.com/heaths/go-console/blob/d0d8e65f7196273cfb7f2354f48c13beae9d60d8/internal/writer/writer.go#L27>).

Or, does this really come down to just being able to theme the colors reliably and easily? There are quite a few linked issues/discussions about this. I've read through a couple. But there are quite a few just about the lack of color at all.

---

_Comment by @epage on 2024-08-07 14:20_

@heaths that is a pretty stale comment you are replying to and so a lot of your reply doesn't seem relevant.  This issue has long since been resolved,  We knew of and take care of enabling ANSI escape codes on Windows.

---

_Comment by @heaths on 2024-08-08 06:41_

v4 still shows non-colored text. Searching the issues, I found this issue and nothing obvious more recent. Is there something more recent? Again reviewing the docs and writing a quick sample, I only see underlined text.

---

_Comment by @gibfahn on 2024-08-08 12:53_

I believe this is how you style the text: https://docs.rs/clap/latest/clap/struct.Command.html#method.styles

---

_Referenced in [clap-rs/clap#5636](../../clap-rs/clap/pulls/5636.md) on 2024-08-08 13:02_

---

_Comment by @epage on 2024-08-08 13:51_

@heaths the fact that you saw underlined (and I assume bold) means you were getting ANSI escape codes.

In [4.0.0](https://github.com/clap-rs/clap/blob/master/CHANGELOG.md#400---2022-09-28)

> We've moved to a more neutral palette for highlighting elements (not highlighted above)

See also #2963 which was linked to from the first post of mine that you quoted from.  In CLIs, the term "color" tends to be a placeholder for "any ansi escape codes".

This issue is about customizing it so you don't have to use that color palette and `cargo --help` is an example of that in practice.

With this issue closed, we can be a bit more "daring" with our choice and I am open to people discussing what would be a good "universal" / "safe" default color palette in its own issue.  Note though that changing the color palette would be considered a breaking change according to [our guidelines](https://github.com/clap-rs/clap/blob/master/CONTRIBUTING.md#compatibility-expectations).

---

_Comment by @heaths on 2024-08-08 14:57_

Are their docs on how to change the ANSI escape sequences? I looked prior to replying originally - understanding at least part of this issue was about that - but found nothing. To be honest, I care less about the defaults if it's easy to customize.

---

_Comment by @epage on 2024-08-08 15:10_

@heaths they were linked to earlier in https://github.com/clap-rs/clap/issues/3234#issuecomment-2275752109, see https://docs.rs/clap/latest/clap/struct.Command.html#method.styles

---

_Comment by @heaths on 2024-08-08 15:12_

Apologies. I missed that. Any objection to linking that or even adding an example to the tutorial docs? I'd be happy to submit a PR if you're cool with that.

---

_Comment by @heaths on 2024-08-08 15:19_

Actually, looking more carefully through https://docs.rs/clap/latest/clap/_derive/index.html#command-attributes it seems that's not currently possible. I will open a separate issue to perhaps accept a function to supply styles or something, or has that been discussed already as well? I prefer derive to builder myself and would love to more easily take advantage of this behavior.

---

_Comment by @epage on 2024-08-08 15:21_

@heaths I've found that each person has their own problem they've run into and "put it in the tutorial" is a common solution proposed.  To do that in all cases would make it as hard to find things in the tutorial as the API.  I don't have a good answer for where the "line" should be.

However, cargo has adopted custom colors and so it would be reasonable to add color support to our cargo [cookbook entry](https://docs.rs/clap/latest/clap/_derive/_cookbook/index.html) and highlight in the summary that its using custom colors.

> Actually, looking more carefully through https://docs.rs/clap/latest/clap/_derive/index.html#command-attributes it seems that's not currently possible. I will open a separate issue to perhaps accept a function to supply styles or something, or has that been discussed already as well?

What in that is giving the impression its not supported?  I'm using it in cargo-release
https://github.com/crate-ci/cargo-release/blob/e30b1c6c645700f4189b76130de0896e19f8d0b1/src/bin/cargo-release.rs#L54

If its because the attribute isn't explicitly named, thats because it falls under the catch-all "raw attribute" section, see also #4090

---

_Comment by @heaths on 2024-08-08 15:31_

Fair point about where to draw the line, but seems styles is a pretty major feature to at least warrant an example. As much attention as the lack of colors in v4 got across a few projects, seems like a good idea to have a section or even chapter about.

That thread about raw attributes also points out how there's no mention in the tutorial docs. Clap has an expansive API and it seems that features like this getting called out in the tutorial with links to the to docs could help. Personally, your tutorials are good enough that I typically don't need to dig into the ref docs too much. The only time I remember was when writing my own value parser. I don't imagine I'm alone in this.

---

_Referenced in [clap-rs/clap#5638](../../clap-rs/clap/pulls/5638.md) on 2024-08-08 15:31_

---

_Comment by @epage on 2024-08-08 15:36_

#5638 is up to add it to the cookbook.  Part of the role of the cookbook is for people to say "I want this feature from this command, how do I do it".

> That thread about raw attributes also points out how there's no mention in the tutorial docs.

It is called out in the tutorial, likely added based on that feedback, e.g.

> Any [Command](https://docs.rs/clap/latest/clap/struct.Command.html) builder function can be used as an attribute, like [Command::next_line_help](https://docs.rs/clap/latest/clap/struct.Command.html#method.next_line_help).

See https://docs.rs/clap/latest/clap/_derive/_tutorial/chapter_1/index.html

I'm likely to write a documentation generator based on `cargo doc --json` but haven't gotten to it yet.  If someone wants to take that on, I can give them rough guidelines of what I expect (and what is left undecided).

---

_Comment by @heaths on 2024-08-08 18:34_

> I'm likely to write a documentation generator based on cargo doc --json but haven't gotten to it yet. If someone wants to take that on, I can give them rough guidelines of what I expect (and what is left undecided).

Are these written down somewhere, or do we want to take that to a new issue or zulip? I might be game.

---

_Comment by @epage on 2024-08-08 18:51_

The current write up is at https://github.com/clap-rs/clap/discussions/4090#discussioncomment-6973754 but moving to an issue could make it easier to track

---

_Comment by @heaths on 2024-08-09 02:23_

https://github.com/clap-rs/clap/issues/5639

---

_Referenced in [stellar/stellar-cli#1624](../../stellar/stellar-cli/issues/1624.md) on 2024-09-25 20:17_

---

_Referenced in [jj-vcs/jj#5716](../../jj-vcs/jj/pulls/5716.md) on 2025-02-16 02:18_

---
