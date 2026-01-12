```yaml
number: 970
title: Improve zsh completion
type: pull_request
state: merged
author: okdana
labels: []
assignees: []
merged: true
base: master
head: dana/complete-groups
created_at: 2018-07-03T08:24:13Z
updated_at: 2018-07-06T14:24:17Z
url: https://github.com/BurntSushi/ripgrep/pull/970
synced_at: 2026-01-12T18:23:13Z
```

# Improve zsh completion

---

_@okdana_

I'd been meaning to update this, and i think it's in a state now where it's much better than it was.

I propose that we give this a few days to sit before merging, just to make sure i haven't messed anything up (it is a decent number of lines changed). It's been working for me so far in zsh 5.3 and 5.5, though. If any zsh users watching the repo care to test, that would be pretty cool.

Full details:

* I've updated the function to use spec grouping with `_arguments`. This makes it so much easier (IMO) to read and maintain option exclusions. The down side is that grouping is only available in zsh 5.4+ (released in 2017); on older versions of zsh i simply strip the groups out. This makes option exclusivity pretty inaccurate on those older versions, but i think it's worth it — it's not like it breaks anything, it just continues to show options that might not be useful with options you've already supplied

* I mentioned previously that i wasn't sure whether to complete rarely used 'negation' options like `--messages` and `--no-fixed-strings`. I decided i would complete them, but only if it seems like the user is specifically requesting them, or if they've enabled a style requesting that they always be completed. So if you do `rg --`<kbd>Tab</kbd> you won't get the million `--no-whatever` options, but you will if you do `rg --n`<kbd>Tab</kbd>. To be clear, this only affects the (mostly newer) options that were added solely to negate the effects of other options; stuff that's more generally useful like `--no-heading` and `--no-mmap` is still completed unconditionally

* I've improved the accuracy of option exclusivity (for zsh 5.4+)

* I've improved several option/optarg descriptions

* I've improved completion of colour 'whens' (`always`, `never`, &c.) — they now have descriptions

* I've improved completion of colour specs, in particular the handling of the colons. This was the biggest irritation for me with previous iterations of the function tbh

* I've removed some unnecessary work-arounds

* I've generally replaced some uncommon/undesirable constructs by more idiomatic ones (`print`, quoting stuff, tag names, `ret` to support users with `prefix-needed` off, &c.)

ETA: The CI script failed on some of the AppVeyor platforms because they had an older version of zsh than i expected. Hopefully fixed.

---

_@BurntSushi approved on 2018-07-03 10:45_

Lovely! This all looks good to me. Thank you for improving auto complete support in zsh!

I'll let this bake for a bit as requested. Ping me if I forget about it. :-)

---

_Comment by @okdana on 2018-07-06 04:23_

I found two or three trivial wording choices that i didn't like, but otherwise i guess this is as OK as it'll get for now. Rebased my subsequent fixes, ready to merge whenever you like.

As i was messing with it, though, i noticed that the interaction amongst `--heading`, `--passthru`, `--pretty`, and `--vimgrep` is a little weird. In the completion function i've made `--vimgrep` exclusive with each of the other options i mentioned, but `rg` itself doesn't treat them that way. Instead:

* `--heading` and `--pretty` are just ignored. Probably these should override the colour/heading settings as appropriate, depending on the order they're given in, yeah?

* `--passthru` *kind of* works, but for some reason it produces a match for the last column of every line in addition to the (expected) first column. Even though it doesn't seem very useful, i think allowing `--passthru` + `--vimgrep` should be OK if not for that last-column weirdness. I didn't actually look to see what causes that. But we could just make them conflict, too, if you don't think it's a valid use case.

Let me know how you feel and i'll do another PR for those.

---

_Comment by @BurntSushi on 2018-07-06 14:22_

@okdana Thanks for doing this! As to your questions... They are good ones. My feeling is that the interaction between `--vimgrep` and `--passthru` will _probably_ be fixed in libripgrep (I haven't investigated, but it could be related to #441 and/or #690).

As for `--heading` and `--pretty`... They seem like conflicting options to me. I guess in my mind `--vimgrep` always intended to match a specific format to be read by vim itself, and options like `--heading` and `--pretty` kind of negate that. But yeah, maybe they should just work since it seems like there exists sensible behavior there.

In the course of working on libripgrep, I'm going to try and revisit the interactions between flags because it's going to get worse with the rise of support for additional output formats (thinking Ackmate and JSON lines). So I guess I'd say sit tight for now and we can try to knock out all that weirdness in one swoop.

---

_Merged by @BurntSushi on 2018-07-06 14:23_

---

_Closed by @BurntSushi on 2018-07-06 14:23_

---
