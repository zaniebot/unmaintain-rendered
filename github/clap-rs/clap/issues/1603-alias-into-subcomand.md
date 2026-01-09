---
number: 1603
title: Alias into subcomand
type: issue
state: open
author: CAD97
labels:
  - C-enhancement
  - E-help-wanted
assignees: []
created_at: 2019-12-09T02:51:05Z
updated_at: 2024-10-22T18:26:14Z
url: https://github.com/clap-rs/clap/issues/1603
synced_at: 2026-01-07T13:12:19-06:00
---

# Alias into subcomand

---

_Issue opened by @CAD97 on 2019-12-09 02:51_

As far as I can tell, it's currently possible to alias command arguments and subcommands. For things such as https://github.com/rust-lang/rustup/issues/2148#issue-534509269, it'd be really nice if it's somehow possible to alias both "across" and "into" subcommands, e.g. treat `rustup install` the same as `rustup toolchain install`, i.e. including all arguments and such configured for the target subcommand.

---

_Referenced in [rust-lang/rustup#2148](../../rust-lang/rustup/issues/2148.md) on 2019-12-09 07:34_

---

_Referenced in [clap-rs/clap#1606](../../clap-rs/clap/issues/1606.md) on 2019-12-26 13:02_

---

_Comment by @Stupremee on 2020-01-01 18:09_

I would like to work on this issue 

---

_Comment by @Dylan-DPC-zz on 2020-01-01 18:24_

@Stupremee sure. you can go ahead and submit a PR. If you have any questions, you can post them here :)

---

_Label `C: alias` added by @CreepySkeleton on 2020-02-01 11:12_

---

_Label `C: subcommands` added by @CreepySkeleton on 2020-02-01 11:12_

---

_Label `help wanted` added by @CreepySkeleton on 2020-02-01 11:12_

---

_Label `P2: need to have` added by @CreepySkeleton on 2020-02-01 11:12_

---

_Label `T: enhancement` added by @CreepySkeleton on 2020-02-01 11:12_

---

_Added to milestone `3.0` by @CreepySkeleton on 2020-02-01 11:12_

---

_Comment by @pksunkara on 2020-02-01 22:13_

I have come up with 2 ways to implement this:

1. Have a new subcommand type thing whose job is just replacing things.

   ```rust
   App::new("rustup")
       .replacer("install", "toolchain install")

   // Or alternatively
   App::new("rustup")
       .subcommand(SubCommand::with_name("install")
           .replace_with("toolchain install")
       )
   ```

   **PROS**: Can also use flags and options in the replace target string
   **CONS**: Parsing logic might become a bit more complex?

2. Have a `alias_up` and `visible_alias_up` which takes number argument on how many levels it has go up. 

   ```rust
   App::new("rustup")
       .subcommand(SubCommand::with_name("toolchain")
           .subcommand(SubCommand::with_name("install")
               .visible_alias_up(2, "install")
           )
       )
   ```

   **PROS**: Works like `alias` api
   **CONS**: Sometimes might not know how many levels it has go up. Also, what about going down?

---

_Comment by @kinnison on 2020-02-01 22:21_

An idea I had was:

```rust
App::new("rustup")
    .mirror("install", "toolchain-install")
    ...
    .subcommand(SubCommand::with_name("toolchain")
        .subcommand(SubCommand::with_name("install")
            .as_mirror("toolchain-install")
            ...
        )
    )
)
```

Where the `.mirror()` and `.as_mirror()` names would have to match up by the time you called .get_matches() or somesuch perhaps?

---

_Comment by @CreepySkeleton on 2020-02-03 12:41_

I like `visible_alias_up` but `mirror` is more general.

Again, I have yet to familiarize myself with clap internals (except for some) so I can't tell which way would be easier to implement. Let's postpone it until we dealt with clap_derive (I'll get to it in ~30 mins)

---

_Comment by @Dylan-DPC-zz on 2020-02-03 14:08_

I was thinking more of `.mirror("toolchain")` in app and then you can add `.delegate()` to each subcommand you want to send to toolchain (I'm assuming they'll be multiple at some point)

---

_Comment by @kinnison on 2020-02-03 14:22_

Are you imagining `.mirror("label")` sets up a subcommand which can be referenced and then `.delegate("name", "label")` is like `.subcommand(with_name("name").....)` matching the subcommand which had the `.mirror()` on it?

---

_Comment by @Dylan-DPC-zz on 2020-02-03 14:49_

Yeah I guess you got it 

---

_Comment by @pksunkara on 2020-02-10 22:43_

So, I was looking to implement this, and as of now, I can simply do delegation to sibling commands. You can't delegate up. Is that okay?

```rust
App::new("rustup")
    .subcommand(App::new("install")
        .delegate(["toolchain", "install"]))
    .subcommand(App::new("toolchain")
        .subcommand(App::new("install")))
```

---

_Comment by @kinnison on 2020-02-11 17:16_

That would be sufficient for `rustup`'s use case, if others in `clap` are OK with that as a limitation.

---

_Comment by @pksunkara on 2020-02-11 17:21_

Actually, we can do something like this:

```rust
App::new("rustup")
    .delegate(["install"], ["toolchain", "install"])
    .subcommand(App::new("install"))
    .subcommand(App::new("toolchain")
        .subcommand(App::new("install")))
```

Which would be the same thing, but it actually allows us to delegate up too. Would require more checks though. 

```rust
// These runs `rustup install` logic if you run `rustup toolchain install`
App::new("rustup")
    .delegate(["toolchain", "install"], ["install"])
    .subcommand(App::new("install"))
    .subcommand(App::new("toolchain")
        .subcommand(App::new("install")))
```

---

_Comment by @Dylan-DPC-zz on 2020-02-11 17:26_

Looks good to me. I don't see much of a usecase of delegating upwords. 

---

_Comment by @kinnison on 2020-02-11 17:31_

From `rustup`'s perspective the **canonical** form is `rustup toolchain install` and we want to support *silently* `rustup install` as an alias for those who have it stuck in scripting etc.  We don't want it showing in help etc, but simply to be internally converted by clap.

---

_Comment by @CreepySkeleton on 2020-02-11 18:30_

I would like propagating things only *down* (this delegation is a form of propagation too), like it has been for all this time; global args and global settings has been propagating down. It's being done in one, up-bottom propagation step. Making an exception for `.delegate` would essentially add another, bottom-up propagation step which may affect performance for other cases. It's hard to say for sure without concrete numbers, but I would still like to keep things simple.

@kinnison You're able to hide subcommands from help messages via `AppSettings::Hidden`.

---

_Comment by @kinnison on 2020-02-11 19:01_

I'm aware of `Hidden` yes -- my point was that whatever the scheme, I'd need to be able to do that.  That's all :D

---

_Comment by @pksunkara on 2020-02-14 02:39_

I have been looking at this the past few days and I have realized that doing this requires large changes in parsing logic. I would like to instead throw an alternative.

```rust
let install = App::new("install"); // The install command

App::new("rustup")
	.subcommand(App::new("toolchain")
		.subcommand(install.clone()))
	.subcommand(install.settings(AppSettings::Hidden))
```

Doesn't this basically solve your use case? Also, this is more correct since if we are just aliasing to a different path, then it might have issues with args on parent subcommands (`toolchain` args here).

---

_Comment by @CAD97 on 2020-02-14 02:43_

@pksunkara the issue is that, while the arguments are correctly parsed in this case, the actual code still has to handle the fact that there are two entry points to the functionality. The advantage of alias over just mounting the same `App` at two places is being able to handle it in one place and have it automatically apply to the other, aliased location.

In short, we want to let the user say `rustup install <args>` and have that automatically expanded to `rustup toolchain install <args>`, replacing "`install`" with "`toolchain install`". The lack of arguments between `toolchain` and `install` is not an issue; the alias is, in effect, an alias to the zero argument (before the subcommand) form.

---

_Comment by @pksunkara on 2020-02-14 02:46_

> the actual code still has to handle the fact that there are two entry points to the functionality

Could you explain why you do not want this?

> The advantage of alias over just mounting the same `App` at two places is being able to handle it in one place and have it automatically apply to the other, aliased location.

You are still writing just one `install` App. All you are doing is adding 2 lines to your root App.

---

_Comment by @CAD97 on 2020-02-14 02:56_

The issue comes after doing `get_matches`. Once you have done the matching, you need to test to see if `subcommand_matches("install")` and then somehow jump into the middle of your code handling `subcommand_matches("toolchain")` to where it handles `subcommand_matches("install")`.

The point of having the alias at the `App` layer is enforcing, at the argument parsing level, that the two are identical functionality, rather than two distinct endpoints that happen to have the same exact syntax.

Or are you saying that if I use your code, then `matches.subcommand_matches("install")` and `mathces.subcommand_matches("toolchain").subcommand_matches("install")` are actually equivalent? We want the processing code to not know (because it doesn't care and _shouldn't_ know) that the alias was used. It _should not be possible_ for the behavior to diverge; that's the exact issue we have with `rustup install` currently that led to the maintenance team wanting to remove it, because they couldn't just say "treat it as if the user invoked the canonical command, `rustup toolchain install`".

---

_Comment by @pksunkara on 2020-02-14 03:01_

My bad, I was thinking of a different architecture for the CLI.

I was thinking `App`s have `run()` and when you get matches, you just iterate over them to get the `App` and then just `run()` on it. (clap_derive/structopt makes this even easier).

---

_Referenced in [clap-rs/clap#1697](../../clap-rs/clap/pulls/1697.md) on 2020-02-17 09:21_

---

_Comment by @pksunkara on 2020-02-17 09:23_

@CAD97 I have the PR ready, please give it a look.

---

_Closed by @bors[bot] on 2020-02-21 19:42_

---

_Referenced in [clap-rs/clap#2011](../../clap-rs/clap/issues/2011.md) on 2021-10-09 10:45_

---

_Referenced in [clap-rs/clap#2836](../../clap-rs/clap/issues/2836.md) on 2021-10-09 15:42_

---

_Referenced in [epage/clapng#158](../../epage/clapng/issues/158.md) on 2021-12-06 20:16_

---

_Comment by @epage on 2023-03-23 20:50_

Cross posting from #2836 for greater visibility

> Due to the lack of interest on this tracking issue, I'm considering removing this unstable feature.
> 
> Any concerns people want to raise?

---

_Reopened by @epage on 2024-10-22 18:26_

---
