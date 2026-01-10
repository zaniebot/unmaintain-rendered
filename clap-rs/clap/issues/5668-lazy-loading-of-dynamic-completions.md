---
number: 5668
title: Lazy loading of dynamic completions
type: issue
state: open
author: alerque
labels:
  - A-completion
assignees: []
created_at: 2024-08-12T12:34:28Z
updated_at: 2024-12-12T19:34:29Z
url: https://github.com/clap-rs/clap/issues/5668
synced_at: 2026-01-10T01:28:15Z
---

# Lazy loading of dynamic completions

---

_Issue opened by @alerque on 2024-08-12 12:34_

I've been looking forward to dynamic completion support for a long time. I see ZSH support has landed as an unstable feature so I [started playing around with a POC on a small project](https://github.com/alerque/git-warp-time/compare/dynamic-completion) to get an idea how it all goes together.

So far I have...

1. Enabled `features = [ "unstable-dynamic" ]` for *clap_complete* in both my dependencies and build-dependencies.

1. Added a new subcommand to my CLI using the derive macros similar to the example in the docs:

    ```rust
    /// Used internally by Clap to generate dynamic completions
    #[command(subcommand)]
    pub _complete: Option<CompleteCommand>,
    ```

1. Edited my `main` function to intercept ARG[1] being 'complete' and handle dynamic completions:

    ```rust
    let cli = Cli::parse();
    if let Some(completions) = cli._complete {
        completions.complete(&mut Cli::command());
    } else {
        // my original app logic
    }
    ```

1. Swapped out the completer on one of my `Arg`s:

    ```diff
    /// Optional list of paths to operate on instead of default which is all files tracked by Git
    -#[arg(value_hint = clap::ValueHint::FilePath)]
    +#[arg(add = ArgValueCompleter::new(|| { vec![
    +        CompletionCandidate::new("foo"),
    +        CompletionCandidate::new("bar"),
    +        CompletionCandidate::new("baz")] }))]
    pub paths: Option<Vec<String>>,
    ```

This got me as far as being abe to use `myapp complete --shell zsh` to generate a shell completion function that I could dump to a file manually that later enable dynamic completions.

What I can't figure out is how to use `generate_to()` (or similar) to output the same completion file with dynamic support at build time:

https://github.com/clap-rs/clap/blob/2d8138a4716fe3b63ef32693211d4cd50531eb81/clap_complete/src/generator/mod.rs#L191

Am I missing some updated way to do that, or has it just not been implemented yet?

---

_Comment by @alerque on 2024-08-12 13:13_

(This was supposed to be opened as a discussion, not even sure how I got it open here without going through the bug template. Please feel free to migrate to discussions...)

---

_Comment by @epage on 2024-08-12 13:35_

The registration script is tightly coupled to the completion itself and so to encourage them being kept in lock-step, we've moved away from supporting any pre-generated completion scripts to [generating the registration function on shell load](https://docs.rs/clap_complete/4.5.14/clap_complete/dynamic/shells/enum.CompleteCommand.html).

---

_Label `A-completion` added by @epage on 2024-08-12 13:38_

---

_Comment by @alerque on 2024-08-12 13:46_

How do you expect that to work with distros? As an Arch Linux packager I know we package hundreds of CLI tools that ship with build-time generated completions. This enables the completions to work out of the box when the user installs a package.

The solution you are suggesting is troublesome for two reasons: first system packages can not/should not ever touch anything in a user's home directory, so we can't modify their shell RC files. For most shells there are some default RC file locations we could modify, but adding/removing lines is problematic. It is much easier (and hence how almost all shells expect us to do it) to drop a file in a directory to be used when completing any given command.

Second the solution you're suggesting would require *running* every CLI command that has completions every time a shell starts up. That's somewhat less than ideal for performance reasons.

Even if pre-generated completions are a thin wrapper around setting up the dynamic completion functions, we still need something pre-generated that we can drop in a directory to inform the shell that it shipped with completion support and how to activate it.

**Edit:**

> to encourage them being kept in lock-step

This concern is not relevant to distro packaging: the build-time generated completions installed by packaging will always be in lock step with the binary users will be running. Obviously to get *dynamic* completion the shell will be calling back to the binary anyway, but to bootstrap the process and enable completions at all distros still need something to install!

---

_Comment by @epage on 2024-08-12 15:15_

> This concern is not relevant to distro packaging: 

Remember that distros are not the only distribution method in the world.  A lot of times, people just download a binary from github or use third-party tools like rustup. I'm working to provide a default workflow that is resilient in less formal environments.

> How do you expect that to work with distros? As an Arch Linux packager I know we package hundreds of CLI tools that ship with build-time generated completions. This enables the completions to work out of the box when the user installs a package.

Either
- drop in your common location a file that does the `source`
- As you generally version both halves together, run the tool to generate the result and save that, with it clearly documented that this is coupling the two pieces together and could run into issues if the user shadows a distribution-provided command, e.g. `cargo` from distribution vs rustup. 
  - Yes, this runs into problems with cross-compilation (#5562).

If there are ways to improve our process for the "drop in the distros common location", all for improving things.  This would especially be good for us to work out before stabilizing this.

> same completion file with dynamic support at build time:

If you mean `build.rs`, this was already problematic.  It works well in some cases but in others, it requires contorting an application to gather all of the right types to make this work.  It also requires every command to opt-in to be of use to you (#5562).

---

_Comment by @alerque on 2024-08-12 15:56_

> Remember that distros are not the only distribution method in the world

Of course it is not, but it is a *major* one. I'm not suggesting that completions should be exclusively optimized for pre-generated distro packaging. I am suggesting that you not architect away this possibility because it really is the may way people get completions at all for much of their systems.

> A lot of times, people just download a binary from github or use third-party tools like rustup. I'm working to provide a default workflow that is resilient in less formal environments.

Sure. I'm fine with supporting that use case too, but people mostly do this with a few specialty or "pet" projects, not with their entire system. They do it with things that are not packaged or that update frequently and they use a lot. Almost everybody on any Linux system starts with a base set of packages and completions and customize from there.

> Either
> 
>     * drop in your common location a file that does the `source`

Again, this will cause the system to have to execute hundreds of CLI tools for every shell spawned on the system. The system I'm on now currently has 481 completions.

>     * As you generally version both halves together, run the tool to generate the result and save that, with it clearly documented that this is coupling the two pieces together and could run into issues if the user shadows a distribution-provided command, e.g. `cargo` from distribution vs rustup.
>       * Yes, this runs into problems with cross-compilation ([Generate shell completions as part of the build process to assist cross compilation #5562](https://github.com/clap-rs/clap/issues/5562)).

If user's shadow anything on provided by the system, there are always going to be issues. This isn't a reason not to provide completions (or man pages).

As @Flowdalic already pointed out, we don't always have access to run binaries on the architecture we're compiling for and generating packages that will be installed on, so this isn't viable for all packaging workflows.

> If there are ways to improve our process for the "drop in the distros common location", all for improving things. This would especially be good for us to work out before stabilizing this.

Why not make the build-time generated completions just a thin wrapper that bootstraps the dynamic completions? That's basically what the output of `myprogram completion` is now, with the exception that it includes some top level of static-ish completions with appropriate hooks for the dynamic bits. Either the build time generated completions definitions should continue to be as they are now with all the static parts generated, or they could be a much thinner wrapper around a purely dynamic completion system.

> If you mean `build.rs`, this was already problematic. It works well in some cases but in others, it requires contorting an application to gather all of the right types to make this work.

Yes, I get the pain of having to gather types at build time... but that wouldn't be necessary if the build time generated completion definition was just a bootstrapping system that didn't have to understand the apps argument handling at all.

> It also requires every command to opt-in to be of use to you (#5562).

I don't understand this comment at all. I have several apps with non-optional sub commands and am able to generate completions for them at build time just fine. I may be misunderstanding what you are saying here, and it may or may not be relevant to how to pre-generate completion definitions for apps that use dynamic completions.

---

_Comment by @epage on 2024-08-12 19:33_

> > drop in your common location a file that does the `source`

> Again, this will cause the system to have to execute hundreds of CLI tools for every shell spawned on the system. The system I'm on now currently has 481 completions.

In the short-term, that isn't a problem.  Not every one of those is written in Rust and not every one of those in Rust is using clap.  What would be our expected "market penetration" in a distribution like this 5 years down the line?  For how much we expect to have, what is the actual cost?

Note that I added a new registration scheme in #5671 which should allow faster registration.  There are possibilities for optimizing it further.

> If user's shadow anything on provided by the system, there are always going to be issues. This isn't a reason not to provide completions (or man pages).

"issues" is relative
- If someone shadows a packaged man-page, the harm is out of date or too-new information.
- If someone has version mismatches on completions, then they could potentially lose completion support.

>  That's basically what the output of myprogram completion is now, with the exception that it includes some top level of static-ish completions with appropriate hooks for the dynamic bits. Either the build time generated completions definitions should continue to be as they are now with all the static parts generated, or they could be a much thinner wrapper around a purely dynamic completion system.

I'm not too sure what you are referring to with "with the exception that" and its not clear to me what you are wanting from your suggestion here.



> > It also requires every command to opt-in to be of use to you (https://github.com/clap-rs/clap/issues/5562).

> I don't understand this comment at all. I have several apps with non-optional sub commands and am able to generate completions for them at build time just fine. I may be misunderstanding what you are saying here, and it may or may not be relevant to how to pre-generate completion definitions for apps that use dynamic completions.

That issue is about getting `clap` to help get build-time generated completions more universally adopted.  That implies there is a problem with them existing.   If a distribution expects all programs to include build-time completion generation, then you've likely already lost as most of them likely don't.  If you're fine patching it into every package, thats on you to deal with that pain.



---

_Comment by @alerque on 2024-08-12 21:03_

> > Again, this will cause the system to have to execute hundreds of CLI tools for every shell spawned on the system. The system I'm on now currently has 481 completions.
> 
> In the short-term, that isn't a problem. Not every one of those is written in Rust and not every one of those in Rust is using clap. What would be our expected "market penetration" in a distribution like this 5 years down the line? For how much we expect to have, what is the actual cost?

Hold my beer.

The system I'm using right now has 49 packages that

1. Are Rust CLI apps.
1. Use Clap
1. Package pre-generated completions...
1. For ZSH...

And I have just a fraction of the available Rust packages with pre-generated completions installed. This isn't my package testing machine, just one I use for daily work on book publishing.

Do you really think it would be okay to spawn 49 (or 10? or 2?) processes for random apps I happen to have installed every time I fire up a new shell? Even if it runs in a small number of milliseconds each? I grant that if a user wants to add something to their RC files that spawn processes on every new shell that's fine. I do it myself for `atuin`, `starfish`, and `zoxide`, but those all have a legitimate reason to run on every shell invocation (and even every command in the shell).

It would be totally fine for an end user to use that sort of mechanism to keep completions fresh to match their hand-installed tooling if they choose, but it's out of the question for a distro to start packaging files that couse *any* processes to run every time a shell starts up.

Given the up-tick in Rust based CLIs I don't think it's unreasonable to believe than *now*, never mind in the next 5 years, tens to hundreds of thousands of users have hundreds of thousands to millions of packages installed with Clap based Rust completions pre-generated. I don't know how fast those projects would be adapting to use dynamic completions, but **pushing the Clap tooling in a direction that actively makes it hard to package completion definitions in a way that works out of the box without end user modifications and without spawning a million processes on every new shell is crazy talk** in my book.

Maybe some other distro packagers can chime in on that, and hopefully we can work out something that is more condusive to making things work nicely out of the box for our end users. And I don't mean just one distro, this is a pretty universal issue across distros.

Note that not every distro has the cross-compilation issue where the completions need to be generated at build-time rather than using a run time phase after build to generate completions for packaging, but with the recent proliferation of architectures in active use that is a growing problem not a reducing one.

> Note that I added a new registration scheme in #5671 which should allow faster registration. There are possibilities for optimizing it further.

The streamlined registration avoiding the need for a dedicated subcommand that can interfere with the actual app is quite nice to see.

Don't think I'm against dynamic completions here, I'm very much for them. I just want to see the bootstrapping a starter definition for them to be usable in a shell out of the box be something distros can easily achieve. This means playing by the shell's own rules on where things should be and what they should look like, at least to get the ball rolling.

> > That's basically what the output of myprogram completion is now, with the exception that it includes some top level of static-ish completions with appropriate hooks for the dynamic bits. Either the build time generated completions definitions should continue to be as they are now with all the static parts generated, or they could be a much thinner wrapper around a purely dynamic completion system.
> 
> I'm not too sure what you are referring to with "with the exception that" and its not clear to me what you are wanting from your suggestion here.

Nevermind how I said it, I was in the weeds of how different shell definitions were looking and now can't even find what I was referencing.

What I was is this: **a way to pre-generate and package completion definitions for any given app and shell** such that distros can package them to just work out of the box with no user intervention and no nonsense about executing all the installed apps once each when spawning a new shell.

While distros can and will hack around the run-time vs. build time issue for generating these (by using a different architecture if need be to run the package after build but before package phases to generate the definitions), with the advent of dynamic completion support I would expect this to be ***much easier*** to support being generated at build time rather than run time because it no longer needs to have all the types and such available at build time. It basically just needs the `$0` for the app name and stick that in the boilerplate that then actually loads the dynamic completions.

> > > It also requires every command to opt-in to be of use to you (#5562).
> 
> > I don't understand this comment at all. I have several apps with non-optional sub commands and am able to generate completions for them at build time just fine. I may be misunderstanding what you are saying here, and it may or may not be relevant to how to pre-generate completion definitions for apps that use dynamic completions.
> 
> That issue is about getting `clap` to help get build-time generated completions more universally adopted. That implies there is a problem with them existing. If a distribution expects all programs to include build-time completion generation, then you've likely already lost as most of them likely don't. If you're fine patching it into every package, thats on you to deal with that pain.

The build-time vs. run-time issue is kind of a side problem, I think you're distracted by that. The *main* issue here is being able to pre-generate and package completion definitions at all. Having to be manually run post-build and pre-package is obnoxious but overcomable. The cross-compilation issue is a further needless encumbrance, that build-time generation fixes. The distros I've looked at all have hundreds of packages each already successfully include pre-generated definitions for Clap based Rust CLIs. Given that the initial definitions file is now even simpler and boiler-plate-ish than ever it seems counter productive to actively work away from that towards making users do stuff to their RC files that spawns processes on every shell just to get completion support.

---

_Comment by @Arian8j2 on 2024-08-16 07:24_

If I'm not mistaken, since the completion script is not changing, you can just run complete once to generate the completion and place it in the completion folders, Is there something I'm missing?

---

_Comment by @shannmu on 2024-08-16 07:29_

Yes, you can use `complete` subcommand to generate the completion script.

---

_Comment by @alerque on 2024-08-16 12:00_

> If I'm not mistaken, since the completion script is not changing, you can just run complete once to generate the completion and place it in the completion folders, Is there something I'm missing?

Yes, but this can only be done *after* a build and by running the generated binary. It used to be possible to generate the completions at build time (hence the command being referenced here) which makes cross compilation a lot easier (you don't have to have access to the target platform to generate completions or build twice, once just for a different architecture where you can execute the result.

The new completion definitions have *much less* tooling and would not need access to nearly as many internals of an app to generate completions at build time, but instead of getting easier to do it seems like the feature is being removed entirely.

---

_Comment by @epage on 2024-09-19 17:53_

@alerque I hope this break has been helpful for this to be a more fruitful discussion.

Let's start by stepping back and discussing the concern.  Fundamentally, your concern is with how the currently recommended way of registering completion scripts will scale as this gets adopted because it requires each command at shell initialization, even if it is able to cut out very early in the command's life cycle.

Is that right?

Your comments came across as framing this as a new problem.  From my perspective, this is an existing problem with our existing completion system.  Your proposed solution is to generate completions at build time.  We document both build time and runtime completion generation, giving users the choice in handling it.  How much people do either is likely biased towards runtime by the pain it is to generate build time completions in `build.rs` due to pulling in application source to do so.  If its runtime generated, then we're in not too different of a situation with the existing completion system as we are with the new one.

Is there something I'm missing about how this isn't a new problem?   You mentioned the anecdote of seeing hundreds of clap completion files in a distribution.  What methods are being used for these?  Are they natively supported upstream, patched, or something else?

When reading your comments, I feel you are failing to internalize the challenge we face in working with applications that are also installed outside of distributions. Dynamic completions cannot entirely live in Rust code as far as I've found so far.  We need a shim within the host shell to adapt to its specific needs.  There is a tight coupling between these two halves.  We cannot just punt on bumping our major version to deal with breakages as this has end-user impact and would require the application to declare an incompatibility and help the user through it.  Also,. we likely do not want to wait for major version bumps.  If we pave a path too smoothly so that application authors don't use approaches to keep these in lockstep, end-users installing through other means will be broken and them, the author, and clap maintainers will have a frustrating experience.

Is there anything about that situation that doesn't make sense or that it doesn't seem important enough?

**So we need a solution that balances distributions and other installation methods.**

Barring that, its a question of which side do we err on.  Frankly, I would err on other installation methods.  That is the case most likely to be wrong, its the status quo, and anything more would require some kind of opt-in by the application author anyways, making it less likely to happen.  Distributions have a lot of options for how they handle this, including
- taking the performance hit
- Running it in a packaging hook
- patching in the trivial `build.rs` (granted, I'm generally against distribution patches)
- using a lookup table from clap version to a registration template (since, as you said, its just substituting a couple of fields).

For now, I'm not leaving that open for discussion by posting a prompting question because I don't want to detract from finding a solution that can handle both needs.

---

_Comment by @epage on 2024-09-19 17:54_

@alerque So all of that was with the focus on "distribution scaling" problem.  Setting that aside and focusing on more literally the initial question of this issue "How can generate_to be configured to use dynamic completions?".  **You know the risks mentioned above and either are mitigating them or accepting them.**  Nothing is stopping you from calling [`EnvCompleter::write_registration`](https://docs.rs/clap_complete/latest/clap_complete/env/trait.EnvCompleter.html#tymethod.write_registration) on each shell you support (e.g. with [`Shells::builtins().iter()`](https://docs.rs/clap_complete/latest/clap_complete/env/struct.Shells.html)) to generate the registration scripts yourself in a static manner.

---

_Comment by @epage on 2024-09-19 17:56_

btw #5733 might make things more difficult for people to generate completions using the CLI/env-var and re-use that in other contexts.

---

_Comment by @epage on 2024-11-05 18:56_

btw saw some more on this topic from https://github.com/carapace-sh/carapace/blob/633eaba072313353d4a211b847049cc1693980ce/docs/src/carapace/gen.md

https://jzelinskie.com/posts/dont-recommend-sourcing-shell-completion/ talks more about this problem

https://github.com/rsteube/lazycomplete offers one way of solving the problem (which still calls a program and sources its output)

---

_Comment by @epage on 2024-11-05 18:58_

> So we need a solution that balances distributions and other installation methods.

One way of solving this is adding another layer of indirection.  I don't remember which completion library I saw do this but the idea is that the user is only `source`ing for convenience and its to access a completion function that then `source`s the version-specific logic.

---

_Referenced in [clap-rs/clap#3166](../../clap-rs/clap/issues/3166.md) on 2024-11-05 18:58_

---

_Renamed from "How can `generate_to` be configured to use dynamic completions?" to "Lazy loading of dynamic completions" by @epage on 2024-12-12 19:34_

---

_Referenced in [clap-rs/clap#5840](../../clap-rs/clap/issues/5840.md) on 2024-12-12 19:37_

---

_Referenced in [fish-shell/fish-shell#11046](../../fish-shell/fish-shell/pulls/11046.md) on 2025-01-16 00:16_

---

_Referenced in [rust-lang/cargo#14520](../../rust-lang/cargo/issues/14520.md) on 2025-06-26 17:51_

---

_Referenced in [clap-rs/clap#5841](../../clap-rs/clap/pulls/5841.md) on 2025-10-27 21:08_

---
