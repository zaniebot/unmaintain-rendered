```yaml
number: 266
title: Style changed between releases
type: issue
state: closed
author: sophiajt
labels:
  - question
assignees: []
created_at: 2016-12-03T16:17:33Z
updated_at: 2017-01-07T01:10:10Z
url: https://github.com/BurntSushi/ripgrep/issues/266
synced_at: 2026-01-12T16:13:21Z
```

# Style changed between releases

---

_@sophiajt_

Originally, ripgrep had nice bold highlights for matches, which made it easy to read on white terminals:

<img width="599" alt="screen shot 2016-12-03 at 8 14 16 am" src="https://cloud.githubusercontent.com/assets/547158/20860601/a80c96ba-b930-11e6-969d-0f48897c525e.png">

The more recent version uses slightly different colors and no bold.  This is a bit harder to read on the white background:

<img width="302" alt="screen shot 2016-12-03 at 8 13 14 am" src="https://cloud.githubusercontent.com/assets/547158/20860606/e1a06136-b930-11e6-95cb-93bdeef3110d.png">

Would love a way to switch back to the earlier colors

---

_Comment by @BurntSushi on 2016-12-03 18:06_

This certainly wasn't intended. The [current default colors](https://github.com/BurntSushi/ripgrep/blob/master/src/args.rs#L623-L629) should all be bold.

I wonder what happens if you try a bit of experimenting. In ripgrep 0.3, you can now customize colors/styles to a limited extent. e.g., What happens if you try this:

```
rg --colors line:style:nobold --colors match:style:nobold --colors path:style:nobold lookup_method
```

Finally, what terminal emulator are you using?

---

_Label `question` added by @BurntSushi on 2016-12-03 18:06_

---

_Comment by @BurntSushi on 2016-12-03 18:07_

I wonder if this is because I've made "bold + color" imply "high intensity color," and as a result, never actually emit the bold ANSI escape sequence.

---

_Comment by @sophiajt on 2016-12-04 18:39_

With your line I get:

<img width="605" alt="screen shot 2016-12-04 at 10 38 33 am" src="https://cloud.githubusercontent.com/assets/547158/20868332/dbadbfde-ba0d-11e6-8e7d-88003fff343a.png">

I'm just using the default macOS terminal with Basic theme.

---

_Comment by @BurntSushi on 2016-12-04 19:23_

Okay. I will try to hook up my mac to a monitor and test this.

---

_Comment by @ivan on 2016-12-12 00:29_

With roxterm (which uses libvte), I also get different shades of red with `match:style:nobold` and `match:style:bold`, but neither is bold now.

---

_Closed by @BurntSushi on 2017-01-07 01:10_

---
