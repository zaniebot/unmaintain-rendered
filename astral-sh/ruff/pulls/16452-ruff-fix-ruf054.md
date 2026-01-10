```yaml
number: 16452
title: "[Ruff] Fix RUF054"
type: pull_request
state: closed
author: VascoSch92
labels:
  - bug
  - preview
assignees: []
draft: true
base: main
head: RUF054
created_at: 2025-03-01T16:54:36Z
updated_at: 2025-12-10T17:39:59Z
url: https://github.com/astral-sh/ruff/pull/16452
synced_at: 2026-01-10T16:42:11Z
```

# [Ruff] Fix RUF054

---

_Pull request opened by @VascoSch92 on 2025-03-01 16:54_

_No description provided._

---

_@ntBre reviewed on 2025-03-02 16:37_

Thanks for opening this! It looks like calls to `check_logical_lines` are gated behind this check:
https://github.com/astral-sh/ruff/blob/0d615b8765d7676e4842cd0e3dc91c6a27d6bc17/crates/ruff_linter/src/linter.rs#L125-L129

so you also have to update the `Rule::lint_source` method here:
https://github.com/astral-sh/ruff/blob/d796961ff78fd64a4399df161218a58ce00ec933/crates/ruff_linter/src/registry.rs#L249

`IndentedFormFeed` is currently in the `PhysicalLines` block here:
https://github.com/astral-sh/ruff/blob/d796961ff78fd64a4399df161218a58ce00ec933/crates/ruff_linter/src/registry.rs#L253-L261

but you'll need to move it into the `LogicalLines` pattern.

After doing that, I was able to trigger your panic in the RUF054 test.

---

_Label `bug` added by @ntBre on 2025-03-02 16:38_

---

_Label `preview` added by @ntBre on 2025-03-02 16:38_

---

_Comment by @VascoSch92 on 2025-03-05 19:25_

Hey @ntBre thanks for the feedback. Now it is working. 

However I have another problem with logical lines.

Suppose I want to consider the following example

```python
  print("!")
```
where the  are form feeds.

Then I have that the tokens of the logical line associated to the code are

```
[LogicalLineToken { kind: Name, range: 378..383 }, LogicalLineToken { kind: Lpar, range: 383..384 }, LogicalLineToken { kind: String, range: 384..387 }, LogicalLineToken { kind: Rpar, range: 387..388 }, LogicalLineToken { kind: Newline, range: 388..389 }]
```

while If I convert the line into text and print it I have

```
"print(\"!\")\n"
```

So it seems that the logical line are completely ignoring the feed forms. Is this normal?

Perhaps is because of this issue that the rule was considering physical lines instead of logical ones.


---

_Comment by @codspeed-hq[bot] on 2025-03-05 19:26_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/VascoSch92%3ARUF054?utm_source=github&utm_medium=comment&utm_content=header)

### Merging #16452 will **degrade performances by 13.55%**

<sub>Comparing <code>VascoSch92:RUF054</code> (8edfcb2) with <code>main</code> (ebd172e)</sub>



### Summary

`❌ 4` regressions  
`✅ 28` untouched  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/VascoSch92%3ARUF054?utm_source=github&utm_medium=comment&utm_content=acknowledge)._

### Benchmarks breakdown

|     | Mode | Benchmark | `BASE` | `HEAD` | Change |
| --- | ---- | --------- | ------ | ------ | ------ |
| ❌ | Simulation | [`` linter/all-with-preview-rules[large/dataset.py] ``](https://codspeed.io/astral-sh/ruff/branches/VascoSch92%3ARUF054?uri=crates%2Fruff_benchmark%2Fbenches%2Flinter.rs%3A%3Apreview_rules%3A%3Abenchmark_preview_rules%3A%3Alinter%2Fall-with-preview-rules%5Blarge%2Fdataset.py%5D&runnerMode=Instrumentation&utm_source=github&utm_medium=comment&utm_content=benchmark) | 21.3 ms | 22.3 ms | -4.66% |
| ❌ | Simulation | [`` linter/all-with-preview-rules[numpy/ctypeslib.py] ``](https://codspeed.io/astral-sh/ruff/branches/VascoSch92%3ARUF054?uri=crates%2Fruff_benchmark%2Fbenches%2Flinter.rs%3A%3Apreview_rules%3A%3Abenchmark_preview_rules%3A%3Alinter%2Fall-with-preview-rules%5Bnumpy%2Fctypeslib.py%5D&runnerMode=Instrumentation&utm_source=github&utm_medium=comment&utm_content=benchmark) | 5.1 ms | 5.4 ms | -5.49% |
| ❌ | Simulation | [`` linter/all-with-preview-rules[numpy/globals.py] ``](https://codspeed.io/astral-sh/ruff/branches/VascoSch92%3ARUF054?uri=crates%2Fruff_benchmark%2Fbenches%2Flinter.rs%3A%3Apreview_rules%3A%3Abenchmark_preview_rules%3A%3Alinter%2Fall-with-preview-rules%5Bnumpy%2Fglobals.py%5D&runnerMode=Instrumentation&utm_source=github&utm_medium=comment&utm_content=benchmark) | 849.5 µs | 895.3 µs | -5.11% |
| ❌ | Simulation | [`` linter/all-with-preview-rules[unicode/pypinyin.py] ``](https://codspeed.io/astral-sh/ruff/branches/VascoSch92%3ARUF054?uri=crates%2Fruff_benchmark%2Fbenches%2Flinter.rs%3A%3Apreview_rules%3A%3Abenchmark_preview_rules%3A%3Alinter%2Fall-with-preview-rules%5Bunicode%2Fpypinyin.py%5D&runnerMode=Instrumentation&utm_source=github&utm_medium=comment&utm_content=benchmark) | 2.6 ms | 3 ms | -13.55% |


---

_Comment by @ntBre on 2025-03-05 19:56_

Yeah, if we're looking at the tokens themselves, I wouldn't expect to see form feeds. They're probably discarded with other whitespace.

It looks like you're using `line.text().as_bytes()` though, does that get you what you need? I think I might be missing something in the question because GitHub ate the form feeds :laughing: 

---

_Comment by @VascoSch92 on 2025-03-06 07:16_

> Yeah, if we're looking at the tokens themselves, I wouldn't expect to see form feeds. They're probably discarded with other whitespace.
> 
> It looks like you're using `line.text().as_bytes()` though, does that get you what you need? I think I might be missing something in the question because GitHub ate the form feeds 😆

Let me explain better. Suppose I have some spaces or tabs at the beginning of a line, followed by a form feed. When I receive the logical line line and convert it to text to inspect its bytes, the spaces and tabs are discarded. This makes sense, as it aligns with the purpose of the rule—form feeds placed between indentation spaces or tabs can cause issues. But that prevent my from running tests to see if the rule implementation is working properly. :-)

For example, consider the following case: `"if True:\n \u{c}\u{c} print(\"!\")\n"`, where \u{c} represents a form feed.

In this scenario, I receive the two logical lines that converted to text are: `if True:\n` and `print(\"!\")\n` (with spaces and form feeds removed, i.e., I cannot check if there is a violation).

I suspect the issue occurs when converting the logical line to text. note that the actual rule code converts a Line directly into bytes without first converting it to text.

---

_Comment by @ntBre on 2025-03-07 18:07_

Thanks for the extra context and sorry for the delay here. I think I'll need to get the code locally and play with it a bit to make sure I'm following.

---

_Comment by @MichaReiser on 2025-03-07 20:47_

I haven't followed the entire discussion but what I understand is that you want to find the form feeds that are between any two tokens of a logical line. I suspect that you have to do something similar to what we do here

https://github.com/astral-sh/ruff/blob/9f3a38d408f473df7a6b3574c5d1389bef303dd5/crates/ruff_python_index/src/indexer.rs#L45-L60

because the lexer doesn't generate any tokens for trivia whitespace (we considered doing that a couple of times and I still think it might be worth it but it isn't something ruff does today)

---

_Comment by @VascoSch92 on 2025-03-07 21:04_

> Thanks for the extra context and sorry for the delay here. I think I'll need to get the code locally and play with it a bit to make sure I'm following.

Ah sorry if my expleination was not clear. If you run the test for this fork and compare the printed output with the python code in the test script, you will se what i mean. I hope i'm not hallucinating or doing stupid errors :)  

---

_Comment by @VascoSch92 on 2025-03-07 21:05_

> I haven't followed the entire discussion but what I understand is that you want to find the form feeds that are between any two tokens of a logical line. I suspect that you have to do something similar to what we do here
> 
> 
> 
> https://github.com/astral-sh/ruff/blob/9f3a38d408f473df7a6b3574c5d1389bef303dd5/crates/ruff_python_index/src/indexer.rs#L45-L60
> 
> 
> 
> because the lexer doesn't generate any tokens for trivia whitespace (we considered doing that a couple of times and I still think it might be worth it but it isn't something ruff does today)

Thanks for the hint.

Just a question: is there a method that given a logical line, it returns an iterator of the physical lines composing it? This will solve my issue

---

_Comment by @MichaReiser on 2025-03-07 21:23_

> Just a question: is there a method that given a logical line, it returns an iterator of the physical lines composing it? This will solve my issue

No, but I'm also not sure why you'd need that? You can iterate over the tokens and each physical line is separated by a `NonLogicalNewline` token.

---

_Comment by @VascoSch92 on 2025-03-08 10:47_

Consider the snippet
```python
  # here there is a form feed
```
the   represent a form feed. This line should trigger and error as there is a white space before the form feed.

Now the tokes of the logical line are:
```text
[LogicalLineToken { kind: Comment, range: 120..147 }, LogicalLineToken { kind: NonLogicalNewline, range: 147..148 }]
```
and if I convert the line to text we have

```text
"# here there is a form feed\n"
```

This makes sense as the logical line mechanism discard whitespaces (and also form feeds). On the other side, it is not possible to detect if there is a violation.

This doesn't happens for physical lines. 

Therefore, to detect a violation one needs both the data from physical lines and the one from logical lines.

---

_Comment by @MichaReiser on 2025-03-08 17:33_

GitHub makes your example difficult to understand because it doesn't show where the form feed is. You may have to use some ASCII art.

I'm probably overlooking something, but it's not clear to me why this would have to be a logical line rule. Could we make it a token-based rule instead? Then, between every `NewLine` or `NonLogicalNewline` and the next token, grab the source text and test if there's a whitespace followed by a form feed character. 



---

_Comment by @ntBre on 2025-03-08 19:20_

Thanks @MichaReiser! I think the logical line suggestion came from the issue (as an alternative to a physical line rule, where it started out), and I didn't know enough about this part of the code base to recommend a token-based rule instead. I think that's probably a better way to go.

@VascoSch92 Thanks for the explanations. I think they are clear, it just helps me to see the code and output locally too.

---

_Comment by @MichaReiser on 2025-12-10 17:39_

Thanks for your contribution. I'll close this PR because it has become stale. Please don't hesitate to resubmit the PR and reference this PR if you plan to keep working on this feature.

---

_Closed by @MichaReiser on 2025-12-10 17:39_

---
