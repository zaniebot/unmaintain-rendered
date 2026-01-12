```yaml
number: 2785
title: JSON indexing off
type: issue
state: closed
author: kochbj
labels: []
assignees: []
created_at: 2024-04-21T22:18:25Z
updated_at: 2024-04-22T01:22:55Z
url: https://github.com/BurntSushi/ripgrep/issues/2785
synced_at: 2026-01-12T16:13:24Z
```

# JSON indexing off

---

_@kochbj_

### Please tick this box to confirm you have reviewed the above.

- [X] I have a different issue.

### What version of ripgrep are you using?

ripgrep 14.0.3 (rev 3f2fe0afee)

features:-simd-accel,-pcre2
simd(compile):+SSE2,-SSSE3,-AVX2
simd(runtime):+SSE2,+SSSE3,+AVX2

PCRE2 is not available in this build of ripgrep.

### How did you install ripgrep?

Compiled myself.

### What operating system are you using ripgrep on?

Red Hat Enterprise Linux Server 7.9 (Maipo)


### Describe your bug.

Running rg with json output sometimes produces an indexing error for the ``start`` and ``end`` indices by 2-5 characters.

### What are the steps to reproduce the behavior?

rg --json "monoclonal antibody" [PMC1395485.txt](https://github.com/BurntSushi/ripgrep/files/15055142/PMC1395485.txt)

### What is the actual behavior?

{"type":"begin","data":{"path":{"text":"PMC1395485.txt"}}}
{"type":"match","data":{"path":{"text":"PMC1395485.txt"},"lines":{"text":"Reagents—The αIIbβ3 antagonist lotrafiban was supplied by GlaxoSmithKline (King of Prussia, PA). The anti-Rac (23A8) monoclonal antibody was purchased from Upstate Biotechnology (TCS Biologicals, Bucks, UK). Anti-Rac2 polyclonal antibody and anti-Rac3 polyclonal antibody were generously provided from Gary Bokoch (Scripps Institute, La Jolla, CA) and Ivan de Curtis (San Rafaele Scientific Institute, Milan, Italy), respectively. The cDNA for the GST-CRIB domain of PAK1 prepared as described previously (21) and the active form of Rac (L61Rac) were the kind gifts from Dr. Doreen Cantrell (Imperial Cancer Research Fund, London, UK). D-Phenyl-alanyl-1-prolyl-1 arginine chloromethyl ketone was purchased from Calbiochem. Fibrinogen depleted of plasminogen, VWF, and fibronectin was from Kordia Laboratory Supplies, Leiden, Netherlands. VWF was a generous gift from Michael C. Berndt (Monash University, Clayton, Australia). All other reagents were from Sigma or previously named sources (22, 23).\n"},"line_number":52,"absolute_offset":7544,"submatches":[{"match":{"text":"monoclonal antibody"},**"start":121,"end":140**}]}}
....
{"type":"end","data":{"path":{"text":"PMC1395485.txt"},"binary_offset":null,"stats":{"elapsed":{"secs":0,"nanos":525009,"human":"0.000525s"},"searches":1,"searches_with_match":1,"bytes_searched":72115,"bytes_printed":2828,"matched_lines":2,"matches":2}}}
{"data":{"elapsed_total":{"human":"0.002775s","nanos":2774501,"secs":0},"stats":{"bytes_printed":2828,"bytes_searched":72115,"elapsed":{"human":"0.000525s","nanos":525009,"secs":0},"matched_lines":2,"matches":2,"searches":1,"searches_with_match":1}},"type":"summary"}

### What is the expected behavior?

{"type":"match","data":{"path":{"text":"/kellogg/proj/dashun/PMC_open_access_subset/20231215/manuscript/txt/PMC001xxxxxx/PMC1395485.txt"},"lines":{"text":"Reagents—The αIIbβ3 antagonist lotrafiban was supplied by GlaxoSmithKline (King of Prussia, PA). The anti-Rac (23A8) monoclonal antibody was purchased from Upstate Biotechnology (TCS Biologicals, Bucks, UK). Anti-Rac2 polyclonal antibody and anti-Rac3 polyclonal antibody were generously provided from Gary Bokoch (Scripps Institute, La Jolla, CA) and Ivan de Curtis (San Rafaele Scientific Institute, Milan, Italy), respectively. The cDNA for the GST-CRIB domain of PAK1 prepared as described previously (21) and the active form of Rac (L61Rac) were the kind gifts from Dr. Doreen Cantrell (Imperial Cancer Research Fund, London, UK). D-Phenyl-alanyl-1-prolyl-1 arginine chloromethyl ketone was purchased from Calbiochem. Fibrinogen depleted of plasminogen, VWF, and fibronectin was from Kordia Laboratory Supplies, Leiden, Netherlands. VWF was a generous gift from Michael C. Berndt (Monash University, Clayton, Australia). All other reagents were from Sigma or previously named sources (22, 23).\n"},"line_number":52,"absolute_offset":7544,"submatches":[{"match":{"text":"monoclonal antibody"},**"start":117,"end":136**}]}}

---

_Comment by @kochbj on 2024-04-21 22:23_

Hi,

Thanks as always for such a great tool. I have noticed inconsistencies in the indexing of matches when using json mode. Sometimes the indexing is as expected, but sometimes the characters are a few characters off from the start of the line when the ``text`` field is loaded as a string in python. If this isn't a bug and there's a reason for the discrepancy, please let me know!

All best,
B 

---

_Comment by @ltrzesniewski on 2024-04-21 22:43_

The `start` and `end` fields represent *byte offsets* (see the [format doc](https://docs.rs/grep-printer/latest/grep_printer/struct.JSON.html)), and that particular line starts with "Reagents—The αIIbβ3", which contains 3 characters that are encoded on more than 1 byte: "—" takes 3 bytes, "α" and "β" take 2, which use 4 more bytes than if they were encoded on a single byte each, hence the difference you see.

---

_Comment by @BurntSushi on 2024-04-21 22:49_

The output you've shown is correct. Here is a verification:

```
$ rg --json "monoclonal antibody" PMC1395485.txt | rg '"type":"match"' | jq '.data.line_number, .data.submatches[].start, .data.submatches[].end'
52
121
140
152
611
630

$ head -n52 PMC1395485.txt | tail -n1 | dd ibs=1 skip=121 count=19 2> /dev/null
monoclonal antibody%

$ head -n152 PMC1395485.txt | tail -n1 | dd ibs=1 skip=611 count=19 2> /dev/null
monoclonal antibody%
```

Please read the JSON format docs closely: https://docs.rs/grep-printer/latest/grep_printer/struct.JSON.html

You'll note that the offsets are very specifically defined as _byte offsets_.

So when you use those offsets in some context, you need to make sure they are being interpreted as byte offsets. They may not be. *Hint*: Since you didn't provide any code and I thus cannot be sure, offsets for Python strings are _codepoint offsets_.

---

_Comment by @kochbj on 2024-04-22 01:22_

Wonderful. thanks guys. When in doubt, RTFM. Sorry for opening an issue!

---

_Closed by @kochbj on 2024-04-22 01:22_

---
