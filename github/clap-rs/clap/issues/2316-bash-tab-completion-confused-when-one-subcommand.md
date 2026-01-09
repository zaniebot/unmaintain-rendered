---
number: 2316
title: bash tab completion confused when one subcommand is a prefix of another
type: issue
state: closed
author: dkg
labels:
  - C-bug
  - E-medium
  - A-completion
  - ":money_with_wings: $5"
assignees: []
created_at: 2021-01-28T19:03:51Z
updated_at: 2024-08-10T00:36:19Z
url: https://github.com/clap-rs/clap/issues/2316
synced_at: 2026-01-07T13:12:19-06:00
---

# bash tab completion confused when one subcommand is a prefix of another

---

_Issue opened by @dkg on 2021-01-28 19:03_

### Make sure you completed the following tasks

- [x] Searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [x] Searched the closed issues

### Code

Please see [sequoia's sq](https://gitlab.com/sequoia-pgp/sequoia/) for code.  The problem i'm reporting is that `sq key<TAB>` does the wrong thing.

I [reported this concern to sequoia](https://gitlab.com/sequoia-pgp/sequoia/-/issues/654) and they indicated that it is a bug in clap's bash tab completion.  I describe it in more detail below.

### Steps to reproduce the issue

1. build and install `sq` from the `sequoia-sq` crate.  `sq` has two subcommands with the unusual property that one is a prefix of another: `key` and `keyring`
2. in bash, run `touch keyfoo`
3. in bash, type `sq k<TAB>` and it fills in to `sq key`
4. now hit `<TAB>` again.

### Version

* **Rust**: 1.48.0
* **Clap**: 2.33.3

### Actual Behavior Summary

`sq key<TAB>` completes to `sq keyfoo` because of the local file named `keyfoo`. 

### Expected Behavior Summary

It *should* require `sq key<TAB><TAB>` to show a list of two possible subcommands, `key` and `keyring`

### Additional context

I note that the problem might be deeper than just when one subcommand is a prefix of another, though the prefix situation makes it more evident.

For example, `sq` has another subcommand with no lexical overlap with other subcommands: `packet`.

If i `touch packetfoo` and then in bash type `sq packet<TAB>` (note the lack of trailing whitespace), I'd expect bash to insert a space (because the subcommand token is complete, so we should move on to generating the next token).  Instead, it autocompletes to `sq packetfoo `.  

This isn't likely to happen during normal usage because `sq p<TAB>` autocompletes to `sq packet ` (note the trailing whitespace) and `sq packet <TAB>` (again, note the whitespace) actually does the right thing.

### Debug output

sorry, i don't have debug output for this case.

---

_Label `T: bug` added by @dkg on 2021-01-28 19:03_

---

_Added to milestone `3.1` by @pksunkara on 2021-03-09 16:54_

---

_Label `C: generator` added by @pksunkara on 2021-03-09 16:54_

---

_Label `:money_with_wings: $5` added by @pksunkara on 2021-03-09 16:54_

---

_Referenced in [epage/clapng#176](../../epage/clapng/issues/176.md) on 2021-12-06 20:21_

---

_Removed from milestone `3.1` by @epage on 2021-12-13 15:49_

---

_Label `E-medium` added by @epage on 2021-12-13 15:49_

---

_Comment by @epage on 2024-08-10 00:36_

As this is not a problem with `clap_complete::dynamic`, I am going to close this.

To track stabilization, see #3166

---

_Closed by @epage on 2024-08-10 00:36_

---
