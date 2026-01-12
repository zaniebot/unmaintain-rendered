```yaml
number: 7053
title: Formatter parenthesizes and expands multiple-assigment values
type: issue
state: closed
author: cnpryer
labels:
  - formatter
  - needs-decision
assignees: []
created_at: 2023-09-01T20:32:26Z
updated_at: 2023-09-13T07:40:22Z
url: https://github.com/astral-sh/ruff/issues/7053
synced_at: 2026-01-12T15:54:46Z
```

# Formatter parenthesizes and expands multiple-assigment values

---

_@cnpryer_

I actually really like this result. Reporting anyway. I thought this was reported already, I but couldn't find any issues.

Line-length: 88

Black (23.7.0):
```py
def some_fn() -> None:
    s_aaaaaaa1, s_aaaaaaa2 = df1["aaaaaaaaaaaaaaaaa"].rename("aaa"), df2[
        "aaaaaaaaaaaaaaaaa"
    ].rename("aaa")
```

Ruff (0.0.287):
```py
def some_fn() -> None:
    s_aaaaaaa1, s_aaaaaaa2 = (
        df1["aaaaaaaaaaaaaaaaa"].rename("aaa"),
        df2["aaaaaaaaaaaaaaaaa"].rename("aaa"),
    )
```

Playground: https://play.ruff.rs/c2cc8a7c-e525-4d3f-8c6c-8063f2272ba1

---

_Label `formatter` added by @charliermarsh on 2023-09-01 23:03_

---

_Comment by @MichaReiser on 2023-09-02 08:12_

This matches [Black's preview style](https://black.vercel.app/?version=stable&state=_Td6WFoAAATm1rRGAgAhARYAAAB0L-Wj4AELAIZdAD2IimZxl1N_WlbvK5V_r8p8RWVQQnw9iapsVWAURY3SxClurVnV7aXA1aSAFyJDV_6KbhdEOAF6iWHwQpsW8LqvZgg0uA8_AllGpgDXu-99fTtWZaXXli_pe1jrTl-Pusr6e6XNQgFQsZYiQf0UEuVCiygH0IBiAkjiBxZoJLpZsqRswVAAAAAA6vtt-_240qIAAaIBjAIAAGWQ6g6xxGf7AgAAAAAEWVo=) and IMO, is more readable overall. 

How frequent was this change in your code base? I recommend leaving as is if it was a rare change.



---

_Added to milestone `Formatter: Beta` by @MichaReiser on 2023-09-02 08:23_

---

_Comment by @charliermarsh on 2023-09-02 11:25_

In general, I find it reasonable not to fix deviations that already exist in Black's preview style, though of course depends on how common they are, how hard they are to fix, etc.

---

_Comment by @cnpryer on 2023-09-02 12:23_

This was the most frequently altered pattern. It's common throughout my test suite.

Very much prefer this though.

---

_Comment by @cnpryer on 2023-09-02 12:34_

I'm going to update the snippet I provided for clarity. Looks like these are all series-based tests.

Edit: Heh, can't copy the new playground link on mobile. I'll fix in a bit.

---

_Label `needs-decision` added by @MichaReiser on 2023-09-04 07:46_

---

_Comment by @MichaReiser on 2023-09-04 07:50_

Okay this has a different reason than I initially thought and you correctly called it out:

> I'm going to update the snippet I provided for clarity. Looks like these are all series-based tests.

The issue is that ruff prefers parenthesizing the tuple in assignment positions rather than breaking them left to right (the default is right to left). I personally find this much easier to read. I struggled a lot scanning what the assignment targets and what the values are. 

This is probably related to another Ruff deviation in relation with Tuples:

Ruff is inserting parentheses for tuple's in more places than Black. I wasn't aware that this is also happening in assignments, but we're deviating in expression statements because I and @konstin believe that it overall improves readability. For example a common incorrect use of tuples in django ([PR](https://github.com/django/django/pull/17181)) was

```python
label_for_field("nonexistent", Article, form=ArticleForm()),
```

Which clearly is an incidental use of tuples. Ruff formats this as 

```
(label_for_field("nonexistent", Article, form=ArticleForm()),)
```

This makes the tuple very prominent, making you aware of the potential mistake (but Ruff removes the parentheses once there are at least two values).

I'm interested to hear @konstin opinion on this issue.

---

_Comment by @MichaReiser on 2023-09-04 07:53_

> This matches [Black's preview style](https://black.vercel.app/?version=stable&state=_Td6WFoAAATm1rRGAgAhARYAAAB0L-Wj4AELAIZdAD2IimZxl1N_WlbvK5V_r8p8RWVQQnw9iapsVWAURY3SxClurVnV7aXA1aSAFyJDV_6KbhdEOAF6iWHwQpsW8LqvZgg0uA8_AllGpgDXu-99fTtWZaXXli_pe1jrTl-Pusr6e6XNQgFQsZYiQf0UEuVCiygH0IBiAkjiBxZoJLpZsqRswVAAAAAA6vtt-_240qIAAaIBjAIAAGWQ6g6xxGf7AgAAAAAEWVo=) and IMO, is more readable overall.
> 
> How frequent was this change in your code base? I recommend leaving as is if it was a rare change.

I need to revise this statement. Black's preview style isn't changing the output. The output only changes when the tuple on the right is parenthesized in the source, in which case Black preserves the parentheses.

I suspect that Black uses the "can omit optional parentheses" layout for the tuple on the right which I think we could mimic by using `in_parentheses_only_group` and `in_parentheses_only_soft_line_break`. We could try using  `TupleParentheses::OptionalParentheses` for assignment values.

---

_Comment by @konstin on 2023-09-04 12:24_

> I'm interested to hear @konstin opinion on this issue.

For the given example, i strongly prefer the ruff style over the black stable style, i find the black stable style confusing wrt to seeing that the RHS is a tuple expression. I'm a bit worried though that there are cases where the deviations are unintended.

---

_Comment by @MichaReiser on 2023-09-13 07:40_

Related to #7317 Closing to track the discussion there.

---

_Closed by @MichaReiser on 2023-09-13 07:40_

---
