---
number: 3743
title: "Is `Arg::validator_regex` even needed anymore?"
type: issue
state: closed
author: epage
labels:
  - C-bug
  - A-validators
assignees: []
created_at: 2022-05-23T20:21:44Z
updated_at: 2022-05-25T19:46:11Z
url: https://github.com/clap-rs/clap/issues/3743
synced_at: 2026-01-10T01:27:45Z
---

# Is `Arg::validator_regex` even needed anymore?

---

_Issue opened by @epage on 2022-05-23 20:21_

It was proposed for addition in #1968.  That included a workaround but the motivating reason for baking it in was yaml support but that was deprecated in #3087.

With #3732, we are getting support for users to define reusable parsers / validators, rather than having every kind baked in.  Users can provide their own validators or a `clap_regex` crate could be created to provide a general one.

The runtime costs of regex support are only paid for by those enabling the feature.  However, there are usability challenges with clap due to how large the API is.  If we could cut the `Arg::validator_regex`, `regex` feature, and `RegexSet`, that will help focus the clap API.  On top of that, we are deprecating the other `validator*` functions and this would stand out.

---

_Label `C-bug` added by @epage on 2022-05-23 20:21_

---

_Label `A-validators` added by @epage on 2022-05-23 20:21_

---

_Added to milestone `3.x` by @epage on 2022-05-23 20:21_

---

_Comment by @epage on 2022-05-23 20:22_

I had considered adding a `RegexValueParser` but I wasn't quite happy with the generic error message and the API and realized it could just as well be supported outside of clap as inside and it would reduce the API burden of clap.  This led me down this path.

---

_Referenced in [clap-rs/clap#2675](../../clap-rs/clap/issues/2675.md) on 2022-05-23 20:22_

---

_Referenced in [clap-rs/clap#1968](../../clap-rs/clap/issues/1968.md) on 2022-05-23 20:23_

---

_Referenced in [clap-rs/clap#3756](../../clap-rs/clap/pulls/3756.md) on 2022-05-25 19:32_

---

_Closed by @epage on 2022-05-25 19:46_

---

_Referenced in [clap-rs/clap#3199](../../clap-rs/clap/issues/3199.md) on 2022-05-25 20:01_

---
