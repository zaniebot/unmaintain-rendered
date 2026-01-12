```yaml
number: 5900
title: "Implement pylint's `empty-comment` rule (`PLR2044`)"
type: pull_request
state: closed
author: jelly
labels:
  - rule
assignees: []
base: main
head: pylint-empty-comment
created_at: 2023-07-19T20:45:31Z
updated_at: 2023-11-30T22:23:50Z
url: https://github.com/astral-sh/ruff/pull/5900
synced_at: 2026-01-12T15:55:19Z
```

# Implement pylint's `empty-comment` rule (`PLR2044`)

---

_@jelly_

Implement empty-comment
https://pylint.pycqa.org/en/latest/user_guide/messages/refactor/empty-comment.html

Issue #970

Note, I've written some small programs in Rust but don't call myself experienced :)
Last note, the `--fix` will leave trailing spaces, but I guess that's fine as it might be tricky to determine what more to remove? Or should the code be a bit smarter and `rtrim()` after dropping the trailing `#`?

## Summary

Implement pylint's `empty-comment` rule.

Pylint implementation https://github.com/pylint-dev/pylint/blob/c4281bcff86b66fdf8518e7f57dc3405c8da3a4f/pylint/extensions/empty_comment.py#L43

## Test Plan

Manually and by running the unit test from https://github.com/pylint-dev/pylint/blob/c4281bcff86b66fdf8518e7f57dc3405c8da3a4f/tests/functional/ext/empty_comment/empty_comment.py#L4

---

_Comment by @github-actions[bot] on 2023-07-19 21:36_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.07     12.3±0.64ms     3.3 MB/sec    1.00     11.5±0.71ms     3.5 MB/sec
formatter/numpy/ctypeslib.py               1.08      2.4±0.10ms     6.9 MB/sec    1.00      2.2±0.08ms     7.5 MB/sec
formatter/numpy/globals.py                 1.00   267.2±13.48µs    11.0 MB/sec    1.00   266.2±14.09µs    11.1 MB/sec
formatter/pydantic/types.py                1.04      5.2±0.26ms     4.9 MB/sec    1.00      5.0±0.15ms     5.1 MB/sec
linter/all-rules/large/dataset.py          1.04     17.5±0.99ms     2.3 MB/sec    1.00     16.9±0.83ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.10      4.4±0.28ms     3.8 MB/sec    1.00      3.9±0.11ms     4.2 MB/sec
linter/all-rules/numpy/globals.py          1.13   578.8±44.42µs     5.1 MB/sec    1.00   512.2±29.32µs     5.8 MB/sec
linter/all-rules/pydantic/types.py         1.09      7.9±0.47ms     3.2 MB/sec    1.00      7.3±0.25ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.09      8.8±0.40ms     4.6 MB/sec    1.00      8.1±0.31ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.05  1867.0±95.64µs     8.9 MB/sec    1.00  1782.5±94.20µs     9.3 MB/sec
linter/default-rules/numpy/globals.py      1.03   218.8±12.50µs    13.5 MB/sec    1.00   213.0±14.95µs    13.9 MB/sec
linter/default-rules/pydantic/types.py     1.05      4.0±0.38ms     6.4 MB/sec    1.00      3.8±0.20ms     6.8 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     11.1±0.24ms     3.7 MB/sec    1.00     11.2±0.16ms     3.6 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.2±0.06ms     7.6 MB/sec    1.00      2.2±0.06ms     7.6 MB/sec
formatter/numpy/globals.py                 1.00    246.4±8.95µs    12.0 MB/sec    1.04   255.5±16.78µs    11.5 MB/sec
formatter/pydantic/types.py                1.00      4.7±0.16ms     5.4 MB/sec    1.03      4.9±0.16ms     5.2 MB/sec
linter/all-rules/large/dataset.py          1.00     15.4±0.20ms     2.6 MB/sec    1.03     15.9±0.41ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.07ms     4.1 MB/sec    1.01      4.1±0.05ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    490.3±7.16µs     6.0 MB/sec    1.02   500.2±10.72µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.0±0.17ms     3.6 MB/sec    1.03      7.2±0.16ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.00      8.1±0.11ms     5.0 MB/sec    1.01      8.2±0.12ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1676.9±31.40µs     9.9 MB/sec    1.01  1693.3±23.28µs     9.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    189.7±4.81µs    15.6 MB/sec    1.04   196.6±12.24µs    15.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.05ms     7.1 MB/sec    1.01      3.6±0.06ms     7.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @zanieb on 2023-07-19 23:02_

Welcome! Thanks for contributing :)

> This PR is still draft as locally running cargo dev generate-all add the new PLR2044 rule, but also removes RUF014 which does not seem expected.

This should be resolved by #5832 — sorry about that!

---

_Marked ready for review by @jelly on 2023-07-20 15:28_

---

_Label `rule` added by @charliermarsh on 2023-07-20 16:38_

---

_Comment by @charliermarsh on 2023-07-20 16:42_

Haven't had a chance to dig in yet but one thing to be mindful of is that this rule is part of a Pylint extension and so isn't enabled by default. We typically mark these as "nursery" as a hack to require explicit opt-in.

---

_Comment by @jelly on 2023-07-20 18:14_

> Haven't had a chance to dig in yet but one thing to be mindful of is that this rule is part of a Pylint extension and so isn't enabled by default. We typically mark these as "nursery" as a hack to require explicit opt-in.

Ah that makes sense, I just picked the easiest rule to implement. I've queued it up locally as I suppose  there will be more things to address :) 

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pylint/rules/empty_comment.rs`:58 on 2023-07-25 06:58_

I'm not entirely sure if I understand this code correctly but I think this can go wrong for multiline strings

```python
a = """A multiline string
that goes contains a # that looks like a comment""" #
```

There's no good way for you to detect this by just looking at the source (the same can happen for single quoted strings). 

From what I understand you're trying to achieve is to see if the comment is all empty. You can do this by using `trim_end_matches(|c| is_python_whitespace(c) || c == '#')`. This should trim all whitespace and `#` from the comment text. The comment is empty if the resulting string is `""`, and the comment is non empty if there's some text remaining.

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pylint/snapshots/ruff__rules__pylint__tests__PLR2044_empty_comment_0.py.snap`:75 on 2023-07-25 06:59_

Would you mind adding a. test with a multiline string?

---

_@MichaReiser requested changes on 2023-07-25 06:59_

Thanks for your contribution. I believe there's an issue with `#` in multiline strings that we need to address. See my inline comments for a more in-depth explanation.

---

_@jelly reviewed on 2023-07-25 17:20_

---

_Review comment by @jelly on `crates/ruff/src/rules/pylint/rules/empty_comment.rs`:58 on 2023-07-25 17:20_

Right, I am happy to use that implementation. I thought the requirement was to re-implement the existing pylint logic. Happy to change that of course.

---

_@jelly reviewed on 2023-07-25 17:35_

---

_Review comment by @jelly on `crates/ruff/src/rules/pylint/snapshots/ruff__rules__pylint__tests__PLR2044_empty_comment_0.py.snap`:75 on 2023-07-25 17:35_

Locally, I've added it and it indeed does not get marked as empty comment with pylint or this current PR.

---

_@MichaReiser reviewed on 2023-07-25 17:47_

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pylint/rules/empty_comment.rs`:58 on 2023-07-25 17:47_

Let's hope my suggestion is correct :sweat_smile: 

Our goal is to implement rules with the same semantic but we're free to diverge on how we implement the rule to get better performance or higher correctness.

---

_@jelly reviewed on 2023-07-25 18:12_

---

_Review comment by @jelly on `crates/ruff/src/rules/pylint/rules/empty_comment.rs`:58 on 2023-07-25 18:12_

So, I'm trying out your suggestion but this logic will also trim `########` as an empty comment and `a = x # fooo #` also as an empty comment.

---

_@MichaReiser reviewed on 2023-07-25 18:21_

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pylint/rules/empty_comment.rs`:58 on 2023-07-25 18:21_

The fist example makes sense to me. Should it only consider comments as empty if the # are separated by a space?


The second example is surprising to me. Can you share your code?

---

_@jelly reviewed on 2023-07-25 18:29_

---

_Review comment by @jelly on `crates/ruff/src/rules/pylint/rules/empty_comment.rs`:58 on 2023-07-25 18:29_

The second example is in the test, oh woops:

```
print(b)  #
print(b)  # this is fine#
b = "#this#is#fine#"  # 
```

My assumption is that you compare the trimmed line with the normal line, if anything was removed it has an empty comment? But maybe that's wrong :)

---

_@MichaReiser reviewed on 2023-07-25 18:45_

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pylint/rules/empty_comment.rs`:58 on 2023-07-25 18:45_

I think that's too aggressive because it also is true if only part of a comment was removed. Can you try testing if the trimmed comment is empty (it won't sole the first problem)


---

_Review comment by @jelly on `crates/ruff/src/rules/pylint/rules/empty_comment.rs`:58 on 2023-07-25 19:06_

So, that would mean only useless comments on an a line would be stripped? And not `print(1) #`

---

_@jelly reviewed on 2023-07-25 19:06_

---

_@MichaReiser reviewed on 2023-07-26 06:24_

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pylint/rules/empty_comment.rs`:58 on 2023-07-26 06:24_

What I understand is that the rule should flag the following comments:

```python
# 
end_of_line #
end_of_line_nested # # # 
```

But it should not flag

```python
#########
"""abcd
efg # hij" 
efg # hij #" 
```

How about

```python
end_of_line ######
```

Assuming this is correct. The way I would approach the rule is by only looking at the comment text and ignoring the line entirely to detect empty comments (fixing requires detecting if it is an own-line or end of line comment)

```rust
let comment_text = locator.slice(comment_range);

if comment_text.trim_start_matches(|c| c == "#" || is_python_whitespace(c)).is_empty() {
	// create diagnostic
}
```

The condition is true if the comment only consists of whitespace or `#` (there are no remaining other characters). This incorrectly flags `######`. 

```rust
let comment_text = locator.slice(comment_range);
let mut rest = comment_text;

loop {
	rest = comment_text.trim_start_matches(is_python_whitespace).trim_start_matches('#');	

	if rest.is_empty() {
		// Comment consist only of `#...whitespace` sequences. 
		break;
	}

	// `##` or `# a`
	if !rest.starts_with(is_python_whitespace) {
		return;
	}
}

// create diagnostic
```

---

_Comment by @zanieb on 2023-11-30 22:23_

Closing this as stale. Feel free to re-open if you are interested in working on it again!

---

_Closed by @zanieb on 2023-11-30 22:23_

---
