```yaml
number: 10477
title: Better error recovery for match patterns
type: pull_request
state: merged
author: dhruvmanila
labels:
  - parser
assignees: []
merged: true
base: dhruv/parser
head: dhruv/pattern-recovery
created_at: 2024-03-19T17:51:33Z
updated_at: 2024-03-25T14:17:10Z
url: https://github.com/astral-sh/ruff/pull/10477
synced_at: 2026-01-10T22:47:02Z
```

# Better error recovery for match patterns

---

_Pull request opened by @dhruvmanila on 2024-03-19 17:51_

## Summary

This PR updates the match statement's pattern parsing logic to better facilitate the error recovery. The following changes are made:

### Disallow star pattern (`*<name>`) outside of sequence pattern

This is done by passing an enum stating whether to allow star pattern or not and is based on where the parser is in the stack.

<details><summary>Call stack for pattern parsing functions:</summary>
<p>

I took notes on where would I requires changing this, so sharing it for documenting purposes:

* `parse_match_patterns`
  * `parse_match_pattern` (`AllowStarPattern::Yes`)
    * Error if it’s a star pattern and it's not a sequence pattern
  * `parse_sequence_match_pattern`
    * `parse_match_pattern` (`AllowStarPattern::Yes`)
* `parse_match_pattern` (`allow_star_pattern: AllowStarPattern`)
  * `parse_match_pattern_lhs` (`allow_star_pattern: AllowStarPattern`)
    * `parse_match_pattern_mapping`
      * `parse_match_pattern_lhs` (`AllowStarPattern::No`)
      * `parse_match_pattern` (`AllowStarPattern::No`)
    * `parse_match_pattern_star`
    * `parse_delimited_match_pattern`
      * `parse_match_pattern` (`AllowStarPattern::Yes`)
      * `parse_sequence_match_pattern`
    * `parse_match_pattern_literal`
      * `parse_attr_expr_for_match_pattern`
    * `parse_match_pattern_class`
      * `parse_match_pattern` (`AllowStarPattern::No`)
    * `parse_complex_literal_pattern`
      * `parse_match_pattern_lhs` (`AllowStarPattern::No`)

</p>
</details> 

The mapping pattern `FIRST` set has been updated to include `*` to allow the parser to recover from this error in mapping pattern. Otherwise, the list parsing would completely skip parsing it.

### Convert `Pattern` to `Expr`

In certain cases, the parser expects a specific type of pattern and uses that to extract information. For example, in complex literal, we parse the LHS and RHS as patterns but the LHS can only be a real number while the RHS can only be a complex number. This means that if the parser found any other pattern kind then we would convert the pattern into an equivalent expression and add an appropriate error. This will help any downstream tools.

Refer to `recovery::pattern_to_expr` for more details.

### Extract complex number literal pattern parsing logic

Earlier, this was inside the `parse_match_pattern_lhs` which made the method a bit more complex than it needed to be. The logic has been extracted to a new `parse_complex_pattern_literal` and is called when the parser encounters a pattern followed by either `+` or `-`.

The parser only allows a valid complex literal pattern which is `real_number +/- complex_number`. For any other case, an appropriate error will be raised.

## Review

Refer to the test cases to help in the review process.

## Test Plan

Add test cases and update the snapshots.


---

_Label `parser` added by @dhruvmanila on 2024-03-19 17:51_

---

_Comment by @github-actions[bot] on 2024-03-19 18:11_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_@dhruvmanila reviewed on 2024-03-21 04:09_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/resources/valid/statement/match.py`:278 on 2024-03-21 04:09_

Star pattern is only allowed inside a sequence pattern as per the grammar.

---

_@dhruvmanila reviewed on 2024-03-21 04:37_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/pattern.rs`:539 on 2024-03-21 04:37_

TODO: disallow (`-1j + 2j`) as well

---

_@dhruvmanila reviewed on 2024-03-21 04:38_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/pattern.rs`:670 on 2024-03-21 04:38_

This is mainly moved from down below because class comes first and then the arguments. It's only to maintain the flow of parsing.

The change here is to use `recovery::pattern_to_expr`.

---

_@dhruvmanila reviewed on 2024-03-21 04:39_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/pattern.rs`:679 on 2024-03-21 04:39_

To disallow:

```python
match subject:
	case Foo(a as b = 1):
		pass
```

---

_Marked ready for review by @dhruvmanila on 2024-03-21 04:40_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-03-21 04:40_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/resources/invalid/statements/match/invalid_class_pattern.py`:17 on 2024-03-21 08:38_

Is this intentionally commented out? 

---

_@MichaReiser reviewed on 2024-03-21 08:38_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/tests/snapshots/invalid_syntax@statements__match__invalid_class_pattern.py.snap`:331 on 2024-03-21 08:43_

What I understand is that the "invalid" part here is that `x as y` is not a valid pattern key. Should we only highlight the key and reword the error message to that only names are accepted as keys?

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/tests/snapshots/invalid_syntax@statements__match__invalid_lhs_or_rhs_pattern.py.snap`:812 on 2024-03-21 08:44_

Nit: It might not be worth, but should we peek ahead and only consume the `:` if the peeked token isn't an `Indent`?

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/tests/snapshots/invalid_syntax@statements__match__invalid_lhs_or_rhs_pattern.py.snap`:1086 on 2024-03-21 08:52_

We should avoid jargon like `rhs`. Does this match the literal_expr grammar? Does it only allow numbers after a `+`?

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/tests/snapshots/invalid_syntax@statements__match__invalid_lhs_or_rhs_pattern.py.snap`:843 on 2024-03-21 08:54_

The wording assignment target is a bit confusing here. Any chance we chould rephrase it to invalid pattern and possibly hint the users to what's not allowed?

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/tests/snapshots/invalid_syntax@statements__match__invalid_mapping_pattern.py.snap`:396 on 2024-03-21 08:55_

Not for now, but we should change our `Display` implementation of `TokenKind` to emit `'{'` instead of Rbrace haha

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/tests/snapshots/invalid_syntax@statements__match__invalid_mapping_pattern.py.snap`:454 on 2024-03-21 08:55_

that's really nice!

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/tests/snapshots/invalid_syntax@statements__match__invalid_mapping_pattern.py.snap`:464 on 2024-03-21 08:55_

You're rocking it!

---

_@dhruvmanila reviewed on 2024-03-21 08:56_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/resources/invalid/statements/match/invalid_class_pattern.py`:17 on 2024-03-21 08:56_

Yes. So, positional pattern cannot follow keyword pattern in a class pattern (`case Foo(x=1, y): ...` is invalid) but this means the pre-order assert in the invalid code fails.

I'll probably need to have a mathod on `PatternArguments` similar to how's one for `Arguments` (`arguments_source_order`) to return an argument in the order defined in source.

I need to think about this because it's only required while testing.

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/pattern.rs`:71 on 2024-03-21 08:58_


```suggestion
            // We know it's not a sequence pattern now, so check for star pattern usage.
```

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/pattern.rs`:188 on 2024-03-21 09:11_

Hm, this is a problem because we now drop "nodes", meaning the `MatchPatternMapping` range covers nodes that aren't represented in the AST. 

Any chance we could convert the star to a starred expression instead and push a pattern at the right position? If not, then we may need to bail out early here, even if it means that the error recovery isn't as good as it could be (or we have to change the AST representation)

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/pattern.rs`:384 on 2024-03-21 09:12_

I like the "passing an enum" pattern

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/pattern.rs`:539 on 2024-03-21 09:13_

Do you plan to do this as part of this PR or as a follow up PR?

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/recovery.rs`:140 on 2024-03-21 09:50_

Does that mean we drop the `as <name>` part in this case? I'm not quiet sure what we should do in this case to still get as nice error recovery. What are the cases where we need this branch? What I understand is that it is for:

```python
match invalid_rhs_pattern:
    case {"x" as y:1 + Foo()}:
        pass
```

I think it might be best to disallow `As` when parsing the `key` and make recovery panic (or return a Result) if you try to convert a `MatchAs`.

---

_@MichaReiser reviewed on 2024-03-21 09:50_

---

_@dhruvmanila reviewed on 2024-03-21 09:52_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/pattern.rs`:539 on 2024-03-21 09:52_

I just pushed a fix for it (https://github.com/astral-sh/ruff/pull/10477/commits/acbaff2b48a4956b5325cb79207434d3f3bf0377). Sorry, I should've done it in a follow-up PR.

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/tests/snapshots/invalid_syntax@statements__match__invalid_lhs_or_rhs_pattern.py.snap`:812 on 2024-03-21 09:54_

This was a bug in the way the pattern parsing logic was coded. It shouldn't have been exclusive and has been fixed in https://github.com/astral-sh/ruff/pull/10477/commits/80b091a91a89d85e7b95124ea5ddab415fcc9aa8

---

_@dhruvmanila reviewed on 2024-03-21 09:54_

---

_@dhruvmanila reviewed on 2024-03-21 09:55_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/tests/snapshots/invalid_syntax@statements__match__invalid_lhs_or_rhs_pattern.py.snap`:843 on 2024-03-21 09:55_

Sorry! This is same bug as mentioned earlier:

> This was a bug in the way the pattern parsing logic was coded. It shouldn't have been exclusive and has been fixed in https://github.com/astral-sh/ruff/pull/10477/commits/80b091a91a89d85e7b95124ea5ddab415fcc9aa8

---

_@dhruvmanila reviewed on 2024-03-21 09:56_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/tests/snapshots/invalid_syntax@statements__match__invalid_mapping_pattern.py.snap`:396 on 2024-03-21 09:56_

Yes, I've that in the parser TODO, thanks for the suggestion :)

---

_@dhruvmanila reviewed on 2024-03-21 10:02_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/tests/snapshots/invalid_syntax@statements__match__invalid_lhs_or_rhs_pattern.py.snap`:1086 on 2024-03-21 10:02_

I understand.

> Does this match the literal_expr grammar?

No, I borrowed it from the existing code.

> Does it only allow numbers after a `+`?

Only complex literal is allowed after a `+` or `-` because it can only occur in a complex literal pattern. I'll update the error message to better reflect that.

I'll update the LHS / RHS terminology in a follow-up PR.



---

_Review comment by @snowsignal on `crates/ruff_python_parser/src/error.rs`:195 on 2024-03-22 03:17_

Nit: this should probably be in lowercase to match the other parsing error messages.

---

_Review comment by @snowsignal on `crates/ruff_python_parser/resources/invalid/statements/match/invalid_mapping_pattern.py`:20 on 2024-03-22 05:12_

Nit: should this have a newline at the end?

---

_@snowsignal reviewed on 2024-03-22 05:16_

---

_@dhruvmanila reviewed on 2024-03-22 05:52_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/tests/snapshots/invalid_syntax@statements__match__invalid_class_pattern.py.snap`:331 on 2024-03-22 05:52_

Yes, your understanding is correct. I've reworded the error message to "Expected an identifier for the keyword pattern", another option is "Expected an identifier for the key part of a keyword pattern" but I guess that's too verbose. The highlighted range will cover the fact that the identifier was expected at that position.

---

_@dhruvmanila reviewed on 2024-03-22 07:17_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/pattern.rs`:188 on 2024-03-22 07:17_

> Any chance we could convert the star to a starred expression instead and push a pattern at the right position?

Not really because there's no way to represent a double starred expression in the AST, there's only a `ExprStarred` which is for `*foo`.

> If not, then we may need to bail out early here, even if it means that the error recovery isn't as good as it could be (or we have to change the AST representation)

I could see what bailing would look like but I'd prefer to change the AST representation even it means a bit more work to be done.

Thinking quickly about the AST representation, there are two solutions which comes to my mind:
1. Add a new `Pattern` variant for `**data` and store it in `MatchPatternMapping::patterns` field. This is probably not a good idea
2. Replace the `keys`, `patterns` and `rest` fields with a single `entries` field which is an enum with two kind (`MappingPattern::DoubleStar` and `MappingPattern::KeyValue`)

---

_@dhruvmanila reviewed on 2024-03-22 07:19_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/pattern.rs`:188 on 2024-03-22 07:19_

Also, this is happening in a list parsing so might involve using the parser context flags.

---

_@MichaReiser reviewed on 2024-03-22 07:32_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/pattern.rs`:188 on 2024-03-22 07:32_

I'm also leaning on changing the AST (e.g. similar to Pyright's AST). What's important to me is that we don't spend too much time now working on error recovery. I'm also okay marking this as a todo (creating an issue) and following up on this later because the focus now is on replacing the parser, not on perfect error recovery.

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/pattern.rs`:188 on 2024-03-22 07:34_

Yeah, I'd prefer to not to do it now as well. I'll open an issue for it.

---

_@dhruvmanila reviewed on 2024-03-22 07:35_

---

_@dhruvmanila reviewed on 2024-03-25 12:58_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/resources/invalid/statements/match/invalid_mapping_pattern.py`:20 on 2024-03-25 12:58_

I don't think it matters but I'll add it anyways. Not sure why my editor removed it lol. Thanks for noticing!

---

_@dhruvmanila reviewed on 2024-03-25 12:59_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/error.rs`:195 on 2024-03-25 12:59_

The cases are mixed now but I'm consistently trying to use uppercase. I'll look at the error messages later on. 

---

_@dhruvmanila reviewed on 2024-03-25 12:59_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/recovery.rs`:140 on 2024-03-25 12:59_

Fixed in https://github.com/astral-sh/ruff/pull/10523

---

_@dhruvmanila reviewed on 2024-03-25 13:11_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/pattern.rs`:188 on 2024-03-25 13:11_

#10570 

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-03-25 13:14_

---

_@MichaReiser approved on 2024-03-25 14:14_

---

_Merged by @dhruvmanila on 2024-03-25 14:17_

---

_Closed by @dhruvmanila on 2024-03-25 14:17_

---

_Branch deleted on 2024-03-25 14:17_

---
