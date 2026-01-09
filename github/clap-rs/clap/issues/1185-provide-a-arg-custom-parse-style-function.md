---
number: 1185
title: "Provide a `Arg::custom_parse` style function"
type: issue
state: closed
author: kbknapp
labels:
  - C-enhancement
  - A-parsing
  - S-wont-fix
assignees: []
created_at: 2018-02-16T03:10:59Z
updated_at: 2022-01-11T18:45:48Z
url: https://github.com/clap-rs/clap/issues/1185
synced_at: 2026-01-07T13:12:19-06:00
---

# Provide a `Arg::custom_parse` style function

---

_Issue opened by @kbknapp on 2018-02-16 03:10_

Something I've been thinking about for a while and mentioned in BurntSushi/ripgrep#809 is to add some additional functions which allow users to do something mid parse. From the linked issue:

> The other way would involve something I've been thinking about, which is to provide companions to the validators, namely the ability to provide functions to Filter/Skip, Map, and Action arguments mid parse. These would do things such as do something potentially impure (Action), allow one to filter/skip values (Filter/Skip haven't decide on naming yet), or transform actual values themselves (Map) to be saved in the matches struct. These would all have access to the matches struct as it exists at that time, in order to perform decisions or possibly even mutate. I should say though that this method is further off and would not be available until v3 at the earliest (if ever, pending design and implementation, etc.).

However, now that I'm thinking about it, all three of those could actually be combined into a single `Arg::custom_parse` which accepts a function that takes the arg/value currently being parsed, and returns an `Option<T>` where `None` is skip this value (i.e. parse it as a different arg), or `Some(T)` is "save this to the matches" it could be the value as passed in, or it could be something different entirely (i.e. a `map`).

This could also require a few companion methods on the matches struct that allows mutation. I'd like to expose these on a newtype wrapper `PartialMatches(ArgMatches)` so as not to expose these mutation functions on a regular basis. Functions could be something like `PartialMatches::remove_{value,arg}`, `PartialMatches::add_{value,arg}`, etc. It would also have all the normal `ArgMatches` methods provided via a facade. 

---

_Label `T: new feature` added by @kbknapp on 2018-02-16 03:11_

---

_Label `D: intermediate` added by @kbknapp on 2018-02-16 03:11_

---

_Label `P3: want to have` added by @kbknapp on 2018-02-16 03:11_

---

_Label `C: parsing` added by @kbknapp on 2018-02-16 03:11_

---

_Label `W: 3.x` added by @kbknapp on 2018-02-16 03:11_

---

_Added to milestone `v3.0` by @kbknapp on 2018-07-22 02:59_

---

_Comment by @Zooce on 2019-07-16 15:55_

This is exactly what I'm looking for. I have a use case where I'd like to give the user the ability to abbreviate a positional argument if they want to. I could then use the `custom_parse` (or whatever) function to see if they did abbreviate, and if so, map it to the full argument.

---

_Referenced in [clap-rs/clap#1206](../../clap-rs/clap/issues/1206.md) on 2020-04-27 17:02_

---

_Label `W: 3.x` removed by @pksunkara on 2021-05-26 03:12_

---

_Removed from milestone `3.0` by @pksunkara on 2021-05-26 10:57_

---

_Comment by @pksunkara on 2021-05-26 10:57_

Probably related #1880 

---

_Referenced in [bootandy/dust#147](../../bootandy/dust/pulls/147.md) on 2021-06-05 13:24_

---

_Referenced in [epage/clapng#88](../../epage/clapng/issues/88.md) on 2021-12-06 16:40_

---

_Referenced in [epage/clapng#90](../../epage/clapng/issues/90.md) on 2021-12-06 16:41_

---

_Label `T: new feature` removed by @epage on 2021-12-08 21:17_

---

_Label `C-enhancement` added by @epage on 2021-12-08 21:17_

---

_Referenced in [clap-rs/clap#2683](../../clap-rs/clap/issues/2683.md) on 2021-12-09 18:09_

---

_Comment by @epage on 2021-12-09 18:10_

Going to close this in favor of #2683.

---

_Closed by @epage on 2021-12-09 18:10_

---

_Label `S-wont-fix` added by @epage on 2022-01-11 18:45_

---
