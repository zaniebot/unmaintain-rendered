```yaml
number: 15341
title: Add f-string formatting to the docs
type: pull_request
state: merged
author: dhruvmanila
labels:
  - documentation
assignees: []
merged: true
base: ruff-0.9
head: dhruv/f-string-docs
created_at: 2025-01-08T09:37:21Z
updated_at: 2025-01-09T05:16:15Z
url: https://github.com/astral-sh/ruff/pull/15341
synced_at: 2026-01-10T20:34:00Z
```

# Add f-string formatting to the docs

---

_Pull request opened by @dhruvmanila on 2025-01-08 09:37_

## Summary

This PR adds a new f-string formatting section to the documentation.

It adds it to the https://docs.astral.sh/ruff/formatter page and provides a backlink in the deviation section as well.

There isn't a great place to add this kind of section in our documentation as ideally it would go under "Ruff code style" (similar to [Black code style](https://black.readthedocs.io/en/stable/the_black_code_style/current_style.html)) section.

### Question

I've found having the "Quotes" and "Line breaks" section in structuring the content but I'm open to removing them. Any opinions are welcome!

There can be more content to it but I'm not sure if we want to provide a lot of details. For example, we can expand on the "Quotes" section to enumerate all additional cases where we preserve the quote style.

### Preview

<img width="755" alt="Screenshot 2025-01-08 at 9 26 39 PM" src="https://github.com/user-attachments/assets/4c77c012-3a2a-4fc1-92c7-b6e4f1ebad71" />

<!-- <img width="1203" alt="Screenshot 2025-01-08 at 3 51 03 PM" src="https://github.com/user-attachments/assets/1e8cb401-9584-4705-9c15-3fe308466c62" /> -->


---

_Label `documentation` added by @dhruvmanila on 2025-01-08 09:37_

---

_Marked ready for review by @dhruvmanila on 2025-01-08 10:23_

---

_Review requested from @MichaReiser by @dhruvmanila on 2025-01-08 10:23_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2025-01-08 10:23_

---

_Comment by @MichaReiser on 2025-01-08 12:49_

The section does feel a bit out of place. That makes me wonder if we should rename the *Black compatibility* section to *Style guide*, rephrase the section to say that we follow Black's style guide with a few exceptions and that this section documents where we go beyond what black does? 

---

_Review comment by @MichaReiser on `docs/formatter.md`:230 on 2025-01-08 12:51_

```suggestion
_Stabilized in Ruff 0.9.0_
```

It's probably easier for users to know what Ruff version they're using.

---

_Review comment by @MichaReiser on `docs/formatter.md`:233 on 2025-01-08 12:51_

Nit: By default suggests that there's a way to disable the behavior.

```suggestion
Unlike Black, Ruff formats f-string expressions. 
```



---

_Review comment by @MichaReiser on `docs/formatter.md`:246 on 2025-01-08 12:57_

This is one reason. The other is that it tries to minimize escapes, similar to what's described here https://black.readthedocs.io/en/stable/the_black_code_style/current_style.html#strings

---

_Review comment by @MichaReiser on `docs/formatter.md`:240 on 2025-01-08 12:58_

I'm leaning toward removing those bullets because they're each covered by a sub-heading (except how to skip f-string formatting)

---

_Review comment by @MichaReiser on `docs/formatter.md`:249 on 2025-01-08 13:02_

I think this is slightly misleading. Ruff can change the quotes for:

```py
f'{10 + len("hello \" ")}'
```

It gets formatted to 

```
f"{10 + len('hello " ')}"
```

It can only not change the quotes if they're inside a debug expression (or format specifier). Let's double check with the implementation if there are other reasons when the preferred quote style can't be applied

---

_Comment by @dhruvmanila on 2025-01-08 13:03_

> That makes me wonder if we should rename the _Black compatibility_ section to _Style guide_, rephrase the section to say that we follow Black's style guide with a few exceptions and that this section documents where we go beyond what black does?

Interesting. This seems like a good idea.

---

_Review comment by @MichaReiser on `docs/formatter.md`:260 on 2025-01-08 13:05_

```suggestion
For nested f-strings, Ruff alternates quote styles, starting with the configured style for the
outermost f-string.
```

It wasn't clear to me if this now demonstrates a case where Ruff doesn't change the quote style because it would result in invalid syntax but I think it doesn't. I'm leaning towards omitting this sentence because it's kind of expected that the formatter doesn't produce invalid syntax.

---

_Review comment by @MichaReiser on `docs/formatter.md`:280 on 2025-01-08 13:06_

I'm leaning towards motivating this closer to what Prettier wrote

> Template literals can contain interpolations. Deciding whether it's appropriate to insert a linebreak within an interpolation unfortunately depends on the semantic content of the template - for example, introducing a linebreak in the middle of a natural-language sentence is usually undesirable. Since Prettier doesn't have enough information to make this decision itself, it uses a heuristic similar to that used for objects: it will only split an interpolation expression across multiple lines if there was already a linebreak within that interpolation.



---

_@MichaReiser reviewed on 2025-01-08 13:06_

---

_@AlexWaygood reviewed on 2025-01-08 13:09_

---

_Review comment by @AlexWaygood on `docs/formatter.md`:233 on 2025-01-08 13:09_

I think also it might be worth making clear exactly what we mean by "f-string expressions". I.e., I believe black will change `f'{foo()}'` to `f"{foo()}"` -- it just won't format the interpolated expressions inside the braces, right?

---

_@AlexWaygood reviewed on 2025-01-08 13:09_

---

_Review comment by @AlexWaygood on `docs/formatter.md`:236 on 2025-01-08 13:09_

```suggestion
Ruff employs several heuristics to determine how an f-string should be formatted, including:
```

---

_@dhruvmanila reviewed on 2025-01-08 13:10_

---

_Review comment by @dhruvmanila on `docs/formatter.md`:233 on 2025-01-08 13:10_

Yes, that's correct. Good point, I'll make the change.

---

_@MichaReiser reviewed on 2025-01-08 13:11_

---

_Review comment by @MichaReiser on `docs/formatter.md`:233 on 2025-01-08 13:11_

Yes, kind of, it's complicated. Black does change the quotes but only if there are no nested expressions with quotes.

---

_@dhruvmanila reviewed on 2025-01-08 13:19_

---

_Review comment by @dhruvmanila on `docs/formatter.md`:240 on 2025-01-08 13:19_

Yeah, makes sense. I'll add the one for the last bullet point.

---

_@dhruvmanila reviewed on 2025-01-08 13:23_

---

_Review comment by @dhruvmanila on `docs/formatter.md`:249 on 2025-01-08 13:23_

Thanks for sharing that although I'm referring to a self-documenting f-string which shouldn't change the quote.

I'll add other reasons when we don't change the quotes as well.

---

_@MichaReiser reviewed on 2025-01-08 13:27_

---

_Review comment by @MichaReiser on `docs/formatter.md`:249 on 2025-01-08 13:27_

oh, I didn't notice that (and probably wouldn't have understood what it is). It might be worth adding a reference link 

---

_Added to milestone `v0.9` by @MichaReiser on 2025-01-08 14:55_

---

_@dhruvmanila reviewed on 2025-01-08 15:37_

---

_Review comment by @dhruvmanila on `docs/formatter.md`:280 on 2025-01-08 15:37_

I've provided a link to the relevant section in Prettier which is why I thought not to add the same detail here and keep it short.

---

_Comment by @dhruvmanila on 2025-01-08 15:58_

@MichaReiser I've made some changes to introduce a "Style Guide" section. I've removed some content from previous section because it doesn't really fit in the new section. Happy to hear any thoughts on it, I can refine the section a bit more and add some more content if it looks good.

---

_Review comment by @MichaReiser on `docs/formatter.md`:410 on 2025-01-08 16:18_

```suggestion
invalid syntax for the target Python version or requires more backslash escapes
```

---

_Review comment by @MichaReiser on `docs/formatter.md`:280 on 2025-01-08 16:21_

Including a link to the source of our inspiration is great but I don't expect many users to click it and one important aspect of the style guide section is that we motivate our formatting. That's why I'd still prefer at least a few words why we trade consistency with flexibility.

---

_@MichaReiser approved on 2025-01-08 16:23_

Nice. I really like where this is going. We might have to port some more deviations to the style guide section in the future but this is a great start. 

One more nit and if we could motivate the line break behavior a bit more would be great.

---

_Review comment by @dylwil3 on `docs/formatter.md`:431 on 2025-01-08 16:51_

```suggestion
For all target Python versions, when a [self-documenting f-string expression] contains an expression between
```

---

_Review comment by @dylwil3 on `docs/formatter.md`:432 on 2025-01-08 16:53_

```suggestion
the curly braces (`{...}`) with a format specifier containing the configured quote style:
```

---

_Review comment by @dylwil3 on `docs/formatter.md`:490 on 2025-01-08 16:53_

```suggestion
[self-documenting f-string expression]: https://realpython.com/python-f-strings/#self-documenting-expressions-for-debugging
```

---

_Review comment by @dylwil3 on `docs/formatter.md`:455 on 2025-01-08 16:54_

```suggestion
Starting with Python 3.12 ([PEP 701](https://peps.python.org/pep-0701/)), the expression parts of an f-string can
```

---

_Review comment by @dylwil3 on `docs/formatter.md`:456 on 2025-01-08 16:55_

```suggestion
span multiple lines. Ruff adopts a heuristic similar to [Prettier](https://prettier.io/docs/en/next/rationale.html#template-literals)
```

---

_Review comment by @dylwil3 on `docs/formatter.md`:458 on 2025-01-08 16:56_

```suggestion
if there was already a line break within any of the expression parts of the f-string.
```

---

_@dylwil3 approved on 2025-01-08 16:56_

Looks great! A few grammatical nits

---

_Merged by @dhruvmanila on 2025-01-08 17:10_

---

_Closed by @dhruvmanila on 2025-01-08 17:10_

---

_Branch deleted on 2025-01-08 17:10_

---

_Branch restored on 2025-01-09 05:15_

---

_Branch deleted on 2025-01-09 05:16_

---

_Branch restored on 2025-01-09 05:16_

---
