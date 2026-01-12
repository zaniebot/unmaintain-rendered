```yaml
number: 3189
title: Derive documentation can be improved
type: issue
state: closed
author: epage
labels:
  - C-bug
  - A-docs
  - A-derive
  - S-waiting-on-design
assignees: []
created_at: 2021-12-16T15:54:06Z
updated_at: 2022-08-18T14:07:54Z
url: https://github.com/clap-rs/clap/issues/3189
synced_at: 2026-01-12T16:14:14Z
```

# Derive documentation can be improved

---

_@epage_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Rust Version

rustc 1.55.0 (c8dfcfe04 2021-09-06)

### Clap Version

v3.0.0-rc.0

### Description

> the docs on the new derive feature is not great

From [mitsuhiko](https://www.reddit.com/r/rust/comments/rbyi41/ann_clap_300rc0/hnrmwhy/)

> I think the docs about derive are currently the biggest issue as well. Really liked having all the docs about it in rustdoc, like structopt does.
> 
> It's very annoying if you have to jump back into the repo or the GitHub wiki pages to get some extra details. Instead it can be a module just to document the derive feature, doesn't even have to have anything in it, just the docs. And then the docs should be as detailed as in structopt.
> 
> So far I always had to go to structopt docs to check how to use the clap derive feature 😅

From [dnaka91](https://www.reddit.com/r/rust/comments/rbyi41/ann_clap_300rc0/hnx14h8/)

I wanted to make sure we captured this feedback and raised visibility of it so we can hear from more voices to help in working out what we shoud do improve this

---

_Label `C-bug` added by @epage on 2021-12-16 15:54_

---

_Label `A-docs` added by @epage on 2021-12-16 15:54_

---

_Label `S-waiting-on-design` added by @epage on 2021-12-16 15:54_

---

_Added to milestone `3.0` by @epage on 2021-12-16 15:54_

---

_Comment by @epage on 2021-12-16 15:56_

My reply on reddit to dnaka91 which hasn't been responded to

> So the main point of frustration is going inbetween two different places?
> 
> For myself, I have found serde.rs really useful for undertanding their derives while I've always been frustrated with finding anything in structopt's documentation, so I modeled it more off of serde. This ended up both being in structure and not being in docs.rs. I think it really was the structure that was the frustration point for me but there was interest elsewhere in moving stuff out of docs.rs.
> 
> So any input on this is helpful for finding how to iterate on this, in whatever form it ends up in the end!
> 
> EDIT: I should also add, is your input specifically on the derive or also on the tutorial? The latter especially benefits from not being in docs.rs because we can show the output of the commands and verify its correct! I thought I was going to use this more for the derive docs than I ended up.



---

_Comment by @mcronce on 2021-12-16 20:11_

FWIW, for me as a `clap` consumer, an (easily discoverable, maybe linked from or included in the doc comment on the derive macro) comprehensive list of the attributes supported by the macro would be absolute perfect-world

Love the new derive macro, btw.  It's fantastic.

---

_Comment by @epage on 2021-12-16 20:24_

I considered including the [derive reference](https://github.com/clap-rs/clap/blob/v3.0.0-rc.7/examples/derive_ref/README.md) directly in the doc comments but there is a lot shared between `Parser`, `Args`, and `Subcommand` (and where on them you specify it).  I felt we needed some place more cross-cutting.

We do link to the derive reference from the top-level docs but linking directly from the traits themselves would be a big help.  We've been versioning the links to external documentation, relying on `cargo-release` to update them.  I've been trying to keep those updates to a minimum to reduce boilerplate churn but, unless we move it into an empty module, it pretty much is required here.

Still debating on whether to keep the derive reference in the repo or to move it do an empty module.  The tutorials being separate is helped a lot by our ability to test them and link to the example code easily.  So far the derive reference only has 1 tested code sample.

---

_Referenced in [clap-rs/clap#3052](../../clap-rs/clap/pulls/3052.md) on 2021-12-24 11:54_

---

_Referenced in [clap-rs/clap#2856](../../clap-rs/clap/issues/2856.md) on 2021-12-27 15:18_

---

_Label `A-derive` added by @epage on 2021-12-28 15:50_

---

_Removed from milestone `3.0` by @epage on 2021-12-30 20:37_

---

_Referenced in [clap-rs/clap#3233](../../clap-rs/clap/pulls/3233.md) on 2021-12-30 20:41_

---

_Comment by @pinkforest on 2022-01-08 04:07_

I actually completely missed the link to derive reference and it gave me headache for hours and then it was all there along front of README.md but for some reason I missed it - it probably has to have a heading in README.md under "Derive" section and then some text around it to give a reference as the plain link is easily ignored - or at least for me it was.

I only got to the refernce documentation when reading the source and associated doc comments somewhere thick deep.

I've created #3269 in order to document what is expected to be supported with flattened in the future.

I've also done https://github.com/crate-ci/clap-cargo/pull/28 to document downstream clap-cargo limitations with the associated required flatten with no alternative to use/inject methods either with individual arguments or as a collective.

---

_Comment by @epage on 2022-01-20 22:02_

https://internals.rust-lang.org/t/rustdoc-include-an-external-markdown-file-as-a-separate-page/15994/5 seems relevant for this discussion.

---

_Referenced in [clap-rs/clap#3490](../../clap-rs/clap/issues/3490.md) on 2022-02-20 00:55_

---

_Referenced in [clap-rs/clap#3499](../../clap-rs/clap/pulls/3499.md) on 2022-02-22 23:36_

---

_Referenced in [clap-rs/clap#3529](../../clap-rs/clap/pulls/3529.md) on 2022-03-02 22:17_

---

_Comment by @camsteffen on 2022-03-18 16:51_

> I actually completely missed the link to derive reference

I had the same experience. I had skipped over the table of contents and quickly saw a "Derive API" link which very much looked like it would link to a reference page. When it took me to the tutorial page, I thought "oh I guess this is their idea of documentation". Perhaps these links should instead look like "[Derive API](https://github.com/clap-rs/clap/blob/v3.1.6/examples/derive_ref/README.md) ([tutorial](https://github.com/clap-rs/clap/blob/v3.1.6/examples/tutorial_derive/README.md))" or "Derive API ([reference](https://github.com/clap-rs/clap/blob/v3.1.6/examples/derive_ref/README.md), [tutorial](https://github.com/clap-rs/clap/blob/v3.1.6/examples/tutorial_derive/README.md))".

---

_Referenced in [clap-rs/clap#3638](../../clap-rs/clap/issues/3638.md) on 2022-04-19 11:42_

---

_Comment by @jaskij on 2022-04-19 11:50_

Since I randomly found this issue while googling something else, I'll chip in: my main pain point with a Markdown file on Github vs Docs.rs is the lack of navigation - I need to scroll manually, or scroll back to the top and click on a header, there's no floating ToC.

To give an example: reading through [Arg magic attributes](https://github.com/clap-rs/clap/blob/v3.1.9/examples/derive_ref/README.md#arg-attributes), I came across `verbatim_doc_comment`, and wanted to read up more about Clap's doc comment handling. On Docs.rs I would have had a chance to notice the separate section in the ToC in upper right. On Github, if I don't want to loose the place I'm at, I need to copy the address, open a new tab, paste the address, and then I have ToC. Or keep a tab scrolled to ToC.

---

_Comment by @epage on 2022-04-19 12:24_

@jaskij thats a really good point about the value of a floating ToC.  Unfortunately, custom documentation on docs.rs can't participate in the ToC (e.g. [clap](https://docs.rs/clap/latest/clap/), [structopt](https://docs.rs/structopt/latest/structopt/)).

So let me re-summarize the benefits of the different documentation platforms

mdbook
- Floating, automatic ToC
- Manual versioning
- Inline, testable examples
- Three places for all project content (docs.rs, GH, clap.rs)
- Manual cross-linking with rustdoc
- [example](https://serde.rs/)

github markdown source rendering
- Manual ToC
- Easy versioning
- Testable examples
- Only two places for all project content (docs.rs, GH)
- Manual cross-linking with rustdoc
- [example](https://docs.rs/clap/latest/clap/) which links out to [gh](https://github.com/clap-rs/clap/blob/v3.1.9/examples/tutorial_derive/README.md)

github wiki
- Floating, manual ToC
- Manual versioning
- No compiling or testing of examples
- Only two places for all project content (docs.rs, GH)
- Manual cross-linking with rustdoc

docs.rs
- Manual ToC
- Easy versioning
- Compileable examples
- Single place for all documentation (docs.rs)
- Easy cross-linking with rustdoc
- [example](https://docs.rs/structopt/latest/structopt/)

---

_Comment by @jaskij on 2022-04-19 12:49_

Actually, GH wiki supports a sidebar - you need a file called `_Sidebar.md` if my search is correct. But that is still a separate repo

As for mdbook - couldn't you just have the book as a subdirectory in the main repo, and build and publish from there? No need for a separate repository that way.

---

_Comment by @epage on 2022-04-19 13:59_

With Github, I'm referring to the markdown rendering of source.  I've added Github Wiki to the comparison.

As for "places for project content", I'm not referring to repos but places the user has to browse to as that was one of the complaints of using mdbook.

---

_Comment by @CraftSpider on 2022-05-25 00:26_

A quick comment unrelated to the location of the documentation: It took me a while to notice the part that says 'any arg method can also be used as an attribute', because I was looking for how to set a delimiter, and my instinct was to search for 'delimiter', 'comma', 'list', etc. on the documentation, assuming that like the magic attributes they would be listed out. I'm not sure it's worth copying that information, but on the other hand the current setup makes it feel like you need to just read pages of unrelated options to discover it.

---

_Comment by @epage on 2022-05-25 00:50_

Yes, I've not wanted to maintain a duplicate of the builder API in the derive documentation.  I'm trying to think how to further improve the situation.  Maybe we should put the raw attribute description for each type of attribute first since its short, it won't obscure things too badly for the developer more familiar with clap's derive that is wanting more details on a specific magic attribute.

If that sounds good or if you have any other thoughts, let us know!

---

_Comment by @jaskij on 2022-05-25 07:41_

As someone who is somewhat familiar with the API, I find myself checking the examples in the repo and then just going off of my knowledge. This is mostly useful for the basics I forgot - like which trait to use.

With the option to just call methods in the attribute, for stuff I don't know off-hand, I often go the other way around - check the methods on the trait, and either check the derive docs for an equivalent or just use the method.

While they are slightly more readable, I'm not entirely sure separate attributes are worth the maintenance effort.

For attributes which are just trait methods with clearer syntax, maybe keep it link-only?

---

_Comment by @epage on 2022-05-25 11:17_

Yes, my plan is to just keep linking to docs.rs for people to look up the builder methods to see what they can use as attributes, not even enumerating the builder methods.  The challenge at hand, which has been an ongoing one judging by the number of duplicate questions of https://github.com/clap-rs/clap/discussions/3538, is how to help users understand this.  My proposal was a tweak to make the raw attribute section for each type of attribute come before the magic attribute one.  Any other ideas to help users understand the relationship of the APIs would be appreciated.

---

_Comment by @CraftSpider on 2022-05-30 21:04_

I think putting raw first might help. Looking at the page, If I jump to 'Command Attributes' and skim my eyes down it, I see 'Magic Attributes', then text, then 'Arg Attributes' is the next thing that catches my eye, skipping over 'Raw Attributes'. I think it's a mix of how the header nesting is deep enough that they aren't much bigger than surrounding text, the large amount of block text and occasional bolded text in the magic attributes, and that the magic attributes section is very long while the raw attributes section is short. All together, I missed it the first time because my eyes treated it as part of the above section, moving it first would help because as the shorter section, it will be clear where it starts and ends.

(I'll admit part of the problem is skimming over reading the documentation properly, but I also feel that should be expected of a reference - it's like a dictionary or index, users expect to jump to the part relevant to them)

---

_Referenced in [clap-rs/clap#3771](../../clap-rs/clap/pulls/3771.md) on 2022-05-30 23:41_

---

_Comment by @epage on 2022-05-31 00:26_

@CraftSpider docs are updated.  Hopefully this works out better for the next person!

---

_Comment by @Be-ing on 2022-07-15 00:06_

As a new user, I am also finding the documentation scattered and confusing. I've spent like 3-4 hours now trying to understand the basics of clap and evaluating whether it's a good choice for what I'm building. I'm including in this time that I spent looking into alternatives on https://libs.rs because the clap documentation was so confusing and hard to follow. I feel like it shouldn't take nearly this much effort and time for new users to get started and decide if clap is a good fit for their project.

I'll walk through some of my thought process trying to figure out how to start to hopefully illustrate the issues. So, being a Rust library, the first place I expect documentation to be is https://docs.rs/clap. Right away, it's confusing: "This allows using the [Derive API](https://github.com/clap-rs/clap/blob/v3.2.12/examples/tutorial_derive/README.md) which provides access to the [Builder API](https://github.com/clap-rs/clap/blob/v3.2.12/examples/tutorial_builder/README.md) as attributes on a struct" You're using one API to access another API? What does that mean?? I'm already confused. Which API should I use and why? Alright, I'll click the links to the documentation for each API. Start reading each page... I'm still confused, which API should I use and why? I'll try going back to docs.rs, hopefully it's clearer than these overwhelming tutorials on GitHub. Scroll down... aspirations section looks good. Ah, finally an explanation of the differences between the APIs.

Okay, it seems the derive API is probably a good fit for my purposes, but I'm not sure yet, so I want to learn a bit more about each API first. Okay, I'll go back to the tutorials on GitHub. Wait, what even are these APIs? I don't see any code, just command output. Keep scrolling hoping to find code to get a better understanding... nope, still no code. Oh, there are "Example" links... but I thought I was already reading the examples? Alright, I'll click the link anyway. Cool, finally found some code. Wait, what is going on in this code?

```rust
#[clap(author, version, about, long_about = None)]
```

Where do the author, verison, and about information come from?? What is a `value_parser` and `action`? What are the differences between these, which one do I want? I guess I'll try to find them in a reference. Go back to docs.rs, can't easily find them. Go back to the derive API tutorial, find the link to the derive API reference. Immediately I'm overwhelmed and give up on that reference. Ah, I guess I'll move on without understanding everything. Okay, the second example has this straightforward code:

```rust
#[clap(name = "MyApp")]
#[clap(author = "Kevin K. <kbknapp@gmail.com>")]
#[clap(version = "1.0")]
#[clap(about = "Does awesome things", long_about = None)]
```

I'm still confused about the very first example without those inline values. Dang, I have a lot of browser tabs open at this point, what was I reading again? Keep reading the tutorial, finally, I find this sentence: "You can use #[clap(author, version, about)] attribute defaults to fill these fields in from your Cargo.toml file." Great! But why wasn't that explained in an inline comment in the source code of the very first example? Keep reading... "Any Command builder function can be used as an attribute." I thought I was reading the Derive documentation, why is this talking about builder functions now? Also, where are those builder functions documented? Should I be using the builder API after all? "Command" looks like the name of a struct, I'll try looking it up on docs.rs... okay, but I'm still lost what the relationship is between what's documented in docs.rs and what I'm reading in this tutorial.

Here are some suggestions to improve this:
1. docs.rs should immediately give a very high level overview with *very simple self-explanatory* examples of each API so users can decide which one to read more about
2. clearer explanations of the relationship between the two APIs with examples
3. Many more inline comments in the example code. Lots of details feel magical and aren't explained in the tutorials.
4. Cross referencing from example code to reference documentation
5. Maybe most importantly, the code should be inline in the tutorials. It's way too hard to follow the tutorials when having to flip back and forth between browser tabs. I think switching to mdbook would be really helpful.

---

_Comment by @tshepang on 2022-07-15 01:09_

I wonder if a (gentle) guide should not be written and pointed to at the top of lib.rs as a "start here". It probably should start with the Builder API, then move to the Derive API.

As a sidenote, I been wanting to write such a guide for a while, sort of a Rust version of [this Python one](https://docs.python.org/3/howto/argparse.html). Other things always got in the way of course, but my main excuse was waitin for clap v3.

---

_Comment by @Be-ing on 2022-07-15 04:32_

From fumbling around the documentation and piecing together scattered information, this is my understanding of the APIs. Please correct me if I'm wrong:

1. The derive API is generally recommended for new code.
2. The derive API is syntax sugar for the builder API. With the builder API you need to use quite a bit of function-like macros and learn their special syntax; with the derive API you instead use attribute macros on a struct.
3. With the derive API, you declare what data you want to get out of the CLI in a struct. Clap automagically sets the members of that struct to values parsed from the CLI input.
4. With the builder API, you create a Command and call various methods to specify the details of how it should parse input, then query the resulting ArgMatches.

---

_Comment by @epage on 2022-07-16 02:26_

Thank you @Be-ing for working through that and taking the time to reflect on your experience and share it!  This will be a big help in finding ways to improve the docs.  I especially agree that not providing a more direct guidance on when to use which API is not ideal.

Some of the structure issues and non-inline code blocks is coming from a bad experience where the examples compiled but did not work.  Our current form of documentation is tested to make sure that the specified arguments will produce the expected output.  I talk more about the different documentation approaches [earlier](https://github.com/clap-rs/clap/issues/3189#issuecomment-1102580052) however, I think I thought of a way around some of the problems we had and will take a crack within the next week or so to try to address it.

In supporting users in clap, one of the hardest things has been trying to get across how to use the builder documentation to use the derive API.  I still feel like trying to reproduce all of the builder documentation as derive documentation would be a big task that would be hard to keep in sync, so it falls on finding ways to communicate that the derive API is a wrapper around the builder API and how to look things up in the builder documentation to use it in the derive API.

---

_Comment by @epage on 2022-07-16 02:26_

@tshepang another related resource is https://rust-cli.github.io/book/index.html

---

_Comment by @Be-ing on 2022-07-16 03:41_

> In supporting users in clap, one of the hardest things has been trying to get across how to use the builder documentation to use the derive API. I still feel like trying to reproduce all of the builder documentation as derive documentation would be a big task that would be hard to keep in sync, so it falls on finding ways to communicate that the derive API is a wrapper around the builder API and how to look things up in the builder documentation to use it in the derive API.

I agree that having nearly duplicate documentation wouldn't be maintainable. Perhaps it could help to have derive macro examples inline in the rustdoc documentation for the builder API?

---

_Referenced in [clap-rs/clap#3952](../../clap-rs/clap/pulls/3952.md) on 2022-07-19 18:47_

---

_Comment by @epage on 2022-07-19 20:00_

The new version of the documentation is now out: https://docs.rs/clap/latest/clap/

Please use this issue for concerns with this approach resolving the concerns in this issue.  If things seem settled on the general approach, I will then close this issue.

Please open new issues for new concerns or specific concerns that can be iterated on with the new approach.

---

_Comment by @Be-ing on 2022-07-19 20:26_

That's much better, thanks!

---

_Referenced in [clap-rs/clap#3957](../../clap-rs/clap/pulls/3957.md) on 2022-07-19 20:38_

---

_Comment by @Be-ing on 2022-07-20 18:03_

I think this issue can be closed now.

---

_Closed by @epage on 2022-07-21 15:12_

---

_Comment by @epage on 2022-08-18 14:07_

As a follow up, raw attributes continue to be a point of confusion so I've created https://github.com/clap-rs/clap/discussions/4090 to further discuss their documentation

---

_Referenced in [hyperium/hyper#3099](../../hyperium/hyper/issues/3099.md) on 2022-12-30 05:31_

---
