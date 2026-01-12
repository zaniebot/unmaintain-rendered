```yaml
number: 2808
title: "Builder and `clap_derive` should be WYSIWYG"
type: issue
state: closed
author: epage
labels:
  - C-bug
  - M-breaking-change
  - A-help
  - S-waiting-on-design
assignees: []
created_at: 2021-10-04T18:52:30Z
updated_at: 2022-07-22T21:18:53Z
url: https://github.com/clap-rs/clap/issues/2808
synced_at: 2026-01-12T16:14:13Z
```

# Builder and `clap_derive` should be WYSIWYG

---

_@epage_

### Rust Version

1.55.0

### Affected Version of clap

v3.0.0-beta.4

### Expected Behavior Summary

Args are ordered as I declare them

### Actual Behavior Summary

Args are sorted, without any exception (exceptions are common)

### Context

`DeriveDisplayOrder` is a [very popular flag to turn on](https://github.com/clap-rs/clap/discussions/2627#discussion-3478195).  When further researching this, [it is popular across CLIs and even when args are sorted, its one manually to allow exceptions](https://github.com/clap-rs/clap/discussions/2627#discussioncomment-1120949).

`rg` is one major exception for this
> I don't enable `DeriveDisplayOrder` because I do actually want everything to be alphabetical. See [group command line args into affected processing steps BurntSushi/ripgrep#1814](https://github.com/BurntSushi/ripgrep/issues/1814) and [organized flags in man page according to logical section? BurntSushi/ripgrep#1022](https://github.com/BurntSushi/ripgrep/issues/1022) for more details.

I chalk this up to `rg` being an exceptional case in its scope and design and being limited by clap2's lack of `help_heading`.

#### Trade offs

Current Sorting Implementation
- Strengths
  - Immediate polish for quick and dirty programs
  - Maintainable polish with large CLIs that have no good sort order and can't use `help_heading` (ie `rg`)
- Weaknesses
  - Doesn't group `--no-` flags with their positive versions
  - Sorts all, even `--help` and `--version` which are generally left unsorted

Deriving Order
- Easily discoverable path to both behaviors (can manually sort without finding the Setting)
  - Except when composing CLIs, like with `#[clap(flatten)]`
- Commonly what is expected

#### Survey of CLIs

Sorted
- `rg`
  - sort order is always respected, even for `--help` and `--version` (uses clap, author confirmed its intentional)
- `tokei`
  - sort order is always respected, even for `--help` and `--version` (uses clap)
- gnu `ls`
  - sort order is broken for `--help` / `--version`
- gnu `df`
  - sort order is broken for `--help` / `--version`
- `less`
  - sorts by short name, then long name (so long names without a short name are last)
  - Even sorts `no-` variants

Custom
- `cargo`
- `bat`
- `fd`
- `delta`
- `hyperfine`
- `rsync`
- `vim`
- gnu `chmod` 
  - Even if this was sorted, I suspect the `no-` variants would not be sorted
- `mount`
- `curl`
- `wget`
- `iptables`
- `traceroute`
- gnu `grep`
- gnu `find` (mostly sorted but there are exceptions

*(List of commands was inspired by prior Rust command list, some off the top of my head, and a random [50 linux command list](https://www.ubuntupit.com/best-linux-commands-to-run-in-the-terminal/) I found.  I only did a random sampling of the GNU CoreUtils though)*

---

_Label `T: bug` added by @epage on 2021-10-04 18:52_

---

_Label `C: help message` added by @epage on 2021-10-04 18:52_

---

_Label `D: easy` added by @epage on 2021-10-04 18:52_

---

_Added to milestone `4.0` by @epage on 2021-10-04 18:52_

---

_Comment by @joshtriplett on 2021-10-04 21:37_

I really do want options alphabetized by default, even if I only have a few of them. For small, quick tools, that'll avoid having options in a haphazard order. For larger tools with more options, I'd want to decide some kind of logical grouping of options, and in that case I might consider turning on DeriveDisplayOrder, but I would want that to be a deliberate choice.

---

_Comment by @epage on 2021-10-04 21:42_

@joshtriplett thats an interesting use case to keep in mind!

This presents a bit of a tension.  Unorganized prototypes might want things alphabetized and these benefit most from defaults.  In contrast, I know a lot of my CLIs have logical ordering but I forget to set the display order to derive in them.

I'd be curious what other precedence there is.  I know `argparse` only has the equivalent of `DeriveDisplayOrder`.  I've not looked at Go or any other arg parser from a modern development ecosystem.

---

_Comment by @joshtriplett on 2021-10-04 21:51_

> In contrast, I know a lot of my CLIs have logical ordering but I forget to set the display order to derive in them.

Wouldn't you notice this when reviewing the help (e.g. to polish, or to prepare screenshots or screencasts or manual pages)?

---

_Comment by @epage on 2021-10-04 23:55_

- Often, I overlook reviewing `--help`
- When I do screenshots, they generally don't have `--help`
- I've not yet done man pages.  I've been waiting for the mythical day when clap can code-gen the majority of it for me :)  Granted, git's behavior of overriding my command's `--help` with a manpage lookup might push me to write one before then.

---

_Comment by @kbknapp on 2021-10-05 11:13_

I've been thinking about this issue on and off for a while; about making DeriveDisplayOrder the default.

@joshtriplett makes a great point, but if I may make a counter point to get some people's opinions. When building a CLI, small or not, upon first running `--help` if you see the options are in the same order you declared them (WYSIWYG) I think that's decently unsurprising, even if it's not the desired behavior at the time. Assume `AppSettings` wasn't a thing for a moment, and after running `--help` you see your args are not sorted, but you'd like them sorted by default the solution would be to "quickly" sort them by hand. Even if that's a manual and less than pleasant experience it's at least easy to grok. 

Contrast that with our current behavior of sorting them by default; if that's what you wanted it's no big deal but if that's *not* the desired behavior there is no clear solution unless you already know about `AppSettings` and take a gamble to see if there is a setting that does what you want.

It's anecdotal but in my experience in the wild I've run into more people who either expect the WYSIWYG ordering and are somewhat surprised by the sorted args, or don't care either way.

One last comment about alphabetizing is that with `--[no-]flag` style flags, all the `--no-*` options are grouped together instead of with the option they relate to. Not too big an issue unless there are a significant number of flags. However, if we're able to ever add the "automatically add `--no-*` flags for CLIs", that could become a bigger issue because I suspect more people will end up using `--no-*` flags than currently do because they'd be easier to add.

So I'm leaning towards DeriveDisplayOrder should be the default, but I'm open to being swayed by more opinions and discussion.


---

_Comment by @epage on 2021-10-05 13:42_

I've updated the issue with a summary of the conversations and included my survey of different tools `--help`

For cases like `rg` (large CLI with sorting), almost seems like a custom comparator would be needed to handle the different exceptions (`--no-`, `--help`, `--version`) or alternative approaches (sort by short vs long)

---

_Comment by @kbknapp on 2021-10-05 14:11_

For completeness sake, it'd be possible for us to always sort `--no-*` flags next their positive value even if we maintained the current alphabetical sort *and* added an automatic `--no-*` flag generation option. So I don't want people to think that would be the sole reason for leaning away from alphabetical sort by default.

Unfortunately for us, it's difficult to tell if those tools (that use clap) that are using alphabetical sort currently are doing so because that's what they prefer, or simply because that's what clap v2 did by default. Likewise, it'd be interesting to see if any of those tools *also* have their declarations in alphabetical order or not. I'm not suggesting we hunt down that info, just musing on the subject :stuck_out_tongue_winking_eye: 

---

_Comment by @epage on 2021-10-05 15:16_

Yes, in the Issue, I intentionally referred to this as "Current Sorting Implementation" so we kept in mind that this was changeable.  I brought up the custom comparator because it felt like I was seeing a lot of different sort policies and I worry that it'd be hard to have "one true policy".  For example, I know `rg` intentionally sorts and I'd want to check in with burntsushi to see how any sorting changes or new features (custom comparator) would impact `rg`.    It'd also be good to get clap3 out to see if `help_heading` ends up having an impact on `rg` though I expect that to be a slow process.

---

_Referenced in [clap-rs/clap#2812](../../clap-rs/clap/issues/2812.md) on 2021-10-05 15:18_

---

_Referenced in [clap-rs/clap#2807](../../clap-rs/clap/issues/2807.md) on 2021-10-11 16:18_

---

_Label `E: breaking change` added by @epage on 2021-10-19 16:34_

---

_Comment by @epage on 2021-11-03 18:29_

Talking about discoverable paths forward (whether we support derive-only or just make derive the default), in addition to manually ordering items, people can also override the display order position.  In particular, we sort all items within the same display order position.

I was curious about providing "sort items of same display position" as a way of allowing sorting to discover we already do it :)

---

_Comment by @pksunkara on 2021-11-03 18:43_

FWIW, I am okay with changing the default for this in v4 if the user feedback we collect doesn't prefer keeping the current sorting.

---

_Comment by @epage on 2021-11-04 16:30_

I ended up on a rabbit trail and as part of it, was reading through https://github.com/clap-rs/clap/issues/1365.  Remembering this issue, I realized we can drop `IndexMap` from our dependencies by making `DeriveDisplayOrder` the only* option.

\* It will no longer be globally configured but sorting would still be available via:
- Manual sorting
- Assigning the same display order to everything
- Or, if its important enough, we can add `App::next_display_order(dsplay_order: Option<usize>)` where `None` is a default pool of sorted items
  - Can't be `App::display_order` because that is used for subcommands.  Maybe we should rename `App::help_heading` to `App::next_help_heading` to be consistent

We do this by
- Remove `DeriveDisplayOrder`
- Add a `App { ..., next_disp_order: usize }` (or `Option<usize>`) 
- In `App::arg`, assign `next_disp_order` to `Arg::disp_ord` and increment `next_disp_order`

By assigning `Arg::disp_ord` at the time of `App::arg`, we no longer care what order items are inserted into the arg map.  We can then switch away from `IndexMap` to `HashMap` or `BTreeMap`, dropping a depedency and removing the need for `App::_derive_display_order`.

---

_Referenced in [clap-rs/clap#1365](../../clap-rs/clap/issues/1365.md) on 2021-11-04 16:47_

---

_Comment by @pksunkara on 2021-11-05 07:23_

> Or, if its important enough, we can add App::next_display_order(dsplay_order: Option<usize>) where None is a default pool of sorted items

I am not sure why we need that if we are allowing them to assign the same display order value and then clap sorts them.

---

_Comment by @epage on 2021-11-05 13:22_

> I am not sure why we need that if we are allowing them to assign the same display order value and then clap sorts them.

I was exploring ideas to make it easier to disable the display order en-masse in case concern was raised over having to set it one-by-one, much like we have `App::help_heading` to make it easier to not set it on everything.

Whether to do this depends on
- How many people expect to sort
- How many args those sorting will have to be setting it on (e.g. `rg` has quite a bit)
- How comfortable we feel with the proposed API, balanced with the above two

A counter idea for this is to have them use #2976 to bulk set it.  #2976 doesn't work as well for `help_heading` because that is done in groups but would work quite well here since people will want it applied to all or have only one or two exceptions (e.g. help and version are generally no sorted)

EDIT:  the downside with bulk editing is that users of `clap_derive` would need to manually do the steps of `Parser`'s inherent methods to inject custom `App` logic like this.  If the case is small enough (users of `clap_derive` that also want sorting) than it is probably fine.

---

_Comment by @epage on 2021-11-05 13:33_

I was considering whether we can implement this now, and just key the `App::arg` logic off of `DeriveDisplayOrder` so we can drop `indexmap` as a dependency.

The reason we "can't" is ordering.  We can't guarantee `DeriveDIsplayOrder` will be set before calls to `App::arg`.  For example, `clap_derive` applies nearly all app methods *after* calls to `App::arg`.

"can't" is quoted because this is software, there is always something we could do.  For example, we could special case this setting like we special case `help_heading`.  I don't think going down that route is worth it though,

---

_Referenced in [clap-rs/clap#3002](../../clap-rs/clap/issues/3002.md) on 2021-11-08 14:14_

---

_Referenced in [epage/clapng#210](../../epage/clapng/issues/210.md) on 2021-12-06 22:14_

---

_Referenced in [epage/clapng#234](../../epage/clapng/issues/234.md) on 2021-12-06 22:24_

---

_Referenced in [clap-rs/clap#1807](../../clap-rs/clap/issues/1807.md) on 2021-12-13 19:29_

---

_Label `D: easy` removed by @epage on 2021-12-13 21:22_

---

_Label `S-waiting-on-design` added by @epage on 2021-12-13 21:22_

---

_Referenced in [clap-rs/clap#3096](../../clap-rs/clap/issues/3096.md) on 2021-12-14 21:59_

---

_Comment by @epage on 2022-02-08 01:23_

#3414 added `App::next_display_order` which can accept `None` to turn it off.

That just leaves (all for clap4)
- Making `DeriveDisplayOrder` the default, deprecating it
- Removing the `enum DisplayOrder` tracking we won't need anymore

---

_Referenced in [clap-rs/clap#2717](../../clap-rs/clap/issues/2717.md) on 2022-02-10 16:38_

---

_Referenced in [clap-rs/clap#3800](../../clap-rs/clap/pulls/3800.md) on 2022-06-08 17:08_

---

_Referenced in [clap-rs/clap#3975](../../clap-rs/clap/pulls/3975.md) on 2022-07-22 20:51_

---

_Closed by @epage on 2022-07-22 21:18_

---

_Referenced in [foundry-rs/foundry#3459](../../foundry-rs/foundry/pulls/3459.md) on 2022-10-05 16:54_

---

_Referenced in [hashintel/hash#1195](../../hashintel/hash/pulls/1195.md) on 2022-10-13 09:32_

---
