```yaml
number: 10672
title: "Feature request: rule that prohibits any use of type:ignore, and other type-checking-suppression mechanisms"
type: issue
state: open
author: wyattscarpenter
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2024-03-30T17:05:09Z
updated_at: 2024-05-21T17:49:39Z
url: https://github.com/astral-sh/ruff/issues/10672
synced_at: 2026-01-12T15:54:50Z
```

# Feature request: rule that prohibits any use of type:ignore, and other type-checking-suppression mechanisms

---

_@wyattscarpenter_

PEP 484 lists several ways to ignore type checking https://peps.python.org/pep-0484/#compatibility-with-other-uses-of-function-annotations: 
- `# type: ignore` comment
- a `@no_type_check` decorator on a class or function
- a custom class or function decorator marked with `@no_type_check_decorator`

While useful, these get around static type safety without ensuring dynamic type correctness, which can often be sign that something else is wrong in the code. Sometimes you have to use them—you know how it is—but it's not ideal. That means, in my opinion, it would be great to have a linter rule disallowing their use, to ensure code quality in codebases where they are not in fact needed.

This would be similar to https://docs.astral.sh/ruff/rules/blanket-type-ignore/ , but disallow _all_ uses of type: ignore comments, no_type_check decorators, and no_type_check_decorators


---

_Comment by @AlexWaygood on 2024-03-30 17:31_

Thanks for the idea! I'm not _sure_ I see the benefit of prohibiting all `type: ignore` comments, though, since presumably you'd easily be able to switch the "never use a `type: ignore` comment" lint off with a `# noqa` comment. So I feel like if someone really wants to switch the type checker off for a line, they'll just do

```py
1 + "1"  # type: ignore  # noqa
```

rather than

```py
1 + "1"  # type: ignore
```

and you're maybe not any better off than before?

---

_Label `rule` added by @AlexWaygood on 2024-03-30 17:31_

---

_Label `needs-decision` added by @AlexWaygood on 2024-03-30 17:31_

---

_Comment by @wyattscarpenter on 2024-03-30 17:55_

I didn't think of that interaction! It is faintly ridiculous that you can re-enable the comment immediately by using another comment, but on reflection I don't think it's that different than any other situation where you might use `#noqa`. The linter rule is designed to prevent you from footgunning yourself, so to speak, and it's up to your judgement if you want to override that; if you're really really sure :)

You could also do something like a rule that forbids noqa on such lines, somewhat similar in concept to https://docs.astral.sh/ruff/rules/blanket-noqa/ in that it lints the noqas themselves, but that seems a little counter to the spirit of a linter.

---

_Comment by @AlexWaygood on 2024-03-30 17:59_

> The linter rule is designed to prevent you from footgunning yourself, so to speak, and it's up to your judgement if you want to override that; if you're really really sure :)

Sure -- but you could also say that all type checker rules are designed to prevent you from footgunning yourself. By using a `type: ignore` on the line, you're already explicitly "opting into lack of footgun protection" ;)

So the only way I could see that Ruff could add value here would be by providing a rule that, unlike the type checker complaints, you _can't_ switch off with a suppression comment. And that, as you say, feels slightly counter to the spirit of how Ruff usually does things.

(Your `@no_type_check` and `no_type_check_decorator` ideas are slightly different; my critique of your proposal doesn't apply to those :-)

---

_Comment by @wyattscarpenter on 2024-03-30 19:22_

I still think it would be useful, but it's possible I'm ignorant about how most other people tend to use linters and typecheckers. Or perhaps it depends somewhat on whether you think of the typechecker as an orthogonal type of constraint system, or something the linter should lint as well. (Although the decorators seem to me like they would stand and fall on the same merits, so maybe I'm thinking about that wrong.—Maybe it's about whether you think comments should be linted?) And, further, I suppose it depends on how often you suppose someone will use a type ignore comment wrongly, and whether that reaches the (in)tolerance level a linter rule should hypothetically have.

I'm primarily imagining two example situations in which I think the lint rule would be useful, but they are perhaps idiosyncratic to myself:

1. I have a codebase that I expect to be entirely (verifiably) well-typed. Someone else adds a new line that includes a type ignore comment. The linter rule then tells them that this isn't expected, and they should reconsider. They can override this like any noqa situation, of course, but at least they know this is vaguely expected to be wrong, and might reconsider it, or we might reconsider the idea that the codebase is entirely well-typed, or reconsider what we were trying to do with the new code.

2. I inherit a codebase that I would like to make entirely (verifiably) well-typed. In this case, the linter tells me what parts are not, by detecting these comments, and I can address them through refactoring; or at least refactoring until I find some are impossible to resolve, in which case I remove this rule from my linter set, or perhaps noqa the exceptions.

It's worth noting that a typechecker could implement this feature, but so could a linter, and then using _either_ the given typechecker or the given linter would give me the desired behavior, and I would have flexibility to choose between multiple linters and typecheckers, respectively. Although, while I'm at it, it's worth noting that `grep` can also basically implement this feature.

All that now said, I defer to the judgement of others wrt this proposal.

---

_Comment by @Avasam on 2024-04-08 23:27_

It feels to me like this idea assumes no false-positives from type checkers. Which of course isn't the case.

Ensuring no blanket suppression makes sense, but that's already supported by the tools offering said suppression comments.

I understand wanting to promote other devs to understand *why* an issue is raised by a tool, rather than blindly ignoring. That's why at work I enforce that suppression comments should *always* come with an explanation. This way the developer *signals* a clear intent, which can better be caught in review, or whilst reading back older code. (done through https://eslint-community.github.io/eslint-plugin-eslint-comments/rules/require-description.html for JS/TS code, I don't think there's an equivalent for Python, that's a proposal I'd be interested in as well)
Edit: Looks like there's an open request for that: https://github.com/astral-sh/ruff/issues/8454

---

> (Your @no_type_check and no_type_check_decorator ideas are slightly different; my critique of your proposal doesn't apply to those :-)

Same

---

_Comment by @worldmind on 2024-05-21 17:49_

As for me it should be checked on code-review - you already has enabled rule for check typing and if somebody disabling them it's topic for discussion (as sign of possible disagreement in team), but it should be possible to disable anything, because final decision is on humans.

---
