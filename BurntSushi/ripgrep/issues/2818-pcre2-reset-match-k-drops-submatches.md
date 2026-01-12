```yaml
number: 2818
title: pcre2 reset match \K drops submatches
type: issue
state: closed
author: PiotrSokol
labels:
  - wontfix
assignees: []
created_at: 2024-05-25T13:43:31Z
updated_at: 2024-05-25T13:53:04Z
url: https://github.com/BurntSushi/ripgrep/issues/2818
synced_at: 2026-01-12T16:13:25Z
```

# pcre2 reset match \K drops submatches

---

_@PiotrSokol_

### Please tick this box to confirm you have reviewed the above.

- [X] I have a different issue.

### What version of ripgrep are you using?

ripgrep 14.1.0

features:-simd-accel,+pcre2
simd(compile):+SSE2,+SSSE3,-AVX2
simd(runtime):+SSE2,+SSSE3,+AVX2

PCRE2 10.42 is available (JIT is available)

### How did you install ripgrep?

Homebrew

### What operating system are you using ripgrep on?

macos 13.4.1

### Describe your bug.

First off, thank you for you wonderful work-- I can't even explain how insanely helpful `rg` has been!

I'm using `--pcre2` and `\K` to deal with a (variable-length) lookbehind. rg returns the correct match, but fails to return a submatch. 
I compared it to `(?<=...)` which works fine for fixed-length patterns, see below.
And the problem persists regardless of using `--json`, I'm only using it here because it's easier to demonstrate. 

### What are the steps to reproduce the behavior?

```
echo -e 'foo\nbar' > test.txt
rg --pcre2 --multiline --json 'foo\n\Kbar' test.txt
```

### What is the actual behavior?

```
{"type":"begin","data":{"path":{"text":"test.txt"}}}
{"type":"match","data":{"path":{"text":"test.txt"},"lines":{"text":"bar\n"},"line_number":2,"absolute_offset":4,"submatches":[]}}
{"type":"end","data":{"path":{"text":"test.txt"},"binary_offset":null,"stats":{"elapsed":{"secs":0,"nanos":44682,"human":"0.000045s"},"searches":1,"searches_with_match":1,"bytes_searched":8,"bytes_printed":183,"matched_lines":1,"matches":0}}}
{"data":{"elapsed_total":{"human":"0.001371s","nanos":1370923,"secs":0},"stats":{"bytes_printed":183,"bytes_searched":8,"elapsed":{"human":"0.000045s","nanos":44682,"secs":0},"matched_lines":1,"matches":0,"searches":1,"searches_with_match":1}},"type":"summary"}
```

### What is the expected behavior?

running 
```
rg --pcre2 --multiline --json '(?<=foo\n)bar' test.txt
```
returns
```
{"type":"begin","data":{"path":{"text":"test.txt"}}}
{"type":"match","data":{"path":{"text":"test.txt"},"lines":{"text":"bar\n"},"line_number":2,"absolute_offset":4,"submatches":[{"match":{"text":"bar"},"start":0,"end":3}]}}
{"type":"end","data":{"path":{"text":"test.txt"},"binary_offset":null,"stats":{"elapsed":{"secs":0,"nanos":46237,"human":"0.000046s"},"searches":1,"searches_with_match":1,"bytes_searched":8,"bytes_printed":225,"matched_lines":1,"matches":1}}}
{"data":{"elapsed_total":{"human":"0.001357s","nanos":1356890,"secs":0},"stats":{"bytes_printed":225,"bytes_searched":8,"elapsed":{"human":"0.000046s","nanos":46237,"secs":0},"matched_lines":1,"matches":1,"searches":1,"searches_with_match":1}},"type":"summary"}
```

---

_Comment by @BurntSushi on 2024-05-25 13:52_

Thanks for the concise reproduction.

This is probably a `wontfix`. ripgrep doesn't change how it uses PCRE2 APIs based on the presence or absence of `\K`. So if `\K` doesn't work using standard APIs, then I think that's just how the cookie crumbles.

If someone wants to dive in and see if this is actually fixable without needing to know about `\K` in a generic context, then I'd probably be willing to accept a patch if it's small and non-invasive.

---

_Closed by @BurntSushi on 2024-05-25 13:52_

---

_Label `wontfix` added by @BurntSushi on 2024-05-25 13:53_

---
