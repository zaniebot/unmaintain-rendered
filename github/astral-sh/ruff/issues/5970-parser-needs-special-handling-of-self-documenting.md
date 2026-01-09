---
number: 5970
title: parser needs special handling of self-documenting f-strings
type: issue
state: closed
author: davidszotten
labels:
  - parser
assignees: []
created_at: 2023-07-22T07:39:46Z
updated_at: 2023-08-01T05:55:05Z
url: https://github.com/astral-sh/ruff/issues/5970
synced_at: 2026-01-07T13:12:15-06:00
---

# parser needs special handling of self-documenting f-strings

---

_Issue opened by @davidszotten on 2023-07-22 07:39_

python 3.8 and https://bugs.python.org/issue36817 (note no pep "was needed")
added [self documenting expressions for f-strings](https://docs.python.org/3/whatsnew/3.8.html#f-strings-support-for-self-documenting-expressions-and-debugging)

`f"{a=}"` is now short-hand for `f"a={a}"`

in the [parser](https://github.com/astral-sh/RustPython-Parser/blob/main/parser/src/string.rs#L330-L369) the self-documented syntax above generates the same expression nodes as the expanded version, which (i think?) means we can't distinguish them. i suppose we need to actually preserve this info in the ast, e.g. by adding `self_documenting` (currently a free var in the parser) to the node or similar

---

_Referenced in [astral-sh/RustPython-Parser#33](../../astral-sh/RustPython-Parser/pulls/33.md) on 2023-07-22 07:40_

---

_Comment by @MichaReiser on 2023-07-24 14:40_

There's everyday something new to learn about Python :)

I used our [playground](https://play.ruff.rs/#N4KABGBECGA2sHsDuBTAJgWgMYIHYDMBXAZ2gCNYVjIAuMAbQF0AacKMwgS1gBdPdqdJqwiQ0hALYSAnhgBu0AE6dylDIoDmAD1pQAegAoA+gGoAPsZP1oGAF4BBDAC0ADBgCcRxgCprdx64ejCYA-ACUYQAkkCJQKFo8KLiYnBq4CIoousJskPGJyRjEKJRYPNksuflJmPicWqpZQpWi1YWEBPWNFbF5CSiKuHA9uanpmSOisPwoGJS4GjwAFroAHKu9PORFnLZNYAAsvcWl5UJsogCiMRdQAGKQbC1QW5ooPPIDxJx4upAADtIAMwARhcN1E+Fg0AA1ihVhhoLh0ls+HhBGBQBBRDJARh+JwPpkeIRBrp8HBirFRMRCP9-pliMQMOIpLIlBoMRTYFTbpBafTGcz0rhZsTSbh+BpyZSUNSoHBEEgiq9EZpEbhpDKeXK+WMMrMiPBZB0eNJ-uhtcU2ABfXpQ2HwjBkJFoQm6LHYyBLJRoHBodAYHgSf4sziZMoZLXnbFegD0wf+ENjUDjCkUCZDydjkDjAbkceISwkj1jz1EWCWKCwMKD5sD8SwKH+aNwVpQtvt0LhCI4GjIKCUHr5bRSUkIWwosywioxTE7uQdPedXF4-AxntEHG4fAE+LSBumxDODEYC8h3adOBDmSrAh+AmHOcVyDDZWws4wSEJSwwcOkSAZJgHKSEkPBcrK55QEuV4IICygaEsJ6blAKKcE26jaH8BghJwYQAMJwdICFIQAOqRxAmAY5EGPh5FhORlFhCE5FoMABw2gYGCsexNphN42ZQBI-AYHUajfHsuguFBkAwQiAyKBIxDSnQKGQBI0BaCqygLHMSSLCsdDSRAdqLpeCIAI6EAgiQbnquDTKKGBWTZVB-GgCCEFOgnqYQa6ObMLm2e5nnefKYgIFgx46RoznWcFdARV5lA+dAcgIJwmBUDOFq6DwiiEB2Jldo6CInPgT5evqmQYEMEhuTGKaQEYGlwoJohGNAxBumU7VQEYmT-NCTZ9c1dQlGg1Dyh142wGgRgBhSfngaNRh1SgRirQosCFZttxnsVZmlUGmWyJwIYZCtql8i6uDqCU0B8HIsznf8l0YgCShgVNN1IqKwH-JwHo2jJcl1ha2BVjWUqVTS+XoSe3K8jm8QoCGHwSAg4iUHOtyiGagMLKW2LllABW4Hw9UYCg22EI9gYusU2DQkyDWnuF5OU7MNNwHTiSYAGOCKI9GRzgdYCmRex0aO8iQJLD0EdGUD61dA9W4ym-WjTLPBy+U02oTrevExApN9AUtRK62qvqxUoPmfiIbTFghLadgeAzshfIvsqEjLdwMx5QVRUSyVy6vZd7u4M9FMPnZz7TF1bMoaI-yul1fz-Ggo2AkoijIJn0A54gPCwNIAB08QMlQH3-DoBv8oOZAZG2iXEI+DcFMQGRQgXiU8BVDe4JIgJ-LgSYNxpPBDTZ0xkH8TujVPM+l5wZDl4CK+Z7wo08DC-CJIofx7znCDQootewDnSIlJnbYN3AWzhn8j+jUsZ8IHInAoEgH1LHIJsJb22OoCWyHxjzSBStdHMdQEiklmGnTIFMqzFAxPlQq4VEFq3eMoPYNsqDg32JAEkQ0siYKUNg+GeDaYEIJkQo8+s+RYPqlQ2YNDmT52VHQ4+dIUoc2gJwVB90rLhlmFPSsokMjZDxlAAAQknS4WgmwtgfKNRRyjWyjQAGq8xQJcRQ+cj4NwAPIAGV9GGNGgASWMRYqRDdLjR3DHgeqFM7FGJkfySKcIeCVwMVI-a-DBEENHMIrgNVxG-nwFI5o4UNKKFrIgsCKC2boJDpLaCDsOgkEDCBVxV1MR6gPDVdMKger4IgjqGSgjLoK1khkDCSBhahjgCoVB7ZwrRMUBhb4Cw1ABQ6XyXpGh+kzGpkona3x0STC9NeMgYyuqOzeooApSNdQ5mIENV2eAgzCwDrpa8Gkg4YL5EBAYzpZDcLoGkzpDTZjdxWV+H8IlijK2mXQNZ4UZxM2KPeJ6+xPl8i6RhHgCAgxwRmaIGE6QkB3TqBfD4iCzSQqgNC5Ad1ljhkwEi6M7M+RothXMSKcBJFzQGCi82wsVTp0UJgOewtFC4pyDmTI0J-lLPehgM5R9EpEBWSgj4oLmYIFQYwllKARGZDHMsgpzLZks3abEvkOABBbApmLcKpTGgar5OkOYMxmQDi6fsOVUwDWInwIfDlKyMQYBBOFAKhr3ioCSIQjELhbndMDMULBiQKWvOtty6RmtZITngaNY8NK6VrwZVqTuSwsUYBxaNeFx4k1KGRQ3RAM5YCkoDEfQJQzqytg3CDQ6OIsAzgHHUjSWlrykK0ISXFYIZIWn+AiOqMNoFVWKbMdaGsmrFB4AAVQnp4xISgAAi6KI3vFHfhBVu9ByKGnbChdXUfohqHaOgAsljPyZDx3LtXbgPd2ND0hq6tIXAWBTFzrHZe4g16sAABVj0zobtu-4b7jyTsekXBuFJuDwPUc2TRWa8AaB3TXaAMsl6aUnZwfAFVC05iwAqlh78BbVgyCLC+-rUToUw1jFkOHhagvw80Vt0h-RUDNFAwpOZqqzE-gMRAuktjEFrIc767ZqMeSivR-YalmOkaFnhnVOYGRwQGGaMTuGKNi2ozBeOXpQky1FMoKKdty1QEBI5L2CclQYA0hodC8hdFuuDYO-Ko0yDSGCqhr0tbnTCxvSg3QIIABMcTNL3RJIMDEAA2XzWkOQYgAKyhepYkfJkXjKh1yICOkGhhYBjqXCZs6hTTnVmATLtYA1m2hADaSIZWKrAGgAAXhtI8EAFVquVdq0AA) to play with your example and it first seems that they're parsed exactly the same, but the `FormattedValue` of the shorthand notation has `conversion: Repr` whereas the "long form" has `conversion: None`. 


Now, what's a bit awkward is the range of the `FormattedValue` (and enclosing expression) because these nodes are synthesized out of nowhere (they have no corresponding representation in source). I'm leaning towards setting an empty range for `FormattedValue` as well as its enclosing range that starts at the end of `ExprConstant`. 



---

_Comment by @davidszotten on 2023-07-24 15:14_

sorry the missing conversion is my bad, i guess the expanded version should be `f"a={a!r}"` 

but my point is that when we format we need to render `{a=}` which seems hard to reconstruct from the expanded version, and instead we might want the ast to just have a `FormattedValue` with an additional or replaced annotation indicating that it's "self-documenting"

---

_Comment by @davidszotten on 2023-07-24 15:15_

ie rather than playing with the ranges of the synthesised nodes, could we not synthesise them at all?

---

_Comment by @MichaReiser on 2023-07-24 15:42_

> ie rather than playing with the ranges of the synthesized nodes, could we not synthesize them at all?

Ideally yes. I think the overall structure isn't ideal. From a linter perspective. How would you detect that `a` is used in your example if you omit the `ExprConstant`?

> but my point is that when we format we need to render {a=} which seems hard to reconstruct from the expanded version, and instead we might want the ast to just have a FormattedValue with an additional or replaced annotation indicating that it's "self-documenting"

I believe ruff uses 
https://github.com/astral-sh/RustPython-Parser/blob/7c743435f43b69847626045caac503baacb8e85c/format/src/format.rs#L276

today to reconstruct the f-string. This is not ideal.  But I'm a bit hesitant from changing the AST today without taking the RustPython 3.12 changes into account as well, and I'm only familiar with how JS represents template literal expressions.

---

_Comment by @davidszotten on 2023-07-24 15:54_

> Ideally yes. I think the overall structure isn't ideal. From a linter perspective. How would you detect that `a` is used in your example if you omit the `ExprConstant`?

`a` would be inside an `ExprName` inside the `FormattedValue`. i'm only suggesting omitting the synthesized extra `"a="` constant which isn't "using `a`", that's just a string


---

_Comment by @MichaReiser on 2023-07-25 07:35_

So what you're suggesting is the following

```patch
Index: Clipboard.txt
IDEA additional info:
Subsystem: com.intellij.openapi.diff.impl.patch.CharsetEP
<+>UTF-8
===================================================================
diff --git a/Clipboard.txt b/../../Library/Application Support/JetBrains/CLion2023.1/scratches/scratch_1.txt
rename from Clipboard.txt
rename to ../../Library/Application Support/JetBrains/CLion2023.1/scratches/scratch_1.txt
--- a/Clipboard.txt	
+++ b/../../Library/Application Support/JetBrains/CLion2023.1/scratches/scratch_1.txt	(date 1681077307376)
@@ -2,15 +2,6 @@
     ExprJoinedStr {
         range: 0..7,
         values: [
-            Constant(
-                ExprConstant {
-                    range: 0..7,
-                    value: Str(
-                        "a=",
-                    ),
-                    kind: None,
-                },
-            ),
             FormattedValue(
                 ExprFormattedValue {
                     range: 0..7,

```

But only for `f"{a=}"`. The representation of `f"a={a}"` and `f"a={a!r}"` would remain unchanged. 

I would like that! If that's the case. Could we fix (if not already done) the ranges of the non-synthesized nodes in your PR and we can then tackle the removal of the node as separate PR (we may need to review if some of ruff's rule depend on the presence of the node). CC: @charliermarsh 

---

_Comment by @davidszotten on 2023-07-25 07:47_

yes, that's the suggestion (sorry if i was being unclear). in addition i expect we'll need some extra metadata (on the `ExprFormattedValue` that contains the `ExprName(a)`) to capture the fact that there is an `=`, currently called `self_documenting` in the code but not attached to any node. not sure yet how to best model that

---

_Comment by @davidszotten on 2023-07-25 08:12_

> Could we fix (if not already done) the ranges of the non-synthesized nodes in your PR

i think they were already correct when you last reviewed. probably the synthesized ones were confusing things. do you have a preference for what to do with the ranges for the synthesized ones for now? just leave (as the same range as the formattedvalue we want to keep)?

---

_Comment by @MichaReiser on 2023-07-25 09:09_

> i think they were already correct when you last reviewed. probably the synthesized ones were confusing things. do you have a preference for what to do with the ranges for the synthesized ones for now? just leave (as the same range as the formattedvalue we want to keep)?

I would leave them as is, considering that we intend to remove them entirely. The ranges could mess up the comment placement but this is not a problem in this very specific occasion because, from what I understood, comments aren't allowed in f-strings anyway.


I plan to think about your proposal to add an extra flag later today or do you want to work out a more detailed proposal first?

---

_Comment by @davidszotten on 2023-07-25 09:19_

please go for it. could do with someone who a) has a broader knowledge of the project and b) can dedicate longer chunks of time to thinking about implications

---

_Comment by @MichaReiser on 2023-07-25 16:45_

I spend some more time looking into this and are concluding that omitting the `Constant` for `f"{a=}"` complicates things, because it becomes unclear whether the `FormattedValue` should print the `{a=` or not. 

Let's say we have the following input


```python
f"{a=}"
f"{a=!r}"
```

The proposed AST is:

```
StmtExpr {
    range: 0..7,
    value: JoinedStr(
        ExprJoinedStr {
            range: 0..7,
            values: [
                FormattedValue(
                    ExprFormattedValue {
                        range: 0..7,
                        value: Name(
                            ExprName {
                                range: 3..4,
                                id: "a",
                                ctx: Load,
                            },
                        ),
                        conversion: Repr,
                        format_spec: None,
                    },
                ),
            ],
        },
    ),
},
StmtExpr {
    range: 8..17,
    value: JoinedStr(
        ExprJoinedStr {
            range: 8..17,
            values: [
                Constant(
                    ExprConstant {
                        range: 8..17,
                        value: Str(
                            "a=",
                        ),
                        kind: None,
                    },
                ),
                FormattedValue(
                    ExprFormattedValue {
                        range: 8..17,
                        value: Name(
                            ExprName {
                                range: 11..12,
                                id: "a",
                                ctx: Load,
                            },
                        ),
                        conversion: Repr,
                        format_spec: None,
                    },
                ),
            ],
        },
    ),
},
```

Formatting the first expression's `FormattedValue` requires printing the `a=,` but the formatting of the second expression's `FormattedValue` should only print `a`, because there's a preceding `Constant` that prints the `a=`. That means it is now necessary to look at a sequence of nodes. Furthermore, `a={a=}` is ambiguous because the parsed AST is `Constant a=` followed by a `FormattedValue`, but they represent two different expressions (with different results). 

There's another problem where `f"{today=               }"` is a valid use of a self-documenting formatting string, but it must preserve all whitespace. That means we can't just omit the `Constant expression`. 

That's why I think that keeping the representation as is, at least for now, results in a consistent structure. 

But I think we should remove the merging of constants to make formatting easier:

Remove the automatic merging of `ExprConstant`: RustPython creates a single `ExprConstant` for `f"a={b=}"` that contains `a=b=`. This makes sense from an interpreter perspective (it allows for very fast printing) but messes up formatting because we need to place `a=` before the parentheses and `b=` inside them. 

Regarding the initial ask to add a new field to `FormattedValue`. I'm not entirely sure how to design this yet. We could change `FormatSpec` to either be an expression or `SelfDocument` variant because (from what I understand), it is only valid to have either, add a boolean field, or something else. 

I'm leaning towards not changing the AST for now and instead doing some lightweight parsing inside the formatter. It should be sufficient to test whether `=}` (ignoring whitespace) follows right after the `ExprName`. 


@davidszotten what do you think? Do you see use cases where testing for `=}` wouldn't be sufficient? 


---

_Comment by @davidszotten on 2023-07-25 19:15_

> because it becomes unclear whether the FormattedValue should print the {a= or not.

well the idea was that the `SelfDocument` attribute would indicate this.

but i'm also interested in your idea to not merge the constants, so let me explore that a bit. did you have a chance to think at all about what ranges to use on the synthesized constants?

---

_Comment by @MichaReiser on 2023-07-26 09:30_

> well the idea was that the SelfDocument attribute would indicate this.

I see. I think it would still be problematic in the case of `f"{today=               }"` because the space comes between `today=`  (which would then be part of the `FormattedValue`) and the expression value (again, the `FormattedValue`)

> did you have a chance to think at all about what ranges to use on the synthesized constants?

That's a really good question and something I overlooked yesterday evening. The ranges are awkward, indeed. Ranges, ideally, map to the node's representation in the source code, and the ranges of siblings should never intersect. Satisfying both constraints seems impossible. 

So I ended up playing more with f-strings and found more interesting cases:

```python
f"{a=    }"
f"{               a             =           }"
f"{a=}"

f"a={a}"

# Valid and preserve whitespace
f"{a       =                             !r}"
f"{a     +    1       =                             !r}"
f"{    a =   :#x}"

# Valid, discard whitespace
f"{    a    }"

# Valid with Python 3.12 (intentional?)
#f"{a=!r    }"
```

The most important findings is that whitespace is meaningful in debug expressions. 

> **Formatting**: I might be wrong but black doesn't seem to format anything inside of `f-strings`. Which makes sense (at least for Python 3.11) because the expressions aren't allowed to break (See [`RemoveSoftLineBuffer`](https://github.com/rome/tools/blob/1dcd62b169e875b7048f63efb5f6636d2152334c/crates/rome_js_formatter/src/utils/test_each_template.rs#L239-L251) on how these could be implemented).

I've come up with a few possible representations that seems reasonable for an AST used for static analsis is (that isn't used in an interpreter, Python's representation makes a ton of sense for interpreters):

### `is_debug_expression` field

* Extend `FormattedValue` with a `is_debug_expression`  as you suggested
* Only create `Constant`s for text that is not part of a `FormattedValue`. 

I think this might be what you initially suggested. 

The downside of this representation is that unparsing is awkward because it requires poking into the source content to preserve whitespace. Ruff already fails terribly at this today: `f"{               a             =           }"` becomes `f"               a             =           {a!r}"` which preserves the semantic meaning but is not what the user wrote. 

### `debug_text` field

```rust
pub struct FormattedValue {
	debug_text: Option<String>, // Stores whatever text comes before the conversion / format_spec or }
	expression: Box<Expr>,
	conversion: Conversion,
	format_spec: Option<Box<Expr>>,
}
```

The upside of this is that it allows easy inspection of the debug text. The main downside is that the `expression` is represented twice, allowing for inconsistency (what should `unparse` serialize if the `expression` in the debug text doesn't match the expression field?

### CST
This is a case where a CST would be nice where each node stores leading/trailing trivia (whitespace). Too bad we don't have a CST. 

### Disccard options

I thought about preserving the individual whitespace but that feels awkward because it requires preserving the whitespace between `{` and the expression, after the expression, and after the `=` sign. 


### Recommendation
I'm leaning towards adding a `debug_text` field because it exposes all relevant information, even if it is at the cost of redundant, and possibly inconsistent, data. 

## Other changes

I recommend removing `FormattedValue` from `Expr` and instead change `JoinedString::values` to be a `JoinedStringPart` where each part is either a `String` (with a range) or `FormattedValue`. 





---

_Label `parser` added by @MichaReiser on 2023-07-26 09:32_

---

_Comment by @konstin on 2023-07-26 10:54_

Could `debug_text` save just a range since we don't want to apply any transformations? If possible this would be my preferred solution

---

_Comment by @MichaReiser on 2023-07-26 13:01_

> Could `debug_text` save just a range since we don't want to apply any transformations? If possible this would be my preferred solution

Saving the range of the debug text is possible. My main concern with this is that our current AST tries to be useful without the source text. Meaning, you shouldn't have to use the source text to retrieve any content (you only need it to retrieve anything that's not part of the AST).

---

_Comment by @MichaReiser on 2023-07-26 18:13_

A fourth option (the third is the option @konstin proposed that uses a range) that I personally prefer over the others that I suggested is:

```rust
struct FormattedValue {
	expression: Expr,
	debug_text: Option<DebugText>,
	conversion: Conversion,
	format_spec: Option<Expr>
}

struct DebugText {
	/// The text between the `{` and the expression node. 
	leading: String,
	/// The text between the expression and the conversion, the format_spec, or the `}`, depending on what's present in the source
	trailing: String
}
```



---

_Comment by @davidszotten on 2023-07-27 12:20_

do i understand you correctly that (in option 4), `debug_text` being `Some()`  vs `None` corresponds to being self-documenting vs not?

---

_Comment by @MichaReiser on 2023-07-27 13:43_

> do i understand you correctly that (in option 4), `debug_text` being `Some()` vs `None` corresponds to being self-documenting vs not?

Yes, that's correct. And the `leading` / `trailing` give you the text (e.g. for `unparse`)

---

_Comment by @davidszotten on 2023-07-27 14:38_

👍 

and just to make sure, changing/adding some nodes is expected to require a bunch of boilerplate changes in `ruff_python_ast`, e.g. visitor, comparable etc?

---

_Comment by @MichaReiser on 2023-07-27 14:41_

Yes. It will require changes to visitors, comparable, but it may also impact `ruff_python_literal` (because I believe it does `FString parsing, but maybe not?), and rules that run on `JoinedString`/`FormattedValue`.

---

_Comment by @davidszotten on 2023-07-27 15:13_

ok, will see if i can wrap my head around all that.

before i go too far, is the suggestion this?

```rust
#[derive(Clone, Debug, PartialEq)]
pub struct ExprJoinedStr {
    pub range: TextRange,
    // TODO: rename to `parts`?
    pub values: Vec<JoinedStringPart>,
}

#[derive(Clone, Debug, PartialEq)]
pub enum JoinedStringPart {
    // TODO: i'm guessing not quite this? Another struct? (name?) for the string case?
    String { range: TextRange, value: String },
    FormattedValue(FormattedValue),
}


#[derive(Clone, Debug, PartialEq)]
pub struct FormattedValue {
    pub range: TextRange,
    pub expression: Box<Expr>,
    pub debug_text: Option<DebugText>,
    pub conversion: ConversionFlag,
    pub format_spec: Option<Box<Expr>>,
}

```

---

_Comment by @MichaReiser on 2023-07-27 15:46_

Yes, that's what I had in mind. An inner struct for `String` would be good because it allows for easier passing (and implement the `Node` interfaces. We could also consider using `Constant`, even if all other constants aren't allowed (fewer visitor functions). 

---

_Comment by @davidszotten on 2023-07-27 15:55_

> An inner struct for String would be good 

👍 . any ideas for a good name?

>  using `Constant`

attractive since i'm the one writing all the boilerplate, though in general i prefer encoding as much as possible in the type system (i presume using `Constant` would mean `unreachable!()` or similar in the formatter etc). will have a play

---

_Comment by @MichaReiser on 2023-07-28 17:36_

@davidszotten I don't know if you already started and you are probably in a better situation to assess the amount of work.

I think it's worth considering doing this change in two steps:

1) Add the `debug_text` to `FormattedValue` and no longer generate the `ExprConstant` for the debug expression part 
2) Introduced `JoinedStringPart`

I'm mainly worried that you run into many downstream changes and landing 1 on its own would unblock the formatter work.

Edit: Are you planning on working on this? I would then assign the issue to you

---

_Comment by @davidszotten on 2023-07-28 18:49_

hi. i've started implementing the `JoinedStringPart` etc refactor we discussed above, yes.  i may still get stuck but haven't yet. 

in the mean time, maybe you'd be able to have a look at https://github.com/astral-sh/ruff/pull/5932#discussion_r1276083253 ? you might be better placed than me to implement that (and that's also a blocker for f-strings i think

---

_Assigned to @davidszotten by @MichaReiser on 2023-07-29 07:52_

---

_Comment by @MichaReiser on 2023-07-29 07:57_

> hi. i've started implementing the JoinedStringPart etc refactor we discussed above, yes. i may still get stuck but haven't yet.

Cool.  I'll assign this issue to you then.  Feel free to exclude the `JoinedStringPart` refactor at any point if things start to get out of hand because of it.

Regarding the comment. I'm sorry not to have replied yet. I'm a bit fin stretched at the moment and making progress here seemed more important. It also isn't the case that I have a good answer, I would need to tinker with different options too. 

We just did the monthly planning internally, and f-string formatting is one of our goals for the next four weeks. I'll check in with @konstin to decide who will support or implement f-string formatting. What you be interested in working on the f-string formatting after the parser changes or would you prefer if someone else works on (there's also other formatting work that we want to make progress on, e.g. match statements are an important one)

---

_Referenced in [astral-sh/ruff#6167](../../astral-sh/ruff/pulls/6167.md) on 2023-07-29 08:08_

---

_Comment by @davidszotten on 2023-07-29 08:12_

Hi. I finally had a bit more time to work on this and thus got to the part where `JoinedStringPart` was causing more issues. Need to think more about the return values / refactor much more of the parser in `strings.rs`. So I decided to try the simple approach of `DebugText` only. I think I got that working in #6167

No worries about the other comment, I just wanted to make sure it was on your radar. As for timings and progress (f-strings in general) I know i'm moving much slower than you since it's spare-time only. That seemed ok while there were a bunch of other things also left (eg match statements) but feel free to take over if this becomes the limiting factor (but please let me know so i can stop working on it)

apart from this issue, making sure quote switching doesn't introduce backslashes inside `FormattedValue`s (ie our other discussion) seems like one of the tricky bits on the way to f-strings

---

_Comment by @MichaReiser on 2023-07-29 08:45_

Oh wow, you already have a working implementation. This is awesome! I'll review it on Monday.

> I know i'm moving much slower than you since it's spare-time only. 

Not at all. Our plan is to get f-string formatting done in the next four weeks. You can say that you're interested in tackling it without any commitment and we can check in half time how it is going. That's why I want to leave this up to you. You know best how much time you can and want to invest. 

---

_Comment by @davidszotten on 2023-07-29 09:20_

> We just did the monthly planning internally

is this publicly available? (or could you share some high level details?)

---

_Comment by @MichaReiser on 2023-07-29 09:52_

> is this publicly available? (or could you share some high level details?)

I plan to create an umbrella issue for the formatter on Monday

---

_Closed by @MichaReiser on 2023-08-01 05:55_

---

_Referenced in [astral-sh/ruff#6365](../../astral-sh/ruff/pulls/6365.md) on 2023-08-05 13:34_

---
