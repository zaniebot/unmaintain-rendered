```yaml
number: 3122
title: Fenced code blocks in argument documentation
type: issue
state: closed
author: epage
labels:
  - C-enhancement
  - A-derive
  - S-duplicate
assignees: []
created_at: 2021-12-09T16:16:03Z
updated_at: 2022-01-11T18:43:10Z
url: https://github.com/clap-rs/clap/issues/3122
synced_at: 2026-01-12T16:14:14Z
```

# Fenced code blocks in argument documentation

---

_@epage_

<a href="https://github.com/jonhoo"><img src="https://avatars.githubusercontent.com/u/176295?v=4" align="left" width="96" height="96" hspace="10"></img></a> **Issue by [jonhoo](https://github.com/jonhoo)**
_Saturday Mar 02, 2019 at 20:54 GMT_
_Originally opened as https://github.com/TeXitoi/structopt/issues/166_

----

I have an option that I want to include some example code for (TOML specifically), but it seems like `structopt` (or maybe clap?) eliminates all of the newlines for it. Could we maybe detect fenced code blocks and change the newline handling rules for that segment of the help text?

```rust
#[derive(StructOpt, Debug)]
#[structopt(name = "extractor")]
struct Opt {
    /// Meta file describing the sheets.
    ///
    /// The file should be of the form:
    ///
    /// ```toml
    /// [first_file]
    /// left = [
    ///     # this is row 1 of the left-hand side page
    ///     [
    ///         # these are the tokens in row 1
    ///         "first_token",
    ///         "second_token",
    ///         # the number of columns indicates whether they are big or small
    ///         # (5 | 6 is small, <= 4 is big)
    ///         # to ignore an element, use:
    ///         "",
    ///     ],
    ///     # this is row 2 of the left page
    ///     [
    ///         # ...
    ///     ]
    /// ],
    /// right = [
    ///     # rows of right-hand side page ...
    /// ]
    /// ```
    ///
    /// Files (like `first_file`) are expected to reside adjacent to the TOML file and have a `jpg`
    /// extension.
    #[structopt(parse(from_os_str), default_value = "images/meta.toml")]
    meta: PathBuf,
}
```


---

_Comment by @epage on 2021-12-09 16:16_

<a href="https://github.com/TeXitoi"><img src="https://avatars.githubusercontent.com/u/5787066?v=4" align="left" width="48" height="48" hspace="10"></img></a> **Comment by [TeXitoi](https://github.com/TeXitoi)**
_Sunday Mar 03, 2019 at 09:01 GMT_

----

Related to #163

You'll find some workaround on this issue.


---

_Label `A-derive` added by @epage on 2021-12-09 16:24_

---

_Label `C-enhancement` added by @epage on 2021-12-09 16:24_

---

_Comment by @pksunkara on 2021-12-10 01:39_

This is a dupe of #2389. There's a lot of context in #2401 too as pointed out in that issue.

---

_Comment by @epage on 2021-12-10 20:12_

Closing in favor of #2389 

---

_Closed by @epage on 2021-12-10 20:12_

---

_Label `S-duplicate` added by @epage on 2022-01-11 18:43_

---
