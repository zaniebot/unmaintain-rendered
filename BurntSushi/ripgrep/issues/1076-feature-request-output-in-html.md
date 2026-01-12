```yaml
number: 1076
title: "Feature Request: Output in HTML"
type: issue
state: closed
author: zivcaspi
labels:
  - invalid
assignees: []
created_at: 2018-10-04T10:48:24Z
updated_at: 2018-10-04T11:45:58Z
url: https://github.com/BurntSushi/ripgrep/issues/1076
synced_at: 2026-01-12T16:13:22Z
```

# Feature Request: Output in HTML

---

_@zivcaspi_

#### What version of ripgrep are you using?

ripgrep 0.10.0
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)

#### How did you install ripgrep?

Cargo

#### What operating system are you using ripgrep on?

Windows 10

#### Describe your question, feature request, or bug.

Feature request: Add support in the printer for outputting the text in a way which preserves colors in non-terminal environments. For example, to be able to pipe the output of the tool into the clipboard and then paste it into a document. Thanks




---

_Comment by @BurntSushi on 2018-10-04 10:51_

Please see the documentation for the color flag. In particular, use `--color always`.

---

_Closed by @BurntSushi on 2018-10-04 10:51_

---

_Label `invalid` added by @BurntSushi on 2018-10-04 10:51_

---

_Comment by @zivcaspi on 2018-10-04 11:03_

Thanks, but the problem isn't that the text output isn't colored, it's that (at least on Windows) the colored console output is copied without the color attributes, so that if I want to send the output of ripgrep to somebody over email I have to do a screen capture to get the colors preserved -- If I use tools such as clip (or select + CTRL-C) the colors will be lost.

---

_Comment by @BurntSushi on 2018-10-04 11:06_

That sounds like a feature request for whatever terminal you're using. There ain't nothing ripgrep can do AFAIK.

---

_Comment by @zivcaspi on 2018-10-04 11:10_

The feature request for the terminal is to be able to copy the color attributes (I have already created it). The one on ripgrep is to be able to output HTML.

---

_Comment by @BurntSushi on 2018-10-04 11:16_

OK... I will say that it is frustrating as a maintainer when feature requests don't include relevant details initially. Instead, I seem to be getting details in a slow trickle. Are there any other relevant details you've omitted?

ripgrep is not going to grow an HTML output mode. If that's the only way to achieve colors when copying and pasting, then it sounds like the terminal itself should be improved to support things other than HTML. If that's not feasible, then write a tool that converts ANSI escape sequences (emitted by ripgrep) to HTML.

---

_Comment by @zivcaspi on 2018-10-04 11:36_

Sorry, I thought the title of the request was a dead giveaway...

Cool, thanks.

---

_Comment by @BurntSushi on 2018-10-04 11:45_

Ah, my bad. I initially read this via email and the subject line was cutoff and just had "Feature Request: Output in." Sorry about that!

---
