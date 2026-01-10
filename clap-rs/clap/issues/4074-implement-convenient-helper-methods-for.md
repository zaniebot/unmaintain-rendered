---
number: 4074
title: "Implement convenient helper methods for `PathBufValueParser` to reduce boilerplate"
type: issue
state: open
author: SupImDos
labels:
  - C-enhancement
  - A-builder
  - E-help-wanted
  - E-easy
  - ":money_with_wings: $5"
assignees: []
created_at: 2022-08-13T15:45:54Z
updated_at: 2023-06-26T14:20:45Z
url: https://github.com/clap-rs/clap/issues/4074
synced_at: 2026-01-10T01:27:50Z
---

# Implement convenient helper methods for `PathBufValueParser` to reduce boilerplate

---

_Issue opened by @SupImDos on 2022-08-13 15:45_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

3.2.17

### Describe your use case

#### Summary
Popular command-line argument parser libraries for other languages implement validation helpers for paths for more convenient and terse CLI construction.

#### Example
For example, Python's [`click`](https://github.com/pallets/click) provides a [`click.Path`](https://click.palletsprojects.com/en/8.1.x/api/?highlight=click%20path#click.Path) type for validation, with which you can do things like:
```python
click.Path(exists=True, file_okay=True, dir_okay=False, writable=False, readable=True)
```

#### Why?
This would allow for a reduction of boilerplate for common use-cases (i.e., existing files, existing directories, etc), and ultimately provide a more friendly and convenient "batteries-included" developer experience.

#### TOCTOU?
The TOCTOU issue will still be present for the validated paths, but in my opinion this enhancement would still allow for a more convenient "fail-fast" approach for CLI applications.

### Describe the solution you'd like

#### Existing Helpers
The existing integer value parsers appear to already provide a [`.range(x..y)`](https://github.com/clap-rs/clap/blob/v3.2.17/src/builder/value_parser.rs#L1103) helper method to simplify parsing of narrowed integer ranges.

This allows for usage such as (as per the [tutorial](https://docs.rs/clap/latest/clap/_derive/_tutorial/index.html#validated-values)):
```rust
#[derive(Parser)]
#[clap(author, version, about, long_about = None)]
struct Cli {
    /// Network port to use
    #[clap(value_parser = clap::value_parser!(u16).range(1..))]
    port: u16,
}
```

#### Possible Solution
I would like similar helper methods for *other* types, in this case specifically the [`PathBufValueParser`](https://github.com/clap-rs/clap/blob/v3.2.17/src/builder/value_parser.rs#L768).

For example, this could allow usage such as the follows (exact form up for discussion):

```rust
#[derive(Parser)]
#[clap(author, version, about, long_about = None)]
struct Cli {
    /// Example 1: existing file only
    #[clap(value_parser = clap::value_parser!(PathBuf).exists(PathType::File)]
    file: PathBuf,

    /// Example 2: existing directory only
    #[clap(value_parser = clap::value_parser!(PathBuf).exists(PathType::Dir)]
    dir: PathBuf,

    /// Example 3: existing file or directory
    #[clap(value_parser = clap::value_parser!(PathBuf).exists(PathType::FileOrDir)]
    file_or_dir: PathBuf,
}
```

#### Future Scope
There are other less common use cases - such as checking for readability and writability, or checking that a file/directory does *not* currently exist - that could also be implemented (possibly with more enums, or method chaining?).

For example:
```rust
/// Example: Possible method chaining approach
clap::value_parser!(PathBuf).exists(PathType::File).permissions(Permissions::Read)
```

### Alternatives, if applicable

_No response_

### Additional Context

_No response_

---

_Label `C-enhancement` added by @SupImDos on 2022-08-13 15:45_

---

_Label `A-builder` added by @epage on 2022-08-13 17:04_

---

_Label `S-waiting-on-design` added by @epage on 2022-08-13 17:04_

---

_Comment by @epage on 2022-08-14 02:57_

Huh, I don't think I ever created an issue for this but I had designed this with click's features in mind.  Specifically, we have `ValueParserFactory` which will let us define field types that you can just put in your struct and clap will then automatically do the right thing, including
- `FileReader`
  - Eagerly opens the file
  - Supports `-`
- `FileWriter`
  - Lazily opens the file
  - Supports `-`
  - Option to do atomic writes (default?)
  - Eagerly error if parent directory doesn't exist (maybe an option to create it instead?)

Also supporting customization on `Path` would be useful.

To allow extra dependencies to be pulled in and for faster iteration, we might start this off in a `clap_fs` crate

---

_Comment by @epage on 2022-09-09 19:24_

Something I saw that is important to keep in mind for `-` support from [clig.dev](https://clig.dev/#help)

> If your command is expecting to have something piped to it and stdin is an interactive terminal, display help immediately and quit. This means it doesn’t just hang, like cat. Alternatively, you could print a log message to stderr.

---

_Referenced in [pacak/bpaf#61](../../pacak/bpaf/issues/61.md) on 2022-09-16 16:51_

---

_Label `S-waiting-on-design` removed by @epage on 2022-09-20 14:44_

---

_Label `E-easy` added by @epage on 2022-09-20 14:44_

---

_Label `:money_with_wings: $5` added by @epage on 2022-09-20 14:44_

---

_Label `E-help-wanted` added by @epage on 2022-09-20 14:44_

---

_Referenced in [clap-rs/clap#4564](../../clap-rs/clap/issues/4564.md) on 2022-12-20 02:50_

---

_Referenced in [clap-rs/clap#4643](../../clap-rs/clap/issues/4643.md) on 2023-01-16 16:46_

---

_Referenced in [clap-rs/clap#4686](../../clap-rs/clap/pulls/4686.md) on 2023-01-30 04:34_

---

_Comment by @max-sixty on 2023-06-09 03:11_

IIUC, it's worth checking out https://github.com/aj-bagwell/clio, which does some of these...

---

_Referenced in [aj-bagwell/clio#11](../../aj-bagwell/clio/issues/11.md) on 2023-06-09 14:50_

---

_Comment by @epage on 2023-06-09 14:57_

Thanks for letting us know of that!

I've created
- aj-bagwell/clio#12
- aj-bagwell/clio#11

which I *think* would bring it to parity with clap

---

_Comment by @aj-bagwell on 2023-06-21 09:20_

Thanks for the great ideas and opening the issues. I have implemented most of them.

### [parser/validator](https://docs.rs/clio/latest/clio/clapers/struct.OsStrParser.html)
I have added a bunch of validation options to [parser/validator](https://docs.rs/clio/latest/clio/clapers/struct.OsStrParser.html)

There is also [`InputPath`](https://docs.rs/clio/latest/clio/struct.InputPath.html) which require the path to exist and be a readable file, and [`OutputPath`](https://docs.rs/clio/latest/clio/struct.OutputPath.html) which requres the parent directory to exist and the file to be writeable if it exists.

```
#[derive(Parser)]
#[clap(author, version, about, long_about = None)]
struct Cli {
    /// Example 1: existing file or pipe only, must not be interactive tty
    #[clap(value_parser = clap::value_parser!(PathBuf).exists().is_file().not_tty())]
    file: ClioPath,

    /// Example 2: existing directory only
    #[clap(value_parser = clap::value_parser!(PathBuf).exists().is_dir()]
    dir: PathBuf,

    /// Example 3: existing file or directory
    #[clap(value_parser = clap::value_parser!(PathBuf).exists()]
    file_or_dir: PathBuf,
}
```


### [Input](https://docs.rs/clio/latest/clio/struct.Input.html)
- Eagerly opens the file
- Supports `-`
- has [`is_tty`](https://docs.rs/clio/latest/clio/struct.Input.html#method.is_tty) method to check for interactive terminals

```
#[derive(Parser)]
struct Cli {
    /// must be existing readable file, default to stdin
    #[clap(value_parser, default = "-")]
    input: Input,
}
```

### [OutputPath](https://docs.rs/clio/latest/clio/struct.OutputPath.html)
- Defers opening/creating the file
- Supports `-`
- Option to do [atomic](https://docs.rs/clio/0.3.2/clio/clapers/struct.OsStrParser.html#method.atomic) writes (not default)
- Eagerly error if parent directory doesn't exist
- has [`is_tty`](https://docs.rs/clio/latest/clio/struct.OutputPath.html#method.is_tty) method to check for interactive terminals

```
#[derive(Parser)]
struct Cli {
    /// If path is a directory output will be file with default name in that directory, must not be interactive tty
    /// if the file exists it must be writeable
    #[clap(value_parser = clap::value_parser!(OutputPath).not_tty().default_name("out.log"), default = "-")]
    output: OutputPath,
}

fn main() -> Result<()> {
    let mut cli = Cli::parse();
    if cli.output.is_tty() {
        // enable colors, or show an error or whatever
    }
    cli.output.create()?
}
```

### [Output](https://docs.rs/clio/latest/clio/struct.Output.html)
- Eagerly opens/creats the file (but using [atomic](https://docs.rs/clio/0.3.2/clio/clapers/struct.OsStrParser.html#method.atomic) will mean it will clean up any partialy written files on exit)
- Supports `-`
- Option to do [atomic](https://docs.rs/clio/0.3.2/clio/clapers/struct.OsStrParser.html#method.atomic) writes (not default)
- has [`is_tty`](https://docs.rs/clio/latest/clio/struct.OutputPath.html#method.is_tty) method to check for interactive terminals

```
#[derive(Parser)]
struct Cli {
    /// Atomic output using tempfile and atomic rename.
    #[clap(value_parser = clap::value_parser!(Output).atomic().default_name("out.log"), default = "-")]
    output: Output,
}
```

Pull requests to clio are always welcome, or if this looks useful enough to get pulled into clap proper that would be excelent too.


---

_Comment by @epage on 2023-06-23 17:59_

clap v4.3.6 is released which sugests using clio.

I'd love to hear people's thoughts on it over time to decide where to go from here.

Personally, I would lean towards this being kept as a separate crate so we have more flexibility regarding the implementation.  If anything (and with the author's go ahead), I could see it being renamed to `clap_io` / `clap_fs` and moved into the clap repo or at least `clap-rs` org once we feel comfortable with it.

Personally, I'd prefer not to have HTTP support.  imo arguments that accept URLs should explicitly opt-in, at minimum.

---

_Comment by @aj-bagwell on 2023-06-24 11:50_

Thank you for the official recommendation @epage!

I agree that HTTP support should be opt in which is why it is behind a feature that is off by default. For most uses piping via curl is a better choice. I added it because I needed to stream to S3 which needs to be told the content size when starting the upload, also knowing the size of the input turned out to give nicer progress bars.

If enough people show interest and want to help work on it then we can look into moving the code into the clap-rs.

---

_Comment by @epage on 2023-06-26 14:20_

> I agree that HTTP support should be opt in which is why it is behind a feature that is off by default. For most uses piping via curl is a better choice. I added it because I needed to stream to S3 which needs to be told the content size when starting the upload, also knowing the size of the input turned out to give nicer progress bars.

What I mean is it should be opt-in on a per-argument basis rather than on an application level basis with feature flags

---

_Referenced in [paritytech/polkadot-sdk#1641](../../paritytech/polkadot-sdk/pulls/1641.md) on 2024-11-18 11:03_

---
