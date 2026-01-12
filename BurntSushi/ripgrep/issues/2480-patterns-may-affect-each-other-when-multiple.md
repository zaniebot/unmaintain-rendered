```yaml
number: 2480
title: Patterns may affect each other when multiple patterns are provided
type: issue
state: closed
author: gofri
labels:
  - bug
assignees: []
created_at: 2023-03-29T15:04:23Z
updated_at: 2023-07-08T22:52:49Z
url: https://github.com/BurntSushi/ripgrep/issues/2480
synced_at: 2026-01-12T16:13:24Z
```

# Patterns may affect each other when multiple patterns are provided

---

_@gofri_

#### What version of ripgrep are you using?
13.0.0

#### How did you install ripgrep?
brew 

#### What operating system are you using ripgrep on?
MacOS M1

#### Describe your bug.
When providing multiple patterns via either `-e` or `-f`,
specified flags affect all following patterns. 

#### What are the steps to reproduce the behavior?
```$ echo 'MyText' | rg -o -e '(?i)notintext' -e 'text'
Text
```

#### What is the actual behavior?
rg respect the case-insensitive flag `(?i)` from the first pattern in the second pattern as well.
```$ echo 'MyText' | rg -o --debug -e '(?i)notintext' -e 'text'
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
Text
```

Same goes for:
`echo 'MyText' | rg -f <(printf '(?i)notintext\ntext')`


#### What is the expected behavior?
Empty result (the case-insensitive flag should not affect the second pattern).

#### What do you think ripgrep should have done?
Treat each pattern independently.
Specifically, for these pattern, I'd expect the merged pattern to be:
`'(?:(?i)notintext')|(?:'text')`
rather than:
`'(?i)notintext'|'text'`

#### Side notes:
1. Works as expected if I manually wrap each pattern as `(?:PATTERN)`
2. Works as expected if I replace the first pattern with `(?i:notintext)`
3. Works as expected if I replace the order of the patterns (because the effect of the flag only applies "to the right")

---

_Comment by @BurntSushi on 2023-03-29 15:11_

Yeah so you are indeed correct that ripgrep just does a simple join with `|` as the delimiter: https://github.com/BurntSushi/ripgrep/blob/041544853c86dde91c49983e5ddd0aa799bd2831/crates/core/args.rs#L703

ripgrep could make the situation much better by wrapping each pattern in a `(?:<pattern>)`. It would resolve your specific problem for example. And this would be easy to do I think. And it would work for PCRE2. So we should do that.

But it doesn't quite make things completely independent. Because you could still do things like `-e foo)(?i)bar(` to sneakily insert a `(?i)` (for example). Fixing _that_ probably requires trying to parse each pattern individually and ensuring each pattern is itself valid. A little more work, but not a huge deal to do. However, that won't work for PCRE2 unless you go and actually try to compile every pattern individually. (Because PCRE2 doesn't expose its own parser.) That is potentially a lot more work. Another possibility is to just write a simple little mini parser that just makes sure parentheses are balanced. I think you'd just have to account for escapes and character classes, which should be... somewhat simple?

---

_Label `bug` added by @BurntSushi on 2023-03-29 15:11_

---

_Comment by @gofri on 2023-04-11 10:01_

I wrote the following code that should act as the mini parser you mentioned.
If it looks legit to you (or somewhat legit) I can try to create a PR to use it there (with testing, documentation, etc.).

The guiding principles:
1. we only try to detect cases where invalid balance would affect later patterns, and may result in a valid joined pattern constructed from invalid patterns.
2. assume that any other pattern invalidity would fail the validity check of the combined pattern (i.e., including negative balance, e.g. `ab)c` / `ab]c`)
O(n); optimize for valid patterns (and simplicity); do not try to fail early for invalid patterns.

p.s. the code here is just for sanity check and we can discuss details in the PR, shall there be one.
p.s.2. The code for the kind/error is trivial so I left it out for this comment.

```
fn validate_subpattern_safety(pattern: &str) -> Result<(), BalanceError> {
    let (mut escape, mut round_depth, mut square_depth) = (false, 0, 0);

    for c in pattern.chars() {
        if escape {
            // escape takes precedence over all states; render the current char literal
            escape = false;
        } else if c == '\\' {
            // escape takes precedence over all states; move to escape state
            escape = true;
        } else if square_depth > 0 {
            // character classes take precedence over round brackets
            match c {
                '[' => square_depth += 1,
                ']' => square_depth -= 1,
                _ => {} // any char is literal inside character classes (balance-wise)
            }
        } else if round_depth > 0 && c == ')' {
            // only acconut for round bracket closures for balance correctness
            round_depth -= 1;
        } else {
            // the ground state: move to a higher state or ignore
            match c {
                '(' => round_depth += 1,
                '[' => square_depth += 1,
                _ => {} // ignore literal chars and bracket closers
            };
        }
    }

    let kind = if escape {
        Some(ErrUnbalanceKind::TrailingBackslash)
    } else if square_depth > 0 {
        Some(ErrUnbalanceKind::UnbalancedSquareBrackets)
    } else if round_depth > 0 {
        Some(ErrUnbalanceKind::UnbalanceRoundBrackets)
    } else {
        None
    };

    match kind {
        Some(kind) => BalanceError::new(kind, pattern.to_string()),
        None => Ok(()),
    }
}

fn join_patterns_safely(patterns: &[String]) -> Result<String, BalanceError> {
    match patterns.len() {
        0 => Ok("".to_string()),
        1 => Ok(patterns.first().unwrap().to_owned()),
        _ => patterns // note: can apply to all but the last
            .iter()
            .map(|p| {
                validate_subpattern_safety(p)?;
                Ok(format!("(?:{})", p))
            })
            .collect::<Result<Vec<String>, BalanceError>>()
            .map(|ps| ps.join("|")),
    }
}
```

---

_Comment by @BurntSushi on 2023-04-11 11:10_

Thanks for the effort! Unfortunately, it's just not _quite_ right. It doesn't account for the fact that `[[]` and `[\[]` are equivalent, for example. Honestly, there's probably other stuff. And especially so for PCRE2, which has a fair bit more syntax than the default engine.

After thinking on it some more, I think for now, I'd rather just add the `(?:<pattern>)` wrapping and call that good enough. If folks end up running into problems there, then we can re-evaluate later. But I might just end up marking it as `wontfix`.

Also, one small note: the perf for something like a routine for what you wrote is basically insignificant. Consider how much work a regex engine needs to do to compile a regex into a matcher. It's _a lot_ more work than a simple parser like that. :-)

---

_Comment by @gofri on 2023-04-11 11:36_

Thanks for the quick response!

> It doesn't account for the fact that [[] and [\[] are equivalent, for example.

I actually tested for unescaped brackets inside char-class and got a parse error:
```
echo '\[' | rg '[[]'
regex parse error:
    [[]
     ^^
error: unclosed character class
```


But I tested now with `--engine pcre2` and it does work there, as you said.

I looked for other cases in PCRE2 now and didn't find a particular problem.
Specifically, since names can't include curly/round brackets, we can ignore `<>` because it's within the constraints (i.e. an invalid pattern can't lead to a valid combined pattern).

I feel you on going with the `(?:<pattern>)` wrapping, but I think that fixing the `[[]` issue shouldn't be too hard at this point.
So, let me know if you'd consider merging it if I fixed that part (or otherwise, if you can think of other uncovered cases that I missed, and I'll just quit the attempt anyway)

---

_Comment by @BurntSushi on 2023-04-11 11:42_

Whoops, I meant that `[]]` and `[\]]` are equivalent.

For now, I'd rather go with the `(?:pattern)` approach and just stop there. It's simple and fixes the most pressing problem.

---

_Closed by @BurntSushi on 2023-07-08 22:52_

---
