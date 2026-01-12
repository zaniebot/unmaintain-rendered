```yaml
number: 4746
title: "Empty pathbufs are no longer accepted by `default_value`"
type: issue
state: closed
author: mejrs
labels:
  - C-bug
assignees: []
created_at: 2023-03-05T13:46:08Z
updated_at: 2024-03-12T21:11:09Z
url: https://github.com/clap-rs/clap/issues/4746
synced_at: 2026-01-12T16:14:16Z
```

# Empty pathbufs are no longer accepted by `default_value`

---

_@mejrs_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.69.0-nightly (5243ea5c2 2023-02-20)

### Clap Version

4.1.8

### Minimal reproducible code

```rust
use clap::Parser;
use std::path::PathBuf;

#[derive(Debug, Parser)]
struct Cli {
    #[clap(short, long, default_value = "")]
    pub output: PathBuf,
}

fn main() {
    let dummy: Vec<String> = vec![];
    let cli = Cli::parse_from(dummy);
    assert_eq!(cli.output, PathBuf::new());
}
```


### Steps to reproduce the bug with the above code

cargo run

### Actual Behaviour
```
thread 'main' panicked at 'Argument `output`'s default_value="" failed validation: error: a value is required for '--output <OUTPUT>' but none was supplied

For more information, try '--help'.
', C:\Users\bruno\.cargo\registry\src\github.com-1ecc6299db9ec823\clap-4.1.8\src\builder\debug_asserts.rs:867:13
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
```
### Expected Behaviour

I expect it to run successfully, like it did with clap 3.

### Additional Context

_No response_

### Debug Output

_No response_

---

_Label `C-bug` added by @mejrs on 2023-03-05 13:46_

---

_Comment by @epage on 2023-03-06 14:53_

When comparing 1:1 with clap 3, this does reproduce:
```rust
use clap::Parser;
use std::path::PathBuf;

#[derive(Debug, Parser)]
struct Cli {
    #[clap(short, long, default_value = "", value_parser)]
    pub output: PathBuf,
}

fn main() {
    let dummy: Vec<String> = vec![];
    let cli = Cli::parse_from(dummy);
    assert_eq!(cli.output, PathBuf::new());
}
```

The defaulted `value_parser` attribute opts you into the clap v4 behavior.

The assumption made with the default `PathBuf` value parser is that empty paths are not used, so the old `forbid_empty_values` is automatically enabled.  Changing the default to `.` would fix this.

If you have a use case for accepting an empty `PathBuf`, I'd be open to hearing it.

You can workaround this by
- Changing the type to `OsString`
- Adding the attribute `value_parser = OsStringValueParser::new().map(PathBuf::from))`

---

_Comment by @mejrs on 2023-03-06 22:44_

> If you have a use case for accepting an empty PathBuf, I'd be open to hearing it.

My program produces files and needs to save them somewhere, and uses either
- the value given on the command line, if given
- an environment variable, if present
- the current directory (that's where the empty path comes in)

I think that's a reasonable use case unless there's something I'm missing something about pathbufs and/or clap...

I actually ended up needing more than this for other reasons, so I went with a `impl FromArgMatches for Newtype ` implementation 🙂 


---

_Comment by @epage on 2023-03-06 22:49_

So your program accepts `""` on the command line for CWD?  That seems awkward to enter compared to `.` and not very clear in `--help`.

> I actually ended up needing more than this for other reasons, so I went with a impl FromArgMatches for Newtype implementation 🙂

Wow, what was it that led you to doing that?

---

_Comment by @mejrs on 2023-03-06 23:05_

I decided I also wanted to keep track of *why* it's using the path, [so I used an enum instead of PathBuf](https://github.com/mejrs/rs3cache/commit/371f264f891ae96b4142efe82072a52527de6627#diff-b2bbdb7136980ab236d2398b541fff08911fc2b21fed5a1ee6de17b4b29a3e59).

> So your program accepts "" on the command line for CWD?

My intention was to not require the argument at all.

---

_Comment by @epage on 2023-03-27 21:02_

As this is no longer blocking you and the other case had an alternatives (e.g. using `"."` as the default), I'm going to go ahead and close this.

If someone else finds a use case that is block on this, we can always re-open and re-evaluate.

---

_Closed by @epage on 2023-03-27 21:02_

---

_Referenced in [clap-rs/clap#5368](../../clap-rs/clap/issues/5368.md) on 2024-02-18 12:21_

---

_Comment by @epage on 2024-02-19 18:26_

From #5368

@akanalytics

> Sorry - this is a duplicate of #4746. But I think this is still a bug, that seems to trip people up. Paths are such a common command line parameter.
> 
> Usecase: lots of code can use an empty pathbuf as an alternative to Option. Rust explicitly offers a new() constructor which does exactly this, and it reduces the boilerplate in many places.
> 
> Using "." does not have the same meaning. (ls . and ls '' give different results on my linux system).
> 
> Although personally Im not a fan of empty PathBufs, Rust clearly does support them, and command line tools such as the Linux 'ls' command does distinguish between empty paths and "." paths.



---

_Comment by @epage on 2024-02-19 18:28_

> Rust explicitly offers a new() constructor which does exactly this, and it reduces the boilerplate in many places.

I'm not too sure how `Path::new("")` is relevant as this is focused on how people parse their command line.  Could you clarify that?

> Using "." does not have the same meaning. (ls . and ls '' give different results on my linux system).

`touch ""` fails on my system, so using it as a path without further context seems incorrect which is what this is trying to help users with.

---

_Comment by @akanalytics on 2024-02-24 12:38_

Clarifying #5368

Firstly, clap is an excellent library, and it's trivial for me to work around the "forbidding empty pathbuf default" in my particualr case. However, to me clap is not just a beginner's library, its almost *THE* standard parsing library. And as such I feel it should support everything rust supports. So these are thoughts, in case this is ever explored further.

a) **Empty PathBuf is a genuine concept/thing**
Rust supports the empty Path/PathBuf. Is is used in the default constructor PathBuf::new() - note: no quotes, though equivalent to PathBuf::from(""). 
One can debate whether an empty path is a wise decision or not. I can't say Im a fan personally, but Rust has adopted it.

Java and Posix both use it too, and have explicit logic around it, that Rust replicates. 
Note: Posix says that "" must fail to resolve on its own. This is why touch "" fails.

Thus it can only really be used in combination with other path components. So the use case is always going to be something like
```mycommand --basepath=something --file1=filename1.txt --file2=filename2.txt``` 
where basepath is used as a base or parent directory to files 1 and 2.

```InPath --path=xyz``` 
..a hypothetical command line utility that checks whether xyz is in the PATH environment variable would be another example or usecase. These are path fragments (colon separated on unix), and "" and "." should be searchable, though are functionally equivalent in PATH.

b) **Dot and Empty are not the same**
.. though they are very very close.
PathBuf::new() has behaviour different from PathBuf::from(".")
path.parent() will return Some(empty-path) on occasion. Mixing the two (empty and '.') seems wrong. 

A command line parameter of base_path = "" (empty) will satisfy

```PathBuf::from("filename.txt").parent() == Some(base_path)```

but base_path = "." will not. 

Forbidding empty_path doesnt quite work for the hypothetical  InPath command (above) either.

I think a) and b) means that using a default of "." is not quite correct, though this works in 95% of cases. 

c) **OsString is a poor substitute (due to serialization and communication of intent)**
OsString as a substitute for PathBuf has the disadvantage that it does not serialize (in a platform compatible way). Serialization of clap structures matters as I think progress is being made towards combining command line args and config files (read using Serde).
Often an additional macro or serialization is added to a clap structure to make this work. We're not quite there yet, but I can see several approaches being explored.
Having a path or path component stored as a PathBuf rather than an OsString surely clarifies, and makes handling this in a cross-platform manner easier (in the future).

Summary: 
My particular use case is fine, I can work around. Im happy.
The above are just thoughts for the future, if this comes up again. 
The approach to leave this as it is for now, seems fine.




---

_Comment by @epage on 2024-02-26 21:40_

> However, to me clap is not just a beginner's library, its almost THE standard parsing library. And as such I feel it should support everything rust supports. So these are thoughts, in case this is ever explored further.

This also means that we have to move carefully. We have a lot of users but only a couple will interact on any issue and we could change things without anyone noticing until its too late.

> Mixing the two (empty and '.') seems wrong.

I'm still confused as to why discussing the APIs for `Path` affects what we expose as the CLI.  Also not the strongest argument to focus on things being "wrong" as that can vary by people.

> OsString as a substitute for PathBuf has the disadvantage that it does not serialize (in a platform compatible way).

Could you clarify what you mean here?

> Having a path or path component stored as a PathBuf rather than an OsString surely clarifies, and makes handling this in a cross-platform manner easier (in the future).

You can still use a `PathBuf` and just provide a custom `value_parser` that builds on top of the `OsString` value parser (`value_parser = clap::builder::OsStringValueParser::new().map(|os| PathBuf::from(os))`)

---

_Comment by @akanalytics on 2024-03-02 12:33_

Moving carefully -> agree 100%.

"Seems wrong" perhaps better phrased as "because of point b regarding difference in behaviour between empty and dot".  Apologies.

OsString as a substitute, and serialization ->
If you store default configuration as data types similar/identical to clap command line types, (and some helper configuration crates do this btw), then OsString does not serialize in a platform independent way. 

For example if the default for a file/path in a configuration file is stored as a PathBuf in a struct: this serializes to/from string text in a  platform independent manner for utf8 pathbufs (via serde).

But OsString serializes (via serde) rather badly.
```filename = "foo"```
  serializes as 
```"filename": { "Unix": [102, 111, 111] }``` 

This is because OsString is a platform dependent enum.
Very few would want a config file that has entries like the above. Most would want simply
filename: "foo" (in the equivalent yaml/json/toml format)

 custom value_parser => sounds promising, I will look into doing that.


---

_Comment by @epage on 2024-03-12 21:11_

> OsString as a substitute, and serialization 

As I mentioned, you can still make your field a `PathBuf` by using a custom `value_parser` that starts with receiving an `OsStr` from clap and turns it into a `PathBuf` in your own way.

---
