```yaml
number: 1344
title: Last argument needs to be quoted
type: issue
state: closed
author: marceloboeira
labels: []
assignees: []
created_at: 2018-09-24T09:46:47Z
updated_at: 2019-03-25T20:42:32Z
url: https://github.com/clap-rs/clap/issues/1344
synced_at: 2026-01-12T16:14:10Z
```

# Last argument needs to be quoted

---

_@marceloboeira_



### Rust Version

`rustc 1.27.0 (3eda71b00 2018-06-19)`

### Affected Version of clap
```
"checksum clap 2.32.0 (registry+https://github.com/rust-lang/crates.io-index)" = "b957d88f4b6a63b9d70d5f454ac8011819c6efa7727858f458ab71c756ce2d3e"
```

### Expected Behavior Summary
My command line tool is supposed to work like this
`foo -u <user> <command>`
e.g.:
`foo -u jack echo s`
`foo -u paul ls -al`

So, I need to get options such as user, but the `<command>` itself, I need to be the `rest` of the args.

### Actual Behavior Summary
The code below, results on a behavior where I can't get the value of `<command>` unless is quoted:

`foo -u jack echo s` -> `error: Found argument 's' which wasn't expected, or isn't valid in this context`

whereas:
`foo -u jack 'echo s'` works fine.

Is there anyway of avoiding the quotes?


### Sample Code or Link to Sample Code

```rust
    let matches = App::new("foo")
                          .version("0.1")
                          .arg(Arg::with_name("user")
                               .short("u")
                               .long("user")
                               .required(true)
                               .takes_value(true))
                          .arg(Arg::with_name("command")
                               .help("The command to run")
                               .required(true)
                               .takes_value(true)).get_matches();
```


---

_Referenced in [marceloboeira/awsudo#3](../../marceloboeira/awsudo/issues/3.md) on 2018-09-24 09:47_

---

_Comment by @marceloboeira on 2019-03-25 13:14_

Any updates? Is the project still maintained?

---

_Comment by @marceloboeira on 2019-03-25 20:42_

If it's of anyones interest:


By default, clap will only parse any argument once. This means that in `-u jack echo s`, it will parse `-u jack` as the "user" option, `echo` as the "command" argument, and have an argument `s` that it doesn't know what to do with (hence it "wasn't expected").

I'm assuming you want all of the non-option arguments instead of just one, in which case setting `.multiple(true)` on your non-option argument ("command") is what you want. In that case, it will parse all remaining, non-option arguments as "command", and give you a list back with each of the arguments (which you can deal with as you please):


```rust
let matches = App::new("foo")
    .version("0.1")
    .arg(
        Arg::with_name("user")
            .short("u")
            .long("user")
            .required(true)
            .takes_value(true),
    )
    .arg(
        Arg::with_name("command")
            .help("The command to run")
            .required(true)
            .takes_value(true)
            .multiple(true),
    )
    // parse as if program ran as:   foo -u jack echo s
    .get_matches_from(&["foo", "-u", "jack", "echo", "s"]);

// get command arguments
let command: Vec<&str> = matches.values_of("command").unwrap().collect();
println!("{:?}", command); // ["echo", "s"]
```

More info: https://stackoverflow.com/questions/55345730/how-can-i-prevent-the-last-argument-from-needing-to-be-quoted-with-clap/55345899#55345899

---

_Closed by @marceloboeira on 2019-03-25 20:42_

---
