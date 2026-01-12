```yaml
number: 2940
title: No way to really hide an argument
type: issue
state: closed
author: ctrlcctrlv
labels: []
assignees: []
created_at: 2021-10-25T11:28:01Z
updated_at: 2021-10-26T21:26:19Z
url: https://github.com/clap-rs/clap/issues/2940
synced_at: 2026-01-12T16:14:14Z
```

# No way to really hide an argument

---

_@ctrlcctrlv_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Clap Version

2.33.3

### Describe your use case

In #1820 it's recommended to create two `Arg`'s to solve the issue. Fine.

But then, it shows up in USAGE, even if not in OPTIONS:

```
USAGE:
    glif2svg <input> --input <input_file>

FLAGS:
    -h, --help       Prints help information
    -V, --version    Prints version information

OPTIONS:


ARGS:
    <input>    The path to the input file.
```

This is of course documented in `Arg::hidden`:

```
NOTE: This does not hide the argument from usage strings on error
```

### Describe the solution you'd like

Please either fix `Arg::hidden` or add a flag to hide from usage. Possible names being `Arg::really_hidden`, `Arg::easter_egg` (also another entirely valid reason to want this, what fun is Rust if we can't hide an easter egg or two in our binaries) or `Arg::usage_hidden`.

### Alternatives, if applicable

N/A

### Additional Context

N/A

---

_Label `T: new feature` added by @ctrlcctrlv on 2021-10-25 11:28_

---

_Label `C: usage strings` added by @pksunkara on 2021-10-25 11:31_

---

_Comment by @epage on 2021-10-25 13:11_

Could you provide a code sample for creating the help output you listed?

---

_Comment by @ctrlcctrlv on 2021-10-25 13:38_

Certainly—

```rust
fn main() -> ! {
    let matches = App::new("glif2svg")
        .setting(AppSettings::ArgRequiredElseHelp)
        .version("0.1.0")
        .author("Fredrick R. Brennan <copypasteⒶkittens⊙ph>; MFEK Authors")
        .about("Convert between glif to SVG")
        .arg(Arg::with_name("input_file")
            .short("in")
            .long("input")
            .takes_value(true)
            .hidden(true)
            .required(true))
        .arg(Arg::with_name("input")
            .index(1)
            .takes_value(true)
            .help("The path to the input file.")
            .required_unless("input_file"))
        .get_matches();
}
```

---

_Comment by @ctrlcctrlv on 2021-10-25 14:07_

Okay, I figured it out. But, `clap`'s behavior is still strange.

```rust
fn main() {
    let matches = App::new("glif2svg")
        .setting(AppSettings::ArgRequiredElseHelp)
        .version("0.1.0")
        .author("Fredrick R. Brennan <copypasteⒶkittens⊙ph>; MFEK Authors")
        .about("Convert between glif to SVG")
        .arg(Arg::with_name("input_file")
            .short("in")
            .long("input")
            .takes_value(true)
            .conflicts_with("input")
            .hidden(true))
        .arg(Arg::with_name("input")
            .index(1)
            .help("The path to the input file.")
            .conflicts_with("input_file")
            .required_unless("input_file"))
        .arg(Arg::with_name("output_file")
            .short("out")
            .long("output")
            .takes_value(true)
            .conflicts_with("output")
            .required_unless("output")
            .display_order(1)
            .help("The path to the output file.\n\n\n"))
        .arg(Arg::with_name("output")
            .index(2)
            .hidden(true))
        .arg(Arg::with_name("no-transform")
            .short("T")
            .long("no-transform")
            .help("Don't put transform=\"translate(…)\" in SVG"))
        .arg(Arg::with_name("no-glif-metrics")
            .short("M")
            .long("no-glif-metrics")
            .help("Don't consider glif's height/width when writing SVG, use minx/maxx/miny/maxy"))
        .arg(Arg::with_name("fontinfo")
            .short("F")
            .long("fontinfo")
            .takes_value(true)
            .help("fontinfo file (for metrics, should point to fontinfo.plist path)\n\n"))
        .get_matches();
}
```

Giving:

```
glif2svg 0.1.0
Fredrick R. Brennan <copypasteⒶkittens⊙ph>; MFEK Authors
Convert between glif to SVG

USAGE:
    glif2svg [FLAGS] [OPTIONS] <input> --output <output_file>

FLAGS:
    -h, --help               Prints help information
    -M, --no-glif-metrics    Don't consider glif's height/width when writing SVG, use minx/maxx/miny/maxy
    -T, --no-transform       Don't put transform="translate(…)" in SVG
    -V, --version            Prints version information

OPTIONS:
    -o, --output <output_file>    The path to the output file.
                                  
                                  
    -F, --fontinfo <fontinfo>     fontinfo file (for metrics, should point to fontinfo.plist path)
                                  

ARGS:
    <input>    The path to the input file.
```

The issue here comes down to `Arg::required` and `Arg::required_unless`. You must _remove both functions from the one you want to not show in the documentation_. Consider this variation, which panics:

```
thread 'main' panicked at 'Found positional argument which is not required with a lower index than a required positional argument: "input" index 1', /home/fred/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-2.33.3/src/app/parser.rs:643:21
stack backtrace:
   0: rust_begin_unwind
             at /rustc/ae90dcf0207c57c3034f00b07048d63f8b2363c8/library/std/src/panicking.rs:517:5
   1: std::panicking::begin_panic_fmt
             at /rustc/ae90dcf0207c57c3034f00b07048d63f8b2363c8/library/std/src/panicking.rs:460:5
   2: clap::app::parser::Parser::verify_positionals
             at /home/fred/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-2.33.3/src/app/parser.rs:643:21
   3: clap::app::parser::Parser::app_debug_asserts
             at /home/fred/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-2.33.3/src/app/parser.rs:146:17
   4: clap::app::parser::Parser::get_matches_with
             at /home/fred/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-2.33.3/src/app/parser.rs:894:23
   5: clap::app::App::get_matches_from_safe_borrow
             at /home/fred/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-2.33.3/src/app/mod.rs:1639:25
   6: clap::app::App::get_matches_from
             at /home/fred/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-2.33.3/src/app/mod.rs:1519:9
   7: clap::app::App::get_matches
             at /home/fred/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-2.33.3/src/app/mod.rs:1460:9
   8: glif2svg::main
             at ./src/main.rs:47:19
   9: core::ops::function::FnOnce::call_once
             at /rustc/ae90dcf0207c57c3034f00b07048d63f8b2363c8/library/core/src/ops/function.rs:227:5
note: Some details are omitted, run with `RUST_BACKTRACE=full` for a verbose backtrace.

shell returned 101
```

```diff
@@ -54,21 +54,22 @@ fn main() {
             .long("input")
             .takes_value(true)
             .conflicts_with("input")
+            .required_unless("input")
             .hidden(true))
         .arg(Arg::with_name("input")
+            //.index(1)
+            .help("The path to the input file."))
-            .index(1)
-            .help("The path to the input file.")
-            .conflicts_with("input_file")
-            .required_unless("input_file"))
         .arg(Arg::with_name("output_file")
             .short("out")
             .long("output")
             .takes_value(true)
-            .conflicts_with("output")
-            .required_unless("output")
             .display_order(1)
             .help("The path to the output file.\n\n\n"))
         .arg(Arg::with_name("output")
+            .conflicts_with("output")
+            .required_unless("output_file")
+            //.index(2)
-            .index(2)
             .hidden(true))
         .arg(Arg::with_name("no-transform")
             .short("T")
```

Putting `Arg::required_unless` on either one should have the same result, but doesn't. So this is now a combination bug/FR.

---

_Comment by @epage on 2021-10-25 14:14_

Yes, as is, the usage shows all required arguments and collapses the optional ones.

And it doesn't surprise me that the parser is straightforward in its handling of positional arguments.

So if I understand, this boils down to request either
- Hiding or collapsing an item in the usage
- Allowing multiple optional positional arguments in a scenario like this
  - With this requiring us defining what scenarios these would be so we can prevent ambiguities

---

_Comment by @ctrlcctrlv on 2021-10-25 14:18_

Personally I'd prefer #1820 just be solved. ~~It wasn't mentioned by anyone there but the case of input/output args is a _classic_ case where you can provide either positional or flag (`-i`, `-o`) because users forget which programs require which (this being free software, there are plenty of programs which barf on one or the other, we shouldn't do that).~~ (Silly me, was mentioned, I'm just tired.)

---

_Comment by @pksunkara on 2021-10-26 21:09_

> Hiding or collapsing an item in the usage

If the arg is required, why do we want to hide it from usage? If the arg is required depending on certain situations we do hide it. (Check [this](https://github.com/clap-rs/clap/issues/2940#issuecomment-950965939) comment. Ignore the panic because that's fixed in v3).

I am leaning towards closing this issue because the usage/required interaction.

---

_Comment by @ctrlcctrlv on 2021-10-26 21:26_

If the panic is fixed there's nothing left to discuss :)

---

_Closed by @ctrlcctrlv on 2021-10-26 21:26_

---
