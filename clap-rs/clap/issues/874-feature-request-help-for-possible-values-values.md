```yaml
number: 874
title: "Feature request: Help for `possible_values` values"
type: issue
state: closed
author: wfraser
labels:
  - A-help
assignees: []
created_at: 2017-02-25T07:08:54Z
updated_at: 2018-08-02T03:30:02Z
url: https://github.com/clap-rs/clap/issues/874
synced_at: 2026-01-12T16:14:10Z
```

# Feature request: Help for `possible_values` values

---

_@wfraser_

I'm converting some old code of mine with hand-rolled arg parsing code to use clap (because it's awesome, thank you!), but this is one thing I really miss. We have `Arg::possible_values` to show what valid values for an option are, but no built-in way of showing help text for them, other than just putting it in the option's help text.

Given the current code, the best I've managed to do is hack around it with something like:
```
clap::App::new("foo")
    .arg(Arg::with_name("errors")
        .long("errors")
        .takes_value(true)
        .value_name("error-policy")
        .possible_values(&["halt", "skip", "replace"])
        .default_value("halt")
        .next_line_help(true)
        .hide_possible_values(true)
        .help(concat!("What to do when an error is encountered in input.\n",
             "halt: exit on errors\n",
             "skip: skip over the erroneous input\n",
             "replace: substitute with the encoding's replacement character")))
    .get_matches();
```

which outputs the following help text:

```
foo

USAGE:
    foo [OPTIONS]

FLAGS:
    -h, --help       Prints help information
    -V, --version    Prints version information

OPTIONS:
        --errors <error-policy>
            What to do when an error is encountered in input.
            halt: exit on errors
            skip: skip over the erroneous data
            replace: substitute with the encoding's replacement character
             [default: halt]
```

The code is kind of ugly, I have to have the set of values listed in two places, and the output isn't as nice as the rest of what clap generates. :(

I propose adding something to `Arg`, either a separate help function like `fn values_help(self, help_texts: &[&'b str]) -> Self` where you give help text for each value in the same order you specified them in `possible_values`, or a separate function that lets you specify both as a tuple maybe: `fn possible_values_with_help(self, values: &[(&'b str, &'b str)]) -> Self`.

This could produce nice tab-aligned output like this:

```
foo

USAGE:
    unicoder.exe [OPTIONS]

FLAGS:
    -h, --help       Prints help information
    -V, --version    Prints version information

OPTIONS:
        --errors <error-policy>    What to do when an error is encountered in input.
                halt               exit on errors [default]
                skip               skip over the erroneous data
                replace            substitute with the encoding's replacement character
```



---

_Comment by @kbknapp on 2017-02-27 01:24_

Thanks for the detailed suggestion! I'm not against adding this feature, although there are a few features I need to knock out first. I'll also need to play with the implementation a bit, because what I'd really like to do is combine the two somehow so it doesn't impact the performance of those not using this feature. Granted two string slices vs one isn't that big a deal, but still. Perhaps something like:

```rust
clap::App::new("foo")
    .arg(Arg::with_name("errors")
        .long("errors")
        .takes_value(true)
        .value_name("error-policy")
        .possible_values(&[
            "halt:exit on errors", 
            "skip:skip over the erroneous data", 
            "replace:substitute with the encoding's replacement character"])
        .default_value("halt")
        .possible_values_contain_help(true) // treats `:` as a value/help sep
        .next_line_help(true)
        .hide_possible_values(true)
        .help("What to do when an error is encountered in input."))
    .get_matches();
```

---

_Label `C: help message` added by @kbknapp on 2017-02-27 01:25_

---

_Label `D: intermediate` added by @kbknapp on 2017-02-27 01:25_

---

_Label `P4: nice to have` added by @kbknapp on 2017-02-27 01:25_

---

_Label `T: new feature` added by @kbknapp on 2017-02-27 01:25_

---

_Label `W: 2.x` added by @kbknapp on 2017-02-27 01:25_

---

_Comment by @kbknapp on 2017-04-05 18:27_

I've done some thinking on this, and with the recent feature of distinguishing between `-h`/`--help` I'm going to close this.

I'm still not totally opposed to adding this feature, it's just a somewhat niche case that I don't personally have the bandwidth to maintain right now. There are still questions like, should `:` be unconditionally the separator? I'm less inclined to add something like `Arg::possible_value_help_separator`, etc. In fact, I'd like to pair down `App` and `Arg`s API space, as the docs are becoming massive and I'd like to find a way to "declutter" the niche settings in 3x.

So this is closed. But i'm willing to consider PRs on the matter if they're limited in scope.

This works on clap 2.23.1, by the way.

```rust
clap::App::new("foo")
    .arg(Arg::with_name("errors")
        .long("errors")
        .takes_value(true)
        .value_name("error-policy")
        .possible_values(&[ "halt",  "skip",  "replace"])
        .default_value("halt")
        .hide_possible_values(true)
        .help("What to do when an error is encountered in input.")
        .long_help("\
            What to do when an error is encountered in input. \n\
                halt:          exit on errors\n\
                skip:          skip over the erroneous input\n\
                replace:       substitute with the encoding's replacement character")))
    .get_matches();
```


---

_Closed by @kbknapp on 2017-04-05 18:27_

---
