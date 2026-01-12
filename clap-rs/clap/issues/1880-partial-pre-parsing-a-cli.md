```yaml
number: 1880
title: Partial / Pre Parsing a CLI
type: issue
state: closed
author: kbknapp
labels:
  - C-enhancement
  - A-parsing
  - S-wont-fix
assignees: []
created_at: 2020-04-29T15:38:15Z
updated_at: 2023-09-09T00:05:58Z
url: https://github.com/clap-rs/clap/issues/1880
synced_at: 2026-01-12T16:14:11Z
```

# Partial / Pre Parsing a CLI

---

_@kbknapp_

[Comment on closing](https://github.com/clap-rs/clap/issues/1880#issuecomment-990173791):
> In the short term, we now have IgnoreErrors.
> 
> Longer term, I feel like #1404 is a more general solution and has existing precedence (Python's argparse at least) compared to a `get_partial_matches(arg_name)` which only works in a narrow set of cases.
---
### Make sure you completed the following tasks

- [x] Searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [x] Searched the closed issues

### Describe your use case

A way to declare a subset of arguments to be parsed, then return from parsing early. With the expectation that parsing will be called again, and run to full completion.

This most often comes up when you want some argument to handle some aspect of the CLI itself (such as `--color` flags to control error messages during parsing, or in the above `--arg-file` which needs to determine a file to load prior to parsing the rest of the CLI).


### Describe the solution you'd like

From a *user* stand point, nothing changes. The run the CLI and things just work. From the a clap *consumer* stand point, it essentially allows you to pull out some values from the CLI (ignoring everything else), change something about the CLI and continue parsing.

```rust
// .. already have some App struct
let pre_matches = app.get_partial_matches("color"); // Only parse the `--color` option, ignore all else
app = if let Some("never") = pre_matches.value_of("color") {
    app.setting(AppSettings::ColorNever)
} else {
    app
};

let matches = app.get_matches();
```

Maybe it's biting off more than we want to chew for now, but this issue seems to come up time and time again where we want to parse some particular subset of arguments, and use those values to inform how the CLI itself behaves.

### Alternatives, if applicable

You can already reparse your CLI twice.

```rust
let matches = app.get_matches();
app = if let Some("never") = matches.value_of("color") {
    app.setting(AppSettings::ColorNever)
} else {
    app
};

let matches = app.get_matches();
```

But this doesn't help if there are errors in the initial parse, or options included included in the initial parse that the initial app isn't aware of. For example, an `iptables` style CLI where `-m <foo>` enables many other arguments and options.

### Additional context

This could potentially be used in #1693 #1089 #1232 #568 

---

_Label `T: new feature` added by @kbknapp on 2020-04-29 15:38_

---

_Label `C: parsing` added by @kbknapp on 2020-04-29 15:38_

---

_Label `D: medium` added by @kbknapp on 2020-04-29 15:38_

---

_Label `P4: nice to have` added by @kbknapp on 2020-04-29 15:38_

---

_Label `T: RFC / question` added by @kbknapp on 2020-04-29 15:38_

---

_Label `W: 3.x` added by @kbknapp on 2020-04-29 15:38_

---

_Referenced in [clap-rs/clap#380](../../clap-rs/clap/issues/380.md) on 2020-05-01 21:00_

---

_Comment by @alerque on 2020-05-05 07:53_

This feature would also be immensely useful for implementing #380, localizing a CLI interface.

> You can already reparse your CLI twice.

This is not _quite_ true. You already mentioned some exceptions such as in the case of parse errors, but there is one more road block that makes that technique unworkable. A number of things happen in `get_matches()` that have the potential to short circuit the rest of the application life-cycle. For example having a `help` subcommand. If this is detected it immediately outputs stuff to the terminal and quits. This means you don't get a chance to reliably parse the CLI a second time.

This makes the technique useless for localization because by the time we get information about a potential language flag the App has already spit out strings that may have needed localization.

---

_Referenced in [clap-rs/clap#1910](../../clap-rs/clap/issues/1910.md) on 2020-05-11 18:43_

---

_Comment by @CreepySkeleton on 2020-06-02 20:11_

As a minimal viable solution, we could simply allow ignoring unknown or missing positionals and options. 

```rust
// first "probe" app
let preprocess = App::new("app")
    .settings(IgnoreErrors) // also disables --help, version, and others
    .arg("--color <color> 'coloring policy: never/always'")
    .get_matches(); // will succeed no matter what

let mut app = App::new("main_app");

match preprocess.value_of("color") {
    Some("always") => app = app.setting(ColorAlways),
    Some("never") => app = app.setting(ColorNever),
    _ => {}
}

// real parsing starts here
app.get_matches()
```

This kind or partial parsing would allow for most, if not all, cases and would be fairly easy to implement.

---

_Referenced in [clap-rs/clap#1232](../../clap-rs/clap/issues/1232.md) on 2020-06-02 20:16_

---

_Comment by @alerque on 2020-06-02 21:17_

Yes that does indeed sound like a MVS, and something along the lines of what I expected to be able to hack together already and failed. It's not the most elegant solution maybe, but it would get me by for my use case of localization.

---

_Referenced in [sharkdp/fd#600](../../sharkdp/fd/issues/600.md) on 2020-06-05 08:39_

---

_Comment by @wabain on 2020-06-07 14:10_

@alerque It sounds like [get_matches_safe](https://docs.rs/clap/2.33.1/clap/struct.App.html#method.get_matches_safe) could be what you're looking for? That would address the immediate problem of clap prematurely exiting with help or an error.

---

_Comment by @CreepySkeleton on 2020-06-07 15:10_

@wabain But it wouldn't give you the partial mach, see the color example above

---

_Comment by @acheronfail on 2020-07-01 23:35_

I am building an app which spawns another process, and needs to "sniff" if certain flags were passed to the subprocess. Having an `AppSettings::IgnoreErrors` would be invaluable, since I only want to match some specific args, and leave the rest alone.

Right now I believe this is impossible with `clap`, and I have to parse the args manually.

---

For some more detail, my app is used like this:

```
app <args sent to subprocess>
```

The subprocess supports _hundreds_ of arguments, and I don't want to have to write code to match _all_ of them if I'm just interested in 2 or 3 flags.

The `get_matches_safe` doesn't work here for me though, since that bails out and completely fails.

---

_Comment by @kenoss on 2020-07-15 20:06_

Let me have some examples because my English is poor and I might miss points.

The original problem is "lax position of arguments."

```
# Good.  `--project` option belongs to `Arg("gcloud")` and `--zone` to `Arg("ssh")`.
$ gcloud --project=hoge compute ssh --zone=asia-northeast1-a instance-name

# Good.  `--project` option can descend to subcommands.
$ gcloud compute --project=hoge ssh --zone=asia-northeast1-a instance-name

# Also good.
$ gcloud compute ssh --project=hoge --zone=asia-northeast1-a instance-name
 
# Bad.  `--zone` option belongs to `ssh` subcommand.
$ gcloud --project=hoge compute --zone=asia-northeast1-a ssh instance-name
```

I think this is solved by adding option to `Arg`.  No tricks.
There's possibility the implementation of parsing become little complicated, but developer using clap can use intuitively.
This is also glad with the viewpoint of https://github.com/clap-rs/clap/issues/1232 .  Tricks like alternative `App` and `get_matches_safe` make the systematic completion harder.

This approach will be match with the @acheronfail 's case.  I guess there're two types of app:

```
time othercommand ...
cargo run -- othercommand-or-options ...
```

The former is very limited.  `time` do not have subcommands because subcommands conflict with `othercommand`.  It can have only root `App` and parse will terminate with `othercommand`.  Hence the above approach has no ambiguity.

The later is similar.  Parsing will terminate with first "--".



---

_Comment by @CreepySkeleton on 2020-07-17 16:55_

@kenoss Thanks for looking into this, we appreciate that. I didn't quite get your comment, so let's try to get on the same page first.

> The original problem is "lax position of arguments."

Could you please elaborate? I think this is where the misunderstanding comes from. The original problem in my understanding is to "loosely parse flags, options, and subcommands but ignore positional arguments entirely". The `--` thing is another topic; I'm not sure why you brought it up. 

> Good.  `--project` option belongs to `Arg("gcloud")` and `--zone` to `Arg("ssh")`.
 
You have introduced "belongs to" relationship between two arguments. I can't wrap my head around it - arguments can't belong to each other. An argument can "belong" to a subcommand or the top-level app. 

Maybe you meant `App`?

---

The complexity of implementation... Guys, I think we've all been missing the elephant in the room. The current parsing algorithm can't just "ignore" arguments, _especially positional_ ones. Well, It can _ignore_, but it can't _recover_. I think this issue belongs to "postponed until after I finally rewrite the parser algorithm" category.

---

_Comment by @kenoss on 2020-08-05 15:39_

Sorry for late reply.

> The original problem in my understanding is to "loosely parse flags, options, and subcommands but ignore positional arguments entirely".

I read the issue again and I've got the point.  Thanks to catch me up the argument.

> The -- thing is another topic; I'm not sure why you brought it up.

Sorry.  I misunderstood by `get_matches_safe` and `app <args sent to subprocess>`.

> Maybe you meant App?

Yes.

I've found that this is a critical blocker of https://github.com/clap-rs/clap/issues/1232 and I should parsing implementation.

---

_Referenced in [clap-rs/clap#1404](../../clap-rs/clap/issues/1404.md) on 2021-03-30 15:35_

---

_Referenced in [knurling-rs/flip-link#31](../../knurling-rs/flip-link/issues/31.md) on 2021-04-05 14:23_

---

_Referenced in [clap-rs/clap#2480](../../clap-rs/clap/pulls/2480.md) on 2021-05-13 14:30_

---

_Referenced in [clap-rs/clap#1185](../../clap-rs/clap/issues/1185.md) on 2021-05-26 10:57_

---

_Referenced in [bootandy/dust#147](../../bootandy/dust/pulls/147.md) on 2021-06-05 13:24_

---

_Referenced in [clap-rs/clap#2354](../../clap-rs/clap/issues/2354.md) on 2021-07-30 00:44_

---

_Label `W: 3.x` removed by @pksunkara on 2021-08-13 10:40_

---

_Referenced in [epage/clapng#92](../../epage/clapng/issues/92.md) on 2021-12-06 17:33_

---

_Referenced in [epage/clapng#156](../../epage/clapng/issues/156.md) on 2021-12-06 20:15_

---

_Referenced in [epage/clapng#179](../../epage/clapng/issues/179.md) on 2021-12-06 21:12_

---

_Label `T: RFC / question` removed by @epage on 2021-12-08 21:01_

---

_Label `T: new feature` removed by @epage on 2021-12-08 21:01_

---

_Label `C-enhancement` added by @epage on 2021-12-08 21:01_

---

_Comment by @epage on 2021-12-09 19:40_

In the short term, we now have IgnoreErrors.

Longer term, I feel like #1404 is a more general solution and has existing precedence (Python's argparse at least) compared to a `get_partial_matches(arg_name)` which only works in a narrow set of cases.

If there is any concern with this, feel free to comment and we can re-open this.

---

_Closed by @epage on 2021-12-09 19:40_

---

_Comment by @alerque on 2021-12-14 11:08_

I don't see anything in #1404 covering the use cases [I commented on above](https://github.com/clap-rs/clap/issues/1880#issuecomment-623910845). An option would need to be added to *stop* the default short-circuit behavior where the app prints something and exits when encountering some flags, e.g. `--help`.

---

_Comment by @epage on 2021-12-14 14:24_

> I don't see anything in #1404 covering the use cases [I commented on above](https://github.com/clap-rs/clap/issues/1880#issuecomment-623910845). An option would need to be added to _stop_ the default short-circuit behavior where the app prints something and exits when encountering some flags, e.g. `--help`.

So if I understand, you would define one `App` with the localization flag(s), parse that to know how to handle it, and then do a full parse.

For the partial parse, couldn't you either set `NoAutoHelp` / `NoAutoVersion` or `DisableHelpFlag` / `DisableVersionFlag` / `DisableHelpSubcommand`?

---

_Comment by @alerque on 2021-12-14 14:57_

> For the partial parse, couldn't you either set `NoAutoHelp` / `NoAutoVersion` or `DisableHelpFlag` / `DisableVersionFlag` / `DisableHelpSubcommand`?

Honestly, I hadn't considered that — but it seems like quite a song and dance for what I want. First I'd have to set both of those (since they aren't the default), do one parsing pass, then unset both of them again for the second pass. I would suggest that should be an option on the parsing function (or an alternate function) to disable all current & future `Auto____` functionality during a parse.

Should I open a new issue for that?

---

_Comment by @epage on 2021-12-14 15:04_

> > For the partial parse, couldn't you either set `NoAutoHelp` / `NoAutoVersion` or `DisableHelpFlag` / `DisableVersionFlag` / `DisableHelpSubcommand`?
> 
> Honestly, I hadn't considered that — but it seems like quite a song and dance for what I want. First I'd have to set both of those (since they aren't the default), do one parsing pass, then unset both of them again for the second pass. I would suggest that should be an option on the parsing function (or an alternate function) to disable all current & future `Auto____` functionality during a parse.
> 
> Should I open a new issue for that?

You can, it would be a better place to continue to discuss it.  With that said, I am concerned about the number of parsing functions we already have today and would be preferring we find ways to reduce the number, rather than increase it.  As long as we provide the building blocks, that is my main concern.  How nice we make things past that is dependent on how prevalent the workflow is.

Already, you either have to be setting `IgnoreErrors` or calling the partial parsing proposed in #1404 with subset of arguments you want verified.  This seems like something that is already going to be a specialized parse pass, so the level of burden of setting a couple more flags seems low.

---

_Label `S-wont-fix` added by @epage on 2022-01-11 18:25_

---

_Comment by @scottidler on 2022-03-14 01:16_

```rust
fn extract(item: (ContextKind, &ContextValue)) -> Option<&ContextValue> {                                                                                         
    let (k, v) = item;                                                                                                                                            
    if k == ContextKind::InvalidArg {                                                                                                                             
        return Some(v);                                                                                                                                           
    }                                                                                                                                                             
    None                                                                                                                                                          
}                                                                                                                                                                 
                                                                                                                                                                  
fn parse_known_args(cmd: &Command) -> Result<(ArgMatches, Vec<String>), Error> {                                                                                  
    let mut rem: Vec<String> = vec![];                                                                                                                            
    let mut args: Vec<String> = env::args().collect();                                                                                                            
    loop {                                                                                                                                                        
        match cmd.clone().try_get_matches_from(&args) {                                                                                                           
            Ok(matches) => {                                                                                                                                      
                return Ok((matches, rem));                                                                                                                        
            },                                                                                                                                                    
            Err(error) => {                                                                                                                                       
                match error.kind() {                                                                                                                              
                    ErrorKind::UnknownArgument => {                                                                                                               
                        let items = error.context().find_map(extract);                                                                                            
                        match items {                                                                                                                             
                            Some(x) => {                                                                                                                          
                                match x {                                                                                                                         
                                    ContextValue::String(s) => {                                                                                                  
                                        rem.push(s.to_owned());                                                                                                   
                                        args.retain(|a| a != s);                                                                                                  
                                    },                                                                                                                            
                                    _ => {                                                                                                                        
                                        return Err(error);                                                                                                        
                                    },                                                                                                                            
                                }                                                                                                                                 
                            },                                                                                                                                    
                            None => {                                                                                                                             
                                return Err(error);                                                                                                                
                            },                                                                                                                                    
                        }                                                                                                                                         
                    },                                                                                                                                            
                    _ => {                                                                                                                                        
                        return Err(error);                                                                                                                        
                    },                                                                                                                                            
                }                                                                                                                                                 
            },                                                                                                                                                    
        }                                                                                                                                                         
    }                                                                                                                                                             
}   
```
```rust
fn main() {                                                                                                                                                       
    let cmd = Command::new("cmd")                                                                                                                                 
        .allow_hyphen_values(true)                                                                                                                                
        .trailing_var_arg(true)                                                                                                                                   
        .disable_help_flag(true)                                                                                                                                  
        .disable_version_flag(true)                                                                                                                               
        .arg(                                                                                                                                                     
            Arg::new("config")                                                                                                                                    
                .takes_value(true)                                                                                                                                
                .short('c')                                                                                                                                       
                .long("config")                                                                                                                                   
                .help("path to config"),                                                                                                                          
        );                                                                                                                                                        
    let result = parse_known_args(&cmd);                                                                                                                          
    let (matches, rem) = result.unwrap();                                                                                                                         
    println!("matches={:#?}", matches);                                                                                                                           
    println!("rem={:#?}", rem);   
    let matches = cmd.get_matches_from(&rem);
}
```

For anyone who comes across this and is looking for an equivalent to Python Argparse's `parse_known_args`, I hacked this code together. I am sure it could be improved as I am new to Rust. It would be nice to have some ergonomics in the Clap code base for this use case, but until then maybe this provides some usefulness to someone.

For anyone who doesn't understand why you would want this, it is a VERY common pattern for me to supply the config file in the first pass, that will either effect the default values if not alter the program's options entirely. 

---

_Referenced in [jj-vcs/jj#824](../../jj-vcs/jj/pulls/824.md) on 2022-12-01 20:28_

---

_Comment by @novafacing on 2023-09-09 00:02_

Sorry for the long time bump, I liked @scottidler's answer here so I've made it a little more generic. Slap this at the top of your cli code and you're good to go. My use case is for wrapping `clang` for LibAFL use -- I want to extract some arguments and pass everything else down to the clang command I'm wrapping.

```rust
use anyhow::{anyhow, bail, Result};
use clap::{
    error::{ContextKind, ContextValue, ErrorKind},
    ArgMatches, Command, CommandFactory, FromArgMatches, Parser,
};
use std::env::args;

#[derive(Debug, Clone)]
struct Known<T>
where
    T: FromArgMatches,
{
    matches: T,
    rest: Vec<String>,
}

impl<T> Known<T>
where
    T: FromArgMatches,
{
    pub fn new<I, S>(matches: ArgMatches, rest: I) -> Result<Self>
    where
        I: IntoIterator<Item = S>,
        S: AsRef<str>,
    {
        Ok(Self {
            matches: T::from_arg_matches(&matches)?,
            rest: rest.into_iter().map(|a| a.as_ref().to_string()).collect(),
        })
    }
}

trait ParseKnown: FromArgMatches {
    fn parse_known() -> Result<Known<Self>>;
}

impl<T> ParseKnown for T
where
    T: CommandFactory + FromArgMatches,
{
    fn parse_known() -> Result<Known<T>> {
        let command = Self::command();
        let mut rest = Vec::new();
        let mut args = args().collect::<Vec<_>>();

        loop {
            match command.clone().try_get_matches_from(&args) {
                Ok(matches) => {
                    return Known::new(matches, rest);
                }
                Err(e) if matches!(e.kind(), ErrorKind::UnknownArgument) => {
                    if let Some(ContextValue::String(v)) = e
                        .context()
                        .find_map(|(k, v)| matches!(k, ContextKind::InvalidArg).then_some(v))
                    {
                        let unknown = args
                            .iter()
                            .find(|a| a.starts_with(v))
                            .cloned()
                            .ok_or_else(|| anyhow!("No argument starts with {}", v))?;

                        if args.contains(&unknown) {
                            args.retain(|a| a != &unknown);
                            rest.push(unknown);
                        } else {
                            bail!("Got unknown argument {} but it is not in args at all.", v);
                        }
                    } else {
                        bail!("No string value found for unknown argument: {}", e);
                    }
                }
                Err(e) => bail!("Error getting matches from args '{:?}': {}", args, e),
            }
        }
    }
}

#[derive(Parser, Debug, Clone)]
#[clap(
    allow_hyphen_values = true,
    trailing_var_arg = true,
    disable_help_flag = true,
    disable_version_flag = true
)]
struct Args {
    #[arg(long)]
    libafl_no_link: bool,
    #[arg(long)]
    libafl: bool,
    #[arg(long)]
    libafl_ignore_configurations: bool,
    #[arg(long, value_delimiter = ',')]
    libafl_configurations: Vec<String>,
}

fn main() -> Result<()> {
    let args = Args::parse_known()?;
    println!("{:#?}", args);
    Ok(())
}
```

Handles e.g. `cargo run -- -x --libafl -y -zexecstack -fsanitize=fuzzer-no-link -fno-common --libafl-ignore-configurations` (the above example will fail with single-hyphen commands)

---
