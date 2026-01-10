---
number: 5840
title: Dynamic completion for Nushell
type: issue
state: open
author: chklauser
labels:
  - C-enhancement
  - A-completion
assignees: []
created_at: 2024-12-11T23:23:28Z
updated_at: 2025-10-24T00:39:59Z
url: https://github.com/clap-rs/clap/issues/5840
synced_at: 2026-01-10T01:28:17Z
---

# Dynamic completion for Nushell

---

_Issue opened by @chklauser on 2024-12-11 23:23_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

master

### Describe your use case

Bash, zsh, fish, etc. have very cool support for dynamic completion. Dynamic completion is a huge boost to the usability of a tool.

The `clap_complete_nushell` crate doesn't support dynamic completion yet. While the "external" completion story in Nushell is still a bit shaky, it's nonetheless possible for Nushell to get completions from external programs. We just need to implement it.

### Describe the solution you'd like

The `source <(COMPLETE=nushell some-clap-binary --)` pattern fundamentally won't work in Nushell. Nushell requires all code to be statically available in the file system _before_ configuration gets loaded. This makes it a bit tricky to make installation of the dynamic completion support both convenient _and_ "auto-updating" (in case a new version of a clap-based CLI gets installed).

Additionally, and unlike every other shell that we support, Nushell only supports a single, globally registered "external completer". For users, who want to have more than one external completer, the suggestion is to have a "meta-completer" that looks at the command line (first arg, which is the binary name) and dispatches the completion request to an appropriate completer.

A solution could look like this:
(1) The user asks the clap-based CLI tool to generate a "completion generator" source file
```nushell
COMPLETE=nushell some-clap-tool -- | save --raw ~/.config/nushell/generate-some-clap-tool-completions.nu
```

This "completion generator" needs to be as stable and minimal as possible. It could look something like this:
```nushell
COMPLETE=nushell COMPLETE_nu=module some-clap-tool -- | save --raw ($env.NU_LIB_DIRS.0 | path join some-clap-tool-completer.nu)
```
Each time `env.nu` is loaded, this writes an updated version of the _actual_ completion hook. The additional `COMPLETE_nu=module` environment variable signals that the clap-based tool should generate the completion hook and not the "completion generator" file.

(2) The user (manually) includes this source file in their **env.nu**
```nushell
# env.nu
source ./generate-some-clap-tool-completions.nu
```

(3) The user (manually) uses the regularly re-generated module in their **config.nu**
```nushell
# config.nu
use some-clap-tool-completer
# and then EITHER
some-clap-tool-completer install
# OR
$env.config.completions.external.completer = { |spans| 
  if (some-clap-tool-completer handles $spans) {
    some-clap-tool-completer complete $spans
  } else {
    # other completers that the user wants to dispatch to
  }
}
```

### Alternatives, if applicable

**Autoload Directories**
A hidden feature of Nushell. It will be officially documented in version 0.101. Files in those autoload directories get loaded _after_ `config.nu`. They sound like a good target for completions _except_ for the issue that there can only be one global completer. If we just plonk the clap-generated file into an autoload directory, we take away the user's control over how external completion works in their shell.

There are some [upcoming changes to Nushell configuration](https://www.nushell.sh/blog/2024-12-04-configuration_preview.html). 

### Additional Context

Nushell docs
 - [External Completions](https://www.nushell.sh/book/custom_completions.html#external-completions)
 - [Suggestion for the case of multiple completers](https://www.nushell.sh/cookbook/external_completers.html#multiple-completer)
 - [Configuration (env.nu vs config.nu)](https://www.nushell.sh/book/configuration.html#nushell-configuration-with-env-nu-and-config-nu)

---

_Label `C-enhancement` added by @chklauser on 2024-12-11 23:23_

---

_Referenced in [clap-rs/clap#5841](../../clap-rs/clap/pulls/5841.md) on 2024-12-11 23:31_

---

_Label `A-completion` added by @epage on 2024-12-12 19:32_

---

_Referenced in [clap-rs/clap#3166](../../clap-rs/clap/issues/3166.md) on 2024-12-12 19:33_

---

_Comment by @epage on 2024-12-12 19:37_

> Nushell requires all code to be statically available in the file system before configuration gets loaded. This makes it a bit tricky to make installation of the dynamic completion support both convenient and "auto-updating" (in case a new version of a clap-based CLI gets installed).

In https://github.com/clap-rs/clap/issues/5668#issuecomment-2457934682  there is discussion of lazy loading.  Unsure if that would be sufficient for nushell.

> Additionally, and unlike every other shell that we support, Nushell only supports a single, globally registered "external completer". For users, who want to have more than one external completer, the suggestion is to have a "meta-completer" that looks at the command line (first arg, which is the binary name) and dispatches the completion request to an appropriate completer.

Ouch, this sounds like a big limitation.

If I'm understanding correctly, we basically require users to hook us into their hand-written meta-completer?  Not too ideal.  Is there any talk about fixing this so custom completers can better coordinate and have a good out-of-the-box experience?

---

_Comment by @Hofer-Julian on 2024-12-26 14:27_

Thanks for looking into this @chklauser!

In your PR you added `install`, which
> `globally registers a completer that falls back to whatever completer was previously installed if handles rejects completing a command line.

I think that is a good way of working around nushell's current limitations.

> There are some [upcoming changes to Nushell configuration](https://www.nushell.sh/blog/2024-12-04-configuration_preview.html).

Also, this is nowadays released. Users now only have things in their config that they explicitly changed, without all the defaults as it was before.

> Autoload Directories
A hidden feature of Nushell. It will be officially documented in version 0.10

This is now official documented: https://www.nushell.sh/book/configuration.html#configuration-overview
Looking at the docs `($nu.data-dir)/vendor/autoloads` would be a good place to add an autoloading script: https://www.nushell.sh/book/configuration.html#startup-variables

Personally, I think it's fine if the first iteration of dynamic completion support for nushell cannot conveniently auto-update.

Ideally, one only needs to run a command like this to set up completions or to update them:

```nushell
COMPLETE=nushell some-clap-tool -- | save --raw `($nu.data-dir)/vendor/autoloads/generate-some-clap-tool-completions.nu
```

That script would set up the completion as well as wrap existing completers similar to how `install` in https://github.com/clap-rs/clap/pull/5841 works



---

_Comment by @chklauser on 2024-12-27 14:33_

There is a trade off here. Nushell differentiates between "modules" and "scripts". That means a file either exports definitions (when used together with `use`) _or_ it executes code (when `source`ed). The autoloads directory is the latter kind. If this is the only script we generate, we can essentially _only_ offer the "install" version (not the "handles" + "complete" version). We cannot generate a file that can be used as a module and a script at the same time. 

Users who would like to write their own meta-completer will not be able to use the script we generate directly. That's probably not the end of the world, though. Because nushell and clap both understand JSON, the integration is probably very stable.

The [carapace-bin completer](https://github.com/carapace-sh/carapace-bin/blob/9b919426388978591e5dd9815ad30ddd8180edce/cmd/carapace/cmd/lazyinit/nushell.go#L35) makes the same trade off. With one (IMO weird) difference: their completer only installs itself _if there is no existing completer installed_. I think, as a user, I would prefer the chaining approach in #5841. 

---

_Comment by @Hofer-Julian on 2024-12-27 16:13_

> The [carapace-bin completer](https://github.com/carapace-sh/carapace-bin/blob/9b919426388978591e5dd9815ad30ddd8180edce/cmd/carapace/cmd/lazyinit/nushell.go#L35) makes the same trade off. With one (IMO weird) difference: their completer only installs itself if there is no existing completer installed. I think, as a user, I would prefer the chaining approach in https://github.com/clap-rs/clap/pull/5841.

Agreed, I was considering linking to carapace's approach in my reply, but its behaviour doesn't make too much sense IMO.

@epage is there anything else you'd like to discuss before moving on to the actual implementation?

---

_Comment by @epage on 2024-12-30 17:05_

Could you clarify for me how a user who has two binaries using this feature would register both of them?

Also, should we block on lazy loading (#5668)?

---

_Comment by @Hofer-Julian on 2024-12-30 17:09_

> Could you clarify for me how a user who has two binaries using this feature would register both of them?

You mean two different applications, right?
That should just work, since the first one wraps the original completer, and the second one wraps that again. If the binary name matches, the completer will be used, if not the next one will be tried and so on.

> Also, should we block on lazy loading (#5668)?

As useful as that would be, I don't see a reason to block on it.


---

_Closed by @chklauser on 2025-01-05 09:47_

---

_Reopened by @chklauser on 2025-01-05 09:47_

---

_Comment by @chklauser on 2025-01-05 09:51_

One more idea from the nushell discord: combine [custom completers (for a single external command decl)](https://www.nushell.sh/book/custom_completions.html) with [rest parameters](https://www.nushell.sh/book/custom_commands.html#rest-parameters)

```
def "nu-complete foo" [ctx: string, pos: int] { }
def foo [...rest: any@"nu-complete foo"] { }
```

I initially discounted custom completions because I didn't want nushell to handle the completion of `--args` since clap has more detailed knowledge about what is permitted in a particular command line. But if we can let the clap completer handle the entire command line, that's not an issue.

Custom completions on an external command has the advantage that we don't mess with globally installed external completers.

I have not tested it though. This approach is not documented explicitly.

---

_Comment by @chklauser on 2025-01-05 09:55_

Note for the implementation: we need to check that we handle `some pipeline | some-clap-tool arg1` correctly. Nushell includes the `|` as a first argument. I suppose some tools might exhibit different behavior as part of a pipeline and thus be interested in this distinction. For clap, we just need to filter that out.

---

_Comment by @Hofer-Julian on 2025-01-05 10:17_

> One more idea from the nushell discord: combine [custom completers (for a single external command decl)](https://www.nushell.sh/book/custom_completions.html) with [rest parameters](https://www.nushell.sh/book/custom_commands.html#rest-parameters)

Oh, that is interesting. This approach would indeed be pretty clean, assuming that it works as expected

---

_Comment by @chklauser on 2025-01-07 22:53_

(@Hofer-Julian if you are itching to tackle this issue, please feel free. I won't find time in the next couple of weeks myself)

---

_Comment by @Hofer-Julian on 2025-01-07 23:19_

> (@Hofer-Julian if you are itching to tackle this issue, please feel free. I won't find time in the next couple of weeks myself)

Thanks for the notice!
Might give it a go this weekend.

---

_Comment by @Hofer-Julian on 2025-01-07 23:20_

@chklauser could you please link your jj fork where you implemented dynamic completions for nushell?

---

_Comment by @chklauser on 2025-01-08 14:55_

@Hofer-Julian that would be https://github.com/jj-vcs/jj/compare/main...chklauser:jj:push-rppypvvytuqv

---

_Referenced in [nushell/nushell#14806](../../nushell/nushell/issues/14806.md) on 2025-01-11 18:28_

---

_Comment by @Hofer-Julian on 2025-01-11 18:33_

> One more idea from the nushell discord: combine [custom completers (for a single external command decl)](https://www.nushell.sh/book/custom_completions.html) with [rest parameters](https://www.nushell.sh/book/custom_commands.html#rest-parameters)
> 
> ```
> def "nu-complete foo" [ctx: string, pos: int] { }
> def foo [...rest: any@"nu-complete foo"] { }
> ```

I've tried out this approach, but `nushell` needs to adapt to allow this workflow, I've opened an issue for that: https://github.com/nushell/nushell/issues/14806

For now, I consider the implementation in https://github.com/clap-rs/clap/pull/5841 as the best solution.
I tested it locally and everything works fine.
Also, the usage is the same as with the non-dynamic completions: people add the generation of the completion in `env.nu` and call it then in `config.nu`.




---

_Comment by @epage on 2025-01-13 17:13_

> > Could you clarify for me how a user who has two binaries using this feature would register both of them?
> 
> You mean two different applications, right? That should just work, since the first one wraps the original completer, and the second one wraps that again. If the binary name matches, the completer will be used, if not the next one will be tried and so on.

I'm referring to the following from the Issue

> Additionally, and unlike every other shell that we support, Nushell only supports a single, globally registered "external completer". For users, who want to have more than one external completer, the suggestion is to have a "meta-completer" that looks at the command line (first arg, which is the binary name) and dispatches the completion request to an appropriate completer.

---

> > Also, should we block on lazy loading (#5668)?
> 
> As useful as that would be, I don't see a reason to block on it.

I'm referring to the following from the Issue

> The source <(COMPLETE=nushell some-clap-binary --) pattern fundamentally won't work in Nushell. Nushell requires all code to be statically available in the file system before configuration gets loaded. This makes it a bit tricky to make installation of the dynamic completion support both convenient and "auto-updating" (in case a new version of a clap-based CLI gets installed).

Auto-updating is a hard requirement imo.

---

_Comment by @chklauser on 2025-01-13 18:15_

> Auto-updating is a hard requirement imo.

Shouldn't that be the responsibility of the distribution/packager? I understand that auto update is convenient for tools that are `cargo install`ed but that distribution method should be the exception, not the norm.

---

_Comment by @Hofer-Julian on 2025-01-13 18:17_

> > Auto-updating is a hard requirement imo.
> 
> Shouldn't that be the responsibility of the distribution/packager? I understand that auto update is convenient for tools that are `cargo install`ed but that distribution method should be the exception, not the norm.

I tend to agree, but I think it's not that relevant for this discussion. Auto-updating will work exactly the same as it did with static completions. 🙂

---

_Comment by @epage on 2025-01-13 18:22_

That is a discussion for #5668.  Unless the direction of that is changed, auto-updating is a requirement.

---

_Comment by @Hofer-Julian on 2025-01-15 16:17_

> auto-updating is a requirement

@epage I will try my best to summarize the situation, in order to make sure we talk about the same thing.

What I mean when I talk about auto-updating of nushell completions with clap-complete as is (no dynamic completions), is the following:
With `rattler-build` it works like this: 

Add the following `env.nu`:
```nushell
mkdir ~/.cache/rattler-build
rattler-build completion --shell nushell | save -f ~/.cache/rattler-build/completions.nu
```

Add the following to `config.nu`
```nushell
use ~/.cache/rattler-build/completions.nu *
```

During shell start, `env.nu` will be called first and generates the completions, then `config.nu` is called and imports them.
The split is necessary since nushell needs all included code available during parsing time. I assume that's what @chklauser meant with:

> The source <(COMPLETE=nushell some-clap-binary --) pattern fundamentally won't work in Nushell. Nushell requires all code to be statically available in the file system before configuration gets loaded. 

The result is the same as with other shells: if you update your CLI programs, your completions will also be updated automatically.

Now back to dynamic completions and https://github.com/clap-rs/clap/pull/5841:

That will work exactly the same. You generate completions in `env.nu` and import them in `config.nu`.



---

_Comment by @chklauser on 2025-10-12 07:51_

I have updated the implementation in the PR to do auto-updating. It still requires more machinery than other shells, but from the user's perspective, it's fairly transparent. I have had a variant of this active for months and the overhead of updating the completions every time a shell is started was not noticable.

Here is what installing completions for jj looks like in my fork:

The user must run this
```nushell
COMPLETE=nushell jj | save --append --raw $nu.env-path
```
(or manually insert the generated code into their env.nu)

This appends the following snippet in `env.nu`:
```nushell
# Refresh completer integration for jj (must be in env.nu)
do {
  # Search for existing script to avoid duplicates in case autoload dirs change
  let completer_script_name = 'jj-completer.nu'
  let autoload_dir = $nu.user-autoload-dirs
    | where { path join $completer_script_name | path exists }
    | get 0 --optional
    | default ($nu.user-autoload-dirs | get 0 --optional)
  mkdir $autoload_dir

  let completer_path = ($autoload_dir | path join $completer_script_name)
  COMPLETE=nushell _COMPLETE__mode=integration ^r#'/home/chris/.cargo/bin/jj'# | save --raw --force $completer_path
}
```

Because we are using autoload directories, the user doesn't need to modify their `config.nu`.

The auto-loaded-script looks like this:
```nushell

# Determines whether the completer for jj is supposed to handle the command line
def handles [
    spans: list # The spans that were passed to the external completer closure
]: nothing -> bool {
    ($spans | get --optional 0) == r#'jj'#
}

# Performs the completion for jj
def complete [
    spans: list # The spans that were passed to the external completer closure
]: nothing -> list {
    COMPLETE=nushell ^r#'/home/chris/.cargo/bin/jj'# -- ...$spans | from json
}

# Installs this module as an external completer for jj globally.
#
# For commands other jj, it will fall back to whatever external completer
# was defined previously (if any).
$env.config = $env.config
  | upsert completions.external.enable true
  | upsert completions.external.completer { |original_config|
      let previous_completer = $original_config
        | get --optional completions.external.completer
        | default { |spans| null }
      { |spans|
        if (handles $spans) {
            complete $spans
        } else {
            do $previous_completer $spans
        }
      }
  }
```

(more or less the same code as before but now intended to be sourced/loaded directly as opposed to as a module).

### Notes
1. The `mkdir $autoload_dir` is necessary because the default autoload directory on a fresh nushell installation won't exist
2. I feel the check for an existing version of the `xxx-completer.nu` script is important because having multiple copies loaded (and the wrong one auto-updated) would be a disaster. The user can configure multiple autoload directories and/or change the order of autoload directories at any time.
3. Whether it's smart to write the full path to the CLI binary into env.nu, I don't know. I figured users might intentionally have a different PATH in interactive vs non-interactive mode (and env.nu is always sourced). Opinions?
4. The auto-update snippet (part that goes into `env.nu`) should be fairly stable as long as completions can be installed from auto-loaded scripts.

---

_Comment by @Hofer-Julian on 2025-10-15 15:23_

@chklauser nushell just added support for command-wide completion handlers. I think it makes sense to start using that in your PR. Do you agree?

https://www.nushell.sh/blog/2025-10-15-nushell_v0_108_0.html#command-wide-completion-handler-16765-toc

---

_Comment by @chklauser on 2025-10-15 15:49_

I'd say so,yes. With command-wide completion handlers, the  completions are scoped to just the jj command. Question is if this changes anything about the solution as a whole. 

Like: if the integration becomes a simple enough "one-liner", then maybe auto-updating won't be necessary?
```nushell
# using the tool `jj` as an example (UNTESTED)
def jj-completer [spans: list<string>] {
    COMPLETE=nushell ^jj ...$spans
}
@complete jj-completer
def --wrapped jj [...args] {
  ^jj ...$args
}
```

_Theoretically_, it should never be necessary to change this incantation. Everything else is either handled by nushell or the completion integration in Clap. 

---

_Comment by @Hofer-Julian on 2025-10-15 15:53_

Yeah, agreed.

Let me know if you can need help with progressing this. With the latest nushell release it should be doable to get this into a mergable state 

---

_Comment by @chklauser on 2025-10-20 21:52_

PR is updated. Works fine 👍 

```nushell
# Performs the completion for jj
def jj-completer [
    spans: list<string> # The spans that were passed to the external completer closure
]: nothing -> list {
    COMPLETE=nushell ^r#'/home/chris/.cargo/bin/jj'# -- ...$spans | from json
}

@complete jj-completer
def --wrapped jj [...args] {
  ^r#'/home/chris/.cargo/bin/jj'# ...$args
}
```

The auto-update bit feels pretty silly now (it's more complex than the actual integration).

Tests are still an open topic. I've looked into the existing setup a bit. It doesn't seem straightforward. At least not using `completest`. Unless I'm misinterpreting things, that library is built against a pretty old version of nushell (0.80-something). It seems to work by pretending that the user writes code that loads each completer as a module (I think?). That doesn't feel representative of how nushell works/works today.

If we drop the whole auto-update thing, then it becomes more realistic again (because then, the user would _actually_ load the completer file as a module), but if we keep the auto-update, then we either get completest updated or ignore it entirely.

---

_Comment by @Hofer-Julian on 2025-10-21 09:01_

> The auto-update bit feels pretty silly now (it's more complex than the actual integration).

What does auto-update even do now? Isn't everything dynamic then in the binary?

---

_Comment by @chklauser on 2025-10-23 13:33_

It writes the actual registration snippet
```
# Performs the completion for jj
def jj-completer [
    spans: list<string> # The spans that were passed to the external completer closure
]: nothing -> list {
    COMPLETE=nushell ^r#'/home/chris/.cargo/bin/jj'# -- ...$spans | from json
}

@complete jj-completer
def --wrapped jj [...args] {
  ^r#'/home/chris/.cargo/bin/jj'# ...$args
}
```

The other shells will source the registration snippet straight from the binary. We cannot do that without the auto-update part. My gut feeling is, that because nushell's way of registering external completers for commands is so straightforward/standardized, we don't need the auto-update part. I'm confident that user's won't have to change this registration.

---

_Comment by @Hofer-Julian on 2025-10-23 13:37_

Thanks for the explanation.

> My gut feeling is, that because nushell's way of registering external completers for commands is so straightforward/standardized, we don't need the auto-update part. I'm confident that user's won't have to change this registration.

I agree that, that there's no need for the auto-update part then.

Maybe you could remove it and then ping @epage again for a review?

---

_Comment by @epage on 2025-10-24 00:39_

> The other shells will source the registration snippet straight from the binary. We cannot do that without the auto-update part. My gut feeling is, that because nushell's way of registering external completers for commands is so straightforward/standardized, we don't need the auto-update part. I'm confident that user's won't have to change this registration.

I think I'm missing something here as to why the shell matters.  Auto-updating is a concern because the interface between the completer and the binary is not stable.  Update the application and the completer may be broken.

---

_Referenced in [typst/typst#7447](../../typst/typst/issues/7447.md) on 2025-11-24 08:56_

---
