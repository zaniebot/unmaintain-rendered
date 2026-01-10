```yaml
number: 14679
title: "[`ruff`] Extend unnecessary-regular-expression to non-literal strings (`RUF055`)"
type: pull_request
state: merged
author: ntBre
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: brent/ruf055-infer-str
created_at: 2024-11-29T14:52:50Z
updated_at: 2024-12-03T15:17:21Z
url: https://github.com/astral-sh/ruff/pull/14679
synced_at: 2026-01-10T20:42:27Z
```

# [`ruff`] Extend unnecessary-regular-expression to non-literal strings (`RUF055`)

---

_Pull request opened by @ntBre on 2024-11-29 14:52_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

This is a follow-up to #14659 to try to resolve variable bindings for the `pattern` argument in `re` methods like `sub` and `match`. The rule currently only matches string literals, but these changes enable detection of patterns like this:

```python
import re

pat1 = "xyz"
pat2 = pat1
pat3 = pat2

re.sub(pat3, "", s)
```

For `sub` specifically, it also handles non-literal `repl` arguments, which also have to be strings for the suggested fix to be valid.

## Test Plan

<!-- How was it tested? -->
`cargo test` with a new snapshot test based on the example above.

---

_Comment by @github-actions[bot] on 2024-11-29 14:59_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_regular_expression.rs`:260 on 2024-11-29 15:04_

Nit: Do we need both lifetimes or can this be simplified to

```suggestion
fn resolve_name<'a>(
    name: &'a Expr,
    semantic: &'a SemanticModel,
) -> Option<&'a ExprStringLiteral>
{
```

---

_@MichaReiser reviewed on 2024-11-29 15:04_

---

_@ntBre reviewed on 2024-11-29 15:07_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_regular_expression.rs`:260 on 2024-11-29 15:07_

Oh yes, it can be simplified, thank you!

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_regular_expression.rs`:274 on 2024-11-29 15:07_

Does the following of aliases find many new diagnostics? 

I'm otherwise leaning towards not supporting it. It seems rare that you have

```py
pat = "string"
alias = pat

re.sub(alias, ...)
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_regular_expression.rs`:254 on 2024-11-29 15:09_

Nit: Maybe rename to `resolve_pattern` or `resolve_string_literal` because it isn't returning a name. 

---

_@MichaReiser approved on 2024-11-29 15:09_

Neat

---

_Label `rule` added by @MichaReiser on 2024-11-29 15:09_

---

_Label `preview` added by @MichaReiser on 2024-11-29 15:09_

---

_@ntBre reviewed on 2024-11-29 15:11_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_regular_expression.rs`:274 on 2024-11-29 15:11_

It looks like there's nothing new from the ecosystem check, which surprised me. I'm happy not supporting this either, in light of that. The literal case definitely seems like the main one.

---

_@MichaReiser reviewed on 2024-11-29 15:52_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_regular_expression.rs`:274 on 2024-11-29 15:52_

Let me know what you decide or when it's ready to merge. 

---

_@ntBre reviewed on 2024-11-29 16:01_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_regular_expression.rs`:274 on 2024-11-29 16:01_

Do you think it's worth following any variables? I can remove the loop to follow only a direct reference, or we can even close the whole PR if you think the literal case is all we need. I'm happy with any of these options.

---

_@MichaReiser reviewed on 2024-11-29 16:37_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_regular_expression.rs`:274 on 2024-11-29 16:37_

Oh, I like that the rule now follows a direct reference. We should merge the PR. The only part I'm unsure about is whether it's worth to follow a chain of aliases. 

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_regular_expression.rs`:274 on 2024-11-29 16:41_

Okay, I think I'll go with just the direct reference. I was a little worried this could somehow become an infinite loop anyway.

---

_@ntBre reviewed on 2024-11-29 16:41_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_regular_expression.rs`:179 on 2024-11-29 17:12_

If I understand correctly, I think the need here is slightly different to the need on lines 95-96. On lines 95-96, we do need to know the _value_ of the string in order to be able to check it doesn't have any metacharacters in it (so only a string literal will do, or something that we can resolve to a string literal). But here, we just need to know it's a string; any string will do, as long as the user _isn't_ passing in a function.

Is that the case? If so, it _might_ be worth adding back the `is_str` function you added in https://github.com/astral-sh/ruff/pull/14679/commits/efcc4cfbe121203443d146961f5fce2c9d117a69 and using that here, rather than using `resolve_string_literal` in both places. The advantage of the `is_str` technique is that it also understands basic type hints, e.g. it would understand that `re.sub()` is being passed a string for the `repl` argument in something like this:

```py
import re

def foo(input_str: str, repl: str):
    re.sub("foobar", repl, input_str)
```

---

_@AlexWaygood reviewed on 2024-11-29 17:12_

Nice!

---

_@AlexWaygood reviewed on 2024-11-29 17:25_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_regular_expression.rs`:179 on 2024-11-29 17:25_

Now that I look at it, maybe we _do_ need to know (and analyze) the value of the `repl` string for a fully accurate analysis, though. For example, it seems like the initial version of the check that we merged yesterday emits a false-positive diagnostic (and incorrect autofix) for this:

```py
import re

re.sub(r"a", r"\g<0>\g<0>\g<0>", "a")
```

Now, this is a massive edge case -- I had to work quite hard to find it! I believe the only way you get a false positive with the rule's current logic is if there's a `\g` in the replacement string but no backslashes or metacharacters in the pattenr string, and it's _almost_ impossible to think of a way you could plausibly have a `re.sub()` call with those characteristics. So maybe we shouldn't worry about this -- I'm interested in your thoughts and @MichaReiser's!

---

_@ntBre reviewed on 2024-11-29 17:35_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_regular_expression.rs`:179 on 2024-11-29 17:35_

Oh wow, good catch! I was working on adding `is_str` back in, but maybe instead I need to check for metacharacters in `repl` too.

I thought we were safe from backreferences by avoiding `(` in the pattern, but I overlooked `\g<0>`. That exact sequence seems like the only way to trigger this behavior?

---

_@AlexWaygood reviewed on 2024-11-29 17:38_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_regular_expression.rs`:179 on 2024-11-29 17:38_

> I thought we were safe from backreferences by avoiding `(` in the pattern, but I overlooked `\g<0>`. That exact sequence seems like the only way to trigger this behavior?

I think so, yes! Although we also emit a RUF055 diagnostic on _invalid_ `re.sub()` calls like this, and maybe we should just ignore them? It feels like it might be outside of this rule's purview to autofix invalid `re.sub()` calls into valid `str.replace()` calls. We probably don't really know what the user intended exactly if the `re.sub()` call is invalid:

```pycon
>>> import re
>>> re.sub(r"a", r"\1", "a")
Traceback (most recent call last):
  File "<python-input-12>", line 1, in <module>
    re.sub(r"a", r"\1", "a")
    ~~~~~~^^^^^^^^^^^^^^^^^^
  File "/Users/alexw/.pyenv/versions/3.13.0/lib/python3.13/re/__init__.py", line 208, in sub
    return _compile(pattern, flags).sub(repl, string, count)
           ~~~~~~~~~~~~~~~~~~~~~~~~~~~~^^^^^^^^^^^^^^^^^^^^^
  File "/Users/alexw/.pyenv/versions/3.13.0/lib/python3.13/re/__init__.py", line 377, in _compile_template
    return _sre.template(pattern, _parser.parse_template(repl, pattern))
                                  ~~~~~~~~~~~~~~~~~~~~~~^^^^^^^^^^^^^^^
  File "/Users/alexw/.pyenv/versions/3.13.0/lib/python3.13/re/_parser.py", line 1070, in parse_template
    addgroup(int(this[1:]), len(this) - 1)
    ~~~~~~~~^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/Users/alexw/.pyenv/versions/3.13.0/lib/python3.13/re/_parser.py", line 1015, in addgroup
    raise s.error("invalid group reference %d" % index, pos)
re.PatternError: invalid group reference 1 at position 1
```

---

_@ntBre reviewed on 2024-11-29 17:56_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_regular_expression.rs`:179 on 2024-11-29 17:56_

I've added back `is_str` locally, along with your function argument test case. It's really nice to handle that case, but I'm a bit bothered by this edge case too, so I could go either way. I'm interested to hear which approach you and Micha think is best overall.

---

_@AlexWaygood reviewed on 2024-11-29 17:59_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_regular_expression.rs`:179 on 2024-11-29 17:59_

Why don't you push the version with `is_str` to this PR, and we can see if it results in any more ecosystem hits? That might give us some more data on how useful it is to be able to detect that the `repl` argument is a string from the function annotation

---

_@AlexWaygood reviewed on 2024-11-29 18:20_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_regular_expression.rs`:179 on 2024-11-29 18:20_

Hmm, it doesn't look like it adds any new ecosystem hits :/

I guess in that case, I'd vote for removing `is_str` again, and fixing the false positives on `\g<0>` and `\1` in `repl` arguments.

Thanks for putting up with my pernickitiness here!

---

_@ntBre reviewed on 2024-11-29 18:33_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_regular_expression.rs`:179 on 2024-11-29 18:33_

No problem, thanks for the thorough review! Should I reuse the other code to reject *any* metacharacters, or are references to named or numbered capture groups the only problems? I'm picturing checking for `\` followed by `g` or `1` through `9`. That seems a bit nicer than rejecting any metacharacter like I did for the patterns but possibly less safe.

---

_@AlexWaygood reviewed on 2024-11-29 18:47_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_regular_expression.rs`:179 on 2024-11-29 18:47_

> I'm picturing checking for `\` followed by `g` or `1` through `9`. That seems a bit nicer than rejecting any metacharacter like I did for the patterns but possibly less safe.

I think actually we could check for `\` followed by any ASCII character _except_ one of `abfnrtv`. Other than `0-9` and `g` (which both have special behaviour in `repl` strings, as we've just been discussing!), I believe those are the _only_ ASCII escapes that will be permitted in a `repl` string by `re.sub()`, Anything else causes `re.PatternError` to be raised -- meaning it's probably out of scope for us to emit this diagnostic on it:

```pycon
>>> re.sub(r"a", r"\d", "a")
Traceback (most recent call last):
  File "<python-input-13>", line 1, in <module>
    re.sub(r"a", r"\d", "a")
    ~~~~~~^^^^^^^^^^^^^^^^^^
  File "/Users/alexw/.pyenv/versions/3.13.0/lib/python3.13/re/__init__.py", line 208, in sub
    return _compile(pattern, flags).sub(repl, string, count)
           ~~~~~~~~~~~~~~~~~~~~~~~~~~~~^^^^^^^^^^^^^^^^^^^^^
  File "/Users/alexw/.pyenv/versions/3.13.0/lib/python3.13/re/__init__.py", line 377, in _compile_template
    return _sre.template(pattern, _parser.parse_template(repl, pattern))
                                  ~~~~~~~~~~~~~~~~~~~~~~^^^^^^^^^^^^^^^
  File "/Users/alexw/.pyenv/versions/3.13.0/lib/python3.13/re/_parser.py", line 1076, in parse_template
    raise s.error('bad escape %s' % this, len(this)) from None
re.PatternError: bad escape \d at position 0
```

---

_@ntBre reviewed on 2024-11-29 19:19_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_regular_expression.rs`:179 on 2024-11-29 19:19_

Let me know what you think about this version. If it looks good, it might be nice to reuse this escape check for `pattern` as well instead of rejecting `\` entirely.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_regular_expression.rs`:183 on 2024-12-02 08:57_

Does this work for non-raw strings where `\a` is not a valid escape? Instead, it should be `\\a`.

>  if it is a string, any backslash escapes in it are processed.


---

_@MichaReiser reviewed on 2024-12-02 08:57_

---

_@MichaReiser reviewed on 2024-12-02 09:06_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_regular_expression.rs`:183 on 2024-12-02 09:06_

Here another case where using `replace` alters behavior:

```
>>> re.sub(r"a", r"\z", "a")
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "C:\Users\Micha\AppData\Local\Programs\Python\Python312\Lib\re\__init__.py", line 186, in sub
    return _compile(pattern, flags).sub(repl, string, count)
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "C:\Users\Micha\AppData\Local\Programs\Python\Python312\Lib\re\__init__.py", line 334, in _compile_template
    return _sre.template(pattern, _parser.parse_template(repl, pattern))
                                  ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "C:\Users\Micha\AppData\Local\Programs\Python\Python312\Lib\re\_parser.py", line 1075, in parse_template
    raise s.error('bad escape %s' % this, len(this)) from None
re.error: bad escape \z at position 0
>>> "a".replace("a", r"\z")
'\\z'
```

The relevant section of the spec:


> Unknown escapes of ASCII letters are reserved for future use and treated as errors. Other unknown escapes such as \& are left alone. Backreferences, such as \6, are replaced with the substring matched by group 6 in the pattern. For example:

---

_@MichaReiser reviewed on 2024-12-02 09:08_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_regular_expression.rs`:183 on 2024-12-02 09:08_

And numbered groups:

```
re.sub(r"(a)", r"\1\1\1", "a")
'aaa'
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_regular_expression.rs`:183 on 2024-12-02 09:10_

Reading code is hard. That's all covered because it uses an allowlist. Sorry

---

_@MichaReiser reviewed on 2024-12-02 09:10_

---

_Comment by @MichaReiser on 2024-12-02 09:11_

This looks good to me and nice find @AlexWaygood 

---

_@AlexWaygood approved on 2024-12-03 15:13_

Thanks @ntBre!! I pushed some minor fixes to address some other edge cases identified by @dscorbett in #14757

---

_Merged by @AlexWaygood on 2024-12-03 15:17_

---

_Closed by @AlexWaygood on 2024-12-03 15:17_

---
