---
number: 1210
title: Support for single hyphen in long arguments
type: issue
state: closed
author: kellpossible
labels:
  - C-enhancement
  - A-parsing
  - S-duplicate
assignees: []
created_at: 2018-03-18T06:17:20Z
updated_at: 2022-10-10T13:24:06Z
url: https://github.com/clap-rs/clap/issues/1210
synced_at: 2026-01-10T01:26:45Z
---

# Support for single hyphen in long arguments

---

_Issue opened by @kellpossible on 2018-03-18 06:17_

Maintainer's notes:
- This is blocked on pulling out the parser (https://github.com/clap-rs/clap/issues/2915) so we can then abstract  it and allow alternative syntaxes
---
Hi, I would like to begin porting a piece of command line software to rust which uses arguments of the style "-argument" with a single hyphen instead of the usual "--argument" with two hyphens preceding the name of the argument, I would like to use clap for parsing these arguments, but it sounds like clap does not yet support this functionality? 

---

_Comment by @kbknapp on 2018-03-19 18:12_

We had an issue open for this and it's...*disappeared?*

Thanks for (re)submitting! We'll need to go about the design of this a little bit because there were some open questions in the last discussion about this feature.

First and foremost, this will be an opt-in feature, probably via an `AppSettings` variant.

Now, once someone has opted-in, should `-option` style args be allowed with current double hyphen args (i.e. `-option` *and* `--option`), or should they disable/replace double hyphen longs? 

Should they be allowed with `-o` shorts, or should they replace/disable single character shorts?

How should they interact with the current stacking of shorts? (i.e. right now if a CLI defines three shorts `-a`, `-b, and `-c`, one could do `$ prog -abc` which is equivalent to `$ prog -a -b -c`)

Once we get good answers on these questions we can start the implementation.

---

_Comment by @kellpossible on 2018-03-19 22:55_

@kbknapp Hi! Yes that is strange that it disappeared! 

In my particular use case, it would be fine for single hyphen `-option` to completely replace double hyphen `--option`  longs, and to disable shorts and disable short stacking. I can see now this change has quite a few ramifications, sorry! 

---

_Comment by @kellpossible on 2018-03-19 23:04_

@kbknapp I would also be happy to help out with the implementation if it's not considered too complex for someone new to the project 

---

_Comment by @kbknapp on 2018-03-19 23:55_

@kellpossible I think this one will be a rather complex implementation, so I'd rather wait until 3.x where the internals are a little more clean and easy to work with.

I'm also good with it disabling double hyphen longs and stacking, although I'm ok with shorts still sticking around.

---

_Comment by @kbknapp on 2018-03-19 23:55_

This could also play into my wanting to support Windows style CLIs where `/` is used instead of `-`...but I'm not ready to put in an issue for that though as there are a bunch of other things to do first :wink: 

---

_Comment by @divagant-martian on 2020-01-05 14:19_

One way of achieving this would be to have "styles" for long and short versions, where this use case could use two styles "double hyphen" and "single hyphen" so that it is clear that `--long-arg` is still valid, or just "single hypen" to disable the first one.  Somewhat related, thus commenting here.. I'd be interested too in being able to have argument values in the form `argkey:value` in a json style

---

_Label `C: settings` added by @pksunkara on 2020-02-01 07:42_

---

_Label `D: intermediate` added by @pksunkara on 2020-02-01 07:42_

---

_Label `P3: want to have` added by @pksunkara on 2020-02-01 07:42_

---

_Label `T: new feature` added by @pksunkara on 2020-02-01 07:42_

---

_Label `T: new setting` added by @pksunkara on 2020-02-01 07:42_

---

_Added to milestone `3.1` by @pksunkara on 2020-02-01 07:49_

---

_Label `W: 3.x` added by @pksunkara on 2020-02-01 07:56_

---

_Label `W: 3.x` removed by @pksunkara on 2021-08-13 10:40_

---

_Comment by @FibreFoX on 2021-11-01 21:12_

Is there any update on this one?

I need to have some kind of single hyphen "multi-char arguments" in order to comply to a given argument specification: https://developer.elgato.com/documentation/stream-deck/sdk/registration-procedure/#compiled-plugin-registration

These specifications require my application to accept single hypen arguments like `myapp.exe -port 1234 -pluginUUID 14862781-8568-4472-bcfa-dffdeeaaec0d (...)`

Would love to just be able to use `clap` on that one, but without the ability to somehow configure how many dashes/hyphens are used, it pushes me to use a different lib (if there are any for that use-case).

Maybe even when not having this supported (yet), maybe some additional note on the long-argument stuff would help to reduce searching this (and getting frustrated by that).

---

_Comment by @epage on 2021-11-01 21:38_

As far as I'm aware, there was no work on this during 3.0's development and we are at this point prioritizing polish to get the the release out.

kbnapp previously said 3.0's code base should help.  I wonder if #2915, with keeping this in mind, could make it even easier by offering lexing strategies.  If we use a trait for this, then we could easily extend to any lexing style.  This would be a big help for keeping the parser code understandable since it wouldn't need special cases for style and whether shorts are supported or not.

While not required, something also for us to consider is how to keep code size down with this.  Any kind of flag-based runtime control will mean only a really powerful link-time optimizer would cut out the extra code.  If we were okay with taking the lexing trait into the public API of clap, directly or indirectly, and if its object safe, we could accept a `Box<dyn ArgLexer>`.  We could then store it in a `enum Lexer { Box(Box<ArgLexer>), Owned(Default) }` (to avoid the allocation on the default case).  This means the default case only pays the price of dispatching on an enum.

---

_Referenced in [clap-rs/clap#2164](../../clap-rs/clap/issues/2164.md) on 2021-11-03 02:04_

---

_Comment by @mkayaalp on 2021-11-14 22:26_

See #2468 for a collection of non-Unix examples that include:
- single hyphen: `-option` (qemu-like),
- slash: `/?` (Windows), 
- colon option argument separator: `argkey:value` (json-like, also similar to Windows style as it happens)

---

_Referenced in [clap-rs/clap#815](../../clap-rs/clap/issues/815.md) on 2021-11-15 14:35_

---

_Referenced in [clap-rs/clap#2468](../../clap-rs/clap/issues/2468.md) on 2021-11-15 14:47_

---

_Referenced in [MarcelRobitaille/last-rs#2](../../MarcelRobitaille/last-rs/issues/2.md) on 2021-11-24 18:28_

---

_Referenced in [epage/clapng#69](../../epage/clapng/issues/69.md) on 2021-12-06 16:32_

---

_Referenced in [epage/clapng#91](../../epage/clapng/issues/91.md) on 2021-12-06 17:33_

---

_Referenced in [epage/clapng#167](../../epage/clapng/issues/167.md) on 2021-12-06 20:18_

---

_Referenced in [epage/clapng#185](../../epage/clapng/issues/185.md) on 2021-12-06 21:14_

---

_Label `C: settings` removed by @epage on 2021-12-08 20:17_

---

_Label `A-parsing` added by @epage on 2021-12-08 20:17_

---

_Removed from milestone `3.1` by @epage on 2021-12-08 20:18_

---

_Label `T: new feature` removed by @epage on 2021-12-08 20:37_

---

_Label `C-enhancement` added by @epage on 2021-12-08 20:38_

---

_Label `T: new setting` removed by @epage on 2021-12-08 20:38_

---

_Label `D: medium` removed by @epage on 2021-12-09 18:16_

---

_Label `P3: want to have` removed by @epage on 2021-12-09 18:16_

---

_Label `S-blocked` added by @epage on 2021-12-09 18:16_

---

_Referenced in [kata-containers/kata-containers#3251](../../kata-containers/kata-containers/pulls/3251.md) on 2021-12-17 07:48_

---

_Referenced in [clap-rs/clap#3309](../../clap-rs/clap/issues/3309.md) on 2022-01-18 16:37_

---

_Referenced in [clap-rs/clap#3635](../../clap-rs/clap/pulls/3635.md) on 2022-04-15 17:59_

---

_Referenced in [rosetta-rs/argparse-rosetta-rs#50](../../rosetta-rs/argparse-rosetta-rs/pulls/50.md) on 2022-09-10 01:25_

---

_Comment by @epage on 2022-10-10 13:23_

Yeah, going to go ahead and close this out in favor of #2468

---

_Closed by @epage on 2022-10-10 13:23_

---

_Label `S-blocked` removed by @epage on 2022-10-10 13:24_

---

_Label `S-duplicate` added by @epage on 2022-10-10 13:24_

---

_Referenced in [clap-rs/clap#5377](../../clap-rs/clap/issues/5377.md) on 2024-02-26 15:32_

---

_Referenced in [AFLplusplus/LibAFL#3130](../../AFLplusplus/LibAFL/pulls/3130.md) on 2025-05-20 20:26_

---
