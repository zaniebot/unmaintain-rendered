```yaml
number: 1412
title: Multiline lookbehind, empty submatches array
type: issue
state: closed
author: roblourens
labels:
  - bug
  - rollup
assignees: []
created_at: 2019-10-28T04:50:25Z
updated_at: 2021-06-01T01:51:27Z
url: https://github.com/BurntSushi/ripgrep/issues/1412
synced_at: 2026-01-12T16:13:23Z
```

# Multiline lookbehind, empty submatches array

---

_@roblourens_

If I search with a lookbehind pattern that matches a newline, and use the `--json` flag, I get the match info but it has an empty `submatches` array. Example:

```
$ echo -e 'foo\nbar' > test.txt
$ rg --auto-hybrid-regex --multiline --json '(?<=foo\n)bar' test.txt
{"type":"begin","data":{"path":{"text":"test.txt"}}}
{"type":"match","data":{"path":{"text":"test.txt"},"lines":{"text":"bar\n"},"line_number":2,"absolute_offset":4,"submatches":[]}}
{"type":"end","data":{"path":{"text":"test.txt"},"binary_offset":null,"stats":{"elapsed":{"secs":0,"nanos":40700,"human":"0.000041s"},"searches":1,"searches_with_match":1,"bytes_searched":8,"bytes_printed":183,"matched_lines":1,"matches":0}}}
{"data":{"elapsed_total":{"human":"0.002328s","nanos":2328499,"secs":0},"stats":{"bytes_printed":183,"bytes_searched":8,"elapsed":{"human":"0.000041s","nanos":40700,"secs":0},"matched_lines":1,"matches":0,"searches":1,"searches_with_match":1}},"type":"summary"}
```

Compare to what I get when matching with lookbehind on one line:

```
$ rg --auto-hybrid-regex --multiline --json '(?<=b)ar' test.txt 
...
{"type":"match","data":{"path":{"text":"test.txt"},"lines":{"text":"bar\n"},"line_number":2,"absolute_offset":4,"submatches":[{"match":{"text":"ar"},"start":1,"end":3}]}}
```

Or just matching that line 

```
$ rg --auto-hybrid-regex --multiline --json 'bar' test.txt
...
{"type":"match","data":{"path":{"text":"test.txt"},"lines":{"text":"bar\n"},"line_number":2,"absolute_offset":4,"submatches":[{"match":{"text":"bar"},"start":0,"end":3}]}}
```

Same with lookahead that matches a newline.

---

_Label `bug` added by @BurntSushi on 2020-03-15 16:36_

---

_Comment by @alessandroasm on 2020-10-05 18:47_

Hi, I looked into what is happening in this case.

https://github.com/BurntSushi/ripgrep/blob/def993bad1a9275cdc249f04048e5b2065b79f05/crates/printer/src/json.rs#L604-L614

The issue is that `grep_searcher::SinkMatch` doesn't store captured groups. When `JSONSink` needs details about capture groups, it reruns the matcher against the bytes in `SinkMatch`, but those bytes only contain the current line, which in turn results in a non-match scenario.

I think the way to go here will be including submatch data in `grep_searcher::SinkMatch`. That way we won't need to rerun the matcher to get this information.

@BurntSushi I can implement that if you agree to this changes

---

_Label `rollup` added by @BurntSushi on 2021-05-31 22:33_

---

_Closed by @BurntSushi on 2021-06-01 01:51_

---
