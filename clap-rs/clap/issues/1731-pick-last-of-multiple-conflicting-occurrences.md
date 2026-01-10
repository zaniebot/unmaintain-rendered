---
number: 1731
title: pick last of multiple, conflicting occurrences
type: issue
state: closed
author: wookietreiber
labels: []
assignees: []
created_at: 2020-03-08T12:26:28Z
updated_at: 2020-03-11T13:38:07Z
url: https://github.com/clap-rs/clap/issues/1731
synced_at: 2026-01-10T01:27:04Z
---

# pick last of multiple, conflicting occurrences

---

_Issue opened by @wookietreiber on 2020-03-08 12:26_

### Describe your use case

Consider a `remove` app with the two flags `--interactive` and `--non-interactive`.

As with `rm`, it's useful to to have `alias remove='remove --interactive'` in your shell to avoid accidents. However, in some situations, overriding this default interactive behavior with `remove --non-interactive` is useful when you know what you're doing. The shell would expand the alias to `remove --interactive --non-interactive`.

Using **clap** I'm assuming an `ArgGroup` would be the way to go. However, there is no easy way (that I know of) to allow multiple, conflicting occurrences, while you're only interested in the **last** occurrence.

### Describe the solution you'd like

```rust
let interactive = Arg::with_name("interactive");
let non_interactive = Arg::with_name("non-interactive");

let interactivity = ArgGroup::with_name("interactivity")
    .args(&["interactive", "non-interactive"])
    .multiple(true)
    .required(false);

let app = App::new("remove")
    .arg(interactive)
    .arg(non_interactive)
    .group(interactivity);

let args = app.get_matches();

let interactive: bool = args.last_variant_of("interactivity", |variant| match variant {
    "interactive" => true,
    "non-interactive" => false,
    _ => false,
});
```

The function would have a signature like this:

```rust
impl ArgMatches {
    fn last_variant_of<T, F>(&self, group: &str f: F) -> T
    where
        F: Fn(&str) -> T,
    {
        unimplemented!()
    }
}
```

### Alternatives, if applicable

Nasty workaround without group and with manually inspecting indices the likes of:

```rust
let interactive = Arg::with_name("interactive")
    .takes_value(false)
    .multiple(true);

let non_interactive = Arg::with_name("non-interactive")
    .takes_value(false)
    .multiple(true);

let app = App::new("remove")
    .arg(interactive)
    .arg(non_interactive);

let args = app.get_matches();

let interactive_last_index = args
    .indices_of("interactive")
    .map(|indices| indices.last())
    .flatten();

let non_interactive_last_index = args
    .indices_of("non-interactive")
    .map(|indices| indices.last())
    .flatten();

let interactivity = match (interactive_last_index, non_interactive_last_index) {
    (Some(interactive), Some(non_interactive)) => {
        if interactive > non_interactive {
            true
        } else {
            false
        }
    }
    (Some(_), None) => true,
    (None, Some(_)) => false,
    (None, None) => false,
};
```

### Additional context

Other applicable use cases are `--quiet` vs `--verbose` or different output modes based on an `enum`. I guess `ArgGroup` is not strictly necessary for this feature, e.g. with `--output-mode variant-of-enum` and `matches.last_variant_of("output-mode").unwrap_or_default()`.

---

_Label `T: new feature` added by @wookietreiber on 2020-03-08 12:26_

---

_Label `C: settings` added by @pksunkara on 2020-03-08 12:27_

---

_Label `T: new setting` added by @pksunkara on 2020-03-08 12:27_

---

_Label `T: new feature` removed by @pksunkara on 2020-03-08 12:27_

---

_Comment by @CreepySkeleton on 2020-03-08 13:19_

(Wow, it's apparently the first time an OP decided to *actually* fill out our issue template. Kudos for that!)

`--interactive` and `--no-interactive` are not actually *conflicting* but rather *overriding*. Conflicting options are options that cannot appear together, and yours very well can. Overriding is when one option cancels out some other option; in your case, `--interactive` and `--non-interactive` simply override each other. 

`clap` already supports this use case through `App::overrides_with`:
```rust
use clap::{App, Arg};

fn build_app() -> App<'static> {
    App::new("prog")
        .arg(Arg::with_name("interactive")
            .long("--interactive")
            .overrides_with("non-interactive"))
        .arg(Arg::with_name("non-interactive")
            .long("--non-interactive")
            .overrides_with("interactive"))
}

fn main() {
    let res = build_app()
        .get_matches_from(&["test", "--interactive", "--non-interactive"]);

    assert!(res.is_present("non-interactive"));
    assert!(!res.is_present("interactive"));

    let res = build_app()
        .get_matches_from(&["test", "--non-interactive", "--interactive"]);

    assert!(!res.is_present("non-interactive"));
    assert!(res.is_present("interactive"));
}
```

---

_Label `C: settings` removed by @pksunkara on 2020-03-08 13:33_

---

_Label `T: new setting` removed by @pksunkara on 2020-03-08 13:33_

---

_Comment by @CreepySkeleton on 2020-03-11 10:07_

Closing due to inactivity and the fact it's apparently solved; feel free to reopen.

---

_Closed by @CreepySkeleton on 2020-03-11 10:07_

---

_Comment by @wookietreiber on 2020-03-11 13:38_

@CreepySkeleton Thanks for letting me know about `overrides_with`, I totally missed that! I will take the time to read all the functions on `Arg` :)

---
