```yaml
number: 625
title: Color --replace text using the match type
type: pull_request
state: merged
author: jamessan
labels: []
assignees: []
merged: true
base: master
head: color-replacment
created_at: 2017-10-07T03:35:03Z
updated_at: 2017-10-08T12:01:12Z
url: https://github.com/BurntSushi/ripgrep/pull/625
synced_at: 2026-01-12T18:23:13Z
```

# Color --replace text using the match type

---

_@jamessan_

Closes #521

---

_Review comment by @BurntSushi on `src/printer.rs`:41 on 2017-10-07 12:02_

Could you put each parameter on its own line please?

---

_Review comment by @BurntSushi on `src/printer.rs`:55 on 2017-10-07 12:02_

I feel like it would be clearer if this were written as `Offset::new(start, end)`.

---

_Review comment by @BurntSushi on `src/printer.rs`:327 on 2017-10-07 12:07_

I think `&line` should be sufficient here? I guess you might need `&*line` because `line` is a cow I think.

---

_@BurntSushi requested changes on 2017-10-07 12:08_

This looks pretty good to me! Thanks for doing this. :-)

---

_Review comment by @jamessan on `src/printer.rs`:55 on 2017-10-07 14:03_

Indeed. :) I originally didn't have the empty replacement check and forgot to change this to `end` when I added it.  Thanks.

---

_Review comment by @jamessan on `src/printer.rs`:327 on 2017-10-07 14:05_

Yeah, `&*line` is needed.

---

_@jamessan reviewed on 2017-10-07 14:10_

Updated

---

_Comment by @jamessan on 2017-10-07 14:11_

> This looks pretty good to me!

Thanks!  My first contribution to a rust project. :)

---

_@BurntSushi approved on 2017-10-07 14:13_

w00t. Thanks for the fixups!

---

_Merged by @BurntSushi on 2017-10-08 12:01_

---

_Closed by @BurntSushi on 2017-10-08 12:01_

---
