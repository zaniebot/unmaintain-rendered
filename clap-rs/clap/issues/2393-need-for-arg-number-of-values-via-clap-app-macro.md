---
number: 2393
title: "Need for Arg::number_of_values via clap_app macro"
type: issue
state: closed
author: neithernut
labels: []
assignees: []
created_at: 2021-03-08T15:21:35Z
updated_at: 2021-03-09T00:49:43Z
url: https://github.com/clap-rs/clap/issues/2393
synced_at: 2026-01-10T01:27:17Z
---

# Need for Arg::number_of_values via clap_app macro

---

_Issue opened by @neithernut on 2021-03-08 15:21_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Describe your use case

If `.multiple(true)` is set on an `Arg` named `p`, clap will accept both `-p A B C` and `-p A -p B -b C`. In conjunction with positional parameters and subcommands, tool authors usually want clap to only accept the latter. As the documentation states, this behavior can be achieved via `.number_of_values(1)`.
However, when using the `clap_app` macro, tool authors can use `...` to make an argument take multiple values but they lack the ability to also set the number of values per occurrence. Setting the minimum and maximum values via `#{1, 1}` does not have the same effect.

### Describe the solution you'd like

We need the possibility to define the `number_of_values` via the `clap_app` macro, e.g. via a construct `#{num}`.

### Alternatives, if applicable

The same could be achieved via a construct similar to `...` which would expand to `.multiple(true).number_of_values(1)` rather than only `.multiple(true)`.

### Additional Context

_No response_



---

_Label `T: new feature` added by @neithernut on 2021-03-08 15:21_

---

_Comment by @pksunkara on 2021-03-09 00:49_

This has been fixed in v3 where the position of `...` either sets `multiple_values` or `multiple_occurrences`.

---

_Closed by @pksunkara on 2021-03-09 00:49_

---
