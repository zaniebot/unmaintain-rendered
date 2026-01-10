---
number: 509
title: Where to find Subcommand argument values
type: issue
state: closed
author: Cyberunner23
labels: []
assignees: []
created_at: 2016-05-20T03:07:51Z
updated_at: 2018-08-02T03:29:49Z
url: https://github.com/clap-rs/clap/issues/509
synced_at: 2026-01-10T01:26:30Z
---

# Where to find Subcommand argument values

---

_Issue opened by @Cyberunner23 on 2016-05-20 03:07_

I'm working on a tool which takes values from an argument inside a subcommand and I'm running into the issue where clap is not giving me the value of the argument. Here is some test code that shows the issue.

```
extern crate clap; 
use clap::{Arg, App, SubCommand};

fn main() {

    let matches = App::new("rustTest2")
        .subcommand(SubCommand::with_name("sub")
            .about("A subcommand")
            .arg(Arg::with_name("title")
                 .short("t")
                 .long("title")
                 .help("Sets the title of the card.")
                 .takes_value(true)
                 .required(true)))
        .get_matches();

    if matches.is_present("title") {
        println!("title present!");
    }

    println!("Value of --title/-t: {}", matches.value_of("title").unwrap_or("Nothing!"));

}
```

Running the program with the following command: 

```
rustTest2 sub --title somevalue
```

will not stop the program because `somevalue` is not missing but when looking for it in the program it is missing. Essentially returning

```
Value of --title/-t: Nothing!
```

when executing the command.


---

_Comment by @kbknapp on 2016-05-20 14:38_

This is because the `title` argument is part of the `sub` matches, and not the top level matches. The reason for the tree hierarchy is to allow multiple subcommands to have the same arguments and not clobber each other.

If you do:

``` rust
extern crate clap; 
use clap::{Arg, App, SubCommand};

fn main() {

    let matches = App::new("rustTest2")
        .subcommand(SubCommand::with_name("sub")
            .about("A subcommand")
            .arg(Arg::with_name("title")
                 .short("t")
                 .long("title")
                 .help("Sets the title of the card.")
                 .takes_value(true)
                 .required(true)))
        .get_matches();

    match matches.subcommand() {
        ("sub", Some(sub_matches)) => {
            if sub_matches.is_present("title") {
                println!("title present!");
                println!("Value of --title/-t: {}", sub_matches.value_of("title").unwrap_or("Nothing!"));
            },
         _ => {}, // no subcommand present, or one not tested for
    }    
}
```

Edit:

In this case you could also take advantage of [default_values](http://kbknapp.github.io/clap-rs/clap/struct.Arg.html#method.default_value) where the default is `"Nothing!"` :wink:


---

_Label `T: RFC / question` added by @kbknapp on 2016-05-20 14:40_

---

_Label `W: 2.x` added by @kbknapp on 2016-05-20 14:40_

---

_Renamed from "Subcommand argument value missing when checking for values." to "Where to find Subcommand argument values" by @kbknapp on 2016-05-20 14:40_

---

_Comment by @Cyberunner23 on 2016-05-20 14:42_

Thanks! I must of missed that...


---

_Closed by @Cyberunner23 on 2016-05-20 14:42_

---

_Comment by @kbknapp on 2016-05-20 14:43_

No worries!

Also look at the [`subcommand_name()`](http://kbknapp.github.io/clap-rs/clap/struct.ArgMatches.html#method.subcommand_name) and [`subcommand_matches()`](http://kbknapp.github.io/clap-rs/clap/struct.ArgMatches.html#method.subcommand_matches) methods if you only have a single subcommand or only want to check for a single subcommand.


---
