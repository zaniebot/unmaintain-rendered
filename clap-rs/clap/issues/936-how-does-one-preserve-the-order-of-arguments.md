```yaml
number: 936
title: How does one preserve the order of arguments?
type: issue
state: closed
author: fschutt
labels: []
assignees: []
created_at: 2017-04-20T15:06:20Z
updated_at: 2018-08-02T03:30:05Z
url: https://github.com/clap-rs/clap/issues/936
synced_at: 2026-01-12T16:14:10Z
```

# How does one preserve the order of arguments?

---

_@fschutt_

### Actual Behavior Summary

I want my application to have arguments in a certain order. Clap reorders them.
Example:
```
App::new("test")
       .args_from_usage("<database> --database, -d '[required] Database to connect to'
                              <user> --user, -u '[required] Database user'
                              [host] --host, -h 'Server host (default: localhost)'
                              <FILE> '[required] file to import'");
```
Output when called directly:
```
error: The following required arguments were not provided:
    <FILE>
    --database,
    --user,

USAGE:
    test [FLAGS] <FILE> --database, --user,

For more information try --help
```

### Expected Behavior Summary

Please make the arguments appear in order, not random or based on "if the value is required". Or at least have an option to display them in order. How it should be:

```
USAGE:
    test  --database, --user, [FLAGS] <FILE>
```
Where `[FLAGS]` would be `--host` in this case.

Now I have to enter: `test --host 192.168.1.20 myfile --user "test" --database "123"`.

How I want it to be: `test --user "test" --database "123" --host 192.168.1.20 myfile`

Either that or I am using the library wrong. I can't seem to have any way to control the order of my arguments, it's always random. The only way to have in-order arguments is making every argument optional. But then I have to do manual parsing, at which point the library sort-of loses its sense.

### Rust Version

rustc 1.18.0-nightly (7627e3d31 2017-04-16)

### Affected Version of clap

clap 2.23.3

---

_Comment by @TedDriggs on 2017-04-20 16:36_

@kbknapp can correct me, but I'm guessing this is "by design" and that clap is designed to be opinionated on how arguments are ordered/declared. It does seem strange to me that the required items _must_ come first.

---

_Comment by @fschutt on 2017-04-20 16:49_

I just noticed that the arguments are sorted alphabetically by their name. This is not documented anywhere. 

Like this:

```
    let matches = App::new("test")
                    .arg(Arg::with_name("a")
                         .required(true))
                    .arg(Arg::with_name("b"))
                    .arg(Arg::with_name("c")
                         .required(true))
           .get_matches();
```

These values will be sorted. If this is by design, then I propose a change, because it is confusing.


EDIT: clap seems to sort the errors by their `App::new()` value, but the arguments by their short name value. If no short name value is present, it will put it at the front. If required / optional arguments are mixed, the optional ones come first. I think that most of the time it would be OK to just have the argument in the order that they are in the file.

However, I am not sure if this would break any scripts that rely on a clap-based application ([t-rex](https://github.com/pka/t-rex) for example).

---

_Comment by @kbknapp on 2017-04-20 17:33_

@sharazam clap orders them alphabetically (by name) unless you specify a [manual ordering](https://docs.rs/clap/2.23.3/clap/struct.Arg.html#method.display_order). You can also tell clap, ["Use the order I define them in"](https://docs.rs/clap/2.23.3/clap/enum.AppSettings.html#variant.DeriveDisplayOrder) instead of manually putting in an order for each one.

---

_Comment by @kbknapp on 2017-04-20 17:36_

@TedDriggs 

>  It does seem strange to me that the required items must come first.

This is only true of positional arguments (with two [[1](https://docs.rs/clap/2.23.3/clap/enum.AppSettings.html#variant.AllowMissingPositional)] exceptions [[2](https://docs.rs/clap/2.23.3/clap/struct.Arg.html#method.last)]). However, required positional arguments do *not* need to come before all other arguments, only before non-required positional arguments. This is simply because clap has no way of distinguishing positional arguments, except by index that they arrive after the binary name.

---

_Label `T: RFC / question` added by @kbknapp on 2017-04-20 17:38_

---

_Renamed from "Clap does not keep order of arguments" to "How does on preserve the order of arguments?" by @kbknapp on 2017-04-20 17:38_

---

_Renamed from "How does on preserve the order of arguments?" to "How does one preserve the order of arguments?" by @kbknapp on 2017-04-20 17:43_

---

_Comment by @fschutt on 2017-04-20 19:40_

@kbknapp Alright. Sorry, I seem to have missed those two arguments. Thanks for your answer.

---

_Closed by @fschutt on 2017-04-20 19:40_

---

_Comment by @kbknapp on 2017-04-20 20:23_

@sharazam no worries! :smile:

---
