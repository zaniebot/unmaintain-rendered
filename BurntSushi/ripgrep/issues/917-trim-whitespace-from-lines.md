```yaml
number: 917
title: trim whitespace from lines
type: issue
state: closed
author: eljulians
labels:
  - enhancement
  - libripgrep
assignees: []
created_at: 2018-05-11T10:29:28Z
updated_at: 2018-08-20T11:10:21Z
url: https://github.com/BurntSushi/ripgrep/issues/917
synced_at: 2026-01-12T16:13:22Z
```

# trim whitespace from lines

---

_@eljulians_

#### What version of ripgrep are you using?

```
ripgrep 0.8.1 (rev c8e9f25b85)
-SIMD -AVX 
```
#### How did you install ripgrep?

From `.deb` binary.

#### What operating system are you using ripgrep on?

Linux Mint 18.1 Serena.

#### Describe your question, feature request, or bug.

First of all, thanks and congratulations for this tool.

I've searched in the help and in the issues, and as I didn't find anything related, I'm opening this suggestion.

I think that would be nice to add an option for trimming the output of the coincident lines. In other words, not to print the indentation of the lines. I think that this could improve the readability of the output.

Currently, for this search:

```
rg POST views/
```

I would get the following output:

```
views/modals/contact.twig
13:               <form action="/contact/send" method="POST" id="form-contact">
```
(Note the code indentation).

With an option for trimming (e.g. `--trim`), we would get the following:

```
views/modals/contact.twig
13:<form action="/contact/send" method="POST" id="form-contact">
```
(The indentation is now removed from the output).

I really think that this could be a very useful option, since almost every part of the code is indented, except class or function definitions.

**PD:** this could also work in combination with `--max-columns` option, so the trimming would be done before the output is filtered with this options. But as I know nothing about ripgrep internals and the feasibility and difficulty of this, I just leave it as a little extra suggestion. Actually this wouldn't be very relevant, since the `--max-columns` is thought for the output we actually don't want to see. So, if you find appropriate the original suggestion, just implement it as you think it's better for ripgrep. :) 

Thanks again and keep up the great work!

---

_Label `enhancement` added by @BurntSushi on 2018-05-11 10:32_

---

_Label `question` added by @BurntSushi on 2018-05-11 10:32_

---

_Comment by @BurntSushi on 2018-05-11 10:32_

This is an interesting request. I'm not sure if it's a fit. What would you use it for? I see your example, but indentation is often a useful artifact to see in the output. Is it just an aesthetic thing?

---

_Comment by @eljulians on 2018-05-11 10:38_

@BurntSushi Thanks for such a quick response.

Actually, yes, just an aesthetic thing. Personally, when grepping code, I don't find the indentation interesting: if I later need to edit the file, I already know the line to go to; and if I just need to read the output, because of the lacking context, as we just get the single line, the indentation doesn't actually help in many cases, IMHO.

---

_Comment by @BurntSushi on 2018-05-11 10:39_

@julenpardo Interesting. So if this flag were added, would you just have it always enabled? How would you use it?

---

_Comment by @okdana on 2018-05-11 10:42_

For whatever it's worth, this is a feature that interests me too, and if you decide that it's a good idea i'd be willing to work on it. I mentioned in #544 that i used to have a wrapper script for `ag` that achieved this effect, but it was ugly and didn't always work well. Like @julenpardo says, the purpose is just readability; i find that indentation in the search output, disconnected as it is from the surrounding context, is very rarely useful — it just adds visual noise that makes it harder to understand the results.

If the feature were added, i would put `--trim` (or w/e) in my default options and then just add `--no-trim` (or w/e) in the rare cases where i needed it turned off.

---

_Comment by @eljulians on 2018-05-11 10:53_

@BurntSushi I was thinking about adding it as a non-default option and then adding it to my ripgrep config file. I mean, I wouldn't make as default such a significant visual change in the output just because it came to my mind :) 

---

_Comment by @BurntSushi on 2018-05-11 11:27_

@julenpardo Oh, no, yeah, this definitely can't be enabled by default. What I meant is whether _you_ would enable it by default. :-) In any case, I think you and @okdana sold me.

I see two ways this can get added:

* Someone wants to brave the current printer and do it.
* I'll add it as part of work on libripgrep. (I have actually started this, and even written down some code. But realistically is probably still a ways away from completion.)

---

_Label `question` removed by @BurntSushi on 2018-05-11 11:28_

---

_Comment by @BurntSushi on 2018-07-06 14:30_

I am working on the new printer right now, and I should be able to add this in with very little fuss.

---

_Label `libripgrep` added by @BurntSushi on 2018-07-06 14:31_

---

_Added to milestone `libripgrep` by @BurntSushi on 2018-07-06 14:31_

---

_Renamed from "[Suggestion] Trim coincident lines" to "trim coincident lines" by @BurntSushi on 2018-07-06 14:58_

---

_Renamed from "trim coincident lines" to "trim whitespace from lines" by @BurntSushi on 2018-07-06 14:58_

---

_Closed by @BurntSushi on 2018-08-20 11:10_

---
