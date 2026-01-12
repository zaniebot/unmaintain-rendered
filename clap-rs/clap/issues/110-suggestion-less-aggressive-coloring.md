```yaml
number: 110
title: "suggestion: less aggressive coloring"
type: issue
state: closed
author: Byron
labels: []
assignees: []
created_at: 2015-05-06T09:49:10Z
updated_at: 2018-08-02T03:29:39Z
url: https://github.com/clap-rs/clap/issues/110
synced_at: 2026-01-12T16:14:08Z
```

# suggestion: less aggressive coloring

---

_@Byron_

In _v0.8.0_, text coloring was introduced which I generally am a great fan of. Having it in usage strings seems like something I definitely want to have.

The first implementation of it applies it to the entire output of the usage string, which doesn't help one to focus on what's actually the problem.
Therefore I would recommend to use color (and/or bold text) only to highlight portions of it, like the required parameters that were missing. This could also be used to highlight suggestions in a friendly color.
Of course, a disclaimer shall be placed here that this is a somewhat cosmetical issue which is to some extend subjective. This issue is nothing more than a suggestion.

As a side-note, thanks for implementing everything with dependencies in a feature-gated fashion to allow an opt-out, which is what I chose as an [intermediate solution](https://github.com/Byron/google-apis-rs/commit/294da41a308e4f4125db876c5b24084ae8cfcb5f) in my case. Generally, that's a great practice that I will adopt were adequate as well.


---

_Comment by @kbknapp on 2015-05-06 12:11_

I agree. Perhaps the entire string being red is a bit much. I want avoid having rainbow output though where errors are one color, suggestions another, regular out another, etc.  Perhaps a good middle ground would be the error and suggestions are red, the usage and "for more information" string are regularly formatted.


---

_Referenced in [clap-rs/clap#111](../../clap-rs/clap/pulls/111.md) on 2015-05-06 12:49_

---

_Comment by @Byron on 2015-05-06 12:59_

Sounds good to me ! I am watching the PR and will give it a whirl once I have a minute.
Thanks for taking care.


---

_Closed by @kbknapp on 2015-05-06 15:43_

---
