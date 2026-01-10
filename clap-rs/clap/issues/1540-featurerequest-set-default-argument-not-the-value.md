---
number: 1540
title: "[FeatureRequest] Set default Argument (NOT THE VALUE)"
type: issue
state: closed
author: Johanneslueke
labels: []
assignees: []
created_at: 2019-09-01T16:04:38Z
updated_at: 2019-09-03T13:28:39Z
url: https://github.com/clap-rs/clap/issues/1540
synced_at: 2026-01-10T01:26:56Z
---

# [FeatureRequest] Set default Argument (NOT THE VALUE)

---

_Issue opened by @Johanneslueke on 2019-09-01 16:04_

<!--
Please use the following template to assist with creating an issue and to ensure a speedy resolution. If an area is not applicable, feel free to delete the area or mark with `N/A`
-->

### Rust Version

* rustc 1.37.0 (eae3437df 2019-08-13)

### Affected Version of clap

* 2.31.2

### Feature Request Summary

I am missing the ability to set a given argument as default so that I do not have to specifically type the argument but instead just handover a value. 



### Expected Behavior Summary

Accepts just the value as input if an argument is set as default
For Instance:

 `echo -e someRandomText` becomes `echo someRandomText`




---

_Comment by @Johanneslueke on 2019-09-03 13:28_

never mind ... i just realized i could create an argument and do not give it any short or long name and internally expect  a certain argument is set as default, even if it is not specified as arg from the user. 

sometimes some things are just to obvious

---

_Closed by @Johanneslueke on 2019-09-03 13:28_

---
