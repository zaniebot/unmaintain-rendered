```yaml
number: 806
title: "Add termcolor docs explaining what intense does (closes #797)"
type: pull_request
state: merged
author: balajisivaraman
labels: []
assignees: []
merged: true
base: master
head: fix_797
created_at: 2018-02-14T17:52:23Z
updated_at: 2018-02-20T15:43:02Z
url: https://github.com/BurntSushi/ripgrep/pull/806
synced_at: 2026-01-12T18:23:13Z
```

# Add termcolor docs explaining what intense does (closes #797)

---

_@balajisivaraman_

I went through the code (and searched the WWW) to understand what the `intense` parameter does in `termcolor`. Hopefully I've captured the intent behind them to the best of my understanding.

---

_@BurntSushi reviewed on 2018-02-14 17:57_

---

_Review comment by @BurntSushi on `termcolor/src/lib.rs`:1257 on 2018-02-14 17:57_

I'm not an ANSI expert, but [Wikipedia (see 8-bit section)](https://en.wikipedia.org/wiki/ANSI_escape_code) seems to just classify these as "intense" colors. I don't know where the canonical documentation is for this though.

I think your Windows description looks good though!

---

_@balajisivaraman reviewed on 2018-02-14 18:08_

---

_Review comment by @balajisivaraman on `termcolor/src/lib.rs`:1257 on 2018-02-14 18:08_

Hmmm... I looked at the same page and got confused by the fact that the escape sequence was for 256-colors. Sorry about that. It does say that he sequence of numbers we're using (9-15) are for high intensity colors.

I also scoured the web and couldn't find the canonical documentation for it. 😞 

I'll just change it to say: "On Unix-like systems, this will output the ANSI escape sequence that will print a high-intensity version of the color specified." (We could use the word gaudy, which is what those colors look like to me but we probably don't want to.)

I understand that's a very vague description, but I cannot come up something better unfortunately.

---

_@BurntSushi reviewed on 2018-02-14 18:11_

---

_Review comment by @BurntSushi on `termcolor/src/lib.rs`:1257 on 2018-02-14 18:11_

Haha! Yes, that wording sounds good. :) If another enterprising individual can make these descriptions more precise in the future, then that's great, but these will do for now!

---

_@okdana reviewed on 2018-02-14 18:14_

---

_Review comment by @okdana on `termcolor/src/lib.rs`:1257 on 2018-02-14 18:14_

The macro is misleadingly named. The 'intense' colours are the standard 8 with the bold/intense parameter set (e.g., `\e[1;35m` for intense magenta), or in the 8-bit set (indexed from `0`) they are `8` through `15`. `write_intense!()` simply uses the more comprehensive foreground/background colour sequence which supports the standard 8, the intense 8, and then 240 more pre-defined colours. If you did `write_intense!("5")` you'd get non-intense magenta, and if you did `write_intense!("251")` you'd get a pre-defined medium grey. ~~I think you should even be able to do `write_intense!("255;0;255")` for 24-bit magenta, since it uses the same leading sequence.~~

Actually, the last part is wrong. It puts a `5` in so you can't do 24-bit. Sorry

---

_@BurntSushi reviewed on 2018-02-14 18:22_

---

_Review comment by @BurntSushi on `termcolor/src/lib.rs`:1257 on 2018-02-14 18:22_

Yeah basically my understanding is that on Unix "intense" is just another variant of a predefined color, where as on Windows it's an actual setting that is toggled on the console.

---

_Merged by @BurntSushi on 2018-02-20 12:11_

---

_Closed by @BurntSushi on 2018-02-20 12:11_

---

_Branch deleted on 2018-02-20 15:43_

---
