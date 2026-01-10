---
number: 2270
title: "clap_derive: Include license files into the crates.io tarball"
type: issue
state: closed
author: igor-raits
labels:
  - E-easy
  - ":money_with_wings: $5"
assignees: []
created_at: 2020-12-25T16:07:20Z
updated_at: 2021-10-25T18:22:34Z
url: https://github.com/clap-rs/clap/issues/2270
synced_at: 2026-01-10T01:27:14Z
---

# clap_derive: Include license files into the crates.io tarball

---

_Issue opened by @igor-raits on 2020-12-25 16:07_

### Make sure you completed the following tasks

- [ ] Searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [ ] Searched the closed issues

### Describe your use case

The Apache-2.0 license requires so that full text of a license will be present along with sources. However since clap stuff gets published on crates.io separately, tarballs of clap_derive do not include LICENSE files.

For example, see: https://crates.io/api/v1/crates/clap_derive/3.0.0-beta.2/download#/clap_derive-3.0.0-beta.2.crate

### Describe the solution you'd like

License files included in all clap sub-crates.

### Alternatives, if applicable

Not I am aware of.

---

_Label `T: new feature` added by @igor-raits on 2020-12-25 16:07_

---

_Added to milestone `3.0` by @pksunkara on 2020-12-25 16:08_

---

_Comment by @pksunkara on 2020-12-28 10:10_

This would need some work in https://github.com/pksunkara/cargo-workspaces since I use it to publish these crates.

---

_Comment by @igor-raits on 2020-12-28 10:12_

@pksunkara I believe it would be enough to just copy mentioned files into the subfolders and during next publish they will be uploaded on crates.io tarball.

---

_Comment by @pksunkara on 2020-12-28 10:17_

I don't think that's the correct approach. You can see how Lerna does the same for javascript workspaces.

---

_Referenced in [pksunkara/cargo-workspaces#36](../../pksunkara/cargo-workspaces/issues/36.md) on 2021-01-07 15:02_

---

_Label `:money_with_wings: $5` added by @pksunkara on 2021-03-09 16:58_

---

_Label `D: easy` added by @pksunkara on 2021-03-09 16:58_

---

_Label `P1: urgent` added by @pksunkara on 2021-03-09 16:58_

---

_Label `Z: good first issue` added by @pksunkara on 2021-03-09 16:58_

---

_Referenced in [clap-rs/clap#2810](../../clap-rs/clap/pulls/2810.md) on 2021-10-04 19:47_

---

_Closed by @epage on 2021-10-06 13:55_

---

_Comment by @decathorpe on 2021-10-25 18:18_

Uhm, was this supposed to be fixed now? Because crates published to crates.io still don't contain license files as of 3.0.0-beta.5.

---

_Comment by @pksunkara on 2021-10-25 18:22_

Fixed by #2919, so next release

---
