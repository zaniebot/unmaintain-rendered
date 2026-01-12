```yaml
number: 1638
title: BOMs are not stripped consistently in --json output
type: issue
state: closed
author: acheronfail
labels:
  - bug
  - rollup
assignees: []
created_at: 2020-07-08T11:54:22Z
updated_at: 2021-06-01T01:51:24Z
url: https://github.com/BurntSushi/ripgrep/issues/1638
synced_at: 2026-01-12T16:13:24Z
```

# BOMs are not stripped consistently in --json output

---

_@acheronfail_

#### What version of ripgrep are you using?

```
ripgrep 12.0.1
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
```

#### How did you install ripgrep?

```
cargo install ripgrep
```

#### What operating system are you using ripgrep on?

```
Arch Linux 5.7.6
```

#### Describe your bug.

When using ripgrep's `--json` flag on a file encoded as "UTF 8 with BOM" the BOM is not accounted for (as opposed to other encodings, such as UTF 16).

#### What are the steps to reproduce the behavior?

UTF8

```bash
# Create a UTF8 encoded file (without BOM)
printf "\x66\x6f\x6f" > utf8
# Run ripgrep
rg foo ./utf8 --json
```

UTF8 BOM

```bash
# Create a UTF8 encoded file (with BOM)
printf "\xef\xbb\xbf\x66\x6f\x6f" > utf8bom
# Run ripgrep
rg foo ./utf8bom --json
```

UTF16

```bash
# Create a UTF16 encoded file (has BOM)
printf "\xff\xfe\x66\x00\x6f\x00\x6f\x00" > utf16
# Run ripgrep
rg foo ./utf16 --json
```

#### What is the actual behavior?

Here is the JSON output for the above three code blocks.

UTF8

```
{"type":"begin","data":{"path":{"text":"./utf8"}}}
{"type":"match","data":{"path":{"text":"./utf8"},"lines":{"text":"foo"},"line_number":1,"absolute_offset":0,"submatches":[{"match":{"text":"foo"},"start":0,"end":3}]}}
{"type":"end","data":{"path":{"text":"./utf8"},"binary_offset":null,"stats":{"elapsed":{"secs":0,"nanos":42600,"human":"0.000043s"},"searches":1,"searches_with_match":1,"bytes_searched":3,"bytes_printed":219,"matched_lines":1,"matches":1}}}
{"data":{"elapsed_total":{"human":"0.007219s","nanos":7218625,"secs":0},"stats":{"bytes_printed":219,"bytes_searched":3,"elapsed":{"human":"0.000043s","nanos":42600,"secs":0},"matched_lines":1,"matches":1,"searches":1,"searches_with_match":1}},"type":"summary"}
```

UTF8 BOM

```
{"type":"begin","data":{"path":{"text":"./utf8bom"}}}
{"type":"match","data":{"path":{"text":"./utf8bom"},"lines":{"text":"﻿foo"},"line_number":1,"absolute_offset":0,"submatches":[{"match":{"text":"foo"},"start":3,"end":6}]}}
{"type":"end","data":{"path":{"text":"./utf8bom"},"binary_offset":null,"stats":{"elapsed":{"secs":0,"nanos":47766,"human":"0.000048s"},"searches":1,"searches_with_match":1,"bytes_searched":6,"bytes_printed":228,"matched_lines":1,"matches":1}}}
{"data":{"elapsed_total":{"human":"0.007849s","nanos":7849144,"secs":0},"stats":{"bytes_printed":228,"bytes_searched":6,"elapsed":{"human":"0.000048s","nanos":47766,"secs":0},"matched_lines":1,"matches":1,"searches":1,"searches_with_match":1}},"type":"summary"}
```

UTF16

```
{"type":"begin","data":{"path":{"text":"./utf16"}}}
{"type":"match","data":{"path":{"text":"./utf16"},"lines":{"text":"foo"},"line_number":1,"absolute_offset":0,"submatches":[{"match":{"text":"foo"},"start":0,"end":3}]}}
{"type":"end","data":{"path":{"text":"./utf16"},"binary_offset":null,"stats":{"elapsed":{"secs":0,"nanos":43947,"human":"0.000044s"},"searches":1,"searches_with_match":1,"bytes_searched":3,"bytes_printed":221,"matched_lines":1,"matches":1}}}
{"data":{"elapsed_total":{"human":"0.006400s","nanos":6399559,"secs":0},"stats":{"bytes_printed":221,"bytes_searched":3,"elapsed":{"human":"0.000044s","nanos":43947,"secs":0},"matched_lines":1,"matches":1,"searches":1,"searches_with_match":1}},"type":"summary"}
```

#### What is the expected behavior?

I personally expected that ripgrep would strip the UTF8 BOM from the JSON report since that's what it does for UTF16 encodings. However, I'm not sure if this should be the case or not, considering that a UTF8 BOM is an optional file header.


---

_Label `bug` added by @BurntSushi on 2021-05-30 17:02_

---

_Label `rollup` added by @BurntSushi on 2021-05-30 17:02_

---

_Closed by @BurntSushi on 2021-06-01 01:51_

---
