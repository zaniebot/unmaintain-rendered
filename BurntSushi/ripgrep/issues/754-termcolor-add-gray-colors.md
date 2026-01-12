```yaml
number: 754
title: "[termcolor] Add gray colors"
type: issue
state: closed
author: KodrAus
labels: []
assignees: []
created_at: 2018-01-18T05:35:53Z
updated_at: 2018-01-19T04:45:16Z
url: https://github.com/BurntSushi/ripgrep/issues/754
synced_at: 2026-01-12T16:13:22Z
```

# [termcolor] Add gray colors

---

_@KodrAus_

Original issue: https://github.com/sebasmagri/env_logger/issues/59

It would be nice to have a gray variant in the `Color` enum, if it's possible. I think on Windows you can bit-twiddle yourself a gray and dark gray.

cc: @quadrupleslap

---

_Renamed from "Add gray color[s]" to "[termcolor] Add gray colors" by @KodrAus on 2018-01-18 05:36_

---

_Comment by @quadrupleslap on 2018-01-18 14:54_

Light-gray is a non-intense white, and dark-gray is an intense black, on windows, and it also looks like that's true with dracula on linux.

---

_Comment by @okdana on 2018-01-18 16:58_

I don't know about Windows, but on UNIX systems, the first 16 ANSI colours are almost always user-configurable, and it's common to have 'white' (`7`) and 'bright black' (`8`) set to shades of grey. This is the way the default palettes of most terminals are set. Sometimes 'black' (`0`) and 'bright white' (`15`) are grey too, though this is less common.

Above the first 16, there are 240 more numbered colours, with `232` to `255` dedicated to various shades of grey. These colours are configurable in some terminals (especially those that take settings from X11), but it's very uncommon for users to mess with them, and in some terminals (like Apple's, AFAIK) they just can't. As a result, it's risky to use them in console applications, because they may clash with the end user's colour theme, or they may be unreadable entirely. Many of the grey colours are illegible on light-background themes, for example.

Then there's also 'true' RGB colour, where you can specify an exact value like in CSS or whatever. These are never configurable, and are thus even more risky for anyone but the end user to mess with.

(On top of that, some terminals don't support 256/RGB colours at all, though all of the major ones do, except the Linux console i think.)

So i don't think it's a good idea to use colours outside of the standard 16 as hard-coded or even default values in something that's intended for other people to use (though there's no harm in giving them the option to do it themselves, obv).

Anyway... `termcolor` would gain support for 256 and RGB ANSI colours if/when PR #452 is merged. It's just been waiting on a more thorough review i think.

---

_Comment by @KodrAus on 2018-01-19 04:45_

As @quadrupleslap has pointed out, we can get shades of gray using the `set_intense` methods that already exist so I'll go ahead and close this.

---

_Closed by @KodrAus on 2018-01-19 04:45_

---
