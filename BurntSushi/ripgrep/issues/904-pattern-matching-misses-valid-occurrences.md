```yaml
number: 904
title: Pattern matching misses valid occurrences 
type: issue
state: closed
author: wltr
labels: []
assignees: []
created_at: 2018-05-02T01:12:36Z
updated_at: 2018-05-02T09:14:06Z
url: https://github.com/BurntSushi/ripgrep/issues/904
synced_at: 2026-01-12T16:13:22Z
```

# Pattern matching misses valid occurrences 

---

_@wltr_

#### What version of ripgrep are you using?

```
ripgrep 0.7.1
-AVX -SIMD
```

#### How did you install ripgrep?

`sudo dnf install ripgrep`

#### What operating system are you using ripgrep on?

Fedora 27 (4.14.13-300.fc27.x86_64)

#### Describe your question, feature request, or bug.

When searching through files, I noticed that ripgrep is missing valid occurrences with certain search patterns. 

#### If this is a bug, what are the steps to reproduce the behavior?

Copy this to a file:
```
hw/building_blocks/xilinx/gt/gt_v1_7_2_vivado2017_4/gtwizard_ultrascale_v1_7_gthe3_channel.v

hw/ethernet/tamba_mac/tamba_mac_top.sv

hw/ethernet/mac_10g_base_r/tech_specific/xilinx_kintex_ultrascale/mac10g_x8_phy_top_xcku.sv
hw/boards/ku3/clk/ip/mac_pll_dummy_xcku.sv
```

**Success:**
```
$ rg tamba_mac_t
test.txt
3:hw/ethernet/tamba_mac/tamba_mac_top.sv 
```

**Failure:**
```
$ rg tamba_mac_top
```

#### If this is a bug, what is the actual behavior?

Pattern doesn't match even though it's clearly in the file.

#### If this is a bug, what is the expected behavior?

I would expect `rg tamba_mac_top` to find the line.


---

_Comment by @BurntSushi on 2018-05-02 08:14_

0.7.1 is not the latest version. Please try the latest version.

---

_Comment by @wltr on 2018-05-02 09:03_

It is the latest version available through `dnf` on Fedora 27.

---

_Comment by @BurntSushi on 2018-05-02 09:11_

There is nothing I can do about that. This is the trade off from using distro packaged software. If you want the latest version, then either build it yourself or download one of the binary releases from github.

---

_Closed by @wltr on 2018-05-02 09:14_

---
