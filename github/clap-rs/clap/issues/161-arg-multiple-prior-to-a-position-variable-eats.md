---
number: 161
title: "Arg::multiple prior to a position variable eats the position variable as well"
type: issue
state: closed
author: bbatha
labels:
  - C-bug
assignees: []
created_at: 2015-07-16T20:06:09Z
updated_at: 2015-07-17T02:18:36Z
url: https://github.com/clap-rs/clap/issues/161
synced_at: 2026-01-07T13:12:19-06:00
---

# Arg::multiple prior to a position variable eats the position variable as well

---

_Issue opened by @bbatha on 2015-07-16 20:06_

I have this matches:

``` Rust
fn main() {
    let list_conflicts = vec!["inline", "command", "host", "host-set", "nossh-ping"];
    let matches = App::new("disburse")
                        .version(&crate_version!()[..])
                        .author("Ben Batha")
                        .arg(Arg::with_name("config")
                            .help("Location of the disburse config file")
                            .short("c")
                            .long("config")
                            .required(true)
                            .takes_value(true)
                            .empty_values(false)
                        )
                        .arg(Arg::with_name("inline")
                            .help("Prefix each host's output with its hostname")
                            .long("inline")
                            .short("i")
                        )
                        .arg(Arg::with_name("list")
                            .help("List all available targets")
                            .short("l")
                            .long("list")
                            .conflicts_with_all(&list_conflicts)
                        )
                        .arg(Arg::with_name("command")
                            .help("Command to run. May also come from STDIN")
                        )
                        .arg(Arg::with_name("host")
                            .help(concat!("Specify a host to run on. ",
                                  "Specify more than one host with a comma separated list"))
                            .long("host")
                            .takes_value(true)
                            .multiple(true)
                            .empty_values(false)
                            .conflicts_with("host-set")
                            .required(true)
                        )
                        .arg(Arg::with_name("host-set")
                            .help("Specify a named set of hosts to run on")
                            .long("host-set")
                            .takes_value(true)
                            .empty_values(false)
                            .conflicts_with("host")
                            .required(true)
                        )
                        .arg(Arg::with_name("representative")
                            .help("Pick a representative host to represent each host set")
                            .short("r")
                            .long("representative")
                            .requires("host-set")
                            .conflicts_with("host")
                        )
                        .arg(Arg::with_name("threads")
                             .help("Number of threads to execute on")
                             .long("num-threads")
                             .short("n")
                             .takes_value(true)
                             .empty_values(false)
                         )
                        .get_matches();
}
```

With the attempted run of:
"disburse --config hosts.json --inline --host host1 --host host2 -n 2 'sleep 10'"

matches.value_of("command") is populated however with:

"disburse --config hosts.json --inline --host host1 --host host2 'sleep 10'"

matches.value_of("command") is not populated.


---

_Comment by @kbknapp on 2015-07-16 21:53_

This is because there isn't a way to determine when the multiple values stops and the next argument starts, unless you state a max or fixed number of values. `Arg::multiple(true)` by default says, "this argument can appear multiple times, OR have multiple values." So this is also a valid way to run your program:

```
$ disburse --config hosts.json --inline --host host1 host2 -- 'sleep 10'
```

There are a few ways to tackle your problem depending on your particular situation. If the argument that accepts multiple values has a fixed or maximum number of values you can specify that by setting `Arg::number_of_values()` or `Arg::max_values()` in conjunction with `Arg::multiple(true)`. But that value must be `> 1`, which may or may not help you. But if for example you knew there always had to be exactly two hosts, and only two, you could set `Arg::number_of_values(2)` and your above invocation line would work fine.

The other more general option is to use `--` and place all positional arguments after that. 

```
$ disburse --config hosts.json --inline --host host1 --host host2 -- 'sleep 10'

OR

$ disburse --config hosts.json --inline --host host1 host2 -- 'sleep 10'
```

Because `--` is somewhat of a unix standard and means, "everything that follows is a positional"

Hopefully this helps, if not please let me know :) And thanks for taking the time to file the issue! :+1: 


---

_Label `T: RFC / question` added by @kbknapp on 2015-07-17 00:19_

---

_Label `C: args` added by @kbknapp on 2015-07-17 00:19_

---

_Comment by @kbknapp on 2015-07-17 00:28_

I also just noticed you are using `concat!()` but you can escape newlines to visually align help strings, or recently added in `clap` v1.1.0 you can use `{n}` inside help strings to insert newlines into help strings and ensure they're all still visually aligned when the user does a `--help`

Example:

``` rust
// Arg with long help string which needs
// a new line printed to the user
Arg::with_name("long_help")
    .short("l")
    .help("This is a super long line that should be printed{n}one two lines to the user, yet still aligned nicely")

// Arg that I just visually want to put on two lines 
// in the source, yet printed as one line to the user
Arg::with_name("shorter")
    .short("s")
    .help("This is a kind of long line which I only want \
           on two lines in the source code.")
```

Assuming I had those in a proper program and ran `--help` it would look like this:

```
$ myprogram --help
myprogram v1.0
Does awesome things
Me, me@mail.com

Usage:
    myprogram [FLAGS]

FLAGS:
    -h, --help    Displays this message
    -l            This is a super long line that should be printed
                  one two lines to the user, yet still aligned nicely
    -s            This is a kind of long line which I only want on two lines in the source code.
```


---

_Comment by @bbatha on 2015-07-17 00:45_

That's an excellent tip with the strings, thanks!

As for the argument problem. What I'd really like is to force it to be multiple option flags with a fixed number of inputs with something like required_inputs(1) to each one so this is legal:
--host foo --host bar
And this is illegal:
--host foo bar

Which should be sufficient to disambiguate between having multiple inputs for an option and positional arguments.


---

_Comment by @kbknapp on 2015-07-17 01:05_

@bbatha 

Actually I think you're right, and that just pointed out a logic bug in `clap`! So `Arg::number_of_values()` is supposed to allow a qty of `>0`, **not** `>1`. Once I fix this bug, add `Arg::number_of_values(1)` but keep `Arg::multiple()` for your `host` argument. This should have the exact effect you're looking for:

`--host host1 --host host2` is **legal**
`--host host1 host2` is **illegal**

I'll post back here once it's fixed.


---

_Label `T: bug` added by @kbknapp on 2015-07-17 01:05_

---

_Label `D: easy` added by @kbknapp on 2015-07-17 01:05_

---

_Label `P3: want to have` added by @kbknapp on 2015-07-17 01:05_

---

_Label `T: RFC / question` removed by @kbknapp on 2015-07-17 01:05_

---

_Assigned to @kbknapp by @kbknapp on 2015-07-17 01:05_

---

_Referenced in [clap-rs/clap#162](../../clap-rs/clap/pulls/162.md) on 2015-07-17 01:35_

---

_Comment by @kbknapp on 2015-07-17 01:36_

Once Travis passes #162 I'll merge into master and upload v1.1.2 to Crates.io


---

_Closed by @kbknapp on 2015-07-17 01:38_

---

_Comment by @kbknapp on 2015-07-17 01:43_

v1.1.2 on crates.io is now good to use :)


---
