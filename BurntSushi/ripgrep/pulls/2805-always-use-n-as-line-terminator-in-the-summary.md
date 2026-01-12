```yaml
number: 2805
title: "Always use `\\n` as line terminator in the summary printer"
type: pull_request
state: closed
author: ltrzesniewski
labels: []
assignees: []
base: master
head: line-term-summary
created_at: 2024-05-12T15:14:58Z
updated_at: 2025-07-12T15:39:07Z
url: https://github.com/BurntSushi/ripgrep/pull/2805
synced_at: 2026-01-12T18:23:14Z
```

# Always use `\n` as line terminator in the summary printer

---

_@ltrzesniewski_

This contributes to #2795.

I took a closer look at the issue, and I noticed the following:

```bash
$ echo hello | rg hello --no-config --count --null-data | xxd
00000000: 3100                                     1.
```

i.e. the summary printer outputs null bytes instead of newlines when `--null-data` is used. I don't think that's intentional, and it's a noticeable behavior, so this PR changes this to always output `\n`.

I wasn't sure if I should use CRLF or LF on Windows, but since LF is used by default unless `--crlf` is enabled I used LF.

---

I also made changes to the standard printer (**not** included here), which I can submit in another PR if you think it's worth it. I just need to know if you'd rather output LF or CRLF on Windows in the standard printer. Using LF is a [simple change](https://github.com/ltrzesniewski/ripgrep/commit/cdfe0854ac21c22f8d86a383744bfec2a5aa4195#diff-9edc643fe15c729d9ad9528b23556005eb27db181e25f5f260962e5ed468b9dd), while using CRLF requires changing lots of unit tests so they pass on Windows.


---

_Comment by @BurntSushi on 2025-07-12 15:39_

See https://github.com/BurntSushi/ripgrep/issues/2795#issuecomment-3065702275

---

_Closed by @BurntSushi on 2025-07-12 15:39_

---
