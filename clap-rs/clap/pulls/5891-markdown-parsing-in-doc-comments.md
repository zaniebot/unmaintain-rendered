```yaml
number: 5891
title: Markdown parsing in doc comments
type: pull_request
state: merged
author: ModProg
labels: []
assignees: []
merged: true
base: master
head: derive-markdown
created_at: 2025-01-24T22:43:27Z
updated_at: 2025-02-04T02:50:22Z
url: https://github.com/clap-rs/clap/pull/5891
synced_at: 2026-01-12T16:14:17Z
```

# Markdown parsing in doc comments

---

_@ModProg_

fixes #2389

- [ ] add tests for markdown parsing


| markdown | output | comment |
| - | - | - |
|  paragraph | surrounded by empty line |
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


---

_@ModProg reviewed on 2025-01-26 03:16_

---

_Review comment by @ModProg on `Cargo.toml`:209 on 2025-01-26 03:16_

We can remove this example or incorporate it in others once it's finished, was more for testing while developing.

---

_@ModProg reviewed on 2025-01-26 03:17_

---

_Review comment by @ModProg on `clap_derive/src/utils/doc_comments.rs`:211 on 2025-01-26 03:17_

Decide whether we want to support colors.

---

_@ModProg reviewed on 2025-01-26 03:17_

---

_Review comment by @ModProg on `clap_derive/src/utils/doc_comments.rs`:242 on 2025-01-26 03:17_

Decide markdown styling

---

_@ModProg reviewed on 2025-01-26 03:18_

---

_Review comment by @ModProg on `clap_derive/src/utils/doc_comments.rs`:368 on 2025-01-26 03:18_

Should we render rule as `---` or is there anything else we can do without terminal width

---

_@epage reviewed on 2025-01-27 16:26_

---

_Review comment by @epage on `clap_derive/src/utils/doc_comments.rs`:182 on 2025-01-27 16:26_

On newlines, we should reset the output and start it up again on the next line

See #4767

---

_@epage reviewed on 2025-01-27 16:26_

---

_Review comment by @epage on `clap_derive/src/utils/doc_comments.rs`:190 on 2025-01-27 16:26_

Like unicode, link support is very iffy between terminal emulators.  In theory, they should just ignore it if its not supported.  Thats no the case in practice.

---

_@epage reviewed on 2025-01-27 16:27_

---

_Review comment by @epage on `clap_derive/src/utils/doc_comments.rs`:145 on 2025-01-27 16:27_

TODOs will need to be handled

---

_@epage reviewed on 2025-01-27 16:28_

---

_Review comment by @epage on `clap_derive/src/utils/doc_comments.rs`:153 on 2025-01-27 16:28_

Ideally this would be in the `literal` style but we don't have access to it

---

_@epage reviewed on 2025-01-27 16:30_

---

_Review comment by @epage on `clap_derive/src/utils/doc_comments.rs`:173 on 2025-01-27 16:30_

We should do something for block quotes and code blocks

For block quotes, we could
- Indent
- Indent with a leading pipe (to look like how its commonly rendered)
- Indent with a leading `>`

For code blocks, we could
- Indent and apply the `style_code`
- Add on a leading pipe or other decor

---

_@epage reviewed on 2025-01-27 16:31_

---

_Review comment by @epage on `clap_derive/src/utils/doc_comments.rs`:192 on 2025-01-27 16:31_

Do we need a link style or should we leave that to terminals?

---

_@epage reviewed on 2025-01-27 16:32_

---

_Review comment by @epage on `clap_derive/src/utils/doc_comments.rs`:379 on 2025-01-27 16:32_

What happens to tables with this PR?

---

_@epage reviewed on 2025-01-27 16:33_

---

_Review comment by @epage on `clap_derive/Cargo.toml`:37 on 2025-01-27 16:33_

A way to merge this more quickly is to have an `unstable-markdown` feature enabling this

---

_@ModProg reviewed on 2025-01-27 17:03_

---

_Review comment by @ModProg on `clap_derive/src/utils/doc_comments.rs`:379 on 2025-01-27 17:03_

I would expect they'd behave similar to now, as I didn't enable them in pulldown-cmark i.e., it should handle them as regular text. but I will add actual tests for this

---

_@ModProg reviewed on 2025-01-27 17:03_

---

_Review comment by @ModProg on `clap_derive/Cargo.toml`:37 on 2025-01-27 17:03_

yes, this would definitely be a good idea 

---

_@ModProg reviewed on 2025-01-27 17:11_

---

_Review comment by @ModProg on `clap_derive/src/utils/doc_comments.rs`:190 on 2025-01-27 17:11_

I checked and disabling colored help will also disable the link escape, is that good enough? Or should we never emit the link and instead, e.g., emit the standard Markdown syntax?

---

_@ModProg reviewed on 2025-01-27 17:13_

---

_Review comment by @ModProg on `clap_derive/src/utils/doc_comments.rs`:153 on 2025-01-27 17:13_

Should I leave dimmed for now? Or remove it and just emit `` ` ``?

---

_@ModProg reviewed on 2025-01-27 17:15_

---

_Review comment by @ModProg on `clap_derive/src/utils/doc_comments.rs`:192 on 2025-01-27 17:15_

At least my terminal emulator didn't do any link highlighting on its own, meaning for cases where something is not a literal URL, one cannot tell.

---

_@ModProg reviewed on 2025-01-27 17:19_

---

_Review comment by @ModProg on `clap_derive/src/utils/doc_comments.rs`:379 on 2025-01-27 17:19_

```
 | Hello | World | | ----- | ----- | | This  | is    | | a     | table |
```
Definitely not what someone would want, but it is the same as it was before this feature.

Though there would still be a little improvement as this table would be rendered “correctly”:
```rs
    /// | Hello | World |\
    /// | ----- | ----- |\
    /// | This  | is    |\
    /// | a     | table |
```

---

_@ModProg reviewed on 2025-01-27 17:21_

---

_Review comment by @ModProg on `clap_derive/src/utils/doc_comments.rs`:379 on 2025-01-27 17:21_

IMO, we can leave that to an extension of this feature, as that would especially profit from happening at runtime with knowledge of the terminal width.

---

_@ModProg reviewed on 2025-01-27 17:28_

---

_Review comment by @ModProg on `clap_derive/src/utils/doc_comments.rs`:182 on 2025-01-27 17:28_

Probably not an issue as markdown styles should all be inline i.e., the reset should have already happened at the newline, but nevertheless better safe than sorry.

---

_@epage reviewed on 2025-01-27 17:31_

---

_Review comment by @epage on `clap_derive/src/utils/doc_comments.rs`:379 on 2025-01-27 17:31_

> Definitely not what someone would want, but it is the same as it was before this feature.

Thats surprising.  I would have expected that the pipes are being emitted as Events, and not pipes, leading to it being
```
Hello World this is a table
```

I'd like us to have a test that covers every markdown feature so we can see how they turn out.  We could include an SVG snapshot to cover any ANSI escape code formatting.

---

_@ModProg reviewed on 2025-01-27 17:34_

---

_Review comment by @ModProg on `clap_derive/src/utils/doc_comments.rs`:182 on 2025-01-27 17:34_

This prompted me to test `-F wrap_help` and that seems to actually have a similar problem:

![grafik](https://github.com/user-attachments/assets/eef28817-004c-47de-b9b6-daadf38fcd38)


---

_@epage reviewed on 2025-01-27 17:38_

---

_Review comment by @epage on `clap_derive/src/utils/doc_comments.rs`:190 on 2025-01-27 17:38_

Like unicode, Cargo controls them separately.  While this feature isn't being used by Cargo, we should consider use cases like it.  Just because a terminal handles links poorly, should users lose color?

According to https://gist.github.com/egmontkob/eb114294efbcd5adb1944c9f3cb5feda#backward-compatibility

> At this moment, terminals known to be buggy (OSC 8 resulting in display corruption) are VTE versions up to 0.46.2 and 0.48.1, Windows Terminal up to 0.9, Emacs's built-in terminal, and [screen](https://savannah.gnu.org/bugs/index.php?57718) with 700+ character long URLs.

VTE is on 0.79, https://gitlab.gnome.org/GNOME/vte/-/releases

Windows Terminal is on 1.22

Not worried about screen's character limit

So that just leaves emacs?  Maybe I'm biased but might be ok to live with that.

Whichever direction we go, we should document this in the PR description, the feature, and the issue.

---

_@epage reviewed on 2025-01-27 17:39_

---

_Review comment by @epage on `clap_derive/src/utils/doc_comments.rs`:153 on 2025-01-27 17:39_

Dimmed is for de-emphasizing which is the opposite of what we want for literals.  I've also heard reports that dimmed is handled poorly by some terminals.

Maybe invert?

---

_@ModProg reviewed on 2025-01-27 17:39_

---

_Review comment by @ModProg on `clap_derive/src/utils/doc_comments.rs`:379 on 2025-01-27 17:39_

> Thats surprising. I would have expected that the pipes are being emitted as Events, and not pipes, leading to it being

Tables are not a standard markdown feature, therefor you'd have to explicitly enable them in pulldown-cmark.
https://docs.rs/pulldown-cmark/latest/pulldown_cmark/struct.Options.html#associatedconstant.ENABLE_TABLES

---

_@ModProg reviewed on 2025-01-27 17:41_

---

_Review comment by @ModProg on `clap_derive/src/utils/doc_comments.rs`:379 on 2025-01-27 17:41_

> We could include an SVG snapshot to cover any ANSI escape code formatting.

Is there any testing library to use for that?

---

_@epage reviewed on 2025-01-27 17:42_

---

_Review comment by @epage on `clap_derive/src/utils/doc_comments.rs`:379 on 2025-01-27 17:42_

Huh, they are so ingrained that I forgot they are an extension.  Granted, that means that users will likely assume markdown support means table support, *especially* if our goal is to mimic rustodc.

On the other hand, they are less likely to show up and have wrapping challenges.

---

_@epage reviewed on 2025-01-27 17:44_

---

_Review comment by @epage on `clap_derive/src/utils/doc_comments.rs`:379 on 2025-01-27 17:44_

> Is there any testing library to use for that?

We use snapbox which allows for SVG snapshots.  We just need the feature enabled and mark the file with a `.term.svg` extension and it will be handled automatically.  It might also be supported in trycmd which means this could be part of `tests/ui`.

---

_@ModProg reviewed on 2025-01-27 19:02_

---

_Review comment by @ModProg on `clap_derive/src/utils/doc_comments.rs`:153 on 2025-01-27 19:02_

> Maybe invert?

IIRC, invert also had some problems sometimes, but we could go for it for now, as you are right, de-emphasizing is not what we should be doing.

Really makes me think that this is only fully completed once we find a way to use the correct styles.

Would there be any value in using the default styles?

---

_@ModProg reviewed on 2025-01-27 19:08_

---

_Review comment by @ModProg on `clap_derive/src/utils/doc_comments.rs`:190 on 2025-01-27 19:08_

> Like unicode, Cargo controls them separately.

I'm all for choice, but not sure how to implement that :D 

Things like this make me think that maybe for a 1.0 or 2.0 of markdown support we do need to add a runtime component, either by generating multiple help literals and switching dynamically or by extending `StyledStr` to support some form of markup, either "inband" (i.e., some form of custom escape/syntax) or "out-of-band", adding some form of styles as a separate field to the `String`.

---

_Review comment by @epage on `clap_derive/src/utils/doc_comments.rs`:190 on 2025-01-27 19:33_

One idea I had was to extend `StyledStr` to store a `Box<Fn>` so we could defer all decisions until render time.

---

_@epage reviewed on 2025-01-27 19:34_

---

_@ModProg reviewed on 2025-01-27 20:22_

---

_Review comment by @ModProg on `clap_derive/src/utils/doc_comments.rs`:190 on 2025-01-27 20:22_

Right, that would definitely be the most flexible one, maybe one could even give it access to the expected width, so it could e.g., layout tables correctly.

---

_@epage approved on 2025-01-31 22:39_

---

_@epage approved on 2025-02-03 21:31_

---

_@ilyagr reviewed on 2025-02-04 00:08_

---

_Review comment by @ilyagr on `clap_derive/src/utils/doc_comments.rs`:190 on 2025-02-04 00:08_

**Update:** Moved this comment to https://github.com/clap-rs/clap/issues/5900#issuecomment-2632502849

I haven't looked at this PR carefully, but I'd appreciate an option to print hyperlink targets normally in addition to possible terminal hyperlinks, so that the user can copy-paste them normally. Even if the terminal understands hyperlinks, users might not expect a terminal to behave like a web browser.

Also, some terminals make it hard to use hyperlinks in spite of supporting them. For example, in Ghostty 1.1.0 on a Mac, if mouse detection is on (the program inside the terminal requests mouse events, which for me is `tmux`), you need to hold both Cmd and Shift to be able to click on a link. I think that might be a bug (I might report it to them), but they might consider it a feature, and in any case it illustrates a problem.

---
