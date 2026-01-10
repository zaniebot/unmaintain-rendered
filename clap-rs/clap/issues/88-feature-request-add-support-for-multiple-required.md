---
number: 88
title: "Feature request: Add support for multiple required arguments, e.g. [-u <mode> <file> <mime>]"
type: issue
state: closed
author: Byron
labels: []
assignees: []
created_at: 2015-04-29T16:26:57Z
updated_at: 2018-08-02T03:29:39Z
url: https://github.com/clap-rs/clap/issues/88
synced_at: 2026-01-10T01:26:23Z
---

# Feature request: Add support for multiple required arguments, e.g. [-u <mode> <file> <mime>]

---

_Issue opened by @Byron on 2015-04-29 16:26_

I need to support an optional argument like the one noted in the subject, but was unable to achieve this. Apparently there is no support for multiple arguments of a single flag. `docopt` can do this though, and I believe its a useful feature to have.

A workaround I tried was to use the `Arg::requires()` method to enforce flags to appear together. However, a doubly-linked requires definition doesn't seem to work. For example, in the following excerpt I want `-u` to appear with `-f` and vice-versa.

``` bash
➜  google-apis-rs git:(clap) ✗ make groupsmigration1-cli-cargo ARGS="run -- archive insert groupid -h"
cd gen/groupsmigration1-cli && cargo run -- archive insert groupid -h
     Running `target/debug/groupsmigration1 archive insert groupid -h`
groupsmigration1-archive-insert
Inserts a new mail into the archive of the Google group.

USAGE:
    groupsmigration1 archive insert [FLAGS] [OPTIONS] <group-id>

FLAGS:
    -h, --help       Prints help information
    -v, --version    Prints version information

OPTIONS:
    -f <file>       The file to upload
    -u <mode>       Specify which file to upload [values: resumable simple]
    -o <out>        Specify the file into which to write the programs output
    -p <v>...       Set various fields of the request structure

POSITIONAL ARGUMENTS:
    group-id    The group ID
```

In code, this looks like this (example is manufactured, and not actual code, might not compile)

``` Rust
let mut uarg = Arg::with_name("u").required(false).takes_value(true);
uarg = uarg.requires("file");
farg = Arg::with_name("file")
           .short("f")
           .required(false)
           .requires("u")
           .takes_value(true));
```

Maybe I am doing it wrong though. The code above results in runtime errors like this:

``` bash
➜  google-apis-rs git:(clap) ✗ make groupsmigration1-cli-cargo ARGS="run -- archive insert groupid -u simple -f file"
cd gen/groupsmigration1-cli && cargo run -- archive insert groupid -u simple -f file
     Running `target/debug/groupsmigration1 archive insert groupid -u simple -f file`
One or more required arguments were not supplied
USAGE:
    groupsmigration1 archive insert [FLAGS] [OPTIONS] -f <file>  <group-id>
For more information try --help
```

In any case, it would be very helpful **to print which dependencies are actually missing** :).
Thanks for your help !


---

_Referenced in [Byron/google-apis-rs#92](../../Byron/google-apis-rs/issues/92.md) on 2015-04-29 16:28_

---

_Comment by @kbknapp on 2015-04-29 18:38_

Ok, I'll have look at how best (most ergonomically) do this. In the mean time, the `requires()` is correct. Here's what I just tested (using `Arg::from_usage()` just for brevity)

``` rust
let modes = ["resumable", "simple"];
let _ = App::new("app")
                  .arg(Arg::from_usage("-u [mode] 'the mode'").possible_values(&modes).requires("file"))
                  .arg(Arg:from_usage("-f [file] 'the file'").requires("mode"))
                  .arg_from_usage("<group-id> 'the group id'")
                  .get_matches();
```

This works by if the user does a `-f` they must also do `-u` and vice versa. And `group-id` is required regardless.

Also, in ref to your last statement, that's exactly what I plan with #82 :+1: It does this minimally right now (i.e. based off your current usage in the example, `-f` and `<group-id>` are required (it doesn't list `-u` because it already matched that). Come to think of it, what does the definition of the `<group-id>` look like? that may be the issue you're experiencing.

But I plan on making it much clearer in the very near future.


---

_Comment by @Byron on 2015-04-29 19:23_

Thanks for much for having a look ! I will try that once again, tomorrow ([note to self](https://github.com/Byron/google-apis-rs/issues/81)). Must have been some silly mistake on my side, amplified by #82 - if it would tell you the possible values of the required flag, it would have been clear which name to put there.
'<group-id>' is just required and has no flag. `takes_value(true)` might also be called on it - but I don't know for sure as the code that does it is [this one](https://github.com/Byron/google-apis-rs/blob/clap/src/mako/cli/lib/argparse.mako#L260).

I will be working like a maniac until 15th of May, and am sure the few remaining kinks will be etched out  until then. Generally `clap` is great, so are the docs ... and you even do videos :) ! Well done - I'd be surprised if `clap` wouldn't become the defacto standard command-line parser for Rust.


---

_Label `feature request` added by @kbknapp on 2015-04-30 01:55_

---

_Referenced in [clap-rs/clap#89](../../clap-rs/clap/issues/89.md) on 2015-04-30 07:28_

---

_Closed by @kbknapp on 2015-04-30 15:15_

---

_Comment by @kbknapp on 2015-04-30 15:16_

Mistakenly closed by commit message


---

_Reopened by @kbknapp on 2015-04-30 15:16_

---

_Closed by @kbknapp on 2015-04-30 23:41_

---

_Comment by @kbknapp on 2015-05-01 02:25_

This is closed with v0.7.1 on crates.io

See the new docs or #89 for details on the changes - it should help out! Also, I'm quite happy with the new smarter usage strings which take the current attempted usage as a template for what's required, not just the defaults.


---

_Comment by @Byron on 2015-05-02 07:43_

Thank you, this defintely works now. I could also verify that `-r foo bar baz` is working similarly to `-r foo -r bar -r baz`. Good work.
Only the smarter usage strings didn't work for me yet, I have made [an issue](https://github.com/kbknapp/clap-rs/issues/96) to show what I mean.


---
