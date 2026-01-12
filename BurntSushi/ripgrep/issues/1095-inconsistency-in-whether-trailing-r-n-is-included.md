```yaml
number: 1095
title: Inconsistency in whether trailing \r\n is included in the results json
type: issue
state: closed
author: roblourens
labels:
  - bug
assignees: []
created_at: 2018-10-28T19:53:02Z
updated_at: 2019-01-26T17:37:09Z
url: https://github.com/BurntSushi/ripgrep/issues/1095
synced_at: 2026-01-12T16:13:22Z
```

# Inconsistency in whether trailing \r\n is included in the results json

---

_@roblourens_

0.10.0, Mac and Win

The `--crlf` flag has an unexpected side effect, that it changes whether the trailing `\r\n` is included in the json output. Very minor issue. I'm not sure which way is right.

Searching a `\r\n` document. When `--crlf` is not given, the trailing `\r\n` in "text" is included:
```
echo 'test\u0d\u0a' | rg --json test

[...]
{"type":"match","data":{"path":{"text":"<stdin>"},"lines":{"text":"test\r\n"},"line_number":1,"absolute_offset":0,"submatches":[{"match":{"text":"test"},"start":0,"end":4}]}}
```

When `--crlf` is included, the trailing `\r\n` in "text" is not included:
```
echo 'test\u0d\u0a' | rg --json test --crlf

[...]
{"type":"match","data":{"path":{"text":"<stdin>"},"lines":{"text":"test"},"line_number":1,"absolute_offset":0,"submatches":[{"match":{"text":"test"},"start":0,"end":4}]}}
```

But when searching a `\n` document, the trailing `\n` is always included:
```
echo 'test\u0a' | rg --json test   

[...]
{"type":"match","data":{"path":{"text":"<stdin>"},"lines":{"text":"test\n"},"line_number":1,"absolute_offset":0,"submatches":[{"match":{"text":"test"},"start":0,"end":4}]}}
```

---

_Comment by @BurntSushi on 2018-10-29 11:08_

Yeah, I suspect this is a bug. I can definitely reproduce it. Interestingly, if you don't use `--json`, then you do get the `CRLF` line ending. This is probably due to the hack imposed by the `--crlf` flag. I'm not quite sure yet what the best way to fix it is.

---

_Label `bug` added by @BurntSushi on 2018-10-29 11:08_

---

_Comment by @roblourens on 2018-10-31 18:09_

We actually found a worse instance of this bug. I assume it's the same issue, I can open a new one if you prefer.

Search for \n in a CRLF document

```
echo 'a\u0d\u0a' | rg --json '\n' --multiline --crlf  
```

We get two `match` messages, one with an empty "submatches" array which should be invalid:

```
{"type":"begin","data":{"path":{"text":"<stdin>"}}}
{"type":"match","data":{"path":{"text":"<stdin>"},"lines":{"text":"a"},"line_number":1,"absolute_offset":0,"submatches":[]}}
{"type":"match","data":{"path":{"text":"<stdin>"},"lines":{"text":"\n"},"line_number":2,"absolute_offset":3,"submatches":[{"match":{"text":"\n"},"start":0,"end":1}]}}
{"type":"end","data":{"path":{"text":"<stdin>"},"binary_offset":null,"stats":{"elapsed":{"secs":0,"nanos":89223,"human":"0.000089s"},"searches":1,"searches_with_match":1,"bytes_searched":4,"bytes_printed":344,"matched_lines":2,"matches":1}}}
{"data":{"elapsed_total":{"human":"0.002074s","nanos":2074308,"secs":0},"stats":{"bytes_printed":344,"bytes_searched":4,"elapsed":{"human":"0.000089s","nanos":89223,"secs":0},"matched_lines":2,"matches":1,"searches":1,"searches_with_match":1}},"type":"summary"}
```

---

_Closed by @BurntSushi on 2019-01-26 17:35_

---

_Comment by @BurntSushi on 2019-01-26 17:37_

Thanks for this report. This was a pretty gnarly bug, and it turned out that I tried to hack around it pretty poorly. I think the fix for this should make ripgrep's CRLF hack a bit more robust in general though, so I hope things work better now!

---
