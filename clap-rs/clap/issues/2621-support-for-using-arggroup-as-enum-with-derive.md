---
number: 2621
title: Support for using ArgGroup as Enum with derive
type: issue
state: open
author: ModProg
labels:
  - C-enhancement
  - E-hard
  - E-help-wanted
  - A-derive
  - ":money_with_wings: $20"
assignees: []
created_at: 2021-07-25T20:41:21Z
updated_at: 2024-08-25T16:48:01Z
url: https://github.com/clap-rs/clap/issues/2621
synced_at: 2026-01-10T01:27:21Z
---

# Support for using ArgGroup as Enum with derive

---

_Issue opened by @ModProg on 2021-07-25 20:41_

Maintainer's notes
- Deriving `Args` on an enum would create a `ArgGroup::new("T")::multiple(false)` 
- More builder methods can be accessed via `#[group(...)]` (split out as #4574)
  - We need to ensure overriding the group name works
- Variants without a type would be flags
- Tuple variants would be arguments like in a struct
- Flattening tuple variants would use `Args::group_id`, flattening struct variants would use the Variant.
  - Flattened struct variant fields should have their `Arg::id` set to `Variant::field`, see #4699.
- Likely, we'll need to refactor `clap_derive` so we can share code between structs and enums
- Note that nesting groups is problematic atm.  See #3165
- Create a Traefik cookbook example based on notes in #4699

~~Blocked on #3165 as that will lay some of the initial groundwork and #4211 to provide a fallback mechanism~~

Related
- #4697

---
### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Clap Version

3.0.0-beta.2

### Describe your use case

I have multiple filters of witch only one can  be set (`--running`, `--exited`, `--restarting`,...). 


### Describe the solution you'd like

I would like something with this usage:
```rs
#[derive(Clap)]
pub struct App{
    #[clap(flatten)]
    pub state_filter: StateFilter,
}

#[derive(Args)]
pub enum StateFilter {
    /// Only running servers are returned
    #[clap(long)]
    Running,
    /// Only exited servers are returned
    #[clap(long)]
    Exited,
    /// Only restarting servers are returned
    #[clap(long)]
    Restarting,
    #[clap(long)]
    Custom(String),
}
```
Resulting in:
```
OPTIONS:
    --exited
           Only exited servers are returned
    --restarting
           Only restarting servers are returned
    --running
           Only running servers are returned
    --cusxtom <CUSTOM>
```

### Alternatives, if applicable

I could use an arggroup for this currently, but I would still need to check each boolean separately and could not just easily (and readable) `match` it.

### Additional Context

_No response_

---

_Label `T: new feature` added by @ModProg on 2021-07-25 20:41_

---

_Comment by @epage on 2021-07-26 14:40_

When I was implementing the `Args` trait, I was thinking of this case.  I was assuming we could do
```rust
#[derive(Clap)]
pub struct App{
    #[clap(flatten)]
    pub state_filter: Option<StateFilter>,
}

#[derive(Args)]
pub enum StateFilter {
    /// Only running servers are returned
    #[clap(long)]
    Running,
    /// Only exited servers are returned
    #[clap(long)]
    Exited,
    /// Only restarting servers are returned
    #[clap(long)]
    Restarting,
}
```
(with an attribute on the enum to override the group name)

Though I hadn't considered
- `Option` in the caller, I like it!
- Enum variants as flags, which I also like!

---

_Label `C: derive macros` added by @pksunkara on 2021-07-27 08:03_

---

_Referenced in [clap-rs/clap#2814](../../clap-rs/clap/pulls/2814.md) on 2021-10-05 17:05_

---

_Comment by @Skirmisher on 2021-10-30 21:17_

This would be lovely for my use case (an archive utility, where the operation modes are exclusive but otherwise share the same options, similar to `tar`). I find clap's subcommand system to be a bit heavy-handed for what I need, but I'm lacking another way to concisely describe and match "exactly one of these options" like is described here.

---

_Comment by @epage on 2021-10-31 00:45_

You can workaround this by manually defining the arg group and adding the arguments to it.  While its using structopt instead of clap-derive, the principle and calls are pretty much the same [in this code of mine](https://github.com/epage/git-stack/blob/main/src/bin/git-stack/args.rs#L8).

---

_Comment by @0x5c on 2021-11-02 04:46_

The problem with argument groups and the struct equivalent is that you can't ask clap what option was specified, all you can do is check each individual option of the group for presence in the arguments. We're back to to arguments not really being parsed into something useful.

With subcommands, clab directly talls you what subcommand was found in the arguments, as either an Enum or as the string ID of the subcommand. In both cases you can actually match on something instead of having an if-else chain or iteration simply to know which one it was


---

_Comment by @MTCoster on 2021-11-02 16:22_

I have a perfect use case for this right now: my app can accept a config file, or (perhaps if the config is very short) a string directly in the command line. For instance:

```
$ app --config 'foo=bar,baz=bat'
$ app --config-file ../longer-config.txt
```

*The config string is not designed to be parsed by the app; it's passed directly through to the output. I have no desire to parse it.*

Ideally, I'd write something like this:

```
#[derive(Parser)]
struct RootArgs {
    #[clap(flatten)]
    config: ConfigStringOrFile,
}

#[derive(Args)]
enum ConfigStringOrFile {
    #[clap(short = 'c', long = "config")]
    String(String),
    #[clap(short = 'C', long = "config-file")]
    File(PathBuf),
}

fn main() {
    let args = RootArgs::parse();

    let config = match args.config {
        ConfigStringOrFile::String(s) => s,
        ConfigStringOrFile::File(path) => { 
            // Read config from file...
        },
    };

    // ...
}
```

---

_Comment by @epage on 2021-11-02 16:56_

> The problem with argument groups and the struct equivalent is that you can't ask clap what option was specified, all you can do is check each individual option of the group for presence in the arguments. We're back to to arguments not really being parsed into something useful.

Yes, its only a workaround to help keep people from being blocked and is not ideal.  We are trying to focus on polishing up clap v3 before before working on new features.  Once v3 is out (and maybe a little after as we do high priority work we postponed), we can look into this or mentoring someone who can look into this,

---

_Referenced in [epage/clapng#192](../../epage/clapng/issues/192.md) on 2021-12-06 21:17_

---

_Label `T: new feature` removed by @epage on 2021-12-08 21:10_

---

_Label `C-enhancement` added by @epage on 2021-12-08 21:10_

---

_Referenced in [clap-rs/clap#3115](../../clap-rs/clap/issues/3115.md) on 2021-12-09 16:15_

---

_Referenced in [clap-rs/clap#3121](../../clap-rs/clap/issues/3121.md) on 2021-12-09 16:23_

---

_Label `E-hard` added by @epage on 2021-12-13 16:55_

---

_Referenced in [clap-rs/clap#3165](../../clap-rs/clap/issues/3165.md) on 2021-12-13 17:01_

---

_Referenced in [NilsIrl/MozWire#25](../../NilsIrl/MozWire/issues/25.md) on 2022-01-02 01:37_

---

_Referenced in [swift-nav/swift-toolbox#378](../../swift-nav/swift-toolbox/pulls/378.md) on 2022-01-22 00:15_

---

_Referenced in [estuary/flow#360](../../estuary/flow/pulls/360.md) on 2022-02-08 20:38_

---

_Comment by @jcdyer on 2022-03-03 18:05_

I'd like to offer another example that I would like to support with this feature.  It's slightly more complex than the existing one, so worth calling out explicitly I think.  I have a tls configuration that requires three pem files, representing the root certificate, the application cert, and the application's private key.  By convention, they all live in the same directory, as `root.pem`, `cert.pem`, and `key.pem`.  I would like users to be able to pass a directory or a path to each file, but not both, meaning I would like something like:
```rust
#[derive(clap::Args)]
enum Pems {
    Dir { 
        #[clap(long)]
        pem_dir: PathBuf,
    }
    Files {
        #[clap(long)]
        key: PathBuf,
        #[clap(long)]
        cert: PathBuf,
        #[clap(long)]
        root: PathBuf,
    }
}
```
or
```rust
#[derive(clap::Args)]
enum Pems {
    Dir(PemDir),
    Files(PemFiles),
}
#[derive(clap::Args)]
struct PemDir {
    #[clap(long)]
    pem_dir: PathBuf,
}
#[derive(clap::Args)]
struct PemFiles {
    #[clap(long)]
    key: PathBuf,
    #[clap(long)]
    cert: PathBuf,
    #[clap(long)]
    root: PathBuf,
}
```

---

_Comment by @blaenk on 2022-03-22 02:53_

For what it's worth, I thought I understood the clap "language" of modeling out commands and flags and naturally reached for this and it took me a while to realize (and find this issue) that it wasn't possible.

I'm making do with the `ArgGroup` workaround @epage helpfully provided, so all is fine, just a shame we can't leverage what would have been a pretty elegant solution (for now?):

``` rust
#[derive(Parser, Debug)]
#[clap(group = clap::ArgGroup::new("image-tag").multiple(false))]
pub struct ImageTag {
    #[clap(long, group = "image-tag")]
    existing: bool,

    #[clap(long, group = "image-tag")]
    new: bool,

    #[clap(long, group = "image-tag")]
    tag: Option<String>,
}
```

versus something like:

``` rust
#[derive(Parser, Debug)]
pub enum ImageTag {
    Existing,
    New,
    Tag(String)
}
```

Just wanted to register my interest. Thanks for providing a workaround!

---

_Referenced in [foundry-rs/foundry#1807](../../foundry-rs/foundry/pulls/1807.md) on 2022-06-02 13:56_

---

_Comment by @Skirmisher on 2022-06-22 03:38_

Are there any plans to implement this yet, now that clap v3 has been out for a while? I did take a crack at it after I first commented here (about 9 months ago now...); I tried a couple different approaches, angling to adapt existing ArgGroup code to accept enum variants, but I kept running into flaws in my type model that made the whole thing fall apart. It didn't help that it was my first experience writing proc macro code, either, though I got impressively far considering. All that being said, if someone can come up with a sound design, I might be able to help with implementation.

---

_Comment by @epage on 2022-06-22 14:06_

> Are there any plans to implement this yet, now that clap v3 has been out for a while? I

This is not a priority of mine.  My focus is on binary size, compile times, and extensibility.  If someone else wants to work on a design proposal and then do an implementation, they are free to do so.

---

_Comment by @StripedMonkey on 2022-07-17 02:09_

I have encountered a situation where supporting this would be extremely useful. Specifically I am wanting to provide different authentication methods for a service. The following is a rough sketch of how I was imagining such a feature:

```rust
#[derive(Debug, Args)]
struct UserPassAuth {
    #[clap(short, long)]
    username: String,
    #[clap(short, long)]
    password: String,
}

#[derive(Debug, Args)]
struct TokenAuth {
    #[clap(long)]
    token: String,
}

#[derive(Debug, Default, MutuallyExclusiveArgs)]
enum AuthenticationMethod {
    #[default]
    Anonymous,
    UserPass(UserPassAuth),
    Token(TokenAuth),
}
```
This isn't necessarily a priority in that argGroups do exist, and they can be made mutually exclusive through that mechanism, but this does *significantly* clean up the interface that you have to deal with externally. 
Without enums you have to check what flags have been set, even if argGroups exist. With enums in theory it's as simple as:
```rust
match authentication {
   AuthenticationMethod::Anonymous => anonymous_login(),
   AuthenticationMethod::UserPass(login_info) => login(login_info),
   // ...
}
```
I'm not nearly confident enough to take a stab at implementing it, but if there's guidance on how this might be correctly implemented I would be interested in having direction.

---

_Added to milestone `4.x` by @epage on 2022-09-13 00:35_

---

_Comment by @epage on 2022-09-13 00:37_

I'd like to get to this during 4.x series.

Some ground work I need to do in 4.0.0
- Reserve the `group` attribute
- Add an empty `ArgGroup::new("TypeName")` to each `Args` derive.
- Add a `fn group_id(&self) -> Option<&str>` to `AugmentArgs`
  - Technically this can be done later.

---

_Referenced in [clap-rs/clap#4209](../../clap-rs/clap/pulls/4209.md) on 2022-09-13 12:18_

---

_Referenced in [clap-rs/clap#4210](../../clap-rs/clap/issues/4210.md) on 2022-09-13 14:38_

---

_Label `:money_with_wings: $20` added by @epage on 2022-09-13 14:41_

---

_Label `S-blocked` added by @epage on 2022-09-13 14:41_

---

_Label `E-help-wanted` added by @epage on 2022-09-20 14:33_

---

_Referenced in [clap-rs/clap#4279](../../clap-rs/clap/issues/4279.md) on 2022-09-28 22:16_

---

_Referenced in [clap-rs/clap#4426](../../clap-rs/clap/issues/4426.md) on 2022-10-27 12:41_

---

_Referenced in [oxidecomputer/hubris#905](../../oxidecomputer/hubris/pulls/905.md) on 2022-11-02 18:56_

---

_Referenced in [clap-rs/clap#4451](../../clap-rs/clap/issues/4451.md) on 2022-11-05 01:25_

---

_Referenced in [shuttle-hq/shuttle#498](../../shuttle-hq/shuttle/pulls/498.md) on 2022-12-01 08:20_

---

_Label `S-blocked` removed by @epage on 2022-12-15 15:09_

---

_Referenced in [wasmerio/wasmer#3324](../../wasmerio/wasmer/pulls/3324.md) on 2022-12-16 14:19_

---

_Comment by @klnusbaum on 2023-01-07 22:03_

@epage I see this is tagged with "e-help wanted". Is there anything specifically I can help with? I'm pretty interested in this.

---

_Comment by @epage on 2023-01-08 01:35_

@klnusbaum a good introduction to the code base that I'm assuming you'll want anyways is https://github.com/clap-rs/clap/issues/4574. If you want to take that, feel free to ask for more details in that issue.

Note that this one is also tagged `E-Hard`.  We have a derive the [struct derive for `Args`](https://github.com/clap-rs/clap/blob/master/clap_derive/src/derives/args.rs) and we'd need to add enum support (which [subcommand support](https://github.com/clap-rs/clap/blob/master/clap_derive/src/derives/subcommand.rs) can serve as an example of enum handling).  A lot of code will be shared with struct support, a lot of code will be unique.  It likely will take doing a prototype implementation that will be thrown away to learn enough to know how to share, if at all.

---

_Referenced in [klnusbaum/unicipher#4](../../klnusbaum/unicipher/issues/4.md) on 2023-01-08 02:57_

---

_Comment by @klnusbaum on 2023-01-08 03:00_

@epage that all sounds total reasonable. I'll start with #4574 and see how far I can get from there. If it turns out I think I might having something, I will indeed start with a prototype.

---

_Referenced in [clap-rs/clap#4701](../../clap-rs/clap/issues/4701.md) on 2023-02-10 15:42_

---

_Referenced in [clap-rs/clap#4837](../../clap-rs/clap/issues/4837.md) on 2023-04-17 15:28_

---

_Referenced in [kevlarr/jrny#43](../../kevlarr/jrny/issues/43.md) on 2023-04-20 02:20_

---

_Comment by @KSXGitHub on 2023-05-18 08:47_

Correct me if I'm wrong, the feature that is being requested here would allow parsing mutually exclusive sets of flags into different variants of an enum, right? I really want to remove all the `conflicts_with` and `required_by` in my code.

---

_Comment by @epage on 2023-05-18 14:14_

Yes, this would allow conflicts to be expressed in the type system

---

_Referenced in [aptos-labs/aptos-core#8346](../../aptos-labs/aptos-core/pulls/8346.md) on 2023-05-26 06:38_

---

_Comment by @stevenroose on 2023-08-08 17:12_

Any updates on this? Is someone working on this?

---

_Comment by @epage on 2023-08-08 17:48_

Not at this time

---

_Referenced in [n0-computer/iroh#1729](../../n0-computer/iroh/pulls/1729.md) on 2023-10-25 16:58_

---

_Referenced in [clap-rs/clap#5199](../../clap-rs/clap/issues/5199.md) on 2023-11-07 19:49_

---

_Referenced in [near/nearcore#10240](../../near/nearcore/pulls/10240.md) on 2023-11-27 13:30_

---

_Referenced in [MystenLabs/walrus#707](../../MystenLabs/walrus/pulls/707.md) on 2024-08-21 09:53_

---

_Referenced in [clap-rs/clap#5700](../../clap-rs/clap/pulls/5700.md) on 2024-08-25 16:42_

---

_Comment by @ysndr on 2024-08-25 16:46_

I took a stab at it here: https://github.com/clap-rs/clap/pull/5700

At the moment it only supports struct like enums so far, but I'd like it to be a bit smarter and support more of the use cases described in this thread eventually.

```rust
#[derive(Parser, Debug, PartialEq, Eq)]
struct Opt {
    #[command(flatten)]
    source: Source,
}

#[derive(clap::Args, Clone, Debug, PartialEq, Eq)]
enum Source {
    A {
        #[arg(short)]
        a: bool,
        #[arg(long)]
        aaa: bool,
    },
    B {
        #[arg(short)]
        b: bool,
    },
}
```

```shell
prog -b -a

error: the argument '-b' cannot be used with:
  -a
  --aaa

Usage: prog -b -a
```



---

_Referenced in [clap-rs/clap#5711](../../clap-rs/clap/issues/5711.md) on 2024-08-30 20:30_

---

_Referenced in [alexpovel/srgn#172](../../alexpovel/srgn/issues/172.md) on 2024-11-10 15:46_

---

_Referenced in [MystenLabs/walrus#1204](../../MystenLabs/walrus/pulls/1204.md) on 2024-11-25 08:50_

---

_Referenced in [alexpovel/srgn#196](../../alexpovel/srgn/pulls/196.md) on 2025-03-23 18:54_

---

_Referenced in [reubeno/brush#473](../../reubeno/brush/issues/473.md) on 2025-05-09 23:21_

---

_Referenced in [clap-rs/clap#6118](../../clap-rs/clap/issues/6118.md) on 2025-09-01 15:16_

---

_Referenced in [microsoft/wassette#250](../../microsoft/wassette/pulls/250.md) on 2025-09-03 19:34_

---

_Referenced in [clap-rs/clap#815](../../clap-rs/clap/issues/815.md) on 2025-09-16 18:43_

---

_Referenced in [clap-rs/clap#6174](../../clap-rs/clap/issues/6174.md) on 2025-11-03 16:17_

---

_Referenced in [Quantco/pixi-pack#246](../../Quantco/pixi-pack/pulls/246.md) on 2025-11-28 15:45_

---
