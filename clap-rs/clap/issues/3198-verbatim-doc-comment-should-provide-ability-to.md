---
number: 3198
title: verbatim_doc_comment should provide ability to escape newlines with \ like in string literals
type: issue
state: closed
author: I60R
labels:
  - C-bug
  - A-help
  - S-waiting-on-design
assignees: []
created_at: 2021-12-18T18:16:03Z
updated_at: 2024-07-08T15:17:55Z
url: https://github.com/clap-rs/clap/issues/3198
synced_at: 2026-01-10T01:27:35Z
---

# verbatim_doc_comment should provide ability to escape newlines with \ like in string literals

---

_Issue opened by @I60R on 2021-12-18 18:16_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [x] I have searched the existing issues

### Rust Version

-

### Clap Version

3.0.0.rc7

### Minimal reproducible code

```rust
/// This is a very long line which I'd like to split in code \
/// but keep wrapped in terminal
/// This text should be on a new line
#[clap(verbatim_doc_comment)]
...
```

### Steps to reproduce the bug with the above code

-

### Actual Behaviour

This is a very long line which I'd like to split in code \\
but keep wrapped in terminal
This text should be on a new line

### Expected Behaviour

This is a very long line which I'd like to split in code but keep wrapped in terminal
This text should be on a new line

### Additional Context

Previously `{n}` allowed to achieve the expected effect

### Debug Output

_No response_

---

_Label `C-bug` added by @I60R on 2021-12-18 18:16_

---

_Comment by @epage on 2021-12-18 18:57_

Di you see this as different than https://github.com/clap-rs/clap/issues/2389?

---

_Comment by @I60R on 2021-12-19 04:50_

Yes. That issue was about preserving newlines and `verbatim_doc_comment` seems to be a proper solution.

This is about escaping newlines away with `verbatim_doc_comment` using slash

---

_Renamed from "verbatim_doc_comment should provide ability to escape newlines like in string literals" to "verbatim_doc_comment should provide ability to escape newlines with \ like in string literals" by @I60R on 2021-12-19 04:51_

---

_Comment by @I60R on 2021-12-20 08:48_

Alternatively, could be the support for `{n}` restored? It worked perfectly with `help_wrap` and with it migration from structopt may be easier for some people.

---

_Referenced in [clap-rs/clap#1810](../../clap-rs/clap/pulls/1810.md) on 2021-12-20 14:40_

---

_Label `A-help` added by @epage on 2021-12-20 14:40_

---

_Label `S-waiting-on-design` added by @epage on 2021-12-20 14:40_

---

_Comment by @epage on 2021-12-20 14:41_

I missed that, thanks!

I went back to try to see why `{n}` was removed but the PR doesn't have an explanation.  I've pinged the people involved at that time to see if we can learn from what was done before.

---

_Comment by @pksunkara on 2021-12-20 15:18_

Can you try replacing `\` with `\n` and see if it works?

---

_Comment by @I60R on 2021-12-20 23:05_

Nope, either with `verbatim_doc_comment` and without it's rendered as `\n` without newline being added

---

_Comment by @I60R on 2021-12-28 01:27_

@pksunkara `\n` works when inserted into `help` attribute e.g. `help="string\n"` but is stripped when placed in doc comment, so the problem is on derive side I guess?

---

_Referenced in [clap-rs/clap#3228](../../clap-rs/clap/pulls/3228.md) on 2021-12-28 04:14_

---

_Comment by @pksunkara on 2021-12-28 04:28_

Yup, check the comment I made on that PR. Approach is correct but we need to check for the `verbatim_doc_comment` attribute first.

---

_Comment by @epage on 2021-12-28 15:15_

So to double check what you are ultimately trying to accomplish, you are wanting one line to be able to overflow onto following lines and you want another line immediately after it without a paragraph break, right?

You originally did this with
```rust
/// This is a very long line which I'd like to split in code
/// but keep wrapped in terminal {n}
/// This text should be on a new line
...
```

Your thought is to either implement this with
```rust
/// This is a very long line which I'd like to split in code \
/// but keep wrapped in terminal
/// This text should be on a new line
#[clap(verbatim_doc_comment)]
...
```
or
```rust
/// This is a very long line which I'd like to split in code
/// but keep wrapped in terminal \n
/// This text should be on a new line
...
```

---

_Comment by @epage on 2021-12-28 15:19_

btw something I've been meaning to do is to explore the feature set and API design of other CLI parsers to see what we can learn or to help open us up on the problem space.

[Click uses escape sequences to customize the help](https://click.palletsprojects.com/en/8.0.x/documentation/#preventing-rewrapping)
- `\b` to turn off wrapping for a paragraph
- `\f` to cut off the `help`, leaving the rest for the doc comment

A key difference is that these are just control codes in a doc string that are probably being ignored by the documentation tools while, as pointed out, Rust doc comments are raw strings so escape sequences are ignored.

---

_Added to milestone `3.0` by @epage on 2021-12-28 15:51_

---

_Comment by @I60R on 2021-12-28 18:50_

@epage correct. It seems that `\n` will be a proper approach because it's very similar to `<br>` in markdown.

When talking about other CLI parsers, Python's argparse allows custom help formatting via [HelpFormatter](https://docs.python.org/3/library/argparse.html#formatter-class) class, so maybe clap should externalize help preprocessing as well? There could be 4 functions for `long_/short_/help/about` and they could take `Vec<String>` from `syn` plus `Vec<(String, String)>` of metadata e.g. `env.`, `default` etc. and users then could implement whatever formatting they want

---

_Comment by @pksunkara on 2021-12-28 21:16_

> ```rust
> /// This is a very long line which I'd like to split in code
> /// but keep wrapped in terminal \n
> /// This text should be on a new line
> ```

How is this shown on rust docs?

As specified in #2389, our default help text should behave similarly to rust docs. And `verbatim_doc_comment` allows people to escape that behaviour.

---

_Comment by @I60R on 2021-12-28 21:35_

This will be rendered as `\n` in rustdoc...

**Okay, another proposal**: in markdown [we can place slash at the end of line to preserve newline](https://www.markdownguide.org/basic-syntax/#line-break-best-practices) which is opposite to what slash does in Rust str literals (weird!). Although this feature isn't recommended for use and is relatively unknown, it still a part of markdown specification and thus supported by rustdoc, so

```
/// This is a very long line which I'd like to split in code
/// but keep wrapped in terminal \
/// This text should be on a new line
```

Will be rendered by rustdoc as 

```
This is a very long line which I'd like to split in code but keep wrapped in terminal
This text should be on a new line
```

If we aim to make --help as most close as possible to rustdoc then turning slash followed by newline into `{n}` replacement should be the most compatible option

---

_Comment by @I60R on 2021-12-29 00:33_

So, I've implemented that behavior in #3228, let's continue discussion there

---

_Closed by @I60R on 2021-12-29 00:33_

---

_Referenced in [clap-rs/clap#2389](../../clap-rs/clap/issues/2389.md) on 2021-12-29 02:51_

---

_Comment by @epage on 2021-12-30 12:17_

In considering the motivating case for this issue being a regression in functionality in clap3 and the relative size of the various proposed replacements for the clap2 behavior, I'm thinking of restoring `{n}` for the life of clap3, assuming we get markdown support in during the clap3 release.  @I60R thoughts?

---

_Referenced in [clap-rs/clap#3230](../../clap-rs/clap/issues/3230.md) on 2021-12-30 17:44_

---

_Comment by @epage on 2021-12-30 21:03_

@I60R can you report back if  `{n}` in 3.0.0-rc.11 unblocks you?

---

_Comment by @I60R on 2021-12-31 00:07_

Sorry for being silent.

Yes, I like to have `{n}` during clap3 or until markdown would be ready

---

_Comment by @tmccombs on 2022-11-11 08:38_

I want this feature as originally described, but `{n}` doesn't work, because I need to use verbatim_doc_comment in order to get proper indentation (because without verbatim_doc_comment whitespace gets compressed). 

AFAICT currently I either have to use really long lines in the comments with verbatim_doc_comment, or pass it in directly to long_help (i.e. don't use the doc comment functionality). Or I suppose break the line myself, and then if the screen is too narrow, it will wrap weirdly.

Alternatively, having a way to toggle between verbatim and non-verbatim mode. I wonder if it would make sense to switch into verbatim mode inside of a code block delimited with triple backticks (and remove those backticks). Or maybe if a line starts with three or more spaces (2 after removing the initial space), then treat it as a new line, and don't remove the initial whitespace? 

Full markdown support isn't really necessary for me here.

---

_Comment by @epage on 2022-11-11 14:04_

>  without verbatim_doc_comment whitespace gets compressed

What do you mean by this?  We should only be stripping off a single leading space that would be between the `///` and the text.

> Full markdown support isn't really necessary for me here.

Except a lot of what you are describing either adds a lot of complexity or is us basically implementing a poor mans version of markdown.   A lot of these  doc comments people post are because of that gulf; they expect to be able to write markdown and we don't support it.

---

_Comment by @tmccombs on 2022-11-12 06:03_

> what do you mean by this?

I am referring to:

> Strip leading and trailing whitespace from every line, if present.

So if verbatim_doc_comment is true, you lose any indentation at the beginning of the line. Initially, I didn't think there was any way to get that back, but actually I did figure out a way to do it. If I put "{n}" at the beginning of the line, then put the space between that and the text of the line, the whitespace is preserved.

 > they expect to be able to write markdown and we don't support it.

No, not really. My example isn't even really markdown. It looks something like:

```
Do something special that requires quite a bit of explanation
that is in a paragraph that should be on multiple
lines in the doc comment, but ideally would be treated
as a single line for wrapping.

Examples:
    --do-something value1
    --do-something 1124
```

If I were to write that in markdown, I suppose I could put the  examples inside triple backquotes. Would that be included in the scope of the markdown support feature?

But as is, it isn't really markdown, just text with some indentation. And it really doesn't need things like headers, emphasis, links, etc. (although such things certainly could be useful in general).



---

_Comment by @epage on 2022-11-13 03:47_

Ah, I had missed the trim on non-verbatim lines.  That seems odd that we trim leading whitespace.

In general
- We need some format for normalizing the text (paragraphs)
- We need some format for knowing to know how to leave things alone
- There is the general expectation that doc comments are comments and doing a format that is anything else will likely lead to more problems

This leads to us supporting markdown, even if its not the most thorough parser.   This is why I created [md-rosetta-rs](https://github.com/rosetta-rs/md-rosetta-rs) was to look for the cheapest parser that gets things done.

> And it really doesn't need things like headers, emphasis, links, etc. (although such things certainly could be useful in general).

I strongly expect [headers will be needed](https://github.com/clap-rs/clap/issues/3108).  While we are talking about doc comments, I expect people will want to write markdown for others parts as well and we should consider how we can support that for people.

---

_Referenced in [clap-rs/clap#3367](../../clap-rs/clap/issues/3367.md) on 2024-05-23 14:21_

---

_Comment by @obskyr on 2024-05-23 14:27_

This issue should probably be reopened – unless there's a different way to escape newlines when using `verbatim_doc_comment`?

---

_Comment by @epage on 2024-06-06 20:20_

The original person's issue was solved a different way.

I am hesitant for us to muck much like this with verbatim because then its no longer verbatim.  It is like saying you need a string literal but allow escaping in one specific case.

What I would recommend is focusing on what your underlying need for this is and we can then explore how to resolve that need.

---

_Comment by @tmccombs on 2024-07-06 22:36_

What if any part of the doc comment that is between code fences ("```") is treated as verbatim, if it is in a non-verbatim doc comment?

That way it is possible to temporarily suppress the pre-processing in a way that works both in a rustdoc output and the help output.

---

_Comment by @obskyr on 2024-07-07 07:28_

> The original person's issue was solved a different way.
> 
> I am hesitant for us to muck much like this with verbatim because then its no longer verbatim. It is like saying you need a string literal but allow escaping in one specific case.
> 
> What I would recommend is focusing on what your underlying need for this is and we can then explore how to resolve that need.

My need is to not have the final period removed (#3367). Currently, there is no way to avoid having the final period removed without also putting every paragraph of the doc comment on a single line (making for lines of hundreds of characters) or introducing manual line breaks to the text (meaning the text will no longer fit the user's terminal window).

---

_Comment by @epage on 2024-07-08 15:17_

@obskyr then any discussion should be on #3367 and from there we should decide whether to further tweak our rules, rather than asking for an unrelated issue to be re-opened.

---
