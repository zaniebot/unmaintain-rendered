```yaml
number: 815
title: Add support for automatic negation flags
type: issue
state: open
author: kbknapp
labels:
  - C-enhancement
  - A-builder
  - A-parsing
  - A-derive
  - S-waiting-on-design
assignees: []
created_at: 2017-01-14T02:17:59Z
updated_at: 2025-09-16T18:43:57Z
url: https://github.com/clap-rs/clap/issues/815
synced_at: 2026-01-12T16:14:09Z
```

# Add support for automatic negation flags

---

_@kbknapp_

Add a way to automatically generate flags that override (or negate) other flags. This can be done manually already, but doing so for an entire CLI can be tedious, painful, and error prone. Manually doing so will also pollute the `--help` output.

This proposal offers a way to automatically have these negation flags generated on a case by case basis, or across all flags in the command. This proposal also offers a way to have these negation flags listed in the `--help` message or hidden.

### Design

A negation flag would simply take the long version of the regular flag and pre-pend `no`; for exmaple `--follow` would get a `--no-follow`. If a flag only specifies a short version, the `no` would be prepended to the short such as `-L` gets `--no-L`.

When parsing occurs, if a negation flag is found, and the negat**ed** argument was used, it functions exactly like a override that is already supported.

Functionally the following two examples are equivilant:

```rust
app.arg(Arg::with_name("regular")
        .long("follow-links")
        .help("follows symlinks")
        .overrides_with("override"))
    .arg(Arg::with_name("override)
        .long("no-follow-links")
        .help("does not follow symlinks"))
```

New proposal:

```rust
app.arg(Arg::with_name("regular")
        .long("follow-links")
        .help("follows symlinks")
        .overridable(true))
```

### Concerns

There are two primary concerns with this approach.

#### Flags that already contian "no"

A flag which already starts with `no` such as `--no-ignore` would end up getting a double `no` in the form of `--no-no-ignore`. This actually makes sense and is consistent, but looks strange at first glance. An alternative would be to check if a flag starts with `no` and simply remove the `no`, i.e. `--no-ignore` becomes `--ignore` but this has the downside of additional processing at runtime, becomes slightly more confusing, and has a higher chance of a conflict.

#### Conflicting names

If a user has selected to auto-generate a negation flag, and the negating flag long conflicts with a flag already in use, a `panic!` will occur. Example, `--ignore` and `--no-ignore` is already defined elsewhere, and the user has selected to automaticlly generate negation flags, this will cause `--ignore` to generate a `--no-ignore` flag which already exists causing a `panic!`. The fix is to either *not* use a sweeping setting that applies ot all flags indescriminantly, or to change/remove the already defined `--no-ignore` flag.

 ---


### Progress

 - [ ] Add `AppSettings::GenerateNegationFlags` which does the above, but automatically for all flags. 
    - [ ] docs
    - [ ] tests 
 - [ ] Add `AppSettings:GenerateHiddenNegationFlags` which hides all these negation flags.
    - [ ] docs
    - [ ] tests

---


See the discussion in burntsushi/ripgrep#196

Relates to #748 

--- 

Edit: Removed `Arg::overridable(bool)` because this can already be done manually by making another flag.

---

_Label `C: args` added by @kbknapp on 2017-01-14 02:18_

---

_Label `C: settings` added by @kbknapp on 2017-01-14 02:18_

---

_Label `D: easy` added by @kbknapp on 2017-01-14 02:18_

---

_Label `P3: want to have` added by @kbknapp on 2017-01-14 02:18_

---

_Label `T: new feature` added by @kbknapp on 2017-01-14 02:18_

---

_Label `T: new setting` added by @kbknapp on 2017-01-14 02:18_

---

_Label `W: 2.x` added by @kbknapp on 2017-01-14 02:18_

---

_Added to milestone `2.21.0` by @kbknapp on 2017-01-30 04:37_

---

_Label `C: flags` added by @kbknapp on 2017-01-30 15:31_

---

_Label `C: args` removed by @kbknapp on 2017-01-30 15:31_

---

_Label `D: intermediate` added by @kbknapp on 2017-02-19 18:51_

---

_Label `D: easy` removed by @kbknapp on 2017-02-19 18:51_

---

_Referenced in [clap-rs/clap#1037](../../clap-rs/clap/issues/1037.md) on 2018-02-15 14:51_

---

_Referenced in [TeXitoi/structopt#93](../../TeXitoi/structopt/issues/93.md) on 2018-04-18 16:34_

---

_Label `W: 3.x` added by @kbknapp on 2018-07-22 01:06_

---

_Label `W: 2.x` removed by @kbknapp on 2018-07-22 01:06_

---

_Removed from milestone `2.21.0` by @kbknapp on 2018-07-22 01:06_

---

_Added to milestone `v3.0` by @kbknapp on 2018-07-22 01:06_

---

_Referenced in [habitat-sh/habitat#6114](../../habitat-sh/habitat/pulls/6114.md) on 2019-03-13 21:53_

---

_Referenced in [TeXitoi/structopt#280](../../TeXitoi/structopt/issues/280.md) on 2019-11-20 12:06_

---

_Referenced in [TeXitoi/structopt#320](../../TeXitoi/structopt/issues/320.md) on 2020-01-06 03:39_

---

_Referenced in [clap-rs/clap#1728](../../clap-rs/clap/pulls/1728.md) on 2020-03-06 15:16_

---

_Removed from milestone `3.0` by @pksunkara on 2020-04-10 13:17_

---

_Added to milestone `3.1` by @pksunkara on 2020-04-10 13:17_

---

_Referenced in [dandavison/delta#307](../../dandavison/delta/issues/307.md) on 2020-10-06 15:40_

---

_Referenced in [dandavison/delta#492](../../dandavison/delta/issues/492.md) on 2021-01-07 01:17_

---

_Referenced in [clap-rs/clap#1649](../../clap-rs/clap/issues/1649.md) on 2021-02-05 07:17_

---

_Referenced in [ducaale/xh#84](../../ducaale/xh/issues/84.md) on 2021-03-03 15:20_

---

_Comment by @jacobsvante on 2021-05-21 08:31_

Great proposal @kbknapp. Having a `--no-*` equivalent just feels natural when it comes to boolean values.

Perhaps this could even be the default, considering that v3 is still in beta? The user would then have to call `.overridable(false)` instead to get the old behavior.

---

_Comment by @pksunkara on 2021-05-21 13:26_

Not all single flags are need overridable flags. From looking at the clis, this is a minority use case. Which is why this won't be a default behaviour.

---

_Label `:money_with_wings: $10` added by @pksunkara on 2021-05-21 13:26_

---

_Referenced in [ducaale/xh#142](../../ducaale/xh/issues/142.md) on 2021-05-23 13:37_

---

_Comment by @Emerentius on 2021-06-08 18:21_

It's actually quite useful if all flags have negation flags because you can effectively switch the default of a flag via aliases or wrappers without hardcoding the behavior, because the user can still override the choice.
It even works with nested wrappers.

For some prior art, the very popular python command line argument library `click` pushes users to always define a positive and a negative flag (but allows having just one):
https://click.palletsprojects.com/en/8.0.x/options/#boolean-flags

---

_Comment by @Mange on 2021-06-08 18:28_

I usually also always add a `no` variant for almost all my options, but it's that little "almost" that make me not want this to be automatic. If there was a simple opt-in argument you could set (`toggleable(true)` maybe?) then that would be enough for me.

---

_Referenced in [TeXitoi/structopt#482](../../TeXitoi/structopt/issues/482.md) on 2021-06-08 18:31_

---

_Comment by @epage on 2021-07-22 08:39_

Random thoughts:
- I don't think I normally see `--no-L`.  Instead what I tend to see is either no short form or toggling of the case (e.g. `-l` vs `-L`).  Having this be different makes this a bit more complicated.
- Sometimes I want the `no` format to be the default, so I'd want the `no-` prefix stripped.

---

_Label `W: 3.x` removed by @pksunkara on 2021-08-13 10:40_

---

_Comment by @mkayaalp on 2021-11-15 06:29_

There are also instances of complementary flags using `+`/`-` as their prefix. Aside from the `bash shopt` and `Xwayland` mentioned in #2468, [`xterm`](https://linux.die.net/man/1/xterm) and [`set`](https://www.gnu.org/software/bash/manual/html_node/The-Set-Builtin.html) (shell builtin) have many such options:
```
usage:  xterm [-/+132] [-/+ah] [-/+ai] [-/+aw]
    [-/+bc] [-/+bdc] [-/+cb] [-/+cjk_width] [-/+cm] [-/+cn] 
    [-/+cu] [-/+dc] [-/+fbb] [-/+fbx] [-/+fullscreen]
    [-/+hm] [-/+hold] [-/+ie] [-/+im] [-/+itc]
    [-/+j] [-/+k8] [-/+l] [-/+lc] [-/+ls] [-/+maximized] [-/+mb]
    [-/+mesg] [-/+mk_width] [-/+nul] [-/+pc] [-/+pob] [-/+rv]
    [-/+rvc] [-/+rw] [-/+s] [-/+samename] [-/+sb] [-/+sf]
    [-/+si] [-/+sk] [-/+sm] [-/+sp] [-/+t] [-/+u8] [-/+uc]
    [-/+ulc] [-/+ulit] [-/+ut] [-/+vb] [-/+wc] [-/+wf]
    <omitted other options>
```

```
set [--abefhkmnptuvxBCEHPT] [-o option-name] [argument …]
set [+abefhkmnptuvxBCEHPT] [+o option-name] [argument …]
```

---

_Comment by @epage on 2021-11-15 14:35_

Though that is a whole different argument scheme and is more related to #1210, especially with [my comment](https://github.com/clap-rs/clap/issues/1210#issuecomment-956656955).

However, that also points out the general challenge with trying to support automatic negation flags: there are a lot of different styles. I think the most important thing is we don't get in the way of people writing their CLI, even if it requires more boiler palte.  After that is us making the core workflows and common / best practices easy to take advantage of.  I think we should define the scope of what kind of automatic flags we support and limit ourselves to that or else I fear it'll either be too complicated by supporting all types or won't be present for anyone.

---

_Referenced in [epage/clapng#69](../../epage/clapng/issues/69.md) on 2021-12-06 16:32_

---

_Label `C: settings` removed by @epage on 2021-12-08 20:18_

---

_Label `C: flags` removed by @epage on 2021-12-08 20:18_

---

_Label `A-builder` added by @epage on 2021-12-08 20:18_

---

_Label `A-parsing` added by @epage on 2021-12-08 20:18_

---

_Label `C-enhancement` added by @epage on 2021-12-08 20:38_

---

_Label `T: new feature` removed by @epage on 2021-12-08 20:39_

---

_Label `T: new setting` removed by @epage on 2021-12-08 20:39_

---

_Label `D: medium` removed by @epage on 2021-12-09 17:00_

---

_Label `P3: want to have` removed by @epage on 2021-12-09 17:00_

---

_Label `A-derive` added by @epage on 2021-12-09 17:00_

---

_Label `S-waiting-on-design` added by @epage on 2021-12-09 17:00_

---

_Removed from milestone `3.1` by @epage on 2021-12-09 17:00_

---

_Referenced in [clap-rs/clap#3146](../../clap-rs/clap/issues/3146.md) on 2021-12-10 21:34_

---

_Referenced in [ducaale/xh#234](../../ducaale/xh/pulls/234.md) on 2022-02-15 20:58_

---

_Comment by @blyxxyz on 2022-02-15 22:12_

**EDIT:** This can be done more cleanly nowadays, see https://github.com/clap-rs/clap/issues/815#issuecomment-1808282085

We're currently using this workaround to generate negation flags for all options:
```rust
    use once_cell::sync::OnceCell;

    let opts: Vec<_> = app.get_arguments().filter(|a| !a.is_positional()).collect();

    // We use a OnceCell to make strings 'static. Watch out that you don't
    // construct the app multiple times with different arguments because it'll
    // only be initialized once and reused thereafter.
    // Box::leak() would also work, but it makes valgrind unhappy.
    static ARG_STORAGE: OnceCell<Vec<String>> = OnceCell::new();
    let arg_storage = ARG_STORAGE.get_or_init(|| {
        opts.iter()
            .map(|opt| format!("--no-{}", opt.get_long().expect("long option")))
            .collect()
    });

    let negations: Vec<_> = opts
        .into_iter()
        .zip(arg_storage)
        .map(|(opt, flag)| {
            clap::Arg::new(&flag[2..])
                .long(flag)
                .hide(true)
                .overrides_with(opt.get_name())
        })
        .collect();

    app = app.args(negations);
```
(It also creates `--no-help` and `--no-version`, which is weird but harmless.)

---

_Referenced in [clap-rs/clap#3405](../../clap-rs/clap/issues/3405.md) on 2022-05-09 21:54_

---

_Comment by @epage on 2022-06-13 17:45_

With the new `ArgAction` API, one approach we could take for this:
```rust
Arg::new("force")
    .action(clap::builder::SetBool)
    .long("force")
    .alias("no-force")
```
The `SetBool` action would effectively be
```
let alias: &str = ...;  // either `force` or `no-force`
let default_missing_value = if alias.starts_with("no-") {
    "false"
} else {
    "true"
};
...
```

The downside is you wouldn't be able to control whether shorts would set true or false.

---

_Comment by @blyxxyz on 2022-06-13 18:04_

In [xh](https://github.com/ducaale/xh) we support negating all options, not just boolean options. `--style=solarized --no-style` becomes a no-op. I don't know how common that is (we imitated it from HTTPie), but it would be nice to have as a supported case.

The prefix check feels too magical, if I found that example in the wild I wouldn't know where to look for an explanation.

---

_Comment by @epage on 2022-06-13 18:27_

> In [xh](https://github.com/ducaale/xh) we support negating all options, not just boolean options. --style=solarized --no-style becomes a no-op. I don't know how common that is (we imitated it from HTTPie), but it would be nice to have as a supported case.

Thanks for bringing up this use case.  I don't tend to see this too often.  Even if we don't provide a native way of supporting it, there would always be `overrides_with` for clearing it out.

Speaking of, how are you implementing that today?  I looked at your `Parser` didn't see a `--no-style` defined.

> The prefix check feels too magical, if I found that example in the wild I wouldn't know where to look for an explanation.

I can understand.  It is dependent on people thinking to check the the documentation for `SetBool`.

---

_Comment by @blyxxyz on 2022-06-13 19:01_

> Speaking of, how are you implementing that today?

See a few comments back: https://github.com/clap-rs/clap/issues/815#issuecomment-1040848452

The new reflection came in very handy!

---

_Comment by @epage on 2022-06-13 19:33_

Oh, I had overlooked in that comment that you meant negations for all options and not just flags.

---

_Comment by @dhruvkb on 2022-08-07 08:43_

Coming from the Python `argparse` camp, I heavily used the `--*/--no-*` pattern in my apps. This feature would be very helpful for me.

> With the new ArgAction API, one approach we could take for this

@epage is it possible to define our own actions apart from [the included standard ones](https://docs.rs/clap/latest/clap/builder/enum.ArgAction.html)? I couldn't find any examples for this so if you could point me to one, that'd be very helpful.

---

_Comment by @tmccombs on 2022-08-07 18:36_

> is it possible to define our own actions apart from [the included standard ones](https://docs.rs/clap/latest/clap/builder/enum.ArgAction.html)?

Not currently. but I think I saw something saying that was on the roadmap.

---

_Comment by @epage on 2022-08-08 14:03_

@dhruvkb the plan is to eventually support it but not yet.

Current blockers:
- Seeing how actions interact with the parser and other systems as the project evolves to make sure the we expose the right behavior.
- Work through how to interact with parts of the system that are currently internal.

---

_Comment by @dhruvkb on 2022-08-08 14:09_

I'll subscribe to the issue for updates. In the meantime I've got a variant of @blyxxyz's snippet working for me. Thanks!

---

_Comment by @epage on 2022-08-25 21:58_

While I don't think this is the way we should go, I thought I'd record how CLI11 solves this problem: runtime parsing of a declaration string.  See https://cliutils.github.io/CLI11/book/chapters/flags.html

---

_Referenced in [n0-computer/beetle#156](../../n0-computer/beetle/issues/156.md) on 2022-10-10 11:24_

---

_Referenced in [dandavison/delta#1280](../../dandavison/delta/issues/1280.md) on 2023-01-13 14:12_

---

_Referenced in [orogene/orogene#219](../../orogene/orogene/issues/219.md) on 2023-03-30 19:56_

---

_Referenced in [neovide/neovide#1885](../../neovide/neovide/pulls/1885.md) on 2023-06-02 21:50_

---

_Referenced in [jwillinghalpern/fm_rainbow_log#41](../../jwillinghalpern/fm_rainbow_log/issues/41.md) on 2023-09-11 00:12_

---

_Referenced in [astral-sh/ruff#7504](../../astral-sh/ruff/pulls/7504.md) on 2023-09-19 02:36_

---

_Comment by @fzyzcjy on 2023-11-13 14:08_

Hi, is there any updates? Thanks! Especially, it would be great to have one in Parser derivation mode, instead of the builder mode.

P.S. I found https://jwodder.github.io/kbits/posts/clap-bool-negate/ but the workaround is not super neat IMHO

---

_Comment by @blyxxyz on 2023-11-13 14:35_

FWIW, in xh we've moved to a [still cleaner implementation](https://github.com/ducaale/xh/blob/1a74a521e1f1def2f9463abcfe05b448f04c27be/src/cli.rs#L583) by enabling clap's `string` feature and doing this:

```rust
let negations: Vec<_> = app
    .get_arguments()
    .filter(|a| !a.is_positional())
    .map(|opt| {
        let long = opt.get_long().expect("long option");
        clap::Arg::new(format!("no-{}", long))
            .long(format!("no-{}", long))
            .hide(true)
            .action(ArgAction::SetTrue)
            // overrides_with is enough to make the flags take effect
            // We never have to check their values, they'll simply
            // unset previous occurrences of the original flag
            .overrides_with(opt.get_id())
    })
    .collect();

app.args(negations)
    .after_help("Each option can be reset with a --no-OPTION argument.")
```
It no longer feels like a hacky workaround so I'm pretty happy with it. It's cool how flexible clap has become.

---

_Comment by @spenserblack on 2023-11-13 14:58_

For my specific use-case, it would be perfect for me if I could use `#[derive(Parser)]` and use this feature to create an `Option<bool>` which would automatically have `--foo` and `--no-foo` combined as a single `--[no-]foo` in the help message. IMO `--[no-]foo` is a very readable and concise way to say "there are two mutually exclusive flags, and the app will decide if neither are specified." For example, `--[no-]color` to either force colors to be included or excluded in a CLI output, and the executable would try to detect color support if neither are specified.

---

_Comment by @pacak on 2023-11-13 15:41_

> use this feature to create an `Option<bool>` which would automatically have

In my experience you only use `Option<bool>` if you want to handle case when user did not specify a value differently than from it being `true` or `false`, otherwise you just stick to `bool` and populate the default value from the parser. `Option<bool>` works on small examples but if you share configurations between apps or even use it in multiple places in the same app you can get into cases where different parts of the app treat `None` differently.

---

_Comment by @fzyzcjy on 2023-11-13 23:39_

Thank you! That looks helpful

---

_Comment by @kenchou on 2023-11-23 10:39_

If the #[derive(Parser)] could implement a boolean flag like [Python click](https://click.palletsprojects.com/en/8.1.x/options/#boolean-flags),
it would be perfect, combining both readability and flexibility.

```python
@click.option('--color/--no-color', default=False)
@click.option('--enable-option/--disable-option', default=False)
@click.option('--with-something/--without-something', default=False)
```

Imagine:
```rust
    /// Given that the default value is true, providing a short option should be considered as false. And vice versa.
    #[arg(short = "C", long = "--color/--no-color", default_value = "true")]
    color_flag: bool,

    /// Explicitly providing a short option maps to true/false.
    #[arg(short = "o/O", long = "--enable-option/--disable-option", default_value = "true")]
    some_option_flag: bool,
```

---

_Referenced in [MercuryTechnologies/ghciwatch#186](../../MercuryTechnologies/ghciwatch/pulls/186.md) on 2023-12-13 18:46_

---

_Referenced in [mozilla/neqo#1753](../../mozilla/neqo/pulls/1753.md) on 2024-03-17 01:52_

---

_Referenced in [neovide/neovide#2441](../../neovide/neovide/pulls/2441.md) on 2024-03-25 20:22_

---

_Referenced in [tummychow/git-absorb#110](../../tummychow/git-absorb/pulls/110.md) on 2024-04-11 22:53_

---

_Comment by @esemeniuc on 2024-05-18 04:44_

Would love to see this added!

---

_Label `:money_with_wings: $10` removed by @pksunkara on 2024-06-07 06:53_

---

_Referenced in [clap-rs/clap#5543](../../clap-rs/clap/issues/5543.md) on 2024-06-21 18:18_

---

_Comment by @joshtriplett on 2024-08-06 15:01_

I would love this as well. Like others in this thread, I don't think we should try to do this *automatically*; I'd be happy to see an explicit way for the builder and declarative APIs to add a `--no-` option for a given option.

This option could work for a boolean, as well as for an `Option<T>` that has a default of `Some(default_value)` (with the `--no-` option setting `None`).

```rust
    #[arg(long, negation)]
    pub auto: bool,

    #[arg(long, default_value = "hello", negation)]
    pub value: Option<String>,
```

---

_Comment by @epage on 2024-08-06 15:07_

@joshtriplett interesting to also include  this for options.

We also would need to define the builder API and what the semantics for this would be (parser, help, etc).

---

_Comment by @ia0 on 2024-08-06 15:27_

If `negation` should not be automatically enabled, I would also argue that `negation` should also not automatically add `default_value_t = true` for booleans (even though I agree it would make a lot of sense). If we want to be explicit with unsurprising syntax, I expect the `--auto/--no-auto` use-case to look like:

```rust
#[arg(long, default_value_t = true, negation)]
pub auto: bool,
```

In some way, `negation` only make sense if `long` is already present and if `default_value` is already present. If `long` is not implicitly added, then `default_value` shouldn't either. And since we want to support `Option`, then we can't implicitly add `default-value` (unless we use the syntax `support_negation_of_default = <default>`, in which case both `long` and `default_value` can be inferred).

---

_Comment by @epage on 2024-08-06 19:14_

> If negation should not be automatically enabled, I would also argue that negation should also not automatically add default_value_t = true for booleans (even though I agree it would make a lot of sense).

`default_value_t` (what to do when no flag is present) and `default_missing_value_t` (what to do when the flag is present) are both customizable for flags.  I don't think I'd want to say `default_value_t` should be required though.

`Option<bool>` (as mentioned earlier) is also a good way of handling the "not present" state.

> In some way, negation only make sense if long is already present and if default_value is already present. If long is not implicitly added, then default_value shouldn't either. And since we want to support Option, then we can't implicitly add default-value (unless we use the syntax support_negation_of_default = <default>, in which case both long and default_value can be inferred).

I don't quite get this.  Flags already require long/short.  This is a modification to flag state.  For someone to add `negation` to an existing flag to now have to explicitly set flags that they were ok with before this doesn't make sense to me.

> And since we want to support Option, then we can't implicitly add default-value (unless we use the syntax support_negation_of_default = <default>, in which case both long and default_value can be inferred).

There are lots of ways of implementing this and they don't necessarily preclude this:

- We can support `Option` and implicit `default_value_t` by checking the `ValueSource`
- We could skip the implicit default if `Option` is present

Overall though, this discussion has a fatal flaw: we are designing a feature around derive semantics and discussing what is or isn't possible.  We need to focus on the builder semantics and then decide what automatic behavior we want to the derive semantics to have on top of that.  There can be some back and forth on that (the derive influencing the builder design).  Thats a big value-add of merging structopt into clap!  We've run into many problems where the builder API made choices that work well in isolation but don't work well for builder and have worked to correct them.

---

_Comment by @ia0 on 2024-08-07 06:52_

> I don't think I'd want to say `default_value_t` should be required though.

Maybe I'm misunderstanding what you imply. I didn't say that `default_value_t` should be required. I said that `negation` should not act as an implicit `default_value_t`, and thus correct usage of `negation` should specify a `default_value_t` (it's not required, it's just probably wrong to not do it). Not sure about `default_missing_value_t` though, some specified semantics (as you mention at the end of your comment) would indeed be useful. What I inferred from @joshtriplett comment would be a semantic like:
- If `negation` is specified, then an additional conflicting flag is added.
- It is named `--no-LONG` where `LONG` is the long name of the negated flag (which must thus have a long name).
- It doesn't have a short name.
- When present it behaves by setting the field to `false` if it is a `bool` and to `None` if it is an `Option`.

> `Option<bool>` (as mentioned earlier) is also a good way of handling the "not present" state.

This doesn't have the same user experience. The best workaround right now is to explictly define the additional `--no-` flag and make it conflicting with the negated one ([real-life example](https://github.com/google/magika/blob/cbc2cb861b7b54ee7c9a6dfff3fa23ecf94a6007/rust/cli/src/main.rs#L61-L71)). Then in the code, check both flags and behave accordingly ([real-life example](https://github.com/google/magika/blob/cbc2cb861b7b54ee7c9a6dfff3fa23ecf94a6007/rust/cli/src/main.rs#L156-L161)). Note that in this example, the "default value" (i.e. when none of those flags is present) is context-dependent. In that sense, it is similar to the `Option<bool>` except that it's implemented with 2 `bool`s where `(true, true)` is unreachable (thus having exactly the same number of values, namely 3).

> I don't quite get this.

That's probably related to the misunderstanding above. What if @joshtriplett example was actually the following?

```rust
    #[arg(negation)]
    pub auto: bool,
```

What would it mean? What is the name of the added flag? And similarly if it was `#[arg(short, negation)]`. But as you said at the end, we need a clear semantics, which I provided a candidate at the beginning of this comment. Hopefully it makes things clearer.

> We need to focus on the builder semantics and then decide what automatic behavior we want to the derive semantics to have on top of that

I thought the builder and derive semantics were isomorphic since it seems derive attributes cover all builder functions. I never used the builder so maybe I'm missing some functionalities. That said, I agree that proposals need a clear semantics, and I hope the one I gave above is clear enough (for both builder and derive).

---

_Comment by @emilyyyylime on 2024-08-07 06:53_

More generally it might be desirable to support multiple argument names controlling the same value (with different actions) that are all automatically exclusive with eachother, with some shorthands to better support more common use cases (bools, options).

Working out all of the semantics of this more general approach could make it much easier to define the semantics of the shorthand forms, as simply being equivalent to some usage of the general API.


Admittedly the current API is very value-centric, making it a bit hard to imagine how this feature could be used, but being able to assign multiple flag-action pairings to the same value would be your starting point.

---

_Referenced in [catppuccin/delta#12](../../catppuccin/delta/issues/12.md) on 2024-08-30 23:00_

---

_Referenced in [smheidrich/snoozed-todos#1](../../smheidrich/snoozed-todos/issues/1.md) on 2024-09-08 21:47_

---

_Referenced in [starcoinorg/starcoin#4289](../../starcoinorg/starcoin/pulls/4289.md) on 2024-11-19 10:39_

---

_Referenced in [pls-rs/pls#118](../../pls-rs/pls/pulls/118.md) on 2024-12-20 07:04_

---

_Referenced in [tealdeer-rs/tealdeer#396](../../tealdeer-rs/tealdeer/pulls/396.md) on 2024-12-30 22:35_

---

_Referenced in [esp-rs/espflash#717](../../esp-rs/espflash/pulls/717.md) on 2024-12-31 07:22_

---

_Referenced in [YaLTeR/niri#975](../../YaLTeR/niri/pulls/975.md) on 2025-01-14 09:49_

---

_Referenced in [hashintel/hash#7329](../../hashintel/hash/pulls/7329.md) on 2025-06-04 19:56_

---

_Referenced in [hermit-os/uhyve#1034](../../hermit-os/uhyve/pulls/1034.md) on 2025-08-02 22:07_

---

_Comment by @RReverser on 2025-09-15 14:44_

> I would love this as well. Like others in this thread, I don't think we should try to do this _automatically_; I'd be happy to see an explicit way for the builder and declarative APIs to add a `--no-` option for a given option.
> 
> This option could work for a boolean, as well as for an `Option<T>` that has a default of `Some(default_value)` (with the `--no-` option setting `None`).
> 
>     #[arg(long, negation)]
>     pub auto: bool,
> 
>     #[arg(long, default_value = "hello", negation)]
>     pub value: Option<String>,

TBH I don't think we even need a separate `negation`, it would be more natural to just allow multiple `arg()` on the same field.

For example:

```rust
#[arg(long)]
#[arg(long = "no-auto", action = ArgAction::SetFalse)]
pub auto: bool,
```

could allow negation quite cleanly, where the CLI interface shows these as individual options, user can apply own conversion rules for each of them, yet when parsing they all map back to the same field.

---

_Comment by @epage on 2025-09-16 18:11_

The initial Issue asked for `no-` variants to be added automatically.  Specifying a `no-` alias explicitly is a variant of that.

In https://github.com/clap-rs/clap/issues/815#issuecomment-884746951 I mentioned

> Sometimes I want the no format to be the default, so I'd want the no- prefix stripped.

Doing this automatically, there wouldn't be a way to determine it.  If we added the `no-` variants explicitly, that would be:
```rust
Arg::new("flag").long)"no-flag").alias("flag").action(ArgAction::SetTrue)
```
The `no-` variant is what would primarily show up in the help while still allowing the positive variant.

> I don't think I normally see --no-L. Instead what I tend to see is either no short form or toggling of the case (e.g. -l vs -L). Having this be different makes this a bit more complicated.

Short flags become an issue.  How do we know whether they are a negation or not?  There are many different styles, from changing case to changing letters, depending on the circumstances.

---

_Comment by @RReverser on 2025-09-16 18:34_

I agree automatic / dedicated negation can have its merits too, but what I like about my proposal is that I, too, often wished for this request by @emilyyyylime above:

> More generally it might be desirable to support multiple argument names controlling the same value (with different actions) that are all automatically exclusive with eachother, with some shorthands to better support more common use cases (bools, options).

and allowing multiple `arg()` would cover these, more universal, usecases as well as plain negation.

---

_Comment by @epage on 2025-09-16 18:43_

In case its relevant,
- #3146 is our issue for having the same destination.
- #2621 is our issue for declaring multiple arguments mutually exclusive through being an enum

---
