```yaml
number: 18704
title: "Hug closing `}` when f-string expression has a  format specifier"
type: pull_request
state: merged
author: MichaReiser
labels:
  - bug
  - formatter
assignees: []
merged: true
base: main
head: micha/f-string-format-spec
created_at: 2025-06-16T09:29:55Z
updated_at: 2025-06-17T07:06:15Z
url: https://github.com/astral-sh/ruff/pull/18704
synced_at: 2026-01-12T15:56:24Z
```

# Hug closing `}` when f-string expression has a  format specifier

---

_@MichaReiser_

## Summary

This PR changes how we format f-string expressions with a format specifier. 

Before: 

* triple-quoted f- or t-strings with a format specifier were always formatted onto a single line
* single-quoted f- or t-strings could break over multiple lines even when they had format specifiers. The format specifier was separated by a newline + indent from the closing `}`. This is now invalid syntax under Python 3.13.4.
  ```py
  f"testeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeee{
      a:.3f
  }moreeeeeeeeeeeeeeeeeetest"  
  ```

This PR changes two address the invalid syntax and make single and triple quoted strings more consisent:

* f- or t-string elements with a format specifier now hug the closing `}`. This ensures that the formatter doesn't introduce a now invalid line break after the format specifier. 
  ```py
  f"testeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeee{
      a:.3f}moreeeeeeeeeeeeeeeeeetest"  
  ```
* Remove the restriction that triple-quoted f- or t-strings can't use the multiline layout if they have a format specifier. This isn't required to fix the invalid syntax BUT it fixes an issue where the formatter requires two passes to correctly format comments nested inside an interpolated format specifier:
  ```py
  f"""{foo
     :a{
       a # more
      # comment
     }
  }"""
  ```
  which before this PR gets formatted to:

  ```py
  f"""{foo:a{a}
  }"""  # more
      # comment
  ```
  which is fairly different from the source. I think making this change should be fine, if this PR makes it into the minor release tomorrow. 

I considered if we should always keep elements with a format specifier flat. However, this gets very tricky if the format specifier is already formatted over multiple lines and contains comments (see the triple-quoted case above). That's why I opted to allow them and making the behavior consistent between single and triple quoted strings.


Fixes https://github.com/astral-sh/ruff/issues/18672
Fixes https://github.com/astral-sh/ruff/issues/18632

Note, this PR doesn't change the parser / lexer

## Test Plan

Updated / added tests


---

_Label `bug` added by @MichaReiser on 2025-06-16 09:29_

---

_Label `formatter` added by @MichaReiser on 2025-06-16 09:29_

---

_Label `bug` added by @MichaReiser on 2025-06-16 09:29_

---

_Label `formatter` added by @MichaReiser on 2025-06-16 09:29_

---

_@MichaReiser reviewed on 2025-06-16 09:31_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/resources/test/fixtures/ruff/expression/fstring.py`:307 on 2025-06-16 09:31_

This example is a syntax error for a single quoted string, but it's perfectly fine for a triple quoted string

---

_Comment by @github-actions[bot] on 2025-06-16 09:36_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Renamed from "Don't use soft_block indent for f-strings with a foramt specifier" to "Don't use soft_block indent for f-strings with a format specifier" by @MichaReiser on 2025-06-16 14:12_

---

_Renamed from "Don't use soft_block indent for f-strings with a format specifier" to "Hug closing `}` when f-string expression has a  format specifier" by @MichaReiser on 2025-06-16 14:13_

---

_Review requested from @dhruvmanila by @MichaReiser on 2025-06-16 14:23_

---

_Marked ready for review by @MichaReiser on 2025-06-16 14:23_

---

_Review requested from @carljm by @MichaReiser on 2025-06-16 16:30_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-06-16 16:30_

---

_Review requested from @sharkdp by @MichaReiser on 2025-06-16 16:30_

---

_Review requested from @dcreager by @MichaReiser on 2025-06-16 16:30_

---

_Review request for @dcreager removed by @MichaReiser on 2025-06-16 16:32_

---

_Review request for @carljm removed by @MichaReiser on 2025-06-16 16:32_

---

_Review request for @sharkdp removed by @MichaReiser on 2025-06-16 16:32_

---

_Review request for @AlexWaygood removed by @MichaReiser on 2025-06-16 16:32_

---

_Comment by @github-actions[bot] on 2025-06-16 16:33_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Review comment by @ntBre on `crates/ruff_python_formatter/src/other/interpolated_string_element.rs`:244 on 2025-06-16 16:38_

Is this empty branch part of the TODO above?

---

_@ntBre approved on 2025-06-16 16:51_

This makes sense to me, for what it's worth. It definitely makes sense to wait for Dhruv's review, just wanted to take a quick look too in case it helped.

---

_@MichaReiser reviewed on 2025-06-16 16:53_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/other/interpolated_string_element.rs`:244 on 2025-06-16 16:53_

Lol no, that's just me moving stuff around and forgetting to delete it (and clippy not telling me)

---

_Comment by @MichaReiser on 2025-06-16 17:10_

@ntBre for the changelog (assuming this makes it in). We could probably write something like this

Ruff now formats multiline f-strings with format-specifiers differently because of a change to CPython's grammar. Before, Ruff formatted the interpolation expression like so:

```py
f"This is some long string {
    x:d
} that is formatted across multiple lines"  
```

The line break after the `:d` format specifier is now a syntax error with Python 3.13.4 or newer. That's why Ruff now formats the f-string like so:

```py
f"This is some long string {
    x:d} that is formatted across multiple lines"  
```

---

_Comment by @ntBre on 2025-06-16 17:38_

Thanks! I'll add it to the blog post too. There's no real rush on getting the release out from my side, so I think we can wait until this is ready.

---

_Added to milestone `v0.12` by @ntBre on 2025-06-16 17:59_

---

_Review comment by @dhruvmanila on `crates/ruff_python_formatter/resources/test/fixtures/ruff/expression/fstring.py`:299 on 2025-06-17 03:14_

I think 21 is already taken by the previous comment, let's choose another number so that it's easier to differentiate.

---

_Review comment by @dhruvmanila on `crates/ruff_python_formatter/src/other/interpolated_string_element.rs`:269 on 2025-06-17 03:45_

I'm guessing the example mentioned in the comment is specifically to target the `format_spec.is_none` case and not the trailing comment case. And, as the latter case is going to be removed, we don't really need to provide an example for that here.

---

_Review comment by @dhruvmanila on `crates/ruff_python_formatter/src/other/interpolated_string_element.rs`:225 on 2025-06-17 03:46_

I think this will also be a syntax error with the recent CPython parser changes? So, then this case can never occur then unless maybe it's a triple-quoted string?

---

_@dhruvmanila approved on 2025-06-17 03:49_

Looks great! Thank you for the quick fix!

---

_Comment by @dhruvmanila on 2025-06-17 03:55_

> * Remove the restriction that triple-quoted f- or t-strings can't use the multiline layout if they have a format specifier. This isn't required to fix the invalid syntax BUT it fixes an issue where the formatter requires two passes to correctly format comments nested inside an interpolated format specifier:

Yeah, I think I mainly did this because the newline for a triple-quoted f-string is a newline character in the literal string and not an actual newline, so that didn't account for the fact that the f-string is multiline. For example, the following is not a multiline f-string:

```py
aaaaaaaaaaa = f"""asaaaaaaaaaaaaaaaa {aaaaaaaaaaaa + bbbbbbbbbbbb + ccccccccccccccc + dddddddd:.3f
} cccccccccc"""
```

But, it does create inconsistent behavior between triple-quoted and single-quoted f-string which you've noticed. One might argue that is not really an inconsistent behavior as it does follow what we stated regarding when the expression could be broken down i.e., only when the user already has a multiline f-string.

In reality, I guess this shouldn't affect too many users as newlines in format spec should be rare enough especially now that it's a syntax error. So, I'm fine with this change.

---

_Comment by @MichaReiser on 2025-06-17 05:25_

> Yeah, I think I mainly did this because the newline for a triple-quoted f-string is a newline character in the literal string and not an actual newline, so that didn't account for the fact that the f-string is multiline. For example, the following is not a multiline f-string:

I learned this the hard way when working on this PR 😅. Thanks for adding tests for it. 

> In reality, I guess this should affect too many users as newlines in format spec should be rare enough especially now that it's a syntax error. So, I'm fine with this change.

Yeah, that was my thinking too and seems to be confirmed by our ecosyste checks. 

---

_@MichaReiser reviewed on 2025-06-17 05:28_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/other/interpolated_string_element.rs`:269 on 2025-06-17 05:28_

The example is for the `format_spec.is_some` case. It will be removed in my next PR. 

I mainly provided it because I found it helpful to reason about all the changes i've been making. I think I keep it, considering that I already have a PR that removes it again.

---

_@MichaReiser reviewed on 2025-06-17 05:30_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/other/interpolated_string_element.rs`:225 on 2025-06-17 05:30_

Yes, this is correct. I decided to keep supporting this case for now, so that the formatter doesn't mess it up until we make this a syntax error (which will only be in the next release because I want the formatter to fixup all format spec instances with trailing newlines that have no comments.

---

_Comment by @MichaReiser on 2025-06-17 05:33_

@ntBre I'll merge this to main, considering that we plan to do the minor release today.

---

_Merged by @MichaReiser on 2025-06-17 05:39_

---

_Closed by @MichaReiser on 2025-06-17 05:39_

---

_Branch deleted on 2025-06-17 05:39_

---
