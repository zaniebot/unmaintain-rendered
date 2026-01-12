```yaml
number: 1595
title: "[brain-dump]: Build commandline objects in code, for testing"
type: issue
state: closed
author: matthiasbeyer
labels: []
assignees: []
created_at: 2019-11-08T16:04:21Z
updated_at: 2019-11-27T13:21:02Z
url: https://github.com/clap-rs/clap/issues/1595
synced_at: 2026-01-12T16:14:11Z
```

# [brain-dump]: Build commandline objects in code, for testing

---

_@matthiasbeyer_

**Disclaimer**: This is just a brain-dump, I have not looked at other comments/issues/prs whatsoever, so feel free to close this and call me a moron if this already exists or is not possible, ... I'm just dumping my brain here.

---

It would be nice to have an option to build the `ArgMatches` objects that get returned from clap when parsing the commandline in code.
My idea is to generate a `ArgMatches` object, (where the build process actually fails if it does not meet the spec written before, maybe even at compiletime if that is possible), and then being able to feed that `ArgMatches` object to my program like it would be feeded when calling the actual binary...

with that in place, it would be extremely easy to test commandline applications. Right now [I manually call the actual binary](https://git.imag-pim.org/imag/tree/tests/ui/src/imag_header.rs#n30), which is obviously another solution to the problem and also works quite well, but having an option to test the program via generating `clap` objects seems also to be a really nice way of doing things!

---

Maybe such a thing already exists, I don't know... maybe I just missed something. But _maybe_ this is an idea for you... 

Have a nice weekend! :heart: 

---

_Comment by @RaboliotLeGris on 2019-11-16 00:31_

I'm trying to build ArgMatches to tests some functions too. And for now the only thing I've found is that the struct implements the Default traits that allow us to easily built an empty ArgMatches (`clap::ArgMatches::default()`). 

After some digging:
Create an empty ArgMatches it's fairly doable. Then I tried to populate the `args` value that is an Hashmap (It was close) and then you need to build a `MatchArgs` (That implements the Default traits o/), that is not expose in the `libs.rs`, so we can't build it :(


---

_Comment by @igozali on 2019-11-25 22:26_

Couldn't you lift the creation of `clap::App` into a function, and use the `App` object to parse your command line args in main and test? Something like this:

```rust
use clap::{App, Arg};

fn make_app<'a, 'b>() -> App<'a, 'b> {
    App::new("my-app").arg(
        Arg::with_name("name")
            .short("-n")
            .long("--name")
            .takes_value(true),
    )
}

fn main() {
    let app = make_app();
    let matches = app.get_matches();
    println!("name: {}", matches.value_of("name").unwrap_or("None"));
}

#[cfg(test)]
mod test {
    use super::make_app;
    #[test]
    fn test_parse() {
        let app = make_app();
        let matches = app.get_matches_from(&["./my-app", "-n", "first"]);
        assert_eq!(matches.value_of("name").unwrap(), "first");
    }
}
```

---

_Comment by @RaboliotLeGris on 2019-11-26 20:06_

Yep! If we use `get_matches_from`, we can build whatever we want and it solves my tests issues.

---

_Comment by @matthiasbeyer on 2019-11-27 13:21_

As said in the preface, I'm dumb. This is already doable right now with `get_matches_from` of course.

---

_Closed by @matthiasbeyer on 2019-11-27 13:21_

---
