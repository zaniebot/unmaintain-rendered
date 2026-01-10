```yaml
number: 2054
title: "[RUF005] Unexpectedly removes comments"
type: issue
state: closed
author: thomasdesr
labels: []
assignees: []
created_at: 2023-01-21T09:13:44Z
updated_at: 2023-01-22T22:00:30Z
url: https://github.com/astral-sh/ruff/issues/2054
synced_at: 2026-01-10T11:09:45Z
```

# [RUF005] Unexpectedly removes comments

---

_Issue opened by @thomasdesr on 2023-01-21 09:13_

When running `ruff==0.0.228` on the following code sample via: `ruff --select RUF005 .`

```python
first = [
    # The order
    1,  # here
    2,  # is
    # extremely
    3,  # critical
    # to preserve
]
second = first + [
    # please
    4,
    # don't
    5,
    # touch
    6,
]

```

it suggests:
```
ruf005-repro.py:9:5: RUF005 Consider `[*first, 4, 5, 6]` instead of concatenation
```

or auto-fixes it to:
```python
first = [
    # The order
    1,  # here
    2,  # is
    # extremely
    3,  # critical
    # to preserve
]
second = [*first, 4, 5, 6]
```

This is unfortunate as those comments are important :D

Thanks so much as always!

---

_Assigned to @charliermarsh by @charliermarsh on 2023-01-21 12:16_

---

_Comment by @charliermarsh on 2023-01-21 12:31_

Gonna avoid autofixing for now in cases that contain comments.

---

_Closed by @charliermarsh on 2023-01-21 12:37_

---

_Comment by @thomasdesr on 2023-01-22 03:43_

Thanks for the quick fix!

---

_Comment by @LefterisJP on 2023-01-22 21:06_

I also have a question on this @charliermarsh. Thought I should make another issue but first wanna try here.

What's the reason behind this warning? `RUF005` seems to not improve readability much, or make things faster. I ran some tests and concatenation is faster (by not too much, but it is).

The only advantage I know of unpacking is it can work for something like adding list to tuple, but in that case it should be used when needed explicitly.

---

_Comment by @charliermarsh on 2023-01-22 21:30_

I think the way I'd like to answer that question is by clarifying the position Ruff that is taking by including rules (or not).

I generally try to stay _somewhat_ objective with regards to what gets included in Ruff, looking at the following factors:

1. I can understand why someone might want to enforce it.
2. It's sufficiently robust (few false positives and negatives).
3. The complexity of the rule and its implementation doesn't outweigh its usefulness.
4. There's evidence of demand for it.

To me, this rule satisfied criteria 1-3 (I can understand why folks would want to enforce this). 4 is harder to measure in this case, vs. when reimplementing flake8 plugins, where I can see how widely used they are. By including a rule in Ruff, I don't mean to express the opinion that you _should_ have it enabled (there are rules in Ruff that I wouldn't enforce in my own projects, like the `flake8-builtins` rules, but it's a popular plugin, so...).

One thing I _have_ been thinking, about and that this rule arguably raises (as with any new rule), is that if you enable a category like `RUF`, then upgrade Ruff, you run the "risk" of automatically enabling new rules, and thereby having to fix new violations. I don't know what the right philosophy (from the project's perspective) or experience (from the user's perspective) is there. For example: perhaps users should be opting-in to new rules upon upgrade, rather than being required to opt-out.


---

_Comment by @LefterisJP on 2023-01-22 21:48_

> For example: perhaps users should be opting-in to new rules upon upgrade, rather than being required to opt-out.

Hmm I don't dislike seeing new stuff like this @charliermarsh enabled. If it's not auto-enabled I would probably miss it.

Seeing it like this, I get a choice. Let it run (it's auto-fixable so easy to do), or put it in my `ignore`. So take action. I like that as an approach. I expect to need to manually take action and make decisions after an upgrade.

Let me maybe explain what I did when I saw this applied to my project to maybe explain how the process went.

My first reaction when seeing this was to go to the readme and understand why it's recommended. The readme does not explain anything unfortunately. Only says `unpack-instead-of-concatenating-to-collection-literal`. 
**Suggestion**: Something really cool would be to explain the reasoning next to the rule and why it would be a good change. If it's a style preference specify that instead.

Without any explanation I started googling around. I could not find any sufficient explanation. Neither on readability nor on performance. The only plus I could find on unpacking, as I mentioned above is you can use it to concatenate mutable and immutable collections, like say list and tuple.

With googling inconclusive I wrote a silly test:

```python
import random
import timeit

list_1 = random.choices(range(1,1000), k=1000)

print(timeit.timeit(lambda: list_1 + [1, 100]))
print(timeit.timeit(lambda: [*list_1, 1, 100]))
```

Here the first number for me is always smaller, so it does not seem like a good change performance wise.

So no readability improvement, no performance improvement, in the end I disabled the rule.

But I am still very much intrigued on why people would want this enforced. Do you think it would make sense to have a forum/chat (github has a feature for forums) where we could discuss rules and add hints/tips to make each other's codebase better by explaining these stuff?

I think you would not want this in issues as discussion are not actionable and can end up in bike shedding.

---

_Comment by @charliermarsh on 2023-01-22 21:53_

> Suggestion: Something really cool would be to explain the reasoning next to the rule and why it would be a good change. If it's a style preference specify that instead.

Yeah this is great feedback. We're starting to move in that direction but it's going to take some time to formalize and cover the existing rules. The approach we're taking is based on [Clippy](https://rust-lang.github.io/rust-clippy/master/), Rust's linter:

<img width="1624" alt="Screen Shot 2023-01-22 at 4 50 18 PM" src="https://user-images.githubusercontent.com/1309177/213942245-2d0a7600-6f95-4bd4-9276-08b149502c83.png">

You can see that we've started adding these to some of the [in-code documentation](https://github.com/charliermarsh/ruff/blob/ebfdefd110e83bcd1d101ee5a424fc6a82e020b5/src/rules/flake8_bugbear/rules/assert_raises_exception.rs#L1), and eventually those will be surfaced to users.

> But I am still very much intrigued on why people would want this enforced. Do you think it would make sense to have a forum/chat (github has a feature for forums) where we could discuss rules and add hints/tips to make each other's codebase better by explaining these stuff?

Yeah this could be a good use-case for forums. Let me think on it for a bit. I want issues to avoid becoming a place where people debate the usefulness of rules, as you've hinted at.

For this rule: I'd argue that it increases readability in some cases. But it can also increase _consistency_ -- like, if you use this convention everywhere in your codebases, you get a more consistent codebase vs. sometimes unpacking and sometimes concatenating. But, again, I don't want to take a strong position either way -- I'm just looking to provide an answer on why someone might want to include it.


---

_Comment by @LefterisJP on 2023-01-22 22:00_

Thanks for the answers! Would love to see those take form!

Forum could also be per-rule, and have link to the equivalent rule discussion in the readme :smile: 

> If you use this convention everywhere in your codebases, you get a more consistent codebase vs. sometimes unpacking and sometimes concatenating

That's true but again it's a different kind of tool as you need to use unpacking in some cases as I stated above (concatenation can't work for different types, while unpacking can). So not using the same tool everywhere for this is valid.

Anyway I can also see why someone would want it, so no problem. My take-away then for this is that it's a rule that enforces consistency with a small potential performance hint.


---
