---
number: 2150
title: "Consider accepting a Cow instead of Into<&'b str> in App::about"
type: issue
state: closed
author: d-e-s-o
labels:
  - C-enhancement
  - M-breaking-change
  - A-builder
  - E-medium
assignees: []
created_at: 2020-10-04T18:26:25Z
updated_at: 2022-08-22T21:48:17Z
url: https://github.com/clap-rs/clap/issues/2150
synced_at: 2026-01-10T01:27:13Z
---

# Consider accepting a Cow instead of Into<&'b str> in App::about

---

_Issue opened by @d-e-s-o on 2020-10-04 18:26_

I find the signature of methods like [`App::about`](https://docs.rs/clap-v3/3.0.0-beta.1/clap_v3/struct.App.html#method.about) not super useful. It's probably fine when doing everything statically, because you could easily make sure that the string provided outlives the `App` object (because most of the time you have a 'static string anyway as everything is defined statically in the code). But consider the case where you'd want to add subcommands dynamically at runtime. It gets unnecessarily unwieldy now, in my opinion, because of the lifetime requirement.

Couldn't you instead accept a `Cow<'b, str>`? This way, users that do something more dynamic would at least have the possibility to fudge that with a dynamic memory allocation at the call site, instead of doing some dance to make the generated string outlive the `App` object or even boxing the string up just to leak it.

---

_Label `T: bug` added by @d-e-s-o on 2020-10-04 18:26_

---

_Added to milestone `3.0` by @pksunkara on 2020-10-04 18:27_

---

_Label `T: bug` removed by @pksunkara on 2020-10-04 18:27_

---

_Label `T: enhancement` added by @pksunkara on 2020-10-04 18:27_

---

_Comment by @ldm0 on 2020-10-06 01:00_

Related: <https://github.com/clap-rs/clap/pull/1728>

---

_Comment by @pksunkara on 2021-05-26 14:40_

@d-e-s-o Do you think you can take a small attempt at this by only changing the `about`? Would like to see how it improves the API.

---

_Referenced in [clap-rs/clap#2504](../../clap-rs/clap/pulls/2504.md) on 2021-05-27 16:08_

---

_Comment by @d-e-s-o on 2021-05-27 16:08_

> @d-e-s-o Do you think you can take a small attempt at this by only changing the `about`? Would like to see how it improves the API.

Sure, see https://github.com/clap-rs/clap/pull/2504.

---

_Removed from milestone `3.0` by @pksunkara on 2021-06-09 07:45_

---

_Referenced in [geldata/gel-cli#453](../../geldata/gel-cli/pulls/453.md) on 2021-08-10 18:03_

---

_Comment by @epage on 2021-10-04 17:53_

This is related to my proposed fix for #1041 

---

_Added to milestone `4.0` by @pksunkara on 2021-10-09 00:03_

---

_Referenced in [clap-rs/clap#2857](../../clap-rs/clap/pulls/2857.md) on 2021-10-12 20:31_

---

_Label `E: breaking change` added by @epage on 2021-10-19 16:34_

---

_Referenced in [epage/clapng#166](../../epage/clapng/issues/166.md) on 2021-12-06 20:18_

---

_Label `A-builder` added by @epage on 2021-12-13 15:03_

---

_Label `E-medium` added by @epage on 2021-12-13 15:03_

---

_Comment by @epage on 2021-12-14 16:21_

A challenge is `help_heading` takes `Into<Option<&str>>`.  If we make this `Into<Option<Into<CustomStr>>>` then `None` gets an ambiguous type compiler error.

Calling with `None` might be rare enough that its ok.  We'll have to evaluate this.

---

_Referenced in [clap-rs/clap#2983](../../clap-rs/clap/issues/2983.md) on 2021-12-14 17:20_

---

_Referenced in [clap-rs/clap#1206](../../clap-rs/clap/issues/1206.md) on 2022-05-18 11:33_

---

_Referenced in [clap-rs/clap#3737](../../clap-rs/clap/issues/3737.md) on 2022-05-19 12:33_

---

_Referenced in [bootandy/dust#248](../../bootandy/dust/pulls/248.md) on 2022-08-22 16:07_

---

_Referenced in [clap-rs/clap#4103](../../clap-rs/clap/pulls/4103.md) on 2022-08-22 21:02_

---

_Closed by @epage on 2022-08-22 21:48_

---

_Referenced in [solana-labs/solana-program-library#6376](../../solana-labs/solana-program-library/pulls/6376.md) on 2024-03-12 01:15_

---
