```yaml
number: 976
title: "Use `-t .foo` as alias for `-g '*.foo'`"
type: issue
state: closed
author: stepancheg
labels:
  - wontfix
assignees: []
created_at: 2018-07-10T22:28:09Z
updated_at: 2018-07-11T11:34:23Z
url: https://github.com/BurntSushi/ripgrep/issues/976
synced_at: 2026-01-12T16:13:22Z
```

# Use `-t .foo` as alias for `-g '*.foo'`

---

_@stepancheg_

I often use ripgrep is search by various file suffixes, e. g. `rg -g '*.inl'`, when there's no defined type name (e. g. for Idris) or I don't remember what's the type name (e. g. for Python).

A small issue here is too many characters to type:
* `*` sign
* quotes for the shell

I think it would be convenient if `rg` had a quick way to search simply by file suffix.

Currently `-t` option supports a file type for a predefined set.

I think it could be convenient to have an option to execute `rg -t .foo` as an alias for `rg -g '*.foo'`.

This is easy to implement, and sound like bikeshedding, so feel free to close the request.

Using `ripgrep 0.8.1`.

---

_Comment by @BurntSushi on 2018-07-11 11:34_

Thanks for the detailed request! I think I'm going to have to pass on this. I don't think it's a good fit, but reasonable people can disagree. Here are my reasons:

* I like that the `--type` flag accepts exactly one kind of argument: a file type name.
* I think that writing globs is a pretty standard thing to do when running commands, and I do not believe it to be onerous enough to justify changing the semantics of `--type`.
* If you are using a file extension frequently enough, then you might consider adding it to the list of available types, which ripgrep makes extensible. See [the guide](https://github.com/BurntSushi/ripgrep/blob/master/GUIDE.md#manual-filtering-file-types) for instructions on how to do that.

---

_Closed by @BurntSushi on 2018-07-11 11:34_

---

_Label `wontfix` added by @BurntSushi on 2018-07-11 11:34_

---
