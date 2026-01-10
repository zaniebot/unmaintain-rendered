---
number: 2915
title: "[modular] Pull out a clap_lex"
type: issue
state: closed
author: epage
labels:
  - C-enhancement
  - E-hard
  - A-parsing
assignees: []
created_at: 2021-10-19T15:42:57Z
updated_at: 2022-04-15T19:00:29Z
url: https://github.com/clap-rs/clap/issues/2915
synced_at: 2026-01-10T01:27:29Z
---

# [modular] Pull out a clap_lex

---

_Issue opened by @epage on 2021-10-19 15:42_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Clap Version

master

### Describe your use case

We are looking to modular clap
- We can look for opportunities to shrink clap
- We can share logic with other argument parsers

As previously discussed at https://github.com/clap-rs/clap/discussions/2615, one part of this would be pulling out a `clap_lex` crate, similar to `lexopt`.

### Describe the solution you'd like

Clap is a wrapper around `clap_lex`

### Alternatives, if applicable

Expand `lexopt` for our needs (probably easier to do an extraction refactor)

### Additional Context

_No response_

---

_Label `T: new feature` added by @epage on 2021-10-19 15:42_

---

_Assigned to @epage by @epage on 2021-10-19 15:42_

---

_Comment by @pksunkara on 2021-10-19 19:21_

I agree with the author of `lexopt` in that discussion. I think there are quite a few scenarios clap is currently used for where a CFG parser would not work. (To give more context, lexer is used in CFG parsers and is useless in PEG parsers).

From all the examples of clap usage I have seen for the past 2 years, I am confident that we need to keep this as a PEG parser. We can make this PEG parser modular by implementing the plugins proposal in #2832.

I would nominate to close this issue (take your time to do research/experimentation) if needed.

---

_Comment by @epage on 2021-10-19 19:32_

> I agree with the author of lexopt in that discussion. I think there are quite a few scenarios clap is currently used for where a CFG parser would not work. (To give more context, lexer is used in CFG parsers and is useless in PEG parsers).

The `lexopt` author didn't saw it was impossible, just said `lexopt` is insufficient.  They went on to say:

> I like the idea of clap_lexer, though, so I'll follow this discussion with interest.

kbnapp also [thought it was possible](https://www.reddit.com/r/rust/comments/oley2c/lexopt_a_minimalist_pedantic_argument_parser/h5evqhk/)

> Very cool, I like the API a lot. I keep wishing I had more time on my hands to make something analogous to this as an internal clap API that all the clap features are built on top of which would allow using cargo features to aggressively trim down clap's features in an opt in/out manner (and thus be able to get that sweet small binary size).

Could you give a concrete example of why `lexopt`s general approach is incompatible with clap?

> We can make this PEG parser modular by implementing the plugins proposal in #2832.

A future experiment I want to do is something like `clap_derive` that directly populates the struct, without using the builder API.

In general, I would like to raise the bar for all of these non-clap parsers so we can have discussions beyond "does it support non-utf8?" :)

These are what I would like to see unlocked by decoupling our lowest level parsing logic from all of the clap's policy.  https://github.com/clap-rs/clap/discussions/2832 is insufficient for this.

I'm also hopeful this will help us reduce bloat, directly and indirectly by
- Giving us an opportunity to find ways to streamline logic that has grown organically over time to help us reduce our bloat.
- Have more definiable pieces that we can attribute cost towards

---

_Comment by @pksunkara on 2021-10-20 14:58_

I think a simple example here would be `-1`. Would you parse it as a short option or a value by the lexer? There are other ambiguous things like this that would indicate that this is not a context free grammar. And lexers are only useful in CFG.

---

_Comment by @epage on 2021-10-21 13:59_

I get the feeling you are thinking we'd have a signature like
```rust
pub fn lex(args: &[OsString) -> Vec<Token>;
```

Is that correct?

If so, then maybe we aren't using terms correctly but that isn't what `lexopt` does which is the base of my suggestion (both in name and in functionality).

[`lexopt`](https://docs.rs/lexopt/0.1.0/lexopt/) processes one token at a time, driven by the caller.  A simple call to `next()` will auto-detect the token type but you can tell it the next token should be a value by calling `value()`.  We could even have a `peek_next()` to handle some of our precedence cases between values and args or values and subcommands.

This leaves it to the caller to handle what `-1` should be, along with whether `-svalue` is `-s value` or `-s -v -a -l -u -e`.

---

_Comment by @pksunkara on 2021-10-27 02:29_

I understand what you are saying now. Let's agree to not call this `lex` anywhere because it gives off the wrong impression.

Once this is implemented, do you envision this to be used by the plugins?

---

_Comment by @epage on 2021-10-27 17:54_

> Let's agree to not call this lex anywhere because it gives off the wrong impression.

So far I've seen little confusion over `lexopt`s name and suspect it helped people get the intent.  Now, if you have an idea for a better name, I'm open.

> Once this is implemented, do you envision this to be used by the plugins?

I've not seen a case where a plugin would have access to this.  The benefits I see to this work are
- Help clean up clap, hopefully reducing our code size along the way
- Provide a solid base of up and coming argument parser to build on top of, rather than everyone reinventing the wheel, but poorly
- One day, open the door to experimenting on a `clap_derive` that doesn't use the builder.

---

_Comment by @blyxxyz on 2021-10-27 18:30_

> Provide a solid base of up and coming argument parser to build on top of, rather than everyone reinventing the wheel, but poorly

It seems you could end up with something tightly coupled to clap if it needs to know about settings like [`AppSettings::AllowNegativeNumbers`](https://docs.rs/clap/2.33.3/clap/enum.AppSettings.html#variant.AllowNegativeNumbers). And I don't know how clap implements features like pointing out that while some option is invalid *here* it would be valid if you put it *there* after the subcommand, but that also seems like it could get in the way of being generic.

Clap is large and ambitious. If some third-party generic basis already existed then I wouldn't expect clap to use it because it's working at a scale where a bespoke solution is worth the effort. It's like how rustc and GCC use handwritten parsers instead of parser generators.

A basis shared by multiple parsers could be a good idea, but I would guess that extracting it from clap isn't the way to go. (Then again, I know almost nothing about clap's internals.)

---

_Comment by @epage on 2021-10-27 21:19_

My hope is that it doesn't need to know about any settings like that, those would all be driven by the caller. The main risk is that this doesn't simplify our code but makes it more complicated. If it doesn't work then we've learned something. 

---

_Comment by @pksunkara on 2021-10-27 22:21_

> So far I've seen little confusion over lexopts name and suspect it helped people get the intent.

> This library may also be useful if a lot of control is desired, like when the exact argument order matters or not all options are known ahead of time. It could be considered more of a lexer than a parser.

It was more of a general idea, but that library is not lexer. A better name would be `clap_parser` or `clap_parse_stream` etc..

> I've not seen a case where a plugin would have access to this.

> My hope is that it doesn't need to know about any settings like that, those would all be driven by the caller. 

And I envision that the callers here will be plugins.

---

_Comment by @epage on 2021-10-27 23:35_

> And I envision that the callers here will be plugins.

I'm not aware of this fitting in with any of our existing plugin talk (value validation currently being the most concrete).

---

_Comment by @pksunkara on 2021-10-28 00:20_

We discussed that in https://github.com/clap-rs/clap/discussions/2832#discussioncomment-1465059.

---

_Comment by @epage on 2021-10-29 16:37_

> We discussed that in [#2832 (comment)](https://github.com/clap-rs/clap/discussions/2832#discussioncomment-1465059).

There was not enough detail on that for me to know what its referring to or how it ties into this

> > So far I've seen little confusion over lexopts name and suspect it helped people get the intent.
> 
> > This library may also be useful if a lot of control is desired, like when the exact argument order matters or not all options are known ahead of time. It could be considered more of a lexer than a parser.
> 
> It was more of a general idea, but that library is not lexer. A better name would be `clap_parser` or `clap_parse_stream` etc..

All it does is tokenization.  All of the policy of how to parse those tokens is up to the caller.  That sounds closer to a lexer in name than anything else and calling it a parser would be overselling it.  Also, the term "parser" is being used in so many places in clap in different ways, its good for us to look for more specific terms.

---

_Comment by @pksunkara on 2021-10-29 16:52_

> All it does is tokenization.

But we are not planning to do tokenization. We are planning to let them read the items one by one without us categorizing them. That's it. And that's called a parse stream IIUC. Ex: `syn` uses the same word too.

Lexopt does actually categorize those args (and thus tokenize) but as @blyxxyz pointed out, it doesn't handle the corner cases that clap does.

---

_Comment by @epage on 2021-10-29 17:22_

At least from my searching, `syn` is the only user of that term. I did see "Token Stream" used in a similar manner.

> We are planning to let them read the items one by one without us categorizing them

I see what we create being relatively similar to `lexopt` except it will have peek functionality.

---

_Comment by @pksunkara on 2021-10-29 17:42_

> I see what we create being relatively similar to lexopt except it will have peek functionality.

I am not sure how that would work with those corner cases (ex: negative values, multiple values) since we definitely need to leave up the categorization to individual parsers. But I guess it's a design for later.

I realise that we are just bikeshedding on the name of this module at this point. But yeah, I felt that the term `lex` would be too confusing since it's not CFG.

---

_Referenced in [clap-rs/clap#1210](../../clap-rs/clap/issues/1210.md) on 2021-11-01 21:38_

---

_Referenced in [clap-rs/clap#1232](../../clap-rs/clap/issues/1232.md) on 2021-11-15 14:31_

---

_Referenced in [epage/clapng#92](../../epage/clapng/issues/92.md) on 2021-12-06 17:34_

---

_Referenced in [epage/clapng#228](../../epage/clapng/issues/228.md) on 2021-12-06 22:21_

---

_Label `T: new feature` removed by @epage on 2021-12-08 21:08_

---

_Label `C-enhancement` added by @epage on 2021-12-08 21:08_

---

_Label `A-parsing` added by @epage on 2021-12-09 03:15_

---

_Referenced in [clap-rs/clap#3166](../../clap-rs/clap/issues/3166.md) on 2021-12-13 17:55_

---

_Label `S-waiting-on-mentor` added by @epage on 2021-12-13 22:07_

---

_Label `S-waiting-on-mentor` removed by @epage on 2021-12-13 22:07_

---

_Label `E-hard` added by @epage on 2021-12-13 22:07_

---

_Referenced in [clap-rs/clap#3309](../../clap-rs/clap/issues/3309.md) on 2022-01-18 16:37_

---

_Referenced in [uutils/coreutils#3081](../../uutils/coreutils/pulls/3081.md) on 2022-02-06 15:29_

---

_Referenced in [clap-rs/clap#3635](../../clap-rs/clap/pulls/3635.md) on 2022-04-15 17:59_

---

_Closed by @epage on 2022-04-15 19:00_

---
