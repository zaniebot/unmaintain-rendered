```yaml
number: 4899
title: "F504: Carriage return prevents expression extraction"
type: issue
state: closed
author: addisoncrump
labels:
  - bug
assignees: []
created_at: 2023-06-06T09:22:34Z
updated_at: 2023-09-03T03:41:25Z
url: https://github.com/astral-sh/ruff/issues/4899
synced_at: 2026-01-12T15:54:45Z
```

# F504: Carriage return prevents expression extraction

---

_@addisoncrump_

In Python, a standalone carriage return character (0xd or `\r`) is considered whitespace. One could easily argue that this is quite problematic, given that carriage returns can potentially be used to hide parts of source code. That said, it does trigger a failure in F504 (`\r` replaced literally):

```py
"" % {
  'test1': '',\r  'test2': '',
}
```

[And a downloadable version to test against.](https://github.com/charliermarsh/ruff/files/11662694/pathological.txt)

I would simply mark F504 as sometimes fixable, but this solution doesn't feel right. Instead, this feels indicative of incorrect handling of carriage returns as plain whitespace. At the same time, standalone carriage returns should probably be marked as invalid anyways.

I leave this to the discretion of the developers of Ruff :slightly_smiling_face: We should find a fix one way or another to avoid future issues with this being marked as a failure case in the fuzzer.

Discovered by #4822.

---

_Label `bug` added by @charliermarsh on 2023-06-06 14:27_

---

_Comment by @charliermarsh on 2023-08-16 04:14_

Perhaps a bug in LibCST, which is failing to parse it?

---

_Comment by @addisoncrump on 2023-08-17 19:25_

I'm not so clear on that. Is this because libCST is used to parse the expression for repair?

---

_Comment by @charliermarsh on 2023-08-17 19:27_

Yeah

---

_Comment by @charliermarsh on 2023-08-17 19:27_

The LibCST parse call throws an error, but it could be our fault, not theirs. We may need to wrap the expression in parentheses before parsing.

---

_Comment by @dhruvmanila on 2023-08-20 03:45_

> We may need to wrap the expression in parentheses before parsing.

I did a quick experiment and this doesn't help either, it still throws an error:

```rust
libcst_native::parse_expression(&format!("({})", &contents))
```

```
Failed to parse file: Internal error while parsing whitespace: Found newline at (2, 15) but it's not EOL
```

---

_Comment by @MichaReiser on 2023-08-21 08:11_

This is a bug in LibCST. It seems they don't support mixed newlines:

https://github.com/Instagram/LibCST/blob/c91655fbba7c27db05423673609f10203316b9c0/native/libcst/src/tokenizer/whitespace_parser.rs#L67-L88

Changing the `split` to split on all new lines seems trivial enough, but I worry that there's other code that simply doesn't work if you use `\r`

For example: 

https://github.com/Instagram/LibCST/blob/c91655fbba7c27db05423673609f10203316b9c0/native/libcst/src/tokenizer/whitespace_parser.rs#L90-L94


---

_Comment by @MichaReiser on 2023-08-30 11:38_

Upstream fix in LibCST https://github.com/Instagram/LibCST/pull/1007

---

_Comment by @MichaReiser on 2023-09-02 09:03_

The upstream fix is merged. All that we need to do now is to update libcst 🫣

---

_Closed by @dhruvmanila on 2023-09-03 03:41_

---
