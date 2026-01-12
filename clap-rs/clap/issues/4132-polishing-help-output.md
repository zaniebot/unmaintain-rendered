```yaml
number: 4132
title: "Polishing `--help` output"
type: issue
state: closed
author: epage
labels:
  - C-enhancement
  - A-help
  - S-waiting-on-design
assignees: []
created_at: 2022-08-27T00:59:37Z
updated_at: 2023-07-13T12:43:44Z
url: https://github.com/clap-rs/clap/issues/4132
synced_at: 2026-01-12T16:14:15Z
```

# Polishing `--help` output

---

_@epage_

We are looking to polish the help as part of the v4 release since some parts of an applications help might be dependent on how clap renders things (e.g. to match clap's ALL CAPS [help headings](https://docs.rs/clap/latest/clap/builder/struct.App.html#method.next_help_heading), a user might put theirs in ALL CAPS).

So far we have made the following changes:
- The greens and yellows are gone, instead using bold, underline, and ~~dimmed~~: [PR 4117](https://github.com/clap-rs/clap/pull/4117) though [some more work might be needed to enable colored help, depending on the needs](https://github.com/clap-rs/clap/issues/3108)
  - **See PR for screenshot**
  - Dimmed was removed from placeholders in https://github.com/clap-rs/clap/pull/4126
  - Some liked the colors ([example](https://github.com/clap-rs/clap/issues/4132#issuecomment-1229099561)), some hated them ([example](https://github.com/clap-rs/clap/discussions/2906))
  - Color makes it easier to scan
  - Some applications have specific branding that it clashed with (#2963)
  - True colors would be prettier but requires getting into terminal theme detection to make sure it plays nice and hesistant to do that at the moment
  - **Users can't rollback until theming support is in (https://github.com/clap-rs/clap/issues/3234) though they can just disable the coloring with `Command::disable_colored_help(true)`**
- Reduce the ALL CAPS: [PR 4123](https://github.com/clap-rs/clap/pull/4123)
  - **See PR for screenshot**
  - ARGS and OPTIONS matched the usage and worked as non-colored headings but it feels a bit unpolished
  - I was hard pressed to find other CLIs that do this
  - All caps mirrors man pages
  - Mixed with underlining, it can be hard to see the bottom of "y", "g", etc.
  - **Users can rollback to the old behavior with `Command::help_template`, `Command:subcommand_help_heading`, and `Arg::help_heading`**
- List subcommands before args/options: [PR 4125](https://github.com/clap-rs/clap/pull/4125)
  - **See PR for screenshot**
  - At the end represents where someone might put it in the usage compared to everything else but generally a user's primary focus is on the subcommand they are going to call so that is where the help's focus should be
  - **Users can rollback to the old behavior with `Command::help_template`**
- List of positional Arguments use `[]` for optional [PR 4144](https://github.com/clap-rs/clap/pull/4144)
- Always show optional positional arguments in usage, rather than collapsing them [PR 4151](https://github.com/clap-rs/clap/pull/4151)
  - Generally there will be few enough positional arguments that this shouldn't be a big deal
  - Optional flags/options can be more numerous and clog up the usage, making it impossible to deal with (e.g. `git`)
- Renaming SUBCOMMAND value name and Subcommand section headers to COMMAND and Command: [PR 4155](https://github.com/clap-rs/clap/pull/4155)
  - "Subcommand" looks weird to me
  - Hard pressed to find other CLIs that do this
  - **Users can rollback to the old behavior with `Command::subcommand_help_heading` and `Command::subcommand_value_name`**
- Hint to the user the difference between `-h` and `--help`: [PR 4159](https://github.com/clap-rs/clap/pull/4159)
- Remove `<name> <verson>` from the start of `--help`: [PR 4160](https://github.com/clap-rs/clap/pull/4160)
  - It takes up precious vertical space but doesn't seem justified, especially when `--version` exists and when a subcommand doesn't have a version, it feels out of place (see `cargo check -h`)
  - Again, hard pressed to find other CLIs that do this
  - This does leave where to put authors if people specify them
  - Since all help is rendered from a template, this runs into problems when `Command::about` is unspecified as it leaves a blank line at the top of the help.  https://github.com/clap-rs/clap/pull/4156 resolves that
  - **Users can rollback to the old behavior with `Command::help_template`**
- Collapse usage to one line [PR 4188](https://github.com/clap-rs/clap/pull/4188)
  - After looking at several CLIs, it seemed to fit and helps reduce length of output
  - See https://github.com/clap-rs/clap/issues/4132#issuecomment-1233007144 for alternatives considered
- Reduce blank lines when overflowing short help onto next line [PR 4190](https://github.com/clap-rs/clap/pull/4190)
- Reduce indentation from 4 to 2 to reduce the chance of wrapping [PR 4192](https://github.com/clap-rs/clap/pull/4192)
  - flags/options already have a lot of whitespace, we probably don't need as much everywhere
  - Inspired by `podman -h`
  - While https://github.com/clap-rs/clap/pull/4161 will make it easier to do on the implementation side, I'm losing steam and don't want to go update all of those tests

Rejected
- Some CLIs put Usage before the `<about>` and collapse it to `Usage: <usage>` if its a single line (e.g. `find --help`)
  - Putting usage closer to the arguments seems better
  - Otherwise, the difference seems trivial without a reason to go one way or the other
- Should Arguments and Options be under a single heading by default?
  - I feel like there is a distinct enough difference in how they are used to call them out separately
  - A user can force them to be merged via `Arg::help_heading`

Future improvements
- https://github.com/clap-rs/clap/issues/2389
- https://github.com/clap-rs/clap/issues/3234
- https://github.com/clap-rs/clap/issues/3108
- https://github.com/clap-rs/clap/issues/1433
- https://github.com/clap-rs/clap/issues/1553

**Note:** While I mentioning non-clap CLIs, I want to be clear that we don't just want to conform to what is out there.  We should be aiming to make the defaults as polished as possible which can include taking inspiration from others.

---

_Label `C-enhancement` added by @epage on 2022-08-27 00:59_

---

_Label `A-help` added by @epage on 2022-08-27 00:59_

---

_Label `S-waiting-on-design` added by @epage on 2022-08-27 00:59_

---

_Added to milestone `4.0` by @epage on 2022-08-27 00:59_

---

_Comment by @Gankra on 2022-08-27 01:30_

Gonna dump some comparisons/notes here and then do a summary of my thoughts once I've seen it all.

Here's a before (3.2.17, left) and after (current git tip, right) of [minidump-stackwalk](https://github.com/rust-minidump/rust-minidump/blob/main/minidump-stackwalk/src/main.rs) on powershell:

Fixes I needed to make (fairly painless with [the CHANGELOG's advice, nice!](https://github.com/clap-rs/clap/blob/master/CHANGELOG.md)):

* `possible_values = [...]` => `value_parser([...])`
* `DeriveDisplayOrder` => `<deleted, now the default, sick!>`

![image](https://user-images.githubusercontent.com/1136864/187008852-27f35481-4eb8-4fee-a335-30905782e30d.png)
![image](https://user-images.githubusercontent.com/1136864/187008886-b0f78ca4-c7bc-40fc-9e0c-943ebd2e0562.png)
![image](https://user-images.githubusercontent.com/1136864/187008900-63b567f0-24c8-4cc7-b1fd-2182166ed3f3.png)
![image](https://user-images.githubusercontent.com/1136864/187008909-a11e896c-d2b8-4099-b7ba-493c12083a04.png)


---

_Comment by @epage on 2022-08-27 01:41_

> Here's a before (3.2.17, left) and after (current git tip, right) of [minidump-stackwalk](https://github.com/rust-minidump/rust-minidump/blob/main/minidump-stackwalk/src/main.rs) on powershell:
>
> Fixes I needed to make ...

For anyone else, I want to say that I only consider the changelog to be in draft form at the moment plus we'll write a migration guide like we did for clap 3.

---

_Comment by @Gankra on 2022-08-27 01:49_

[cargo-vet](https://github.com/mozilla/cargo-vet/blob/main/src/cli.rs) (this one is a bit messier because we have proper subcommands so lining things up is gonna be wonky...):

required diff

```diff
@@ -215,6 +225,18 @@ dependencies = [
PS C:\Users\ninte\dev\cargo-vet> git diff .\src\cli.rs
diff --git a/src/cli.rs b/src/cli.rs
index f19d89d..2ad2914 100644
--- a/src/cli.rs
+++ b/src/cli.rs
@@ -18,7 +18,6 @@ pub enum FakeCli {
 #[clap(version)]
 #[clap(bin_name = "cargo vet")]
 #[clap(args_conflicts_with_subcommands = true)]
-#[clap(global_setting(clap::AppSettings::DeriveDisplayOrder))]
 /// Supply-chain security for Rust
 ///
 /// When run without a subcommand, `cargo vet` will invoke the `check`
@@ -30,7 +29,7 @@ pub struct Cli {

     // Top-level flags
     /// Path to Cargo.toml
-    #[clap(long, name = "PATH", parse(from_os_str))]
+    #[clap(long, name = "PATH", value_parser)]
     #[clap(help_heading = "GLOBAL OPTIONS", global = true)]
     pub manifest_path: Option<PathBuf>,

@@ -48,7 +47,7 @@ pub struct Cli {
     pub no_default_features: bool,

     /// Space-separated list of features to activate
-    #[clap(long, action, require_value_delimiter = true, value_delimiter = ' ')]
+    #[clap(long, action, value_delimiter = ' ')]
     #[clap(help_heading = "GLOBAL OPTIONS", global = true)]
     pub features: Vec<String>,

@@ -73,7 +72,7 @@ pub struct Cli {
     /// How verbose logging should be (log level)
     #[clap(long, action)]
     #[clap(default_value_t = LevelFilter::WARN)]
-    #[clap(possible_values = ["off", "error", "warn", "info", "debug", "trace"])]
+    #[clap(value_parser(["off", "error", "warn", "info", "debug", "trace"]))]
     #[clap(help_heading = "GLOBAL OPTIONS", global = true)]
     pub verbose: LevelFilter,
```

![image](https://user-images.githubusercontent.com/1136864/187009404-9da3125b-ad42-4324-8283-35f57dfeeb06.png)
![image](https://user-images.githubusercontent.com/1136864/187009413-ad403abb-aa31-469f-9685-b4db226612a1.png)
![image](https://user-images.githubusercontent.com/1136864/187009446-2dcd11ec-016b-447b-a835-02208c6944c6.png)
![image](https://user-images.githubusercontent.com/1136864/187009490-5bd70185-ff91-41ee-95e9-1237d9b252b7.png)

Looking at subcommand's --help:

![image](https://user-images.githubusercontent.com/1136864/187009533-d7a56cc2-5b78-483f-b25a-3eb96765a431.png)


---

_Comment by @epage on 2022-08-27 01:54_

@Gankra

Tip: `value_parser` (without a value) was only needed in clap 3.2 to opt-in to the new clap v4 behavior.  In v4 it is assumed and not needed. 
```diff
@@ -30,7 +29,7 @@ pub struct Cli {
     // Top-level flags
     /// Path to Cargo.toml
-    #[clap(long, name = "PATH", parse(from_os_str))]
+    #[clap(long, name = "PATH", value_parser)]
     #[clap(help_heading = "GLOBAL OPTIONS", global = true)]
     pub manifest_path: Option<PathBuf>,
```

---

_Comment by @epage on 2022-08-27 01:59_

Seeing `cargo-vet-vert-certify`
- I think there is a bug with the positional arguments, they should probably be using `[]`
- We collapse options because there can be so many but we should probably not collapse down to `[ARGS]` as that is most likely the core of what people are doing and the usage is a bit meaningless otherwise.

---

_Comment by @epage on 2022-08-27 02:02_

Seeing `--filter-graph`s help, you might find https://github.com/clap-rs/clap/issues/3108 useful (ie allow formatting in user provided strings).  Everything in the v4 API that accepts `StyledStr` will allow user styling once we implement #3108.  We have a placeholder type in there for now so it isn't a breaking change.

---

_Comment by @Gankra on 2022-08-27 02:07_

So the fact that `cargo-vet 0.3.0` and `cargo-vet-certify 0.3.0` is now `cargo-vet-vet 0.3.0` and `cargo-vet-vet-certify 0.3.0` seems like a minor regression/mess for cargo-subcommands, not sure if the CHANGELOG notes something about this or if I'm using the wrong "hack" for cargo-subcommands to work right.

I definitely like that subcommands come first! I had been considering a hack to do that in the [post-processed markdown version of this output](https://mozilla.github.io/cargo-vet/commands.html) ([the impl if you want to see my current horrors, which are almost certainly going to be broken by 4.0 but I begrudge no one but myself for this fact](https://github.com/mozilla/cargo-vet/blob/main/src/main.rs#L1578)).

I definitely like that DeriveDisplayOrder is the default! I am very particular about my output! In this vein I've also been hurting for a way to create extra headings for subcommand families, a la git (cargo-vet has too many subcommands because it has both plumbing and porcelain commands, and it's hard to indicate that some commands are "core functionality, use these every day" and these others are "utilities that are good to know about but you'll probably never need"):

![image](https://user-images.githubusercontent.com/1136864/187009821-cdeb345c-dd08-4291-9c04-07817760e35d.png)

I'm a bit sad to lose the splash of color, I find that it helps me quickly scan the verbose help output and breaks up the monotony. That said my opinion isn't strong enough for me to kick up a fuss.

ARGS vs Arguments I could take or leave, don't really care either way. (And I've never really understood the motivation of distinguishing arguments from options...)

Looking at these screenshots I am reminded that a higher-impact thing clap could be doing with my help messages is doing some cleanup of artifacts in the text that exist for rustdoc's sake (since this is pulling from docstrings which need to be valid markdown). Obviously "add a fuckin' markdown parser to clap" is a big ask, but it's unfortunate (and iirc I had to do some very careful work to not have the bullets in --filter-graph get completely messed up).

---

_Comment by @Gankra on 2022-08-27 02:11_

> We collapse options because there can be so many but we should probably not collapse down to [ARGS] as that is most likely the core of what people are doing and the usage is a bit meaningless otherwise.

I'm increasingly of the opinion that usage strings basically suck at the best of times, and actually am already hardcoding them away in rust-minidump (3 years ago or whatever clap was deriving absolute nonsense so I just burnt it to the ground).

Like I don't know how anyone ever looks at something like this and goes "yes this helps me" lol

![image](https://user-images.githubusercontent.com/1136864/187010195-13a0ee5d-dc73-4973-b392-cd6c47a6f6c7.png)


---

_Comment by @Gankra on 2022-08-27 02:15_

Oh wait oh god I in fact did not do the migration correct, my cli tests now all die with

```
---- tests::violations::mock_simple_violation_cur_exemptions stdout ----
thread 'tests::violations::mock_simple_violation_cur_exemptions' panicked at 'Mismatch between definition and access of `verbose`. Could not downcast to tracing_core::metadata::LevelFilter, need to downcast to alloc::string::String
', src\cli.rs:77:18
```

Why is --verbose of all things the flag that keeps breaking between clap versions x3

---

_Comment by @epage on 2022-08-27 02:18_

> So the fact that cargo-vet 0.3.0 and cargo-vet-certify 0.3.0 is now cargo-vet-vet 0.3.0 and cargo-vet-vet-certify 0.3.0 seems like a minor regression/mess for cargo-subcommands, not sure if the CHANGELOG notes something about this or if I'm using the wrong "hack" for cargo-subcommands to work right.

I might need to play with that to see whats going on.  We switched up how we render the name to fix https://github.com/clap-rs/clap/issues/992 and I'm unsure if your code is needing a change or if this is a bug.

>  In this vein I've also been hurting for a way to create extra headings for subcommand families,

https://github.com/clap-rs/clap/issues/1553 which I'm hoping to fit into v4 but it could probably be made in any 4.x release once I get the derive macros done for #1807

> I'm a bit sad to lose the splash of color, I find that it helps me quickly scan the verbose help output and breaks up the monotony. That said my opinion isn't strong enough for me to kick up a fuss.

I agree that color helps.

The concerns with color
- Do the colors picked have enough of a polished look
- Do the colors work well with terminal's theme (e.g. I'm trying to avoid true colors due to extra complexity of working with the theme)
- Do the colors work well with application's theme  (#2963)

We do plan to allow people to customize colors during the 4.x release, see https://github.com/clap-rs/clap/issues/3234

> (And I've never really understood the motivation of distinguishing arguments from options...)

Hey, at least we stopped distinguishing between flags and options!

I'll have to give this some thought.

> Obviously "add a fuckin' markdown parser to clap" is a big ask, but it's unfortunate (and iirc I had to do some very careful work to not have the bullets in --filter-graph get completely messed up).

Its a big ask but we need to at least make it possible, even if we make it either opt-in or opt-out: https://github.com/clap-rs/clap/issues/2389 (which builds on top of the other issue I linked to for `--filter-graph`)

> Like I don't know how anyone ever looks at something like this and goes "yes this helps me" lol

Yes, this is why I still think we should collapse to `[OPTIONS]` as they are more plentiful and can easily overwhelm the user.  `[ARGS]` tend to not do that so much.

---

_Comment by @epage on 2022-08-27 02:21_

> Oh wait oh god I in fact did not do the migration correct, my cli tests now all die with
> 
> ```
> ---- tests::violations::mock_simple_violation_cur_exemptions stdout ----
> thread 'tests::violations::mock_simple_violation_cur_exemptions' panicked at 'Mismatch between definition and access of `verbose`. Could not downcast to tracing_core::metadata::LevelFilter, need to downcast to alloc::string::String
> ', src\cli.rs:77:18
> ```
> 
> Why is --verbose of all things the flag that keeps breaking between clap versions x3

```rust
    /// How verbose logging should be (log level)
    #[clap(long, action)]
    #[clap(default_value_t = LevelFilter::WARN)]
    #[clap(possible_values = ["off", "error", "warn", "info", "debug", "trace"])]
    #[clap(help_heading = "GLOBAL OPTIONS", global = true)]
    pub verbose: LevelFilter,
```

See https://github.com/clap-rs/clap/discussions/3855

EDIT: I guess that points back to https://github.com/clap-rs/clap/issues/3822#issuecomment-1155075962 which is a reply to you...

---

_Referenced in [mozilla/cargo-vet#316](../../mozilla/cargo-vet/pulls/316.md) on 2022-08-27 02:43_

---

_Comment by @Gankra on 2022-08-27 02:56_

Yeah I think I was working on migrating to the new 4.0 API and then you added a way to silence the deprecations and I decided to forget about it for now, hoping a better answer would come along.

I just did a hacked up TypedParser and some quick fixups to the markdown hack to get it working again. You can see the full snapshot diffs here, if you're curious: https://github.com/mozilla/cargo-vet/pull/316

---

_Comment by @Gankra on 2022-08-27 03:05_

Oh also: big fan of killing the version info at the start of help, it's not very helpful and also makes snapshots tests randomly churn whenever I cut a releases, which is mildly annoying (my code is also riddled with `#[clap(disable_version_flag = true)]` because subcommand version flags are also weird noise).

---

_Comment by @epage on 2022-08-27 11:16_

>  it's not very helpful and also makes snapshots tests randomly churn 

Which snapshot solution do you use?  With `snapbox`, we support wildcards like `[..]`, see [one of clap's examples](https://github.com/clap-rs/clap/blob/master/examples/demo.md)

> my code is also riddled with #[clap(disable_version_flag = true)] because subcommand version flags are also weird noise

Why not just drop `propagate_version = true`? That will make it so you only get the version flag on the commands you explicitly apply the `version` attribute to.

---

_Comment by @Gankra on 2022-08-28 16:44_

We use [cargo-insta](https://insta.rs/) in the most naive possible way, because I *do* want to get notified about any and all changes to output (and insta makes it really easy to go "yep fine"). Version bumps are just the more annoying one, probably because I haven't setup cargo-release for my projects. X3

---

_Comment by @ducaale on 2022-08-29 09:43_

>The greens and yellows are gone, instead using bold, underline, and dimmed: https://github.com/clap-rs/clap/pull/4117 though https://github.com/clap-rs/clap/issues/3108
>- Dimmed was removed from placeholders in https://github.com/clap-rs/clap/pull/4126
>
>Reduce the ALL CAPS: https://github.com/clap-rs/clap/pull/4123
>- ARGS and OPTIONS matched the usage and worked as non-colored headings but it feels a bit unpolished
>- I was hard pressed to find other CLIs that do this

One argument for keeping ALL CAPS is that it mirrors the heading in man pages. Also, I find the underline to hurt the readability of lowercased headings (see how `g` and `p` are rendered when underlined in https://github.com/clap-rs/clap/issues/4132#issuecomment-1229091197).



---

_Referenced in [clap-rs/clap#4143](../../clap-rs/clap/pulls/4143.md) on 2022-08-29 20:19_

---

_Referenced in [clap-rs/clap#4145](../../clap-rs/clap/pulls/4145.md) on 2022-08-29 20:52_

---

_Referenced in [clap-rs/clap#4148](../../clap-rs/clap/pulls/4148.md) on 2022-08-30 14:34_

---

_Referenced in [clap-rs/clap#4151](../../clap-rs/clap/pulls/4151.md) on 2022-08-30 21:13_

---

_Referenced in [clap-rs/clap#4155](../../clap-rs/clap/pulls/4155.md) on 2022-08-31 13:53_

---

_Comment by @epage on 2022-08-31 14:22_

I thought I'd record my usage experiments

Current:
```
git-stash

Usage:
    git-derive stash [OPTIONS]
    git-derive stash <COMMAND>

Commands:
    push
    pop
    apply
    help     Print this message or the help of the given subcommand(s)

Options:
    -m, --message <MESSAGE>
    -h, --help                 Print help information
```
- Takes up an extra line

Same line
```
git-stash

Usage: git-derive stash [OPTIONS]
       git-derive stash <COMMAND>

Commands:
    push
    pop
    apply
    help     Print this message or the help of the given subcommand(s)

Options:
    -m, --message <MESSAGE>
    -h, --help                 Print help information
```
- Extra indentation

git-style
```
git-stash

Usage: git-derive stash [OPTIONS]
   or: git-derive stash <COMMAND>

Commands:
    push
    pop
    apply
    help     Print this message or the help of the given subcommand(s)

Options:
    -m, --message <MESSAGE>
    -h, --help                 Print help information
```
- Extra indentation and noisier

Shell prompt
```
git-stash

$ git-derive stash [OPTIONS]
$ git-derive stash <COMMAND>

Commands:
    push
    pop
    apply
    help     Print this message or the help of the given subcommand(s)

Options:
    -m, --message <MESSAGE>
    -h, --help                 Print help information
```
- Too clever?

Overall, I'm leaning towards leaving the usage the same

---

_Referenced in [clap-rs/clap#4156](../../clap-rs/clap/pulls/4156.md) on 2022-08-31 14:37_

---

_Comment by @epage on 2022-08-31 14:59_

I believe the original issue has been updated to reflect the current conversation, including dispositions on decisions and links out to any newer PRs

Before I remove name/version/about from the **default** template, I've pinged a couple more people directly to try to make sure this conversation is fairly representative

---

_Comment by @epage on 2022-08-31 15:02_

Looking at `docker -h`, I'm thinking it might be worth dropping the indentation level from 4 to 2 to reduce the likelihood of wrapping.

For flags/options, there already is a lot of space taken up due to the flags.

---

_Comment by @BurntSushi on 2022-08-31 15:20_

I think removing name/version/about is fine. I agree that's probably a good change.

I do personally like `ALLCAPS` headings though. In part because they look natural to me given their use in man pages. The underlining is nice, but it is pretty common for me to do things like `cmd --help | less`, in which case, I imagine the underlining will get disabled by default.

But I don't think I have any super strong opinions since I think this is all pretty customizable by using your own template right?

---

_Comment by @epage on 2022-08-31 15:38_

> But I don't think I have any super strong opinions since I think this is all pretty customizable by using your own template right?

Yes, I've included in each item in the issue and in the changelog how to get back the original behavior.   The one that cannot be revert is the choice in coloring but that is planned for next after 4.0.0 but people can continue to work around it with `Command::disable_colored_help(true)`.

---

_Referenced in [clap-rs/clap#4159](../../clap-rs/clap/pulls/4159.md) on 2022-08-31 19:19_

---

_Referenced in [clap-rs/clap#4160](../../clap-rs/clap/pulls/4160.md) on 2022-08-31 20:07_

---

_Referenced in [clap-rs/clap#4161](../../clap-rs/clap/pulls/4161.md) on 2022-08-31 21:03_

---

_Referenced in [clap-rs/clap#4188](../../clap-rs/clap/pulls/4188.md) on 2022-09-07 16:04_

---

_Referenced in [clap-rs/clap#4190](../../clap-rs/clap/pulls/4190.md) on 2022-09-07 19:03_

---

_Referenced in [clap-rs/clap#4192](../../clap-rs/clap/pulls/4192.md) on 2022-09-07 22:06_

---

_Comment by @epage on 2022-09-09 19:20_

Should we provide built-in pager support for help?  We'd skip the pager if the output isn't a tty

I lean towards doing this for long help (`--help`) to encourage having short help (`-h`) be very short.

I would expect this to be behind an on-by-default feature flag `pager`.

Unsure if there is something newer/fancier but I was looking at the [pager](https://crates.io/crates/pager) crate.

(inspired by [clig.dev output section](https://clig.dev/#output))

---

_Comment by @epage on 2022-09-09 19:26_

And/or we can provide a way for users to tell us what the man page is and to look it up to show instead

> However, not everyone knows about man, and it doesn’t run on all platforms, so you should also make sure your terminal docs are accessible via your tool itself. For example, git and npm make their man pages accessible via the help subcommand, so npm help ls is equivalent to man npm-ls.

---

_Comment by @tshepang on 2022-09-10 04:04_

> Should we provide built-in pager support for help? We'd skip the pager if the output isn't a tty

yes please

---

_Referenced in [clap-rs/clap#4201](../../clap-rs/clap/issues/4201.md) on 2022-09-10 11:20_

---

_Comment by @epage on 2022-09-10 11:43_

I've started planning pager support in https://github.com/clap-rs/clap/issues/4201

---

_Comment by @epage on 2022-09-13 12:21_

Within the scope of the 4.0.0 release, I feel like this is resolved.  Pager support is likely to start as an `unstable-pager` feature and can come after 4.0.0

---

_Closed by @epage on 2022-09-13 12:21_

---

_Referenced in [clap-rs/clap#4218](../../clap-rs/clap/issues/4218.md) on 2022-09-16 12:24_

---

_Referenced in [rsadsb/adsb_deku#181](../../rsadsb/adsb_deku/pulls/181.md) on 2022-09-21 01:33_

---

_Comment by @epage on 2022-09-21 15:09_

More color feedback

> Will clap v4 come with the ability to turn help colors back on? I think it might be a good idea if it did, even if it was only rudimentary. I'd like to upgrade projects to v4, but I quite like having colored help.
> ...
> I find that colors set off the different parts of the help message much better than bolding underlining, and give it a sort of visual structure that makes it easy to skim.
> 
>     Any thoughts on how to have defaults that meet the majority case of needs?
> 
> I think that there should be a few options:
> 
>     No colors or style
>     Bold and underling only
>     Default colors (a la the current colors)
>     Custom colors
> 
> I would go for the third option, and people who didn't like colors would probably go for the second option. 1, 2, or 3 would all be reasonable defaults.

https://www.reddit.com/r/rust/comments/xjlie4/preannouncing_clap_40_a_rust_cli_argument_parser/ip9ihv1/
https://www.reddit.com/r/rust/comments/xjlie4/preannouncing_clap_40_a_rust_cli_argument_parser/ipa8e0g/

> I also like the coloring essentially as-is. More customization is nice, but I always liked and enabled the defaults. I find it makes it very clear what is what in help output and is more pleasant to read than monochromatic.
>
> My preferred solution would be that, when customization is figured out, have the existing color behavior as some sort of easy to select "preset".

https://www.reddit.com/r/rust/comments/xjlie4/preannouncing_clap_40_a_rust_cli_argument_parser/ipa403b/

> I also prefer the colored output and I think it's for the same reason I switched to a colored prompt over a decade ago. Monochromatic output in a context other than something like a Macintosh SE feels stark and unsatisfying to me.

https://www.reddit.com/r/rust/comments/xjlie4/preannouncing_clap_40_a_rust_cli_argument_parser/ip9n5lp/

> The best thing about colours was that I could be sure: the app is written in Rust and uses clap, and that means a certain baseline of quality and behaviour. It also looks pretty and easy to read - assuming that it is visible on your background.

https://www.reddit.com/r/rust/comments/xjlie4/preannouncing_clap_40_a_rust_cli_argument_parser/ipb0z0k/

> For the record, I just made a "Downgrade to Clap 3.x to wait for actual colored output" commit on a brand new "shell script written in Rust for compile-time correctness" I wrote today because I find the use of intense white and underlines so unacceptable for something I intend to dogfood, and I'll probably add an explicit version bound to the ~/bin/add <crate name> shell script I use to block typos and inject preferred default features on commonly used crates.)

https://www.reddit.com/r/rust/comments/xjlie4/preannouncing_clap_40_a_rust_cli_argument_parser/irxnh6s/?context=3

> I like the old style of clap much more than the new one.
The colored output was much prettier as the new underlined text.

#4429

---

> Yay!

https://www.reddit.com/r/rust/comments/xjlie4/preannouncing_clap_40_a_rust_cli_argument_parser/ip9flx3/

> No no, that wasn't what I meant. I like the bold and underline output more than the color. While colors are nice, they get boring fast and the choice of colors wasn't also my favorite. I am currently reading the report and saw the future plans, which BTW was the reason of my mentioning of the future additions.

https://www.reddit.com/r/rust/comments/xjlie4/preannouncing_clap_40_a_rust_cli_argument_parser/ip9kwxi/

>  Also, I have to agree, I like the new help text better after letting it sink in a little.

https://twitter.com/ConradLudgate/status/1572466091334459394

---

_Referenced in [clap-rs/clap#4256](../../clap-rs/clap/pulls/4256.md) on 2022-09-26 15:11_

---

_Referenced in [clap-rs/clap#4267](../../clap-rs/clap/pulls/4267.md) on 2022-09-27 03:00_

---

_Referenced in [ProvableHQ/snarkVM#1124](../../ProvableHQ/snarkVM/pulls/1124.md) on 2022-09-30 08:29_

---

_Referenced in [pkolaczk/fclones#169](../../pkolaczk/fclones/issues/169.md) on 2022-09-30 16:27_

---

_Referenced in [clap-rs/clap#4308](../../clap-rs/clap/issues/4308.md) on 2022-09-30 18:38_

---

_Referenced in [planus-org/planus#147](../../planus-org/planus/pulls/147.md) on 2022-10-03 18:07_

---

_Comment by @epage on 2022-10-03 21:42_

Another idea came up for help output: https://github.com/clap-rs/clap/issues/4341

---

_Referenced in [mindvalley/wukong-cli#21](../../mindvalley/wukong-cli/pulls/21.md) on 2022-10-06 08:30_

---

_Comment by @chevdor on 2022-10-06 09:40_

From what I read color vs no color is very opinionated and this is fine. A good option should be to **not** decide for the end user.

I am one of those missing coloring :) but I agree that safer defaults is a better solution.

Ideally the coloring choice should be left to the **end user** (ie not the dev) of the CLI.

I would not mind a "safe" default (even if this is without any color) as long as the **end user** can set an ENV to turn on help coloring. It would actually be cool to be able to control the coloring of all installed "clap_v4-based-cli" based on a single ENV such as `CLAP_HELP_COLORING=SHOW_ME_RAINBOWS` or `CLAP_HELP_COLORING=KEEP_IT_BORING` 😄  and simply let each **end** user makes his/her own choice.

---

_Comment by @epage on 2022-10-07 00:55_

If an application decides to publicly expose themes to the user, that is for the application to decide.  I don't think its appropriate though for there to be a clap specific variable for it. 
- Part of an applications behavior is now tied to a library its using
- The user is using the application and not clap.  Similarly, I dislike `RUST_BACKTRACE`.  People don't want "a clap" application or a "rust applicatuion" (ok some do) but people want an application that solves there problems.  It feels unpolished to be accessing "internals" like rust env variables or arg parser env variables.

---

_Comment by @tshepang on 2022-10-07 05:34_

@epage do you mean a better way to have color control on the terminal should be with a generic thing like `$COLOR=(auto|always|never)`

---

_Comment by @epage on 2022-10-07 11:58_

A `COLOR` would only control detection.  I'm referring to application-specific theme config or env variables with maybe looking at generalized versions of it.  For example, I believe `bat` has `BAT_THEME`.  `delta` and `broot` reuse some of `bat`s assets, so it could make sense to have a more general theme env variable that all apps can fall back on, like a `COLOR` or a `PAGER` env variable.

---

_Referenced in [clap-rs/clap#3234](../../clap-rs/clap/issues/3234.md) on 2022-10-07 15:36_

---

_Referenced in [wfxr/csview#93](../../wfxr/csview/pulls/93.md) on 2022-10-09 09:50_

---

_Comment by @memark on 2022-10-13 16:32_

Perhaps we unpin this issue now that it's closed/finished?

---

_Comment by @epage on 2022-10-13 16:38_

I've been keeping it pinned for easy finding for people migrating to clap v4 that have feedback on this.

---

_Comment by @melMass on 2022-10-26 21:20_

Sorry I read the whole issue (after not finding anything in the doc), but how do we either bring back colors, or customize the help output all together?

Thanks

---

_Comment by @epage on 2022-10-27 01:51_

That is not supported yet, see
- #3234 (customize colors)
- #3108 (provide styled content)

Those are the highest priority focus area for clap v4.  However, some other things have taken my attention away from clap for a little bit (e.g. the recent cargo-release release) and some Real World stuff has come up.  Hopefully I'll be able to get back to this soon.

---

_Referenced in [quickwit-oss/quickwit#2236](../../quickwit-oss/quickwit/issues/2236.md) on 2022-11-03 15:02_

---

_Referenced in [clap-rs/clap#4470](../../clap-rs/clap/issues/4470.md) on 2022-11-09 19:25_

---

_Comment by @AmionSky on 2022-11-12 12:01_

I just want to say I'm definitely one of those people who likes the colored help. It's way more readable, I can see what I want from a quick glance. Having the color completly removed from the help has been a big dissapointment. (I actually went back to v3)

I would really like to have the option to bring back the colors.

---

_Referenced in [Yamato-Security/hayabusa#817](../../Yamato-Security/hayabusa/pulls/817.md) on 2022-11-19 07:11_

---

_Comment by @Seltyk on 2022-12-07 03:06_

The `man`-like output of v3 was something I really liked, so the move to a more… dare I say, Pythonic style in v4 is a letdown. But more importantly, not having colors is a total dealbreaker, for the exact same reasons as AmionSky wrote above.

---

_Comment by @tijoer on 2022-12-21 12:37_

I saw this wall of text linked on Stackoverflow. I cannot get colors in my help pages to work, even though I have the following: #[clap(author, version, about, arg_required_else_help = true, color=ColorChoice::Always)]

How can I get colors back, as it helps to understand the information in the help page way better.

---

_Comment by @epage on 2022-12-21 13:55_

The very first item in the issue is about that.  Other styling was selected instead for clap v4's default (#4117).

The item ends with

> Users can't rollback until theming support is in (Allow good/warning/error/hint color to be customized #3234) though they can just disable the coloring with Command::disable_colored_help(true)

If you follow that issue, it is still open though it is my priority to work on when I am able to focus more on clap.  I am trying to get some stuff done on another project atm.

---

_Comment by @DianaNites on 2022-12-21 23:34_

The only way to get the colors back is stay on the previous clap version. This is what I do in my new and existing projects, just use clap v3.

---

_Comment by @tijoer on 2022-12-22 12:50_

Thank you for the answer.

---

_Referenced in [youki-dev/youki#1443](../../youki-dev/youki/pulls/1443.md) on 2022-12-23 23:31_

---

_Comment by @NuSkooler on 2023-01-06 03:50_

I'm having a hard time following this -- I was surprised when I upgraded to 4.x and the color went away. Is the official answer to stick with v3? That seems quite off. Is an option for re-enabling the color for --help in the pipeline?

Additionally, was https://no-color.org/ considered?



---

_Comment by @epage on 2023-01-06 03:57_

@NuSkooler 

From the Issue's description

> The greens and yellows are gone, instead using bold, underline, and dimmed: https://github.com/clap-rs/clap/pull/4117 though https://github.com/clap-rs/clap/issues/3108

towards the end is

> Users can't rollback until theming support is in (Allow good/warning/error/hint color to be customized #3234) though they can just disable the coloring with Command::disable_colored_help(true)

This is my top clap priority (though I have another project that is a higher priority atm). If this is a blocker for someone then, yes using clap v3 is the answer.

> Additionally, was https://no-color.org/ considered?

Unsure how this is relevant.  #2963 captures the motivation which in summary which led us to be more neutral in our default terminal styling. no-color would not help with that but instead helps with enabling/disabling application styling.  I also hope to respect more env variables like this by default in the future.

---

_Comment by @NuSkooler on 2023-01-06 04:05_

@epage Thanks for the quick response!

Understood on priority et al., 'tis life!

`NO_COLOR` is specifically designed to allow applications to default to color terminal output unless told otherwise. In general, the _styling_ falls into this category as well as various terminals or environments (say stdout captured to file for example) become full of ANSI escape sequences with any of it enabled.

From the page:
> Command-line software which adds ANSI color to its output by default should check for a NO_COLOR environment variable that, when present and not an empty string (regardless of its value), prevents the addition of ANSI color.



---

_Comment by @dtolnay on 2023-01-06 05:09_

It's wild how opposite my point of view is to all the ones voiced above. I agree with many of the comments from https://github.com/clap-rs/clap/discussions/2906 -- I always hated the v3 `--help` color scheme, I found it gaudy and I disabled it in all of my CLIs. I love the v4 color scheme. I think it looks refined and I think Ed did a terrific job with it.

I've been wondering how much of the difference is people's legitimate personal preference vs people's terminals being differently configured to make one or the other theme look bad.

<table><tr><td><img src="https://user-images.githubusercontent.com/1940490/210934098-a2af5550-d19e-4236-9561-f9b8a5dd893f.png" width="500"></td><td><img src="https://user-images.githubusercontent.com/1940490/210934093-ba3b93eb-8aa8-420f-8146-d38d02765a8d.png" width="500"></td></tr></table>


---

_Comment by @epage on 2023-01-06 05:24_

@dtolnay thanks for providing another voice.  As there is no way to make accurate internet polls on these types of things, its hard to sift through the bias of who is currently speaking.

I look forward to the experimentation and the learning that will happen when theming support is in and am curious to see if someone is able to popularize an alternative that meets the needs discussed in #2906.

---

_Comment by @LuckyTurtleDev on 2023-01-06 11:30_

> I've been wondering how much of the difference is people's legitimate personal preference vs people's terminals being differently configured to make one or the other 

After seeing @dtolnay's screenshots I think the terminal / terminal-theme has an huge effect on this. At the provided screenshots the v3 theme looks more ugly and the v4 theme looks much nicer and modern.
But for example at the [alacritty](https://crates.io/crates/alacritty) terminal with the default configuration it is the opposite.
I extremely like the colored output here, but found the v4 theme ugly and less easy to survey. The v3 looks much modern here.


|v3|v4|
|---|---|
| ![image](https://user-images.githubusercontent.com/44570204/210998254-12d39acd-2de4-44e6-ab6b-373c277a8855.png) | ![image](https://user-images.githubusercontent.com/44570204/210999206-e9665d46-1ddc-4daf-8094-3d81e885a24f.png) |

Of course this is probably also only a personal preference. 

I think the `NO_COLOR` idea would be a very good compromise. So the user can decide what they will prefer and is not bound to the choose of the developer.
So clap can use the v3 theme if `NO_COLOR` is not present otherwise it can use the v4 theme.
The problem with this would be that many users are not aware of this option.
So the developer with does not like v3 would probably hard code v4 again. So we need also on option to explicit enable it again, by the user. Sadly `NO_COLOR=false` is not supported by https://no-color.org/ and will even leads other programs to disable colors. So maybe using a different environment variable like `CLAP_COLOR`  and only fall back to `NO_COLOR`, if `CLAP_COLOR`  does not exist  would be an better option. The downside of this would be that this would be a island solution, only supported by clap.  Maybe suggesting to add a `PREFER_COLOR`  to https://no-color.org/ would be an better option.

---

_Comment by @epage on 2023-01-06 12:55_

> I think the NO_COLOR idea would be a very good compromise. So the user can decide what they will prefer and is not bound to the choose of the developer.

Except `NO_COLOR` really means "no ansi codes" which means it would disable all formatting.  Ideally, the rest of the application would also support it so it would remove everything completely.  This is why I was wondering why it was relevant to this conversation.

At one point someone requested a clap-specific environment variable to allow users to control this but that is a policy that I don't think clap should take on but application authors.  Users interact with the author's application, not clap, and we shouldn't bypass that relationship.  This also gets into compatibility issues for the author if they switch parsers.

---

_Comment by @NuSkooler on 2023-01-06 16:56_

Just to be clear around formatting, this is ANSI/Escape code formatting only (ie: color, underlines, bold, so on). 

I don't want to be a dead horse, but I'd think the following would be the proper path, reflecting what other tools do and `NO_COLOR`:

* Reinstate the previous colors as the *default* that Clap provides unless told otherwise by the developer
* Proceed with the support of themes (this is a great enhancement!)
* Respect `NO_COLOR`. Users that don't want color at all set this in their terminals. Some users may for example use a `--no-color` CLI option as well either built in or supplied by the dev

An aside: By using the base 16 ANSI colors, users can control what "red" for example looks like in their terminal emulator; Or in other words, allows user to control what *their* terminal looks like without you going much at all.

---

_Comment by @epage on 2023-01-06 17:12_

@NuSkooler while I plan to eventually have `NO_COLOR` support, it is not part of solution to this because
- It disables colors for error messages
- It disables colors through the rest of the application

When all a user or developer wanted is to disable color in the help.  Yes, we have `disable_colored_help` for application authors but we are trying to pair down the number of knobs to control things.

The path forward is
1. Theming support
2. People experiment with ideas and a de facto standard arises
3. We evaluate and possibly update `clap` to that de facto standard with theming support being an escape hatch for people who are not happy with it.

---

_Comment by @NuSkooler on 2023-01-06 17:48_

@epage I think that all makes sense. I guess the main thing I'm confused about is why now *default* to no color -- even when I as a developer told Clap to explicitly utilize it? Most if not all of the modern CLI apps that I can think of that support color at all, default to using that color.

---

_Comment by @epage on 2023-01-06 18:54_

@NuSkooler 

> even when I as a developer told Clap  to explicitly utilize it

Because "color" is a common stand-in word for "terminal styling"

> . I guess the main thing I'm confused about is why now default to no color

- I agree with the sentiment that there were problems with the old colors.
- We want to have "safe" defaults
- In coming up with a solution, I mostly had to do it alone.  Few people were willing to collaborate on this, to try understand needs outside of their own position and finding new solutions.  Instead people have been very reactionary, wanting hardline solution that are non-starters (no terminal styling by default, clap v2 colors are the only way)
- I expected theming support to be released soon after.  I was working on it directly after 4.0 but then (1) a higher priority came in and (2) I needed to take some family leave, severely limiting my availability for the last couple of months
- Addressing terminal styling would have gone faster if just a few of those complaining had stepped up to help despite mentioning in various places what the work is that we need to do. 
- In contrast to the above, I've had a drag on my time repeatedly answering the same questions in the same threads.

> Most if not all of the modern CLI apps that I can think of that support color at all, default to using that color.

I feel like there is something missing in this statement as it comes across as circular to me.

---

_Comment by @NuSkooler on 2023-01-06 19:23_

@epage Firstly, I appreciate your willingness to debate here! I hope I'm not coming off as complaining, it's certainly not my intent. Your work is extremely appreciated. I maintain a number of OSS projects myself and know how it can be!

Understood on all notes. FWIW, I'm happy to try and help! I don't know the Clap code base well other than my external usage, but I do know terminals, ANSI, and some of the various standards, caveats, so on around them.

I may not be following the last statement about "circular":
What I'm suggesting is that modern terminal applications that themselves have color at all (some are still just plain text) provide color by default, and generally follow the `NO_COLOR` and/or `--no-color` or similar CLI options. But again, by default, they provide color in a 'default' theme even if exposing the ability for users to provide alternate themes.

And to further point out: Instead of disabling colors all together, one can use the "base" ANSI 0-16 colors, allowing the terminal emulator itself (generally exposed to the user in configuration -- you can see this in MS terminal now even!). This allows the user to define what each of the 0-16 resolve to. In other words, if you say set the "Usage" to color 4, my terminal and your terminal may look quite different based on our individual preferences. If you instead set the color to a _specific_ value (using 256 or true color syntax), the terminal will just show that.

---

_Comment by @epage on 2023-01-06 19:39_

@NuSkooler

> Understood on all notes. FWIW, I'm happy to try and help! I don't know the Clap code base well other than my external usage, but I do know terminals, ANSI, and some of the various standards, caveats, so on around them.

Thats actually more what we need than clap help.  My requirements for theming and custom user styled text is that we don't take on a whole terminal styling API in clap.  The path forward 
- A minimal API for defining terminal styles with no other policy.  The remaining work is being tracked at https://github.com/epage/anstyle/issues/14 and #3234 is for the clap side of things
- Leverage ANSI escape codes in `String`s as the stable API for styled text.  The bulk of the work is being tracked in https://github.com/epage/anstyle/issues/5 and #1433 and #3108 are for the clap side to track the work

> What I'm suggesting is that modern terminal applications that themselves have color at all (some are still just plain text) provide color by default, and generally follow the NO_COLOR and/or --no-color or similar CLI options. But again, by default, they provide color in a 'default' theme even if exposing the ability for users to provide alternate themes.

I feel like we are talking in circles on this topic which makes me think one of us is missing something about the other's responses.  I've previously replied on why I think `NO_COLOR`, while a useful feature to support, is unrelated to this topic.

> And to further point out: Instead of disabling colors all together, one can use the "base" ANSI 0-16 colors, allowing the terminal emulator itself (generally exposed to the user in configuration -- you can see this in MS terminal now even!). This allows the user to define what each of the 0-16 resolve to. In other words, if you say set the "Usage" to color 4, my terminal and your terminal may look quite different based on our individual preferences. If you instead set the color to a specific value (using 256 or true color syntax), the terminal will just show that.

Yes, I'm aware.  That is one of the reasons I prefer the basic 16 / other effects over 256 and truecolor, we are more likely to be able to adapt to the users theme.  However, different themes still optimize for different colors having better contrast or not.  People are not going to theme their terminal specifically for clap.   If we do use any colors, they likely need to have the widest theme compatibility for contrast, light and dark (I know, I'm one of those weird people who prefers light themes).

---

_Referenced in [NNPDF/pineappl#145](../../NNPDF/pineappl/issues/145.md) on 2023-02-01 10:14_

---

_Comment by @epage on 2023-04-18 20:54_

> Support for [theming] is now available as `Command::styles` with v4.2.3 when the `unstable-styles` feature is enabled.
> 
> Note: we are not guaranteeing semver compatibility on this feature until the feature flag is removed.

Please post feedback on #3234

---

_Comment by @corneliusroemer on 2023-07-13 11:16_

Thanks for working on bringing colors back @epage! Questions of taste are always tricky.

@Gankra 
> I'm increasingly of the opinion that usage strings basically suck at the best of times, and actually am already hardcoding them away in rust-minidump (3 years ago or whatever clap was deriving absolute nonsense so I just burnt it to the ground).
>Like I don't know how anyone ever looks at something like this and goes "yes this helps me" lol

I find the compact usage useful to quickly remember exact option names, when I have a rough idea but can't remember the exact syntax. Like in this case with `--interactive` which I would otherwise almost need to scroll for:
<details>
<summary>Show `git commit -h` screenshot</summary>
<img width="2475" alt="image" src="https://github.com/clap-rs/clap/assets/25161793/6f876974-2714-45ee-8f06-6abbea9f1941">
</details>

But I agree that when the dense options become too many, they become useless, like for snakemake (this goes one for another screen...).
<details>
<summary>Show `snakemake -h` screenshot</summary>
<img width="2504" alt="image" src="https://github.com/clap-rs/clap/assets/25161793/d4962d1b-70f4-4f4f-bcc2-cfc30341879e">
</details>




---

_Comment by @epage on 2023-07-13 12:43_

To prevent usages from being overwhelming is why clap only shows positionals and commands and not options.

The downside to overriding the usage is we show that in errors rather than the "smart" usage that adapts to the users arguments.

#4191 explores some ideas for improving usage with hints like
- split a mutually exclusive arg group into its own usage
- force showing an option
- force showing the options in an arg group, with one usage per arg group

There is another issue for flattening the help for subcommands much like `git stash` which will show a usage per subcommand rather than a generic usage

---

_Referenced in [clap-rs/clap#5071](../../clap-rs/clap/issues/5071.md) on 2023-08-15 13:16_

---

_Referenced in [rust-lang/cargo#2290](../../rust-lang/cargo/issues/2290.md) on 2023-09-20 19:35_

---

_Referenced in [astral-sh/uv#3473](../../astral-sh/uv/issues/3473.md) on 2024-05-15 19:46_

---

_Referenced in [astral-sh/rye#1209](../../astral-sh/rye/issues/1209.md) on 2024-07-09 14:56_

---

_Referenced in [robiot/rustcat#69](../../robiot/rustcat/pulls/69.md) on 2024-07-17 18:49_

---

_Referenced in [jj-vcs/jj#5716](../../jj-vcs/jj/pulls/5716.md) on 2025-02-16 01:49_

---
