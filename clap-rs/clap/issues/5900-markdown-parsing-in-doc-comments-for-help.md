```yaml
number: 5900
title: Markdown parsing in doc comments for help messages Tracking Issue
type: issue
state: open
author: ModProg
labels:
  - A-help
  - C-tracking-issue
  - S-waiting-on-design
assignees: []
created_at: 2025-02-03T22:20:38Z
updated_at: 2025-02-07T10:13:12Z
url: https://github.com/clap-rs/clap/issues/5900
synced_at: 2026-01-12T16:14:17Z
```

# Markdown parsing in doc comments for help messages Tracking Issue

---

_@ModProg_

This tracks the feature `unstable-markdown` that implements compile time markdown parsing in the derive macros.

## Implemented Markdown features
| markdown | output | comment |
| - | - | - |
|  paragraph | joined on one line, surrounded by empty line |
| `# heading` | bold + underline + surrounded by empty line | default style for heading |
| `![image]()` | alt text | anyone feeling sixel?!|
| `<html>` | verbatim |
| `> blockquote` | ![image](https://github.com/user-attachments/assets/3eec1b6e-fcc5-4293-9bb5-1fa1e869b270) | alternatively could use `>` |
| code block | ![image](https://github.com/user-attachments/assets/a3ce198c-a4e1-43d9-9a42-bd77dc2733c6)| same styling as `` `code` ``|
| (un)ordered lists | ![ordered list example](https://github.com/user-attachments/assets/880b93b9-b19c-445e-9605-280f41dff941) | |
| `*emphasis*` | italic |
| `**strong**` | bold |
| `~stikethrough~` | strikethrough |
| `[link](url)` | underlined alt text + OSC 8 |
| `` `inline code ` `` | bold | default style for literal |
| ` --- ` hrule | `---` surrounded by empty lines |
| ` \| tables \| ` | verbatim |
| `\\\n` and `  \n` hard break | ends line |

## Open Questions
- [ ] unicode support i.e. `•` for bullet lists, `Options::ENABLE_SMART_PUNCTUATION` for `pulldown_cmark`, unicode lines for blockquotes
- [ ] using the default clap styling for code makes it the same to bold.
- [ ] should we make images a link to the image source
- [ ] should we add custom html tags a la https://docs.rs/color-print/latest/color_print/ this would allow users to use colors without us actually supporting runtime style information.
- [ ]  should we use other enumeration for nested lists? e.g. `a.`, `b.` and `i.`, `ii.`? we could use https://docs.rs/nominals for that
- [ ] styling for code blocks and block quotes
- [ ] separate control of OSC 8 for links & colors
- [ ] option to display URLs next to links.
- [ ] option to diplay URLs at the end of the help.

## Markdown Parser
See https://github.com/rosetta-rs/md-rosetta-rs for a comparison of parsers.

### `pulldown-cmark`
Same behavior as rustdoc (options for reference https://github.com/rust-lang/rust/blob/59588250ad973ce69bd15879314c9769e65f36b3/compiler/rustc_resolve/src/rustdoc.rs#L244-L250).

But higher compile times, runtime performance probably not as impactful as we are doing it at compile time.

### `minimad`
Had some parsing issues but might be fixable.


---

_Referenced in [clap-rs/clap#5891](../../clap-rs/clap/pulls/5891.md) on 2025-02-03 22:21_

---

_Label `A-help` added by @epage on 2025-02-03 22:37_

---

_Label `C-tracking-issue` added by @epage on 2025-02-03 22:37_

---

_Label `S-waiting-on-design` added by @epage on 2025-02-03 22:37_

---

_Referenced in [clap-rs/clap#2389](../../clap-rs/clap/issues/2389.md) on 2025-02-03 22:37_

---

_Comment by @ilyagr on 2025-02-04 00:43_

(I initially posted this in https://github.com/clap-rs/clap/pull/5891#discussion_r1940297251 , but I think this is a better place)

Thanks for a cool feature! 


I haven't looked at this PR carefully, but I'd appreciate an option to print hyperlink targets normally in addition to possible terminal hyperlinks, so that the user can copy-paste them normally. Even if the terminal understands hyperlinks, users might not expect a terminal to behave like a web browser.

Also, some terminals make it hard to use hyperlinks in spite of supporting them. For example, in Ghostty 1.1.0 on a Mac, if mouse detection is on (the program inside the terminal requests mouse events, which for me is `tmux`), you need to hold both Cmd and Shift to be able to click on a link. I think that might be a bug (I might report it to them), but they might consider it a feature, and in any case it illustrates a problem.

---

_Comment by @ModProg on 2025-02-04 10:17_


> I haven't looked at this PR carefully, but I'd appreciate an option to print hyperlink targets normally in addition to possible terminal hyperlinks, so that the user can copy-paste them normally.

Currently you could achieve this by using the `<https://example.com>` syntax.

What would you like to see here additionally?

---

_Comment by @epage on 2025-02-04 18:04_

One question is what parser to use for this.  rustdoc compatibility is great but so is having good build times.  I would say that my bare minimum requirement is that a subset of markdown can be parsed by rustdoc and clap.

In #5891, @dpc mentioned the idea of using a [djot](https://djot.net/) parser.  It was added to https://github.com/rosetta-rs/md-rosetta-rs.   Not as fast to build as minimad but faster than pulldown-cmark.  Release parse times are all in the same ballpark.  Looking over their [quick start](https://github.com/jgm/djot/blob/main/doc/quickstart-for-markdown-users.md), it mostly looks like a subset would work.  The one question I had was about `**strong**` as they only mention `*strong*` support which is `*emphasis*`/`_emphasis_` in markdown.  Looking over [the reference](https://htmlpreview.github.io/?https://github.com/jgm/djot/blob/master/doc/syntax.html#emphasisstrong) and using the play ground, it looks like it can work because `**strong**` creates `<strong><strong>strong</strong></strong>`

It would be good to evaluate minimad further to see if it has a subset of markdown that renders that same as rustdoc and how that compares to djot.

---

_Referenced in [hellux/jotdown#80](../../hellux/jotdown/issues/80.md) on 2025-02-04 18:10_

---

_Comment by @ilyagr on 2025-02-04 20:37_

> > I haven't looked at this PR carefully, but I'd appreciate an option to print hyperlink targets normally in addition to possible terminal hyperlinks, so that the user can copy-paste them normally.
> 
> Currently you could achieve this by using the `<https://example.com>` syntax.
> 
> What would you like to see here additionally?

If I write

```
You must [frobnicate] all inputs!

[frobnicate]: https://example.com
```

I might want it to keep looking like that on output. I'd be quite happy for "frobnicate" to also become a terminal hyperlink, but I'd want to keep the last line in the output in the help text (if it goes to the terminal) for copy-pasting ease.

I'd probably want the same for

```
You must [frobnicate](https://example.com) all the inputs.
```

OTOH, when the same text is used to generate HTML for a [CLI Reference](https://jj-vcs.github.io/jj/prerelease/cli-reference/) (which I currently do with <https://github.com/ConnorGray/clap-markdown>, which has some limitations), then of course I would want only a normal hyperlink.

---

_Comment by @ModProg on 2025-02-06 14:05_

> OTOH, when the same text is used to generate HTML for a [CLI Reference](https://jj-vcs.github.io/jj/prerelease/cli-reference/) (which I currently do with https://github.com/ConnorGray/clap-markdown, which has some limitations), then of course I would want only a normal hyperlink.

This is difficult to impossible with the current approach (actually this feature in its current state will break clap-markdown unless it starts parsing ANSI escapes), the problem is we do the markdown handling at compile time, i.e. we basically cannot change the output at runtime.

If we want to make it configurable, we'd need to add flags to `#[clap(...)]`.

Not an actual solution but a workaround for what you want to render:

```rs
#[derive(Debug, Parser)]
struct Args {
    /// You must [frobnicate] all inputs!
    ///
    /// [frobnicate]: https://example.com
    first: String,
    /// You must [frobnicate](https://example.com) all inputs!
    second: String,
    /// You must [frobnicate] all inputs!
    ///
    /// \[frobnicate]: <https://example.com>
    /// 
    /// [frobnicate]: https://example.com
    working_first: String,
    /// You must [frobnicate](https://example.com)(<https://example.com>) all inputs!
    working_second: String,
}
```

![Image](https://github.com/user-attachments/assets/442f8dde-a992-4cd5-894c-324e06c4f44c)
 



---

_Comment by @ModProg on 2025-02-06 14:09_

> It would be good to evaluate minimad further to see if it has a subset of markdown that renders that same as rustdoc and how that compares to djot.

We should probably create some test cases to compare the “correctness” of different non pulldowncmark implementations to decide whether we can accept/fix them.

---

_Comment by @ilyagr on 2025-02-06 22:35_

Thanks for the workaround!

I appreciate the difficulty; I'm not sure what the best solution is. It seems like this might be a blocker for us for turning on the markdown rendering (which is not the end of the world), but maybe we'll use the workaround, or figure something else out.

I also hope that if markdown rendering stays off, `clap_markdown` won't break.

---

_Comment by @ilyagr on 2025-02-06 22:59_

Here's one possible approach. Instead of trying to reproduce the exact formatting of the input, just detect all the links in the output, and have the option to display them after the text.

So, even if the input is

```rust
    /// You must [frobnicate](https://example.com) all inputs!
    second: String,
```

the output to the terminal could have the link shown in the footnote style:

> `<SECOND>`
> &emsp;&emsp; You must [frobnicate](https://example.com) all the inputs!
>
> &emsp;&emsp; \[frobnicate\]: https://example.com


There could also be square brackets around the first "frobnicate", but I don't think they would be very useful[^1]. OTOH, the output style could look even less like Markdown, this look is just the first one that came to mind and seemed OK.

[^1]: if you are sure the underline gets shown in the terminal, at least. Actually, another upside of this approach is that you could eventually use slightly different output format depending on what you know about the terminal capabilities.

---

_Referenced in [jj-vcs/jj#5605](../../jj-vcs/jj/issues/5605.md) on 2025-02-06 23:36_

---

_Referenced in [maplibre/martin#1736](../../maplibre/martin/pulls/1736.md) on 2025-03-09 21:45_

---

_Referenced in [astral-sh/uv#12807](../../astral-sh/uv/pulls/12807.md) on 2025-04-10 13:24_

---

_Referenced in [astral-sh/uv#13578](../../astral-sh/uv/pulls/13578.md) on 2025-05-21 21:25_

---

_Referenced in [clap-rs/clap#6087](../../clap-rs/clap/issues/6087.md) on 2025-08-04 15:11_

---

_Referenced in [clap-rs/clap#6158](../../clap-rs/clap/issues/6158.md) on 2025-10-30 18:21_

---
