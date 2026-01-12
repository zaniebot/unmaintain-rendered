```yaml
number: 1594
title: "Pyupgrade: Format specifiers"
type: pull_request
state: merged
author: colin99d
labels: []
assignees: []
merged: true
base: main
head: formatspecifiers
created_at: 2023-01-03T12:09:58Z
updated_at: 2023-01-11T01:50:06Z
url: https://github.com/astral-sh/ruff/pull/1594
synced_at: 2026-01-12T05:36:31Z
```

# Pyupgrade: Format specifiers

---

_Pull request opened by @colin99d on 2023-01-03 12:09_

A part of #827. Posting this for visibility. Still has some work to do to be done.

Things that still need done before this is ready:

- [x] Does not work when the item is being assigned to a variable
- [x] Does not work if being used in a function call
- [x] Fix incorrectly removed calls in the function
- [x] Has not been tested with pyupgrade negative test cases

Tests from pyupgrade can be seen here: https://github.com/asottile/pyupgrade/blob/main/tests/features/format_literals_test.py

---

_Comment by @colin99d on 2023-01-04 12:47_

I will try to have this done by Sunday night.

---

_@charliermarsh reviewed on 2023-01-05 02:10_

---

_Review comment by @charliermarsh on `src/pyupgrade/plugins/format_specififiers.rs`:17 on 2023-01-05 02:10_

We use `Lazy` for this, you can grep for some other usages of `Regex` to see an example :)

---

_@colin99d reviewed on 2023-01-05 02:11_

---

_Review comment by @colin99d on `src/pyupgrade/plugins/format_specififiers.rs`:17 on 2023-01-05 02:11_

That was FAST. But thanks.

---

_Comment by @colin99d on 2023-01-06 22:19_

@charliermarsh I have the following function to detect expressions:
```
pub fn match_expression(expression_text: &str) -> Result<Expression> {
    match libcst_native::parse_expression(expression_text) {
        Ok(expression) => Ok(expression),
        Err(_) => bail!("Failed to extract CST from source"),
    }
}
```
It works fine for an expression like this:
```
    'foo{0}' 'bar{1}'.format(1, 2)
```
But the second you make it multi-line like so:
```
    'foo{0}'\n    'bar{1}'.format(1, 2)
```
The whole thing fails. Is this an error with my coding, or an error with libcst_native?

---

_Comment by @charliermarsh on 2023-01-06 22:39_

I'm wondering if you need to dedent the code. You might be passing `LibCST` the indented expression, which would cause an error.

---

_Comment by @colin99d on 2023-01-06 22:50_

It looks like the issue still happens with the de-indented code: 'foo{0}'\n'bar{1}'.format(1, 2).

---

_Comment by @colin99d on 2023-01-07 03:58_

thread 'main' panicked at 'called `Result::unwrap()` on an `Err` value: ParserError(ParseError { location: ParseLoc { start_pos: LineCol { line: 2, column: 4, offset: 13 }, end_pos: LineCol { line: 2, column: 12, offset: 21 } }, expected: ExpectedSet { expected: {"EOF"} } }, "'foo{0}'\n    'bar{1}'.format(1, 2)")', src/pyupgrade/plugins/format_specififiers.rs:57:57

Looks like this is a parse error from the `parse_expression` function in libcst_native. My thoughts are we either:
1. Make a custom patch in your fork
2. Find a way to modify the string to add a `\` at the end of each line so the parser can understand it, and then remove it once we are done
3. Don't use libcst_native ( I dont like this one because I like libcst now)

Just let me know which you would like for me to pursue.

---

_Comment by @charliermarsh on 2023-01-07 23:11_

Would you be open to filing an issue on the LibCST repo? In the meantime, we could just skip those cases. (Are they needed for detection, or just autofixing?)

---

_Comment by @colin99d on 2023-01-08 23:54_

Here is the issue: 
https://github.com/Instagram/LibCST/issues/846
I will just make it a check for now!

---

_Review comment by @charliermarsh on `src/pyupgrade/plugins/format_specififiers.rs`:159 on 2023-01-09 01:04_

Thank you and sorry that this all churned mid-PR for you.

---

_@charliermarsh reviewed on 2023-01-09 01:05_

---

_@colin99d reviewed on 2023-01-09 01:32_

---

_Review comment by @colin99d on `src/pyupgrade/plugins/format_specififiers.rs`:159 on 2023-01-09 01:32_

Im never going to complain about you making this project easier to contribute to.

---

_Comment by @colin99d on 2023-01-09 03:07_

Hey @charliermarsh I got all the positive test cases working (except for the one edge case we talked about), and all the negative cases working except one. I wanted to get your feedback before I implemented it.
The following edge case is what causes issues:
```
'{' '0}'.format(1)
```
Since the format specifier is split into two different strings, it is NOT supposed to be formatted. However, both the Expr
 struct, and the tokens automatically combine this into one string. What would be the best way of detecting that this edge case is preset? Or do we want to ignore it? Since the refactoring would produce changes that could be seen as desirable:
<img width="530" alt="Screenshot 2023-01-08 at 10 07 52 PM" src="https://user-images.githubusercontent.com/72827203/211235188-239290c0-2283-40cb-80fb-9aab0eabdcc3.png">



---

_Comment by @charliermarsh on 2023-01-09 06:13_

I think it's best just to ignore it (i.e., treat it as if there is no space). It strikes me as exceptionally rare, to the point that it's likely either a mistake _or_ that it's not worth adding more complexity to our own implementation to handle it.

---

_Comment by @charliermarsh on 2023-01-09 06:13_

We could _probably_ detect it by tokenizing... but it doesn't seem worth it to me.

---

_Comment by @colin99d on 2023-01-09 12:14_

> I think it's best just to ignore it (i.e., treat it as if there is no space). It strikes me as exceptionally rare, to the point that it's likely either a mistake _or_ that it's not worth adding more complexity to our own implementation to handle it.

100% agree, especially since the python code will still work.

---

_Comment by @colin99d on 2023-01-09 12:25_

I just need to fix one bug with this edge case, and then I should be able to take this PR out of draft.

---

_Marked ready for review by @colin99d on 2023-01-09 15:59_

---

_Comment by @colin99d on 2023-01-10 21:35_

@charliermarsh anything else you want to see from this for merge?

---

_Comment by @charliermarsh on 2023-01-10 21:50_

@colin99d - Will review this tonight!

---

_@charliermarsh reviewed on 2023-01-11 00:18_

---

_Review comment by @charliermarsh on `src/pyupgrade/rules/format_specifiers.rs`:97 on 2023-01-11 00:18_

When does this happen?

---

_@charliermarsh reviewed on 2023-01-11 00:35_

---

_Review comment by @charliermarsh on `src/pyupgrade/rules/format_specifiers.rs`:97 on 2023-01-11 00:35_

Ah nevermind.

---

_@charliermarsh reviewed on 2023-01-11 01:18_

---

_Review comment by @charliermarsh on `src/pyupgrade/rules/format_literals.rs`:85 on 2023-01-11 01:18_

@colin99d - It turns out that we already had a utility for extracting the format positions, which uses the RustPython string parser underneath and so is very robust. Sorry that I didn't flag this earlier -- I didn't realize it could even be reused here, but it should make things more efficient too since we can do one string parse and share that "summary" amongst a bunch of checks.

---

_Review comment by @charliermarsh on `src/pyupgrade/rules/format_literals.rs`:114 on 2023-01-11 01:19_

I feel like one strategy that could work here would be...

- Take the entire text.
- Replace the string part via the regex above.
- Replace the arguments via LibCST.

Kinda tough, hacky, etc... but would solve the missing cases.


---

_@charliermarsh reviewed on 2023-01-11 01:19_

---

_@charliermarsh reviewed on 2023-01-11 01:20_

---

_Review comment by @charliermarsh on `src/pyupgrade/rules/format_literals.rs`:114 on 2023-01-11 01:20_

Actually, I guess that wouldn't solve the `'{' '0}'.format(1)` case.

---

_Merged by @charliermarsh on 2023-01-11 01:21_

---

_Closed by @charliermarsh on 2023-01-11 01:21_

---

_Comment by @colin99d on 2023-01-11 01:50_

Thanks for the review!

---

_Branch deleted on 2023-01-11 01:50_

---
