```yaml
number: 1286
title: "Values doesn't implement Debug"
type: issue
state: closed
author: drrlvn
labels:
  - C-enhancement
assignees: []
created_at: 2018-06-03T14:02:07Z
updated_at: 2018-08-02T03:30:24Z
url: https://github.com/clap-rs/clap/issues/1286
synced_at: 2026-01-12T16:14:10Z
```

# Values doesn't implement Debug

---

_@drrlvn_

<!--
Please use the following template to assist with creating an issue and to ensure a speedy resolution. If an area is not applicable, feel free to delete the area or mark with `N/A`
-->

### Rust Version

* rustc 1.26.1 (827013a31 2018-05-25)

### Affected Version of clap

* clap 2.31.2

### Expected Behavior Summary
Debug printing of `clap::Values` should be allowed.


### Actual Behavior Summary
Compilation error:
```
error[E0277]: `clap::Values<'_>` doesn't implement `std::fmt::Debug`
```


---

_Comment by @kbknapp on 2018-06-04 21:09_

Agreed. However adding it to the 2.x branch is a breaking change (however small), so it probably won't happen until 3.x since that is actively being worked on and the goal is to have it released in the coming months.

---

_Label `T: enhancement` added by @kbknapp on 2018-06-04 21:09_

---

_Label `E: breaking change` added by @kbknapp on 2018-06-04 21:09_

---

_Label `P2: need to have` added by @kbknapp on 2018-06-04 21:09_

---

_Label `D: easy` added by @kbknapp on 2018-06-04 21:09_

---

_Label `C: matches` added by @kbknapp on 2018-06-04 21:09_

---

_Label `W: 3.x` added by @kbknapp on 2018-06-04 21:09_

---

_Comment by @drrlvn on 2018-06-05 12:21_

Just for curiosity - why is deriving `Debug` a breaking change? Can it break existing code?

---

_Comment by @kbknapp on 2018-06-05 13:33_

Ah I think you're correct! I wasn't thinking about the orphan rules. I was thinking if someone impl'd `Debug` for `Values`, however I forgot that's not possible without a newtype.

I can move this into an easy 2.x issue :+1: 


---

_Label `W: 2.x` added by @kbknapp on 2018-06-05 13:33_

---

_Label `E: breaking change` removed by @kbknapp on 2018-06-05 13:33_

---

_Label `W: 3.x` removed by @kbknapp on 2018-06-05 13:33_

---

_Referenced in [clap-rs/clap#1314](../../clap-rs/clap/pulls/1314.md) on 2018-07-02 12:13_

---

_Closed by @kbknapp on 2018-07-09 21:54_

---
