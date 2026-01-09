---
number: 472
title: Help message prints argument name twice if no value name was given
type: issue
state: closed
author: klingtnet
labels:
  - C-bug
  - A-help
assignees: []
created_at: 2016-04-01T11:13:09Z
updated_at: 2018-08-02T03:29:49Z
url: https://github.com/clap-rs/clap/issues/472
synced_at: 2026-01-07T13:12:19-06:00
---

# Help message prints argument name twice if no value name was given

---

_Issue opened by @klingtnet on 2016-04-01 11:13_

Argument names are printed as values in the help message if no `value_name` of `value_names` was given for the CLI argument. The problem is located in [`src/args/help_writer.rs` line 117](https://github.com/kbknapp/clap-rs/blob/master/src/args/help_writer.rs#L117). I tried to replace the line with: `try!(write!(w, "<{}>", self.a.name()));` but this breaks some other tests.
If no `value_name` was set the `long` or `short` argument name is printed twice before the arguments value.

``` rust
extern crate clap;

fn main() {
    let addr = clap::Arg::with_name("address")
                   .long("address")
                   .short("a")
                   .takes_value(true)
                   .default_value("0.0.0.0")
                   .group("osc")
                   .help("Address to listen on for OSC messages.");
    let sr = clap::Arg::with_name("sample-rate")
                 .long("sample-rate")
                 .short("s")
                 .takes_value(true)
                 .default_value("48000")
                 .possible_values(&["44100", "48000", "88200", "96000"])
                 .help("Playback sample-rate");
    let ports = clap::Arg::with_name("ports")
                    .long("ports")
                    .short("p")
                    .required(true)
                    .takes_value(true)
                    .number_of_values(2)
                    .value_names(&["in", "out"])
                    .group("osc")
                    .help("OSC listening and send port.");
    let args = clap::App::new("ytterbium")
                   .version("0.1.0")
                   .author("Andreas Linz <klingt.net@gmail.com>")
                   .arg(addr)
                   .arg(ports)
                   .arg(sr);
    let mut buf: Vec<u8> = Vec::with_capacity(128);
    let mut out = ::std::io::stdout();
    args.write_help(&mut out);
}
```

This prints the following _help_ message:

``` sh
ytterbium 0.1.0
Andreas Linz <klingt.net@gmail.com>

USAGE:
    ytterbium [FLAGS] [OPTIONS] --ports <in> <out>

FLAGS:
    -h, --help       Prints help information
    -V, --version    Prints version information

OPTIONS:
    -a, --address --address <address>            Address to listen on for OSC messages. [default: 0.0.0.0] 
    -p, --ports <in> <out>             OSC listening and send port.
    -s, --sample-rate --sample-rate <sample-rate>    Playback sample-rate [default: 48000]  [values: 44100, 48000, 88200, 96000]
```

If you look at the `OPTIONS` you can see that `--address` is printed twice as well as `--sample-rate`.
Adding `value_name`s fixes the problem:

``` rust
    let addr = clap::Arg::with_name("address")
// ...
                   .takes_value(true)
                   .value_name("ip-address")
// ...
    let sr = clap::Arg::with_name("sample-rate")
// ...
                 .takes_value(true)
                 .value_name("sample-rate")
// ...
```

``` sh
...
OPTIONS:
    -a, --address <ip-address>         Address to listen on for OSC messages. [default: 0.0.0.0] 
    -p, --ports <in> <out>             OSC listening and send port.
    -s, --sample-rate <sample-rate>    Playback sample-rate [default: 48000]  [values: 44100, 48000, 88200, 96000]
```

If this is intended behavior then please add a note in the documentation that setting `value_name`s is mandatory for arguments that `takes_value`.


---

_Label `T: bug` added by @kbknapp on 2016-04-03 04:14_

---

_Label `P1: urgent` added by @kbknapp on 2016-04-03 04:14_

---

_Label `C: args` added by @kbknapp on 2016-04-03 04:14_

---

_Label `C: help message` added by @kbknapp on 2016-04-03 04:14_

---

_Label `W: 2.x` added by @kbknapp on 2016-04-03 04:14_

---

_Comment by @kbknapp on 2016-04-03 04:15_

This is a bug, thanks for pointing it out! I'll get it fixed real quick and put out the updated version. Thanks!


---

_Comment by @kbknapp on 2016-04-03 04:52_

Once #474 merges I'll put out the new version on crates.io


---

_Comment by @klingtnet on 2016-04-03 13:26_

Thank you for the quick fix!


---

_Closed by @klingtnet on 2016-04-03 13:26_

---

_Referenced in [TeXitoi/structopt#173](../../TeXitoi/structopt/issues/173.md) on 2019-06-26 05:56_

---
