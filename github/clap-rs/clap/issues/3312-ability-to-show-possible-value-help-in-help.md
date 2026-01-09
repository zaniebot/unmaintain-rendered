---
number: 3312
title: Ability to show possible value help in --help
type: issue
state: closed
author: kpreid
labels:
  - C-enhancement
  - S-waiting-on-decision
  - A-help
assignees: []
created_at: 2022-01-19T01:12:26Z
updated_at: 2022-03-02T15:21:30Z
url: https://github.com/clap-rs/clap/issues/3312
synced_at: 2026-01-07T13:12:19-06:00
---

# Ability to show possible value help in --help

---

_Issue opened by @kpreid on 2022-01-19 01:12_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Clap Version

3.0.8

### Describe your use case

I have options which take one of several possible values and which would benefit from explaining what each of the values do. I am currently putting this text in the option's `help` but I could easily forget to update that, and a list inside the help text doesn't wrap nicely.

### Describe the solution you'd like

A setting which causes each possible value's [PossibleValue::help](https://docs.rs/clap/3.0.10/clap/struct.PossibleValue.html#method.help) to be displayed as part of the help for the option (instead of just listing the possible values without any help, which is the current default behavior).

(Perhaps it should not be an additional setting, but be the default behavior whenever the possible values have help strings. I don't know what typical use cases look like to say whether this would be undesirable verbosity.)

### Alternatives, if applicable

I currently have hardcoded option help text listing the values, which is what I was hoping `PossibleValue::help` would replace before I tried it and found that it didn't. Another alternative would be to implement this using existing facilities by iterating over the possible values and building a help string. However, clap could do this better than the application can, by formatting the list of values and help in a text-wrapping-aware way.

### Additional Context

_No response_

---

_Label `C-enhancement` added by @kpreid on 2022-01-19 01:12_

---

_Comment by @epage on 2022-01-19 01:19_

@ModProg as the person who implemented this, I assume you use this and would have thoughts / perspective on whether we should show this in `--help`, whether always or behind a flag.

If we do show the `PossibleValue::help`, we would probably limit it to `--help` (long help) and not show it in `-h` (short help)

---

_Comment by @ModProg on 2022-01-19 08:23_

> If we do show the PossibleValue::help, we would probably limit it to --help (long help) and not show it in -h (short help)

Yeah, that makes sense I'd say, should probably be configurable, but the current behavior of only showing the help in the completions seams a bit strange in retrospect. Should have been in the help as well, is my gut reaction.

---

_Comment by @ModProg on 2022-01-19 08:35_

But I'm not sure on what would be the best way of displaying this:
```
    -p, --pos <VAL>      Some vals [possible values: fast, slow]
    -p, --pos <VAL>      Some vals
                         possible values: 
                            fast 
                            slow (not as fast)

```

---

_Comment by @epage on 2022-01-19 15:21_

Yes, I was thinking a bulleted list would probably be how we show this.  iirc I brought up showing them this way in the PR for `clap_man`.

The next question is if this would be considered a "breaking change", meaning that people have crafted their CLIs around the current behavior and it would egregiously impact the UI to change this without a more explicit acknowledgement of breaking behavior.

For builder users, it depends on if they've discovered and started to use this new feature while manually documenting the value meanings.

For derive users, the situation is different because we are picking up the doc comments and users are most likely not using `clap_complete` and if they are, they are unlikely to notice the value descriptions.  We could start showing to users comments meant for developers.

---

_Comment by @ModProg on 2022-01-19 16:20_

Well not if it was off by default, but I see your point.

As I would like this to be the default behaviour as that seams like the natural effect of the `help()` IMO.

This could be done in a two step prosses I guess (first opt in, then opt out)? Not sure, what the best way is.

---

_Comment by @epage on 2022-01-19 16:25_

My preference is to avoid configuration long term.  Clap's API is so bloated, it makes it difficult to know whats there or not.  If we can make a policy that can reasonably apply to nearly all programs, we have issues like #2914 for the rest.

If we are concerned about the transition, we could use either a manifest feature flag or a setting as we transition to the new behavior.  maybe we even toggle it for the major release after that but the following would see the option removed ideally.

---

_Comment by @ModProg on 2022-01-19 17:00_

yeah, I think this is the expected behavior, I just did not think about it when I first started looking into this.

IMO as the doc comments are relevant on all other clap derives, it is reasonable to have them *just* appear in the help message.

We can put a warning in the change log (tho I understand that most people won't look there). 

---

_Comment by @epage on 2022-01-19 17:05_

> We can put a warning in the change log (tho I understand that most people won't look there).

Yeah, another option I was considering was waiting for 3.1 and having a "Backwards Compatibility" section in the changelog, like Rust.

---

_Comment by @ModProg on 2022-01-19 17:10_

> > We can put a warning in the change log (tho I understand that most people won't look there).
> 
> Yeah, another option I was considering was waiting for 3.1 and having a "Backwards Compatibility" section in the changelog, like Rust.

Sounds reasonable, if there are more features like this that are kind of unstable/breaking changes we could put them behind a feature flag until 3.1 or just have an off by default setting till then.

---

_Comment by @kpreid on 2022-01-19 17:36_

Some thoughts as the requester:

I hadn't thought of the documentation not being suitable to be shown, but I was thinking that it might be _too long._ However, long possible value lists are likely to be `hide_possible_values` already, and the documentation for `PossibleValue::help` already says it should be concise, so that shouldn't be a problem.

Some command line tools offer dedicated options to list possible values when there are lots of them (to name a couple of examples I've seen, lists of compatible hardware, or file formats/codecs); it might be interesting to offer help implementing that, but on the other hand it's easy to do and might want customization.

As to specifics of formatting, here is the result of what I picked for my application-specific implementation:

```text
    -g, --graphics <MODE>        Graphics/UI mode; one of the following keywords:
                                 • window   — Open a window (uses OpenGL)
                                 • terminal — Colored text in this terminal (uses raytracing)
                                 • headless — Non-interactive; don't draw anything but only
                                 simulates
                                 • record   — Non-interactive; save an image or video (uses
                                 raytracing)
                                 • print    — Non-interactive; print one frame like 'terminal'
                                 mode then exit
                                  [default: window]
```

I don't entirely recommend exactly this: the bullets are a bit noisy, but I added them because from the outside I can't get clap to indented-wrap the entries for visual clarity (that is, indent the wrapped lines "`simulates`" and "`mode then exit`" so they are visibly part of the preceding item rather than their own items), and even if I did my own text wrapping I wouldn't know how long the left option-name column is.

I do think the columnar alignment is a good idea given enough available width. Here's my implementation:

```rust
static GRAPHICS_HELP: Lazy<String> = Lazy::new(|| {
    let pv_iter = GraphicsType::value_variants()
        .iter()
        .filter_map(|v| v.to_possible_value());

    let max_width = pv_iter
        .clone()
        .filter_map(|pv| pv.get_visible_name())
        .map(str::len)
        .max()
        .unwrap_or(0);

    let mut text = String::from("Graphics/UI mode; one of the following keywords:\n");
    for pv in pv_iter {
        // Note: There's a final newline so that clap's default value text is put on a new line.
        writeln!(
            text,
            "• {:max_width$} — {}",
            pv.get_name(),
            pv.get_help().unwrap()
        )
        .unwrap();
    }
    text
});
```

(This isn't quite fully general: it doesn't handle hidden items or ones without documentation, which didn't matter since it's hardcoded to this one enum.)

---

_Comment by @epage on 2022-01-19 18:45_

> I hadn't thought of the documentation not being suitable to be shown, 

In case it wasn't clear, the concern is more with people who have already written rustdoc comments who might be surprised to see those starting to show up in the help

> I hadn't thought of the documentation not being suitable to be shown, but I was thinking that it might be too long. However, long possible value lists are likely to be hide_possible_values already, and the documentation for PossibleValue::help already says it should be concise, so that shouldn't be a problem.

Yes, that is a concern.  One thing that helps is that clap has distinct `-h` and `--help`.  `-h` is meant to be brief and `--help` is meant to go into all of the gory details.  `-h` would still just show the list of possible values and we'd only be changing `--help`.

> Some command line tools offer dedicated options to list possible values when there are lots of them (to name a couple of examples I've seen, lists of compatible hardware, or file formats/codecs); it might be interesting to offer help implementing that, but on the other hand it's easy to do and might want customization.

A good example of this (though its more dynamic)
```console
$ cargo test --test    
error: "--test" takes one argument.                                      
Available tests:                    
    builder     
    derive                                                               
    derive_ui                       
    examples                                                             
    macros                                                                                                                                         
    yaml                                                                 
```

I feel like we could model our errors off of this.  Right now, we don't use possible values in the errors:
```console
$ git-stack --color
    Finished dev [unoptimized + debuginfo] target(s) in 0.04s
     Running `/home/epage/src/personal/git-stack/target/debug/git-stack --color`
error: The argument '--color <WHEN>' requires a value but none was supplied

USAGE:
    git-stack [OPTIONS]

For more information try --help

$ git-stack --color lklk
    Finished dev [unoptimized + debuginfo] target(s) in 0.04s
     Running `/home/epage/src/personal/git-stack/target/debug/git-stack --color lklk`
error: Invalid value for '--color <WHEN>': Unknown color choice 'lklk'

For more information try --help
```

> As to specifics of formatting, here is the result of what I picked for my application-specific implementation:

If we only do this for `--help`, we'll also be using `NextLineHelp` which will buy us some space (you can do this today with `long_help()`).

It is a good point that we should indent any wrapped lines for the bullets.

As for the exact bullet symbol, we can determine that later.

Using coloring will also help.

---

_Referenced in [clap-rs/clap#3320](../../clap-rs/clap/issues/3320.md) on 2022-01-19 18:48_

---

_Label `A-help` added by @epage on 2022-01-27 14:42_

---

_Label `S-waiting-on-decision` added by @epage on 2022-01-27 14:44_

---

_Referenced in [clap-rs/clap#3360](../../clap-rs/clap/issues/3360.md) on 2022-01-28 21:23_

---

_Added to milestone `3.1` by @epage on 2022-02-02 20:01_

---

_Removed from milestone `3.1` by @epage on 2022-02-08 18:39_

---

_Added to milestone `4.0` by @epage on 2022-02-08 18:39_

---

_Referenced in [clap-rs/clap#3451](../../clap-rs/clap/pulls/3451.md) on 2022-02-11 18:51_

---

_Comment by @ModProg on 2022-02-16 07:40_

Just ran into this exact issue. 

I now needed to specify separate help messages for long and short, because I wanted to have `[possible values ...]` in short help but 
```
- a help message
- b help message
```
in long help.

I thought about adding an aditional `hide_on_short/long_help` to [hide_possible_values](https://docs.rs/clap/latest/clap/struct.Arg.html#method.hide_possible_values), but that would only treat the symptom of not being able to have help messages on possible values.

If you have a plan on how this should be implemented @epage I would go ahead and look into it.


---

_Comment by @epage on 2022-02-16 14:47_

> If you have a plan on how this should be implemented @epage I would go ahead and look into it.

I've not done this yet because I keep oscillating on whether this would be considered a breaking change or not, with the biggest concern being for derive users.

What we can do is add a `unstable-v4` feature flag and guard the logic switch and tests with that flag.

Steps
- Add `unstable-v4` feature flag to `Cargo.toml` and document it in the README
- Add a `_FEATURES_next` to the `Makefile` which is `_FEATURES_full` + `unstable-v4`
- When outputting possible values, use the bulleted list when if printing long and `possible.any(|p| p.get_about().is_some())`

Rough template
```
<arg.get_long_about()>

Possible values:
<for possible in possibles>
- {GOOD}<possible.get_name()>{/GOOD}: <possible.get_help()>
</for>
```

**(of course, willing to hear other ideas on any of this)**

---

_Comment by @ModProg on 2022-02-19 20:01_

Makes sense, if I see correctly, the new possible value display would need to go here: https://github.com/clap-rs/clap/blob/bc2be89f4649fbccdc259952e8c4db72a6224c8b/src/output/help.rs#L436 as the other possible_values are contained in a str and cannot have formatting.

Should extract the logic whether to use the long possible value help in a helper function so that hiding the old help and showing the new help use the same code?

---

_Comment by @epage on 2022-02-20 00:39_

Yes, we'll have possible-value printing in two different places, depending on if its long or short.

That is the place for doing it for long.  The [current place we print possible values](https://github.com/clap-rs/clap/blob/bc2be89f4649fbccdc259952e8c4db72a6224c8b/src/output/help.rs#L575) is exclusively for the `[]` hints and so it wouldn't quite fit for us to output a long list in there, so we should specialize that for short possible values.

---

_Comment by @ModProg on 2022-02-20 12:14_

After some more investigation, a few questions:

1. Is there a case where long help is printed without `next-line`?
2. Should possible value help wrap like the normal help does?
  a. Print behind the `PossibleValue::name` as long as they fit in line, aligned by the longest possible value.
  b. Wrap when they are too long.
  c. When one of the `PossibelValue::name`s surpasses a threshold, they are displayed in the next line. Indented by one `TAB`.

---

_Comment by @epage on 2022-02-21 14:12_

> Is there a case where long help is printed without next-line?

Most likely not but unsure why this is relevant.  We should be basing what we do off of `self.use_long`.

> Should possible value help wrap like the normal help does?

Ideally

---

_Comment by @ModProg on 2022-02-22 20:02_

> Most likely not but unsure why this is relevant.

It is relevant because it means that the indenting logic needs to support it.

---

_Comment by @ModProg on 2022-02-22 20:17_

The possible value name will not be wrapped (when using `wrap_help`), this is the same as for too long argument flags. This is porbably not relevant for a normal usecase, but I thought I'd mention it.

---

_Referenced in [clap-rs/clap#3503](../../clap-rs/clap/pulls/3503.md) on 2022-02-23 09:15_

---

_Closed by @epage on 2022-03-02 15:15_

---

_Comment by @epage on 2022-03-02 15:21_

This is now available in v3.1.4 behind the `unstable-v4` feature flag.

---

_Referenced in [clap-rs/clap#3784](../../clap-rs/clap/issues/3784.md) on 2022-06-03 00:07_

---

_Referenced in [ducaale/xh#272](../../ducaale/xh/issues/272.md) on 2022-08-28 21:56_

---
