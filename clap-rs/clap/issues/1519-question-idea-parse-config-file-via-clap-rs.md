---
number: 1519
title: "Question/Idea: Parse config file via clap-rs?"
type: issue
state: closed
author: michael-smythe
labels: []
assignees: []
created_at: 2019-07-16T03:17:42Z
updated_at: 2020-04-09T08:06:21Z
url: https://github.com/clap-rs/clap/issues/1519
synced_at: 2026-01-10T01:26:56Z
---

# Question/Idea: Parse config file via clap-rs?

---

_Issue opened by @michael-smythe on 2019-07-16 03:17_

I have a program which looks like this:

```
let matches = App::new("Great Program")
                 .version("0.0.1")
                 .arg(Arg::with_name("great_option_a")
                    .short("a")
                    .long("great_option_a")
                    .value_name("A_NAME")
                    .help("Specify great_option_a's name")
                    .required(true)
                 )
                 .arg(Arg::with_name("great_option_b")
                    .short("b")
                    .long("great_option_b")
                    .value_name("B_NAME")
                    .help("Specify great_option_b's name")
                    .required(true)
                 )
                 ==== TRUNCATED ====
                 .arg(Arg::with_name("great_option_z")
                    .short("z")
                    .long("great_option_z")
                    .value_name("Z_NAME")
                    .help("Specify great_option_z's name")
                    .required(true)
                 )
                 .get_matches();
```
The program works great, because its a great program, but something that I would like to be able to do is use a config file instead of specify all of these options via the cli at runtime. Is there a way to leverage the backend logic that gets built by clap where it parses and validates all or some of the options that I would normally have to type as arguments? For instance if I was to make a file that contained the following:
```
cat ./greatest_config
great_option_a="A"
great_option_b="B"
==== TRUNCATED ====
great_option_z="Z"
```
Would it be possible using clap to have something which makes the following commands equivelant:
```
./great_program -a A -b B ==== TRUNCATERD ==== -z Z
./great_program --config_file=./greatest_config
```
 Or is the correct answer that I would have to build a seperate parsing and validating system for when the option of a config file is preseted and ensure that the logic matches the logic setup by the clap parsing engine? If this a completely idiotic question please just point me in the right direction, if this is not a completely dumb question and it does not currently exist inside of clap is it a feature the clap-rs project would be interested in integrating? Also if there is a better venue for this conversation please let me know. Thanks in advance for any help/insight you may be able to provide!

---

_Comment by @comfortablynick on 2019-07-18 21:35_

I had this same idea recently. I am using using clap for my cli and reading from a toml config file, and they share some options. Serde converts the toml file into a separate struct, so for now I'll have to validate them separately. But it would be great to have some type of integration.

---

_Comment by @ebkalderon on 2019-11-07 03:11_

@comfortablynick Would you be able to use `structopt` with its `clap` backend and then derive `Serialize` and `Deserialize` on the struct to achieve the same effect? This won't fully solve this issue, but it might make your current experience a bit more streamlined.

---

_Comment by @davidMcneil on 2019-11-26 13:18_

[Here](https://github.com/davidMcneil/unified-dynamic-rust-app-config) is proof-of-concept work on parsing a config file.

---

_Label `P4: nice to have` added by @CreepySkeleton on 2020-02-01 13:05_

---

_Label `T: new feature` added by @CreepySkeleton on 2020-02-01 13:05_

---

_Label `W: after v3 release` added by @CreepySkeleton on 2020-02-01 13:05_

---

_Label `W: after v3 release` removed by @CreepySkeleton on 2020-02-07 07:14_

---

_Comment by @pksunkara on 2020-04-09 08:06_

#1693 should be fixing this. I would like to close this issue in favour of that. Please reopen with arguments if I missed something.

---

_Closed by @pksunkara on 2020-04-09 08:06_

---

_Referenced in [clap-rs/clap#1693](../../clap-rs/clap/issues/1693.md) on 2020-04-09 13:31_

---
