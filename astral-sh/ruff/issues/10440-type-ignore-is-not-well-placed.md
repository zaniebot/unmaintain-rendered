```yaml
number: 10440
title: "# type:ignore is not well placed"
type: issue
state: closed
author: sylvainmouquet
labels:
  - question
  - suppression
assignees: []
created_at: 2024-03-17T23:53:44Z
updated_at: 2024-03-19T11:55:34Z
url: https://github.com/astral-sh/ruff/issues/10440
synced_at: 2026-01-10T11:09:52Z
```

# # type:ignore is not well placed

---

_Issue opened by @sylvainmouquet on 2024-03-17 23:53_

Hello,

I have this code :
```
result = await query.find_all(self.connection, foo=bar, bar=foo, foofoo=barbar)  # type: ignore
```

ruff generate this result : 
```
result = await query.find_all(
                self.connection, foo=bar, bar=foo, foofoo=barbar
            )   # type: ignore
```
it's possible configure the position of # type : ignore is present (first line and not last line) or let's it where there is a 'syntax' error ?



---

_Label `suppression` added by @zanieb on 2024-03-17 23:59_

---

_Label `style` added by @zanieb on 2024-03-17 23:59_

---

_Comment by @MichaReiser on 2024-03-18 07:41_

Hy @sylvainmouquet 

Are you using `ruff check` or `ruff format`? Would you mind sharing which rules you've enabled if it is `ruff check`?

---

_Comment by @sylvainmouquet on 2024-03-18 09:28_

well i close the issue, i don't reproduce and i am not sure if it's with ruff or others tools. i will open a new issue when i track the error

---

_Closed by @sylvainmouquet on 2024-03-18 09:28_

---

_Comment by @aberres on 2024-03-18 09:29_

I also had some of these when switching to `ruff format`.

`result = await query.find_all(self.connectionnnnnnnnnnnnn, foo=bar, bar=foo, foofoo=barbar)  # type: ignore`

`$ ruff format`

==>

```python
result = await query.find_all(
    self.connectionnnnnnnnnnnnn, foo=bar, bar=foo, foofoo=barbar
)  # type: ignore
```

---

_Comment by @aberres on 2024-03-18 09:30_

@sylvainmouquet Try with a longer line.

---

_Reopened by @sylvainmouquet on 2024-03-18 09:36_

---

_Comment by @sylvainmouquet on 2024-03-18 09:36_

ok i try again

---

_Comment by @sylvainmouquet on 2024-03-18 09:44_

@aberres ok i reproduce

---

_Comment by @MichaReiser on 2024-03-18 14:59_

>  it's possible configure the position of # type : ignore is present (first line and not last line) or let's it where there is a 'syntax' error ?

I suspect a configuration option would need to be per file or line because you probably want a different placement depending on the code snipped. 

Unfortunately, determining which node triggers the type error (it could also be multiple ones) isn't feasible in the formatter because it would come at a huge performance penalty. The formatter would need to ask a type-checker which node the comment is suppressing, and that can easily take seconds if you, e.g., use mypy.

What you can do is to move the type ignore comment manually, depending on which node triggers the typing error

```python
result = await query.find_all( # type: ignore
    self.connection, foo=bar, bar=foo, foofoo=barbar
)   
```

```python
result = await query.find_all(
    self.connection,
    foo=bar,
    bar=foo,
    foofoo=barbar,  # type: ignore
)
```

Putting it at the end of line doesn't work today (see https://github.com/astral-sh/ruff/issues/7368)

```python
result = await query.find_all(
    self.connection, foo=bar, bar=foo, foofoo=barbar # type: ignore
)   
```

I know moving type comments is frustrating, especially if you're migrating a project. I tried to explain some of ruff's reasoning in [this discussion](https://github.com/astral-sh/ruff/discussions/6670) and am interested to find better heuristics for pragma comment placement. 

---

_Comment by @aberres on 2024-03-18 16:11_

I agree that it is not too much of a problem in practice. When switching from Black to Ruff, I moved some ignores/noqas and carried on.

One thing I wonder: In the case above, the comment stays at the end. Is there any case in which this will be the correct line for any tool? Or might moving it right behind the opening `(` be the safer bet?

---

_Comment by @MichaReiser on 2024-03-18 16:35_

> One thing I wonder: In the case above, the comment stays at the end. Is there any case in which this will be the correct line for any tool? Or might moving it right behind the opening ( be the safer bet?

Keeping it at the end could be the right call for `noqa` comments and maybe even `type: ignore`. For example if there's an error in the value-expression: `foo=some_expr_with_a_typing_or_lint_error` 

---

_Comment by @aberres on 2024-03-18 16:51_

Oh right, got some `) # type: ignore[call-arg]` in my code. I thought `mypy` would expect them in the first line.

---

_Comment by @sylvainmouquet on 2024-03-19 06:50_

In this case, I prefer not to format the line when there is `# type: ignore`. And have an option for the user to define the formatting when it depends on the context.
In my opinion, automatic formatting must be idempotent, safe and can be executed as part of an automated process. In addition, when the result requires manual verification, this can be carried out using an option. As in the case of a pull request, the user accepts or corrects the proposal.



---

_Comment by @MichaReiser on 2024-03-19 08:42_

> In this case, I prefer not to format the line when there is # type: ignore.

The problem isn't specific to `type: ignore`. It applies to all pragma comments. That would mean that the formatter should not format any trailing comment that possibly could be a pragma comment, which could be any comment.

> In my opinion, automatic formatting must be idempotent, safe and can be executed as part of an automated process

I agree that that's the ideal and we try hard to place comments so that if it happens to be unsafe, that we reduce the scope of suppression comments so that the tool using the pragma comment can make you aware of a misplaced comment.

> And have an option for the user to define the formatting when it depends on the context.

Could you explain what you mean by "define the formatting" and how that would look/work? 

---

_Comment by @sylvainmouquet on 2024-03-19 11:46_

Comments pragma refers to not python grammar.

I am sharing my ideas. 
To format the code, there are 2 steps: create the AST + apply the formatting. 
Formatting is like CSS, it doesn't interact to the structure of the document, it's just styling
The AST is to create a tree of blocks to follow the grammar of the langage. It's a kind of reverse engineering. (It could be done by IA)

When there is  a pragma, things get more complicated, because this is a human decision, outside the python language.
The user has made a choice that cannot be calculated in advance.

It comes down to a decision tree: you can make the decision for the user, or you can leave the choice to him.

The decision he has made can become a new rule.

So we'd have:
- python rules
- custom user rules

---

_Comment by @MichaReiser on 2024-03-19 11:55_

It's unclear to me how a user decision would be encoded as a rule or how this would be integrated into the formatter (would it need to suspend formatting until the user makes a decision? How does it remember the decision?)
 
Overall, this is working as intended, and the "manual rule" today is that the user manually moves the pragma comment to the right position, which the formatter then remembers (which, in my view, matches the custom user rule). 
 


---

_Closed by @MichaReiser on 2024-03-19 11:55_

---

_Label `question` added by @MichaReiser on 2024-03-19 11:55_

---

_Label `style` removed by @MichaReiser on 2024-03-19 11:55_

---
