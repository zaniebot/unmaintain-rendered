```yaml
number: 1983
title: Unexpected --json output with --multiline
type: issue
state: open
author: roblourens
labels: []
assignees: []
created_at: 2021-09-02T21:29:03Z
updated_at: 2021-09-08T06:53:58Z
url: https://github.com/BurntSushi/ripgrep/issues/1983
synced_at: 2026-01-12T16:13:24Z
```

# Unexpected --json output with --multiline

---

_@roblourens_

#### What version of ripgrep are you using?

```
rg --version
ripgrep 13.0.0
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
```

#### What operating system are you using ripgrep on?

macOS

#### Describe your bug.

When doing a search with a certain regex and the `--json` and `--multiline` flags, I get a single `match` object with multiple lines, but in rg 12 I got one for each line.

#### What are the steps to reproduce the behavior?

Have this file

```
.a1
.a2
```

Do this search

```
$ rg '\Wa' --json --multiline
{"type":"begin","data":{"path":{"text":"main.txt"}}}
{"type":"match","data":{"path":{"text":"main.txt"},"lines":{"text":".a1\n.a2"},"line_number":1,"absolute_offset":0,"submatches":[{"match":{"text":".a"},"start":0,"end":2},{"match":{"text":".a"},"start":4,"end":6}]}}
{"type":"end","data":{"path":{"text":"main.txt"},"binary_offset":null,"stats":{"elapsed":{"secs":0,"nanos":259022,"human":"0.000259s"},"searches":1,"searches_with_match":1,"bytes_searched":7,"bytes_printed":269,"matched_lines":2,"matches":2}}}
{"data":{"elapsed_total":{"human":"0.005201s","nanos":5200971,"secs":0},"stats":{"bytes_printed":269,"bytes_searched":7,"elapsed":{"human":"0.000259s","nanos":259022,"secs":0},"matched_lines":2,"matches":2,"searches":1,"searches_with_match":1}},"type":"summary"}
```

I get a single match with two submatches, and a newline between them. The parsing code in VS Code doesn't handle this - it does handle newlines in a match if it is a multiline match, but not for individual matches with a newline in the middle.

If this is intended, I don't think it's necessarily an issue, but it's not expected. For slightly different queries, I get multiple matches.

```
$ rg '\.a' --json --multiline
{"type":"begin","data":{"path":{"text":"main.txt"}}}
{"type":"match","data":{"path":{"text":"main.txt"},"lines":{"text":".a1\n"},"line_number":1,"absolute_offset":0,"submatches":[{"match":{"text":".a"},"start":0,"end":2}]}}
{"type":"match","data":{"path":{"text":"main.txt"},"lines":{"text":".a2"},"line_number":2,"absolute_offset":4,"submatches":[{"match":{"text":".a"},"start":0,"end":2}]}}
{"type":"end","data":{"path":{"text":"main.txt"},"binary_offset":null,"stats":{"elapsed":{"secs":0,"nanos":69208,"human":"0.000069s"},"searches":1,"searches_with_match":1,"bytes_searched":7,"bytes_printed":393,"matched_lines":2,"matches":2}}}
{"data":{"elapsed_total":{"human":"0.009014s","nanos":9013863,"secs":0},"stats":{"bytes_printed":393,"bytes_searched":7,"elapsed":{"human":"0.000069s","nanos":69208,"secs":0},"matched_lines":2,"matches":2,"searches":1,"searches_with_match":1}},"type":"summary"}
```

And in 12, I get two matches

```
$ ./target/debug/rg --version                                                                                                                                                                                                                                    
ripgrep 12.0.0 (rev b9cd95faf1)
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
$ ./target/debug/rg '\Wa' --json --multiline /tmp/searchtest                                                                                                                                                                                                      
{"type":"begin","data":{"path":{"text":"/tmp/searchtest/main.txt"}}}
{"type":"match","data":{"path":{"text":"/tmp/searchtest/main.txt"},"lines":{"text":".a1\n"},"line_number":1,"absolute_offset":0,"submatches":[{"match":{"text":".a"},"start":0,"end":2}]}}
{"type":"match","data":{"path":{"text":"/tmp/searchtest/main.txt"},"lines":{"text":".a2"},"line_number":2,"absolute_offset":4,"submatches":[{"match":{"text":".a"},"start":0,"end":2}]}}
{"type":"end","data":{"path":{"text":"/tmp/searchtest/main.txt"},"binary_offset":null,"stats":{"elapsed":{"secs":0,"nanos":5538755,"human":"0.005539s"},"searches":1,"searches_with_match":1,"bytes_searched":7,"bytes_printed":441,"matched_lines":2,"matches":2}}}
{"data":{"elapsed_total":{"human":"0.027916s","nanos":27916431,"secs":0},"stats":{"bytes_printed":441,"bytes_searched":7,"elapsed":{"human":"0.005539s","nanos":5538755,"secs":0},"matched_lines":2,"matches":2,"searches":1,"searches_with_match":1}},"type":"summary"}
```

Just let me know if you intend to keep this behavior and I can most likely adapt to it.

Downstream issue https://github.com/microsoft/vscode/issues/131507

---

_Comment by @ashidaharo on 2021-09-08 06:38_

I have found a smaller instance that reproduces this problem.
It's '(? :\n|\\.) a'. By the definition of \W, \W can be matched with \n.
I noticed that there is an explanation related to this problem in the help section for the --multiline option.
> **WARNING**: Because of how the underlying regex engine works, multiline
> searches may be slower than normal line-oriented searches, and they may also
> use more memory. In particular, when multiline mode is enabled, ripgrep
> requires that each file it searches is laid out contiguously in memory
> (either by reading it onto the heap or by memory-mapping it). Things that
> cannot be memory-mapped (such as stdin) will be consumed until EOF before
> searching can begin. In general, ripgrep will only do these things when
> necessary. Specifically, if the --multiline flag is provided but the regex
> does not contain patterns that would match '\n' characters, then ripgrep
> will automatically avoid reading each file into memory before searching it.
> Nevertheless, if you only care about matches spanning at most one line, then it
> is always better to disable multiline mode.
>
> This flag can be disabled with --no-multiline.

Uh... so, based on this warning, the output from the match with '\n' in the given expression is more likely to be correct for "multiline mode"output...?

---
