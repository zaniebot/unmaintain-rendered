```yaml
number: 2299
title: "`--engine kleenexp`"
type: issue
state: closed
author: SonOfLilit
labels: []
assignees: []
created_at: 2022-09-04T07:49:35Z
updated_at: 2022-09-04T16:57:21Z
url: https://github.com/BurntSushi/ripgrep/issues/2299
synced_at: 2026-01-12T16:13:24Z
```

# `--engine kleenexp`

---

_@SonOfLilit_

Hello,

I wrote and am about to launch [Kleenexp](https://github.com/SonOfLilit/kleenexp), a (Rust) compiler from a nice pleasant syntax to regular expressions, with some desirable features like needing a lot less escaping (and no double escaping), macros, number ranges (e.g. `#0..255` => `(?:\d|[1-9]\d|2[0-4]\d|25[0-5]`) and separators (`[separated:, 4 [#digits]]` => `(?:\d+)(?:,\d+){3}`).

Though I assume the project won't be considered mature enough right now, in the long term I'd like submit a PR to `ripgrep` that implements `--engine kleenexp` (so I can `alias kg=rg --engine=kleenexp` and enjoy grepping with the same syntax I use to search in `vscode`), so I'd like to ask in advance what needs to happen for such a PR to be welcomed, so I can plan to evolve the project to meet those requirements.

For example an answer might involve:

* Just "no, it's out of scope and always will be"
* Better support for rust regex syntax in the generator, obviously (which I expect to get done this week), full support for all esoteric regex features
* Some indication of code stability and maturity? Indication that it's likely to stay maintained for many years to come? Which?
* Some code size/performance metrics? Which?

Thanks a lot,
Aur

---

_Comment by @BurntSushi on 2022-09-04 16:57_

I don't think it's out of scope, but yeah I think the answer here is "just no." Scope is maybe _part_ of that, but there's actually a whole host of reasons:

* Kleenexp is one of many many many "regex for humans" flavors. There are even several of them written in Rust. Either one is a clear and obvious winner (which I don't believe is the case) or ripgrep allows all of them (which is impractical). Since I am personally not invested in the "regex for humans" idea---especially in a grep tool---I don't see myself taking any action here.
* The maintenance burden of a third regex engine inside of ripgrep.
* Backwards compatibility. How do I know you aren't going to break the syntax in a future revision? That breakage will be passed on to users of ripgrep.
* A whole new dependency tree. Right now, I do my best to keep a tight leash on ripgrep's dependency tree. Your project doesn't have many dependencies (assuming you split it up into a library and dropped `clap`), but maybe that won't be true a year from now.
* Maturity in general. That is, at what point does one gain confidence that your new regex dialect is appealing to the masses? I'm not sure.

My suspicion is that PCRE2 is and will always be the only other regex engine supported by ripgrep. 

Note that it is possible to maintain a fork of ripgrep with an additional regex engine inside of it. Someone does that for Hyperscan for example: https://sr.ht/~pierrenn/ripgrep/ ... Hopefully, the actual patch to ripgrep is itself small, and most of the changes are just in new code. With that said, it looks like Kleenexp works by compiling down to Rust regex syntax. So I imagine you might actually just be able to write a wrapper script that translates your Kleenexp to a Rust regex and then invoke ripgrep. It'd be fair of work to make it right in all cases, but I bet you could have something simple working without much effort. Otherwise, yeah, I think the best route would be a patch to ripgrep that you maintain.

---

_Closed by @BurntSushi on 2022-09-04 16:57_

---

_Locked by @ghost on 2022-09-04 16:57_

---
