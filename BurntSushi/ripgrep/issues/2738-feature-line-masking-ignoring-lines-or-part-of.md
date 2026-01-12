```yaml
number: 2738
title: "[feature] Line masking (ignoring lines or part of lines in matching but displayed in output)"
type: issue
state: closed
author: quidnu
labels:
  - question
assignees: []
created_at: 2024-02-18T20:41:58Z
updated_at: 2024-02-19T15:07:41Z
url: https://github.com/BurntSushi/ripgrep/issues/2738
synced_at: 2026-01-12T16:13:24Z
```

# [feature] Line masking (ignoring lines or part of lines in matching but displayed in output)

---

_@quidnu_

#### Describe your feature request

I want to search SRT-like subtitle/transcript files using --multiline that have metadata interleaved in them which I would like to ignore for the purposes of string matching but which I want to be returned in the output context.

To elaborate: The files look something like this:
```
3
00:03:33,630 --> 00:03:35,000
On the red stage,

4
00:03:35,090 --> 00:03:37,840
foo removed the bar,
baz green and red.
```

and I would like to be able to match `red stage, foo` even though there is intervening metadata.

I would also be able to do something similar for file formats where meta data is appended to the end of each line

```
red stage, -- 00:30:35
foo -- 00:31:22
```

In both cases I want to output the matching lines as they appear in the input (and possibly context using -C), including metadata (I'm ultimately looking for the timestamp) so simply pre-processing the input and stripping out the metadata won't work.



---

_Comment by @BurntSushi on 2024-02-18 22:43_

Shell pipelines are your best bet. e.g., `rg -U 'red stage,\p{any}*foo' | rg 'red|foo'`.

Otherwise, your task seems pretty specialized. If you need to do one or two ad hoc queries, then ripgrep is probably a good fit. But if you need something more systematic and flexible, I'd probably build a bespoke tool for it personally.

---

_Comment by @quidnu on 2024-02-19 15:04_

Actually I misunderstood what multiline does, I want to match phrases across line breaks something like "red\s\stage\sfoo" but that's easy to do and okay (if you put aside the interleaved metadata problem). I think your example doesn't work because it would match 'red stage, blue stage, foo'.

I think what I'll end up doing is pre-processing the file, removing the metadata, extracting the matching line numbers from rg and printing those lines in the original file using sed/awk. 

(I understand that it's probably too far afield of what rg is intended to do, but what I was originally thinking was that you would be able to, in the most general case, do something like `rg --extractor-fn "jq '.text'"` to be able to search only the text across lines (and still output the entire data structure) of something like `{'ts': 111, 'text': 'this text continues on'}` `{'ts':112, 'text': ' the next line'}`. Or for simpler cases that can be handled with regex: `rg --mask "-- \d\d:\d\d$`")

Anyway thanks and feel free to close.

---

_Comment by @BurntSushi on 2024-02-19 15:07_

Yeah ripgrep isn't arbitrarily flexible like that. With that said, it does have the `--pre` flag, which lets you run any program to transform the input in whatever way you want before ripgrep will search it. The docs give an example of how to use ripgrep to transparently search the plain text of PDF files.

---

_Closed by @BurntSushi on 2024-02-19 15:07_

---

_Label `question` added by @BurntSushi on 2024-02-19 15:07_

---
