---
number: 849
title: No default_value_os for reliably specifying filesystem paths as default arguments
type: issue
state: closed
author: ssokolow
labels: []
assignees: []
created_at: 2017-02-12T00:43:47Z
updated_at: 2018-08-02T03:30:01Z
url: https://github.com/clap-rs/clap/issues/849
synced_at: 2026-01-10T01:26:37Z
---

# No default_value_os for reliably specifying filesystem paths as default arguments

---

_Issue opened by @ssokolow on 2017-02-12 00:43_

### Affected Version of clap

2.20.3 

### Expected Behavior Summary

Functions like `validator_os` and `value_of_os` should be complemented with a `default_value_os`.

### Actual Behavior Summary

Only `default_value` and its variants are provided... which means that it's not possible to specify all valid paths as default values.

### Sample Code or Link to Sample Code

```rust
// env::current_dir is fallible and default_value can't take a callback for lazy eval, so
// resort to "." but future-proof it in case of esoteric platforms.
// (Not perfect, but to_string_lossy() is necessary without a `default_value_os`)
let lazy_currdir = &Component::CurDir.as_os_str().to_string_lossy();

let matches = App::new(env!("CARGO_PKG_NAME"))
    .version(crate_version!())
    // .about("Description text here")
    // TODO: Add args to control slog logging level
    .arg(arg::with_name("inpath")
        .help("file(s) to use as input")
        .multiple(true)
        .empty_values(false)
        .default_value(lazy_currdir)
        // .validator_os(validators::path_readable)
        .required(true))
    .get_matches();
```

(It's an excerpt from the "new project" boilerplate I'm putting together)

---

_Comment by @kbknapp on 2017-02-12 14:55_

While I agree there *could* be a `default_value_os` and I'm not opposed to adding it, do you actually need to add a path with invalid UTF-8? I know it's possible, but that doesn't mean it's practical. I'm asking for my own curiosity :wink:

---

_Label `C: args` added by @kbknapp on 2017-02-12 14:56_

---

_Label `D: easy` added by @kbknapp on 2017-02-12 14:56_

---

_Label `P4: nice to have` added by @kbknapp on 2017-02-12 14:56_

---

_Label `T: new feature` added by @kbknapp on 2017-02-12 14:56_

---

_Label `W: 2.x` added by @kbknapp on 2017-02-12 14:56_

---

_Added to milestone `2.21.0` by @kbknapp on 2017-02-12 14:56_

---

_Comment by @ssokolow on 2017-02-12 18:38_

At the moment, it's more a matter of wanting correctness in how I glue the "get me the current directory" APIs to clap and wanting the APIs I deal with to be complete.

However, there is actually a set of situations where I expect this to be a problem. Some of the projects I have on hold (currently partially prototyped in old Python code using `optparse`) embed a shell in a way where default values can get very dynamic... and I intend to use them on folders where round-tripping through different distros over the years has not been kind.

(ie. Folders full of files with Japanese filenames names so [mojibake](https://en.wikipedia.org/wiki/Mojibake)'d that they're not even valid UTF-8)

As-is, to play it safe in those projects, I'll be setting a dummy default value (probably `\0`) and then reinventing defaulting using `match` if I don't have `default_value_os`.

---

_Closed by @homu on 2017-02-21 04:50_

---

_Comment by @ssokolow on 2017-04-06 22:01_

I finally got around to looking at this and it seems you implemented `default_value_if_os` and `default_value_ifs_os` but, despite the commit message saying it should be there, you neglected to implement `default_value_os`.

I can't find any evidence of it in the diff and here are the search results from locally-built docs for clap 2.23.1:
![2017-04-06-180023_956x248_scrot](https://cloud.githubusercontent.com/assets/46915/24777353/f7c35a76-1af2-11e7-9edc-738e08d8b7f4.png)


---

_Comment by @kbknapp on 2017-04-06 22:07_

Oh wow!

---

_Reopened by @kbknapp on 2017-04-06 22:07_

---

_Comment by @kbknapp on 2017-04-06 22:08_

Thanks for letting me know, I'll get this added shortly!

---

_Referenced in [clap-rs/clap#941](../../clap-rs/clap/pulls/941.md) on 2017-04-24 19:55_

---

_Closed by @homu on 2017-05-05 00:57_

---
