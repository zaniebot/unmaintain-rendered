---
number: 2055
title: Clap v3 ColoredHelp by default
type: issue
state: closed
author: pickfire
labels: []
assignees: []
created_at: 2020-08-09T15:17:29Z
updated_at: 2020-11-05T02:38:56Z
url: https://github.com/clap-rs/clap/issues/2055
synced_at: 2026-01-07T13:12:19-06:00
---

# Clap v3 ColoredHelp by default

---

_Issue opened by @pickfire on 2020-08-09 15:17_

### Make sure you completed the following tasks

- [x] Searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [x] Searched the closed issues

### Describe your use case

Use `ColoredHelp` for `AppSettings` by default if `color` feature is present, optionally available for opt-out. Quite a few crates is not aware of this feature and was surprised that there is colored help setting, this would prove useful for those enabled `color` feature which is being enabled by default.

### Describe the solution you'd like

Change `ColoredHelp` to `NoColoredHelp` for opt-out, by default colored help is enabled.

### Alternatives, if applicable

Not in my mind.

### Additional context

I made a few pull requests to other applications using clap, they were all surprised that there is a feature to have colored help. Only ripgrep that I know explicitly opt out for colored help since they use another crate for coloring and does not want to pull in an additional dependency, not sure if that's true in clap v3.


---

_Label `T: new feature` added by @pickfire on 2020-08-09 15:17_

---

_Comment by @pksunkara on 2020-08-10 00:07_

Nope. Color Help should be opt-in since features are additive.

---

_Closed by @pksunkara on 2020-08-10 00:07_

---

_Comment by @pickfire on 2020-08-10 07:27_

@pksunkara Features are additive but it defaults to "colors" being enabled. I think colored help should be enabled if the feature is enabled and disabled if feature is disabled. Enabling the feature "colors" but not enabling colored help is not helping feature discover-ability.

IIRC @CreepySkeleton commented on this, I think he agrees or maybe I got the wrong idea. https://github.com/clap-rs/clap/pull/1976#issuecomment-671062293

---

_Comment by @pksunkara on 2020-08-10 08:00_

Let me rephrase. CLIs by default are not colored. And then the user should specifically given an option to color it. Lots of people don't like colors. Making it opt-in is a good compromise.

---

_Comment by @CreepySkeleton on 2020-08-10 15:57_

Guys, I think you've all been missing the main point of my comment: the coloring is already gated behind `ColorAlways/ColorAuto/ColorNever`. `ColoredHelp` is redundant. It reminds me of the infamous joke about Windows Explorer popups:
* "Are you sure you want to delete the folder?"
* "All contents will be deleted, please confirm".
* "The folder itself will be removed, too. Click yes to proceed."
* "The folder will be removed Permanently. You will not be able to restore it. Continue?"
* "Didn't you change you mind? Press yes to proceed."
* "Please fill the Directory Removal Confirmation Form and send it to us signed in blood of your firstborn"...

I don't think we need it at all.

Except, the colors are `Auto` by default, but I do think it's fine. I assume that there are more people who like color than who dislike it, so "on by default but you can opt out (either by disabling the feature or by using the Never setting)" is a better compromise.

---

_Comment by @pksunkara on 2020-08-11 00:41_

> `ColoredHelp` is redundant

No it is not. There are 3 controls for colored help message.

1. Feature in cargo.toml
2. `ColorAuto/ColorNever`
3. `ColoredHelp` setting.

By mix-matching these the user (cli developer here) can customise what they want.

---

_Comment by @CreepySkeleton on 2020-08-21 04:17_

Reenergizing the topic!

> By mix-matching these the user (cli developer here) can customise what they want.

Sure. But I don't see where `ColoredHelp` comes into play.

---

The possible scenarios:

1. You don't want colors for any of your apps.
2. You want colors for some of your applications (or subcommands), but don't want for others.
3. You want colors for the all apps/subcommands.

Let's imagine we don't have `ColoredHelp`. 

1. You can just off the `color` feature.
2. You can set `ColorNever` for the latter, the former will have colors by default.
3. You do nothing and enjoy clap because colors are `Auto` by default.

And what is the current situation?

1. You sit and relax - colors are off by default. Whether the `Cargo.toml` feature is on or off matters not (WTF?).
2. You can set `ColoredHelp` for the former, the latter have no colors by default.
3. You can set `ColoredHelp` for all of your apps.

_Important note: `ColorAuto/ColorNever` are useless in this model. The cargo.toml feature is only useful for trimming unused code - it has no effect on coloring behavior because it's already orchestrated by other mechanisms._

---

I think the second system is kinda fucked up (probably because of legacy; you've just got used to taking it for normal) and the first is much better. 

@pksunkara Counter arguments?

---

_Comment by @pickfire on 2020-08-25 04:41_

How about the derive support for colors? It could still be somewhat related so I just discuss it here. From `cargo-expand`

```rust
#[derive(StructOpt, Debug)]
#[structopt(rename_all = "kebab-case")]
pub struct Args {
    /// Coloring: auto, always, never
    #[structopt(long, value_name = "WHEN")]
    pub color: Option<Coloring>,
}

#[derive(Debug, Clone, Copy)]
pub enum Coloring {
    Auto,
    Always,
    Never,
}

impl FromStr for Coloring {
    type Err = String;

    fn from_str(name: &str) -> Result<Self, Self::Err> {
        match name {
            "auto" => Ok(Coloring::Auto),
            "always" => Ok(Coloring::Always),
            "never" => Ok(Coloring::Never),
            other => Err(format!(
                "must be auto, always, or never, but found `{}`",
                other,
            )),
        }
    }
}

impl Display for Coloring {
    fn fmt(&self, formatter: &mut fmt::Formatter) -> fmt::Result {
        let name = match self {
            Coloring::Auto => "auto",
            Coloring::Always => "always",
            Coloring::Never => "never",
        };
        formatter.write_str(name)
    }
}
```

Do we want the users to write their own settings for stuff available in `clap`?

https://github.com/dtolnay/cargo-expand/blob/9ee9cd3fc4d2e8a472f113894a09979ae8fc3358/src/opts.rs#L89-L91

https://github.com/dtolnay/cargo-expand/blob/9ee9cd3fc4d2e8a472f113894a09979ae8fc3358/src/opts.rs#L122-L154

---

_Comment by @pksunkara on 2020-08-27 08:28_

You are missing the scenario where the users wants colors for error messages but no colors for help message. This is why we have 3 controls.

---

_Comment by @CreepySkeleton on 2020-08-27 10:49_

> You are missing the scenario where the users wants colors for error messages but no colors for help message

Does... anybody do that, really? I mean, intentionally, not because they forgot to enable `ColoredHelp`.

---

_Comment by @CreepySkeleton on 2020-08-27 10:53_

This is actually the reason why I'm so insistent on this issue:

> I made a few pull requests to other applications using clap, they were all surprised that there is a feature to have colored help. 

The current system is too easy to misuse.

---

_Comment by @pksunkara on 2020-08-27 12:23_

> Does... anybody do that, really? I mean, intentionally, not because they forgot to enable `ColoredHelp`.

Yes, I do this in one the CLI tools I develop and use, https://github.com/termapps/enquirer.

Regarding making `ColoredHelp` default switched on, as I said before, I think colors in error but not in help messages is a sensible default

---

_Comment by @pickfire on 2020-08-27 16:34_

I have an idea for a middle ground between normal users and more advanced users.

- `Color(Auto|Always|Never)` covers all colors, including help and errors which I think is what most people wanted
- `ColoredError(Auto|Always|Never)` covers only error message but not help which @pksunkara wanted
- `ColoredHelp(Auto|Always|Never)` covers only help message but not error which is the current default @CreepySkeleton and me does not want

Going even further, I think it may be better to have `ColorAuto` by default if `color` feature is used, I don't think people would want color being disabled by default when having the feature enabled so here we have an opt-out approach on `ColorAuto` based on feature.

Different scenarios and steps required which improves the current process for people wanting colors (3 steps)

- I want colors by default
  1. add `color` to features (I think this is the default already)
- I want color only for help by default
  1. add `color` to features (I think this is the default already)
  2. configure `ColoredErrorNever`
- I want color only for error by default
  1. add `color` to features (I think this is the default already)
  2. configure `ColoredHelpNever`
- I don't want color
  1. remove `color` from features

To ease understanding on these, we could mention that `ColorAuto` is on by default on an opt-out basis. Internally, we only keep `ColoredHelp` and `ColoredError`. I personally think this is the sensible default given that 3 out of 3 pull requests I sent to enable colored help were merged and having the owner not knowing this feature.

---

_Comment by @pksunkara on 2020-08-27 17:11_

> I think it may be better to have ColorAuto by default if color feature is used

It already works like that.

And damn, you just converted 3 control flags which can be used together into 9 flags which are exclusive. That's not a good user experience. Let's not go into a tangent and focus on the discussion here.

I think what @CreepySkeleton is proposing is that we convert `ColoredHelp` setting to `DisableColoredHelp` setting. I am okay with that, but I think help not being colored is a sensible default and am thus reluctant. 

---

_Referenced in [oppiliappan/eva#44](../../oppiliappan/eva/pulls/44.md) on 2020-11-05 02:32_

---

_Comment by @pickfire on 2020-11-05 02:37_

> And damn, you just converted 3 control flags which can be used together into 9 flags which are exclusive. That's not a good user experience. Let's not go into a tangent and focus on the discussion here.

No, it is not exclusive. The reason I split into 3 different settings is that `Color` is the general default for the other 2, so setting that is like enabling color globally. `ColoredError` and `ColoredHelp` is the specific color option, if user override this it will override the `Color`.

Essentially two paths are checked
- `Color` -> `ColoredError`
- `Color` -> `ColoredHelp`

However, internally we could represent them using only 2 states, two `bool` is enough. The reason there is multiple options available (I know it is not so good) but we allow the users to have a single change to tweak the behavior to what they wanted. We could just put up a table for them, I do think a table is not so good and wonder if there is a more intuitive way.

---

_Referenced in [clap-rs/clap#2845](../../clap-rs/clap/pulls/2845.md) on 2021-10-17 19:55_

---
