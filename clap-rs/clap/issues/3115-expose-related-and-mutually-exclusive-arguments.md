```yaml
number: 3115
title: Expose related and mutually exclusive arguments via struct / enum?
type: issue
state: closed
author: epage
labels:
  - A-derive
assignees: []
created_at: 2021-12-09T16:11:45Z
updated_at: 2021-12-09T16:41:14Z
url: https://github.com/clap-rs/clap/issues/3115
synced_at: 2026-01-12T16:14:14Z
```

# Expose related and mutually exclusive arguments via struct / enum?

---

_@epage_

<a href="https://github.com/epage"><img src="https://avatars.githubusercontent.com/u/60961?v=4" align="left" width="96" height="96" hspace="10"></img></a> **Issue by [epage](https://github.com/epage)**
_Thursday May 03, 2018 at 02:38 GMT_
_Originally opened as https://github.com/TeXitoi/structopt/issues/104_

----

I'm working on a CLI that currently looks like
```rust
#[derive(StructOpt, Debug)]
#[structopt(name = "staging")]
pub struct Arguments {
    #[structopt(short = "i", long = "input", name = "STAGE", parse(from_os_str))]
    pub input_stage: path::PathBuf,
    #[structopt(short = "d", long = "data", name = "DATA_DIR", parse(from_os_str))]
    pub data_dir: Vec<path::PathBuf>,
    #[structopt(flatten)]
    pub output: Output,
    #[structopt(short = "v", long = "verbose", parse(from_occurrences))]
    pub verbosity: u8,
}

#[derive(StructOpt, Debug)]
pub struct Output {
    #[structopt(short = "o", long = "output", name = "OUT", parse(from_os_str))]
    pub dir: path::PathBuf,
    #[structopt(long = "format",
                raw(possible_values = "&Format::variants()", case_insensitive = "true"),
                raw(default_value = "DEFAULT_FORMAT"))]
    pub format: Format,
    #[structopt(short = "n", long = "dry-run")]
    pub dry_run: bool,
}
```

I'm looking at adding some options that are mutually exclusive with `Arguments::output`.

This gave me the ideas:
- Allow flattened structs to define groups
- Allow `Option<Output>` on an entire flattened struct to say the arguments require each other
- Allow an `enum` to define mutually exclusive flags.  For example:

```rust
#[derive(StructOpt, Debug)]
#[structopt(name = "staging")]
pub struct Arguments {
    #[structopt(short = "i", long = "input", name = "STAGE", parse(from_os_str))]
    pub input_stage: path::PathBuf,
    #[structopt(short = "d", long = "data", name = "DATA_DIR", parse(from_os_str))]
    pub data_dir: Vec<path::PathBuf>,
    #[structopt(flatten)]
    pub flags: Flags,
    #[structopt(short = "v", long = "verbose", parse(from_occurrences))]
    pub verbosity: u8,
}

// `Output` as above

#[derive(StructOpt, Debug)]
pub enum Flags
{
    Output(Output),
    Completions(Completions),
    DumpConfig(DumpConfig),
    DumpData(DumpData),
}

#[derive(StructOpt, Debug)]
pub struct Completions {
    #[structopt(long = "completion", name = "OUT", parse(from_os_str))]
    pub completion: path::PathBuf,
}

#[derive(StructOpt, Debug)]
pub struct DumpConfig{
    #[structopt(long = "dump-config")]
    pub dump_config: bool,
}

#[derive(StructOpt, Debug)]
pub struct DumpData{
    #[structopt(long = "dump-data")]
    pub dump_data: bool,
}
```


---

_Comment by @epage on 2021-12-09 16:11_

<a href="https://github.com/TeXitoi"><img src="https://avatars.githubusercontent.com/u/5787066?v=4" align="left" width="48" height="48" hspace="10"></img></a> **Comment by [TeXitoi](https://github.com/TeXitoi)**
_Thursday May 03, 2018 at 10:00 GMT_

----

First thought on the proposition, that is interesting, but not yet mature enough:
 * `derive(StructOpt)` on an enum will do subcommand, thus it need something else.
 * `flatten` on an enum set subcommand for now, thus this feature might need a special keyword.
 * having DumpData just for a bool is quite sad, it would be better to have `Completions`, `DumpConfig` and DumpData` just be empty enum variant.
 * `Completions` would be better with just the `PathBuf` directly inside.

Maybe something like that:

```rust
#[derive(StructOpt, Debug)]
#[structopt(name = "staging")]
pub struct Arguments {
    #[structopt(short = "i", long = "input", name = "STAGE", parse(from_os_str))]
    pub input_stage: path::PathBuf,
    #[structopt(short = "d", long = "data", name = "DATA_DIR", parse(from_os_str))]
    pub data_dir: Vec<path::PathBuf>,
    #[structopt(group)]
    pub flags: Flags,
    #[structopt(short = "v", long = "verbose", parse(from_occurrences))]
    pub verbosity: u8,
}

#[derive(StructOptGroup, Debug)]
pub enum Flags
{
    // no annotation? an annotation?
    Output(Output), // <- How to handle that with clap?
    #[structopt(long = "completion", name = "OUT", parse(from_os_str))]
    Completions(PathBuf),
    #[structopt(long = "dump-config")]
    DumpConfig,
    #[structopt(long = "dump-data")]
    DumpData,
}

#[derive(StructOpt, Debug)]
pub struct Output {
    #[structopt(short = "o", long = "output", name = "OUT", parse(from_os_str))]
    pub dir: path::PathBuf,
    #[structopt(long = "format",
                raw(possible_values = "&Format::variants()", case_insensitive = "true"),
                raw(default_value = "DEFAULT_FORMAT"))]
    pub format: Format,
    #[structopt(short = "n", long = "dry-run")]
    pub dry_run: bool,
}
```

For this particular need, subcommands seem more appropriate, and is working today. But that may be interesting for other cases.


---

_Comment by @epage on 2021-12-09 16:11_

<a href="https://github.com/epage"><img src="https://avatars.githubusercontent.com/u/60961?v=4" align="left" width="48" height="48" hspace="10"></img></a> **Comment by [epage](https://github.com/epage)**
_Thursday May 03, 2018 at 13:15 GMT_

----

> having DumpData just for a bool is quite sad, it would be better to have Completions, DumpConfig and DumpData` just be empty enum variant.

Yeah, it was more for illustrative purposes of the wider idea.  As you say, even better if I can directly annotate an enum variant to say that its just a flag.

> Output would be better with just the String directly inside.

Except that isn't what I was trying to point out.  This is supposed to take in the `Output` struct.  In clap, I'd say
- the defaulted parts of the `Output` struct require `--output`.
- The members of the `Output` struct are a group and that completions and dump conflict with the group.

> For this particular need, subcommands seem more appropriate, and is working today. But that may be interesting for other cases.

I'd considered that but its pretty common for CLIs to not go the subommand route for non-routine alternative behaviors of the application
- `--version`
- `--help`
- `--completions`
- Creation of configuration files to jump start the user

At that point, it felt awkward to create subcommands just for my debugging, so I went ahead and made my dump state flags do similar.


---

_Comment by @epage on 2021-12-09 16:11_

<a href="https://github.com/TeXitoi"><img src="https://avatars.githubusercontent.com/u/5787066?v=4" align="left" width="48" height="48" hspace="10"></img></a> **Comment by [TeXitoi](https://github.com/TeXitoi)**
_Thursday May 03, 2018 at 13:46 GMT_

----

> >  Output would be better with just the String directly inside.

Sorry, I mean Completions

> Except that isn't what I was trying to point out. This is supposed to take in the Output struct. In clap, I'd say
>  *  the defaulted parts of the Output struct require --output.
>   * The members of the Output struct are a group and that completions and dump conflict with the group.

Is it possible to express such constraints in pure clap? because if that's not possible, we can't do it in structopt.

I'm also afraid that this machinery will be too complicated to be understood by the users. Finding a clear, flexible and usable interface is a challenge here.


---

_Comment by @epage on 2021-12-09 16:11_

<a href="https://github.com/epage"><img src="https://avatars.githubusercontent.com/u/60961?v=4" align="left" width="48" height="48" hspace="10"></img></a> **Comment by [epage](https://github.com/epage)**
_Thursday May 03, 2018 at 14:09 GMT_

----

> > Is it possible to express such constraints in pure clap? because if that's not possible, we can't do it in structopt.

> Thee members of the Output struct are a group and that completions and dump conflict with the group.
```rust
    .group(ArgGroup::with_name("output_struct")
        .multiple(true)
        .args(&["dir", "format", "dry_run"])
        .conflicts_with_all(&["completions", "dump"]))
```

> the defaulted parts of the Output struct require --output.

I'll admit, this one was more aspirational.  One option is to iterate on the fields and, if there are required fields, to mark the optional fields as depending on the required fields.

> I'm also afraid that this machinery will be too complicated to be understood by the users. Finding a clear, flexible and usable interface is a challenge here.

Are you referring to the developer or to the user?

For the user, I think its understandable that some arguments only work in some settings.  I normally visually group these in the help (with python's argparse) but haven't played too much with doing that with clap yet.

For the developer, I'd say they are.  These are the things I'm  intuitively trying to do but can't.  Instead I'm having to drop down into `raw` calls which, as I mentioned on another issue, have been challenging enough to get right, that I've just given up.


---

_Comment by @epage on 2021-12-09 16:11_

<a href="https://github.com/TeXitoi"><img src="https://avatars.githubusercontent.com/u/5787066?v=4" align="left" width="48" height="48" hspace="10"></img></a> **Comment by [TeXitoi](https://github.com/TeXitoi)**
_Thursday May 03, 2018 at 15:09 GMT_

----

Yeah, I mean the user of StructOpt, the developer of the cli.


---

_Comment by @epage on 2021-12-09 16:11_

<a href="https://github.com/porglezomp"><img src="https://avatars.githubusercontent.com/u/1690225?v=4" align="left" width="48" height="48" hspace="10"></img></a> **Comment by [porglezomp](https://github.com/porglezomp)**
_Saturday May 05, 2018 at 05:51 GMT_

----

I wanted something very similar to these proposals, and was actually slightly surprised when nothing like them was available. I'd imagine that you could have something like:
```rust
#[derive(StructOpt)]
#[structopt(flag_group)]
enum Mode {
    #[structopt(short = "s", long = "stack")]
    Stack,
    #[structopt(short = "q", long = "queue")]
    Queue,
}

#[derive(StructOpt)]
#[structopt(name = "letter")]
struct Opt {
    #[structopt(from_flag_group)]
    mode: Mode,
}
```
and then you can either use `letter --stack` or `letter --queue`.
This would generate a required group, and unpack the options into the enum.


---

_Comment by @epage on 2021-12-09 16:11_

<a href="https://github.com/sunshowers"><img src="https://avatars.githubusercontent.com/u/180618?v=4" align="left" width="48" height="48" hspace="10"></img></a> **Comment by [sunshowers](https://github.com/sunshowers)**
_Wednesday Mar 10, 2021 at 00:31 GMT_

----

One random thought I had is that I've noticed some remarkable similarities between structopt's and serde's data models. It seems like subcommands are equivalent to serde's [externally tagged](https://serde.rs/enum-representations.html#externally-tagged) enums, while mutually exclusive options, if modeled through enums, are similar to [untagged](https://serde.rs/enum-representations.html#untagged) enums. It may be worth aligning with serde's design in this respect.

One thing it suggests is the possibility for internally tagged enums, which may be reflected in the CLI as e.g. `path/to/binary --command foo --arg1 x --arg2 y`.


---

_Comment by @epage on 2021-12-09 16:11_

<a href="https://github.com/nathan-at-least"><img src="https://avatars.githubusercontent.com/u/4369700?v=4" align="left" width="48" height="48" hspace="10"></img></a> **Comment by [nathan-at-least](https://github.com/nathan-at-least)**
_Friday Aug 27, 2021 at 16:04 GMT_

----

I just skimmed this because I want this feature. IIUC in https://github.com/TeXitoi/structopt/issues/104#issuecomment-386308405 it *is* possible in `clap`, correct? If so, then only the `structopt` API needs to be defined. I'd be happy with the suggestion in https://github.com/TeXitoi/structopt/issues/104#issuecomment-386246259 .

For my current case, I simply want exclusive options: either `--verbose` or `--quiet` or neither, but not both. Is this already expressable in `structopt`?


---

_Comment by @epage on 2021-12-09 16:11_

<a href="https://github.com/TeXitoi"><img src="https://avatars.githubusercontent.com/u/5787066?v=4" align="left" width="48" height="48" hspace="10"></img></a> **Comment by [TeXitoi](https://github.com/TeXitoi)**
_Friday Aug 27, 2021 at 16:18 GMT_

----

@nathan-at-least https://github.com/TeXitoi/structopt/blob/master/examples/group.rs is almost what you want.

And what you want is:

```rust
use structopt::StructOpt;

#[derive(Debug, StructOpt)]
struct Opt {
    #[structopt(short, long, group = "verbosity")]
    verbose: bool,
    #[structopt(short, long, group = "verbosity")]
    quiet: bool,
    #[structopt(short, long)]
    name: Option<String>,
}

fn main() {
    let opt = Opt::from_args();
    println!("{:?}", opt);
}
```


---

_Comment by @epage on 2021-12-09 16:15_

Going to close this in favor of #2621 

---

_Closed by @epage on 2021-12-09 16:15_

---

_Label `A-derive` added by @epage on 2021-12-09 16:41_

---
