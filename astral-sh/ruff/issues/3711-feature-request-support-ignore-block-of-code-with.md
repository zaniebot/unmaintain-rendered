```yaml
number: 3711
title: "[feature request] Support ignore block of code with noqa"
type: issue
state: open
author: woile
labels:
  - suppression
assignees: []
created_at: 2023-03-24T07:25:40Z
updated_at: 2025-12-17T21:59:14Z
url: https://github.com/astral-sh/ruff/issues/3711
synced_at: 2026-01-10T11:09:46Z
```

# [feature request] Support ignore block of code with noqa

---

_Issue opened by @woile on 2023-03-24 07:25_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

First of all, thank you so much for this tool, it's fantastic 🚀 ! 

I would like to request the possibility to support ignoring a block of code. To my surprise, this is a feature some people have needed as well from flake8, [see SO question](https://stackoverflow.com/questions/64428794/flake8-disable-linter-only-for-a-block-of-code), though it is not supported.

In my current use case, I have a bunch of templates, that are too long, and it becomes cumbersome to add `noqa` to all of them.

I was wondering if adding something like this would be possible (maybe with a different syntax):

```python
# ruff: noqa: on: E501
TEMPLATE = """
Lorem ipsum dolor sit amet, consectetur adipiscing elit. Pellentesque sit amet ligula magna. Nulla ${var1} vehicula leo, eget tincidunt leo pharetra in. Ut egestas felis et vehicula commodo. Aliquam ${var2} placerat tortor sed magna faucibus facilisis ut et mauris. Maecenas vitae sem augue. Cras egestas felis nulla, id porta nisl ${var3} vel
"""
# ruff: noqa: off: E501
```

## Suggested changes

- [ ] Add section-level suppression/ignoring of rules (ruff: noqa: on and ruff: noqa: off)
- [ ] Add scope-level suppression/ignoring of rules (matches [pylint block disables](https://pylint.pycqa.org/en/latest/user_guide/messages/message_control.html#block-disables))

Thanks!


---

_Label `noqa` added by @charliermarsh on 2023-03-24 14:04_

---

_Comment by @charliermarsh on 2023-03-24 23:51_

I'd really like to support something like this. But we may want to couple it with a more holistic redesign of suppression comments. \cc @MichaReiser 

---

_Comment by @charliermarsh on 2023-03-24 23:51_

And thank you for the kind words :)

---

_Comment by @whinee on 2023-03-29 01:00_

Hello, just to add to the already wonderful suggestion, I think also adding a simple and and off switch with just `# ruff: noqa: on` and `# ruff: noqa: off` to completely ignore a block of code would be fantastic!

---

_Comment by @MichaReiser on 2023-03-29 06:00_

What's your expectation if a file only contains an `off` comment without a corresponding `on` comment later in the file? 

---

_Comment by @whinee on 2023-03-29 06:21_

That the program ignores the lines under that comment.

A markdown linter named [markdownlint](https://github.com/DavidAnson/markdownlint) has [that option](https://github.com/DavidAnson/markdownlint#configuration) and if I am not mistaken, if there is no `enable` comment down the line, then the program just ignores all of it.

This program could also have an option to warn the user when an off flag/comment is unterminated (has no corresponding on switch) cuz disabling linting for the rest of the file is not recommended???? I dont knowwwwwwww

---

_Comment by @evanrittenhouse on 2023-04-18 02:25_

I'm pretty interested in this one. Obviously pretty new to the project, but I think that the design proposed above makes sense. 

@MichaReiser saw you were mentioned above, are we good to move forward on this design? Do we want to support ignoring multiple rules at once?

---

_Comment by @MichaReiser on 2023-04-18 08:55_

> @MichaReiser saw you were mentioned above, are we good to move forward on this design? Do we want to support ignoring multiple rules at once?

Technically: I recommend waiting till after #3931 lands because it significantly changes how we handle noqa comments internally. 

Regarding the design. I haven't had much time to look into suppression comments, I got sidetracked by figuring out how a setting format could look like in a world where ruff does formatting and linting (and more?). 

My personal objective for suppression comments is that we should try to keep it simple. I really like Rust's approach where there's a single way to enable or disable lints: Using `allow` and `deny`. The scope is defined by where the attributes are put: In a module -> applies for the whole module, at the top of a function -> applies for the function, at the top of an expression -> applies to the expression. Having a single format keeps things simple and easy to remember. I've had to google ESLint's suppression comment format so many time in the past, got it wrong because I forgot the `on` comment after disabling a rule for a section, or accidentally deleted the `on` comment in a refactor :hand_over_mouth: .

I'm not proposing that we need to implement the same semantics but I think its worthwhile to list the use cases with examples on how the new suppression comments should work. Do we support whole file suppression? Do we want to warn about missing `on` comments after an `off`. etc. 

---

_Comment by @evanrittenhouse on 2023-04-18 12:59_

That makes sense. I'll start working on a design/use-cases - since #1054 asks for block/file-scoped `noqa` directives, closing this issue should also close that one (file-scoped `noqa`s are already implemented)

---

_Comment by @evanrittenhouse on 2023-04-19 02:58_

Editing out the design approach I had here to link to the discussion instead: https://github.com/charliermarsh/ruff/discussions/4051

---

_Comment by @tekumara on 2023-05-13 11:48_

FYI - duplicate of #1289

---

_Comment by @adam-grant-hendry on 2023-08-31 14:28_

@woile Thanks for opening this issue!

Per discussion in #3868 , would you please add the following checklist to the beginning of this issue for PR tracking:

- [ ] Add section-level suppression/ignoring of rules (`ruff: noqa: on` and `ruff: noqa: off`)
- [ ] Add scope-level suppression/ignoring of rules (matches [`pylint` block disables](https://pylint.pycqa.org/en/latest/user_guide/messages/message_control.html#block-disables))

There are several feature requests to add lint ignoring/suppression capabilities, each with slight variations, and some of the project maintainers have asked to keep track of them here. They may need to be implemented at different times or in several stages, so a checklist would be helpful for tracking to PRs.

Thank you!


---

_Comment by @adam-grant-hendry on 2023-08-31 14:45_

FWIW, I believe suppression for any scope as in [`pylint` block disables](https://pylint.pycqa.org/en/latest/user_guide/messages/message_control.html#block-disables) in addition to section-level suppression (i.e. turning on/off linting) would be wise.

Without knowing the internals, I imagine the former would require reparsing the AST, and thus a significant rewrite. However, 

- it would match existing `pylint` behavior
- I’m slowly seeing several different feature requests for suppression in different scopes (global level, function level, etc.).

Rather than rewrite the source for every kind of scope feature request, this would be done in one change and would likely cover 99% of use cases for most users.

---

_Comment by @flbraun on 2024-02-13 10:14_

I'd like to emphasize the importance of this feature for ruff's usability. There are some situations where you _need_ more sophisticated rule control, for example this:

```python
def get_all(cursor, table_name):
    return cursor.execute(
        '''
        SELECT *
        FROM {table_name};
        '''.format(table_name=table_name)
    )
```

ruff rightfully complains that this code contains a possible sql injection (S608). We know that the value of `table_name` is safe, so we want to disable S608 for that statement.
We can't disable the rule with `# noqa` because the affected line opens a multi-line string, so the only other option we have is to disable it for the entire file. Which is bad, because that would also disable it for any other sql statement in that file.
The only correct solution would be to wrap the entire sql string in an off-on-block or utilizing a `disable-next` pragma.

---

_Comment by @ThiefMaster on 2024-02-13 10:16_

Why don't you pass it as a param like you should? Sorry, but this is a perfectly true positive, even if in THIS particular case it's safe - until someone decides to make `cond` come from let's say user-provided input. Boom, SQL injection!

---

_Comment by @smurfix on 2024-02-13 10:28_

Passing it as a param works in this simple case, but when you build a whole query programmatically …?

---

_Comment by @flbraun on 2024-02-13 10:34_

> Why don't you pass it as a param like you should? Sorry, but this is a perfectly true positive, even if in THIS particular case it's safe - until someone decides to make `cond` come from let's say user-provided input. Boom, SQL injection!

Sorry, my example was chosen very poorly, I've replaced it with a less frivolous one where you can't get away with parameter-passing.

---

_Comment by @MichaReiser on 2024-02-13 11:03_

@flbraun I believe your example might be worth raising a separate issue. I would have expected that one of those work, but none of them do.

```python
def get_all(cursor, table_name):
    return cursor.execute(
        f'''
        SELECT *
        FROM {table_name};
        '''  # noqa: S608
    ) 

def get_all(cursor, table_name):
    return cursor.execute(
        (
            """
        SELECT *
        FROM {table_name};
        """  # noqa: S608
        ).format(table_name=table_name)
    )
``` 

---

_Comment by @kaddkaka on 2024-03-20 20:33_

Is there any ongoing work on this issue? 

---

_Comment by @Xavier-Do on 2024-03-26 15:23_

We are also waiting for this feature

Our use case is mainly in tests, we like to disable whitespaces rules the time of an assertions, to present a list/tuple/dict as a table, with aligned rows. 

```python
        self.assertEquals(
            # pylint: disable=bad-whitespace
            report,
            [
                ['Header',              400.00,      1600.00,          100.00],
                ['Other header',         00.00,       600.00,        10000.00],
                ['Some Header',         140.00,         0.00,         1000.00],
                ['',                   1400.00,       600.00,          100.00],
                ['Finally',           10400.00,       600.00,          100.00],
            ]
        )
```

---

_Comment by @Suor on 2024-04-27 05:30_

Maybe instead of `# ruff: noqa: on` and `... off` simply:
```python
# ruff: off
...
# ruff: on

# ruff: off: T201
print()
if ...:
    print("...", a, b, c)
...
# ruff: on
```

And since we have already `# ruff: noqa ...` to do it file level I would warn about non-paired `off` with a suggestion of either close it or replace with `ruff: noqa: whatever`. 

---

_Comment by @Pierre-Sassoulas on 2024-05-08 13:13_

I think there's a performance aspect to consider. In pylint the fine grained message control sometime prevented us from bypassing costly processing because checking if a message is disabled or not (on a directory, file, scope, line, node, or token) is costly in itself. 

---

_Comment by @yyyxam on 2024-05-28 15:35_

I stumbled upon another use case where this would be useful:

A line of code causes one error for mypy and one for bandit. Both support inline-comments for ignoring it, but they don't support ignoring code blocks. I can only ignore the error for one of them and have to ignore the whole file for another (not nice). If I used Ruff (with this feature) as a replacement for Bandit, I could ignore the block of code (1 line) and also add the inline ignore comment for mypy.
So it would improve compatibility with mypy (they also have an [ongoing thread](https://github.com/python/mypy/issues/6948) about implementing this) and other linters without this feature.

---

_Comment by @kaddkaka on 2024-05-28 16:59_

> I stumbled upon another use case where this would be useful:
> 
> A line of code causes one error for mypy and one for bandit. Both support inline-comments for ignoring it, but they don't support ignoring code blocks. I can only ignore the error for one of them and have to ignore the whole file for another (not nice). If I used Ruff (with this feature) as a replacement for Bandit, I could ignore the block of code (1 line) and also add the inline ignore comment for mypy. So it would improve compatibility with mypy (they also have an [ongoing thread](https://github.com/python/mypy/issues/6948) about implementing this) and other linters without this feature.

Why not just put two inline comments on the same line? 

---

_Comment by @AntiSol on 2024-07-26 01:41_

Just to chime in with my $0.02, I also think a good linter should have the ability to ignore a block

> What's your expectation if a file only contains an `off` comment without a corresponding `on` comment later in the file?

It should ignore the rest of the file (but turn itself back on at the end of the file)

---

_Comment by @BobVitorBob on 2024-08-23 18:05_

Another use case for this.

I wanted to leave a comment explaining why I'm turning another rule off, which includes a snippet of a minimal example for a pandas warning. Since it's commented python code, it lights up with `ERA001`. In order to suppress this, I have to do `# noqa: ERA001` for each line, which is unnecessary imo.

```
# import numpy as np                                                                # noqa: ERA001
# import pandas as pd                                                               # noqa: ERA001
# df = pd.DataFrame({"col1": [1, 2, 3, "", 5]}, dtype=object)                       # noqa: ERA001
# print(df)                                                                         # noqa: ERA001
# print(df.dtypes)                                                                  # noqa: ERA001
# df2 = df.replace("", 0)                                                           # noqa: ERA001
# print(df2["col1"])                                                                # noqa: ERA001
# df2.iloc[0, 0] = "a"                                                              # noqa: ERA001
# print(df2["col1"])                                                                # noqa: ERA001
```

---

_Comment by @AntiSol on 2024-08-30 05:11_

Use case: 

```python

from a_module import (          # noqa: I001
   # functions                                               
   do_something,                # noqa: I001
   do_something_else,           # noqa: I001

   # variables
   some_module_config_var,      # noqa: I001
   some_other_module_config_var,# noqa: I001
   fifty_more_of_these,         # fifty more noqa: I001

)

```

because I wasn't doing that awfulness, and there's no way to disable this rule for a block of code, now my code has no import sorting.

---

_Comment by @dhruvmanila on 2024-08-30 08:06_

@AntiSol You shouldn't require to specify `noqa` comment for each line, just one should suffice: https://play.ruff.rs/4c44cfef-9821-4cd6-98fd-6e3187228e68

---

_Comment by @AntiSol on 2024-08-30 08:16_

@dhruvmanila I actually tried that before posting. 

I don't think you're running the checker there? just the formatter? Isort rules are applied by the checker, not the formatter. I don't see any way to run the checker / isort rules there? 

You'll note that removing the noqa comment from your example [has no effect on the formatter output](https://play.ruff.rs/4c44cfef-9821-4cd6-98fd-6e3187228e68?secondary=Format)
![image](https://github.com/user-attachments/assets/e924e051-f8d6-4726-9993-ea732cc99498)
Maybe I'm missing something?

---

_Comment by @dhruvmanila on 2024-08-30 08:21_

Correct me if I'm wrong but I'm assuming that you're trying to ignore the names in that import statement from being sorted, right?


https://github.com/user-attachments/assets/74627bdd-f4a6-4843-9e81-d7709b525ee7

The video shows how to use code actions to apply a fix from the rules in the playground.

---

_Comment by @AntiSol on 2024-08-30 10:00_

that is what I'm trying to do, and that is what I tried before posting. Maybe I got it wrong somehow. I'll try again when I have time.

Aha, thanks... but I assume you're right-clicking in that video? I don't see that context menu, just the regular one
![image](https://github.com/user-attachments/assets/26c3859d-ff76-4fcd-83ed-44fa35092314)
Which means that there appears to be no way to run the checker in the playground? Might be worth a separate bug report I guess

---

_Comment by @dhruvmanila on 2024-09-02 07:12_

> Aha, thanks... but I assume you're right-clicking in that video? I don't see that context menu, just the regular one

No, I'm not right-clicking in the video. I'm just hovering over the squiggly lines and the pop-up will appear and then I'm clicking on the "Quick Fix" action which displays the code actions and then you can select any that's available.

> Which means that there appears to be no way to run the checker in the playground? Might be worth a separate bug report I guess

The checker runs automatically in the playground every time the content changes, so there's no need to run it manually. It's not a bug, it's a feature :)

---

_Comment by @AntiSol on 2024-09-02 10:59_

@dhruvmanila right you are on all counts! And I was also able to confirm that this does indeed solve my issue, so you can just ignore all my comments on this thread. Thank you muchly! :) 

---

_Comment by @MilkClouds on 2024-11-26 04:07_

looking forward to this feature..
![image](https://github.com/user-attachments/assets/27df3f29-9b26-4b2a-b1f9-a318a0b1c208)


---

_Comment by @Daraan on 2025-01-14 09:27_

Any updates on this?

---

_Comment by @j-adamczyk on 2025-01-20 14:33_

Any news on this? It would be super useful in chemoinformatics, where I have lists of very long strings (something like regular expressions), and they cannot be made shorter, causing E501 (line length) error. I would like to wrap the entire multiline list in a block to ignore this specific error.

---

_Comment by @tigerhawkvok on 2025-01-30 21:55_

I think the scope-level implementation is best as I have essentially the exact same reasoning as https://github.com/astral-sh/ruff/issues/3711#issuecomment-1512712449

IMO, I'd:

1. Change the top-of-the-file style (`#ruff: noqa: FOO01`) to internally be a scope-level disable, and just add the understanding of other scoping; 
2. add the special command `#ruff: noqa: [code] off` to disable a scope-level `noqa` (eg, re-enable the rule) from that point forward;
3. add a special `RUF` error message for a top-scope `noqa` begun after the first code line that was never disabled

---

_Comment by @alexchandel on 2025-05-19 15:11_

Is there a workaround for this?  I sometimes have to disable a lint for a region of code.

---

_Comment by @Baksalyar on 2025-06-16 14:56_

Hi! Just wondering — is this still unimplemented, or is it an intentional design decision?
It gets pretty annoying when you need to use something Ruff complains about in some isolated block of code, and the only option is to slap #noqa on every single line. :(

---

_Comment by @Hoernchen on 2025-08-14 14:53_

Yeah this is turning into a problem everwhere because all kinds of packages need special handling/patching and the only sane way is to just skip E402 completely..

---

_Comment by @MichaReiser on 2025-08-14 15:00_

This is on my mind and something I want us to fix in the coming months. I just can't promise you a date yet but we didn't forget about it.

---

_Assigned to @amyreese by @amyreese on 2025-10-07 00:08_

---

_Comment by @maltevesper on 2025-11-19 12:38_

I have not read the entire discussion, but have not found it in the checklist in the issue.
if you do implement it, it might be nice to think about scopes as well: see i.e. https://gcc.gnu.org/onlinedocs/gcc/Diagnostic-Pragmas.html

https://github.com/astral-sh/ruff/issues/3711#issuecomment-1487991823
> What's your expectation if a file only contains an `off` comment without a corresponding `on` comment later in the file?

If you have implemented scopes, the answer to your previous comment about how to handle an `off` without a following `on`, on the implementation side might be simply create implicit per-file scopes. Moreover, if you disable multiple lints for a block of code, you do not have to repeat yourself. Rather than typing out the lint numbers in an off comment and a subsequent on comment you simply pop the scope.



---

_Comment by @amyreese on 2025-12-12 01:37_

For folks interested in testing a new feature in Ruff's preview mode, the 0.14.9 release now supports [block level range suppressions](https://docs.astral.sh/ruff/linter/#block-level) when running with `--preview`:

```py
# ruff: disable[E501]
VALUE_1 = "Lorem ipsum dolor sit amet ..."
VALUE_2 = "Lorem ipsum dolor sit amet ..."
VALUE_3 = "Lorem ipsum dolor sit amet ..."
# ruff: enable[E501]
```

An upcoming release should include new diagnostics for tracking unused or invalid range suppressions, similar to what is currently available for `#noqa`.

Let us know if you have any feedback on the new system. 

---

_Comment by @smurfix on 2025-12-12 05:35_

Question:

> An "enable" comment can only be used to terminate a preceding "disable" comment with identical codes.

Does that mean that

    # ruff: disable[A,B]
    ...
    # ruff: enable[A]
    ...
    # ruff: enable[B]

(or vice versa) won't work?

---

_Comment by @Xavier-Do on 2025-12-12 07:44_

This is an amazing feature! 🥳 

It looks like it works fine in most cases but there is a specific one supported by pylint that doesn't work here regarding "implicit range"

```python
[
    # ruff: disable[E241]
    {'line_id': 1,              'balance': 1000.0,  'amount': 2000.0},
    {'line_id': 2,              'balance': -1000.0, 'amount': -2000.0},
]
```

In this case the disable is ignored. I would expect to only apply on the content of the list

Another small suggestion would be to support rules names instead of code to improve readability 
```python
def check():
    # ruff: disable[multiple-spaces-after-comma] 
    [
        {'line_id': 1,              'balance': 1000.0,  'amount': 2000.0},
        {'line_id': 2,              'balance': -1000.0, 'amount': -2000.0},
    ]
```

In any case the current version is already a game changer and enough for most needs so thanks a lot for adding this to ruff.

EDIT: I wasn't able to make it work by adding preview in the config, looks like it works only using the command line

---

_Comment by @amyreese on 2025-12-16 18:08_

> Question:
> 
> > An "enable" comment can only be used to terminate a preceding "disable" comment with identical codes.
> 
> Does that mean that
> 
> ```
> # ruff: disable[A,B]
> ...
> # ruff: enable[A]
> ...
> # ruff: enable[B]
> ```
> 
> (or vice versa) won't work?

Correct. We wanted to keep the implementation simpler, and make it easier to reason about which suppression comments were matched or unmatched and what ranges of code they cover.

---

_Comment by @kaddkaka on 2025-12-16 18:12_

> > Question:
> > > An "enable" comment can only be used to terminate a preceding "disable" comment with identical codes.
> > 
> > 
> > Does that mean that
> > ```
> > # ruff: disable[A,B]
> > ...
> > # ruff: enable[A]
> > ...
> > # ruff: enable[B]
> > ```
> > 
> > 
> >     
> >       
> >     
> > 
> >       
> >     
> > 
> >     
> >   
> > (or vice versa) won't work?
> 
> Correct. We wanted to keep the implementation simpler, and make it easier to reason about which suppression comments were matched or unmatched and what ranges of code they cover.

So one would ha to write something like:
```py
# ruff: disable[A]
# ruff: disable[B]
...
# ruff: enable[A]
...
# ruff: enable[B]
```

Is this supported?
```py
# ruff: disable[E200]
... 
# ruff: disable[E]
...
# ruff: enable[E]
...
# ruff: enable[E200]
```

---

_Comment by @amyreese on 2025-12-16 18:14_

> This is an amazing feature! 🥳
> 
> It looks like it works fine in most cases but there is a specific one supported by pylint that doesn't work here regarding "implicit range"
> 
> [
>     # ruff: disable[E241]
>     {'line_id': 1,              'balance': 1000.0,  'amount': 2000.0},
>     {'line_id': 2,              'balance': -1000.0, 'amount': -2000.0},
> ]
> 
> In this case the disable is ignored. I would expect to only apply on the content of the list

I'll try to add a test case for that and see if that's feasible to support. The logic currently depends on Indent/Dedent tokens for tracking blocks/scopes rather than arbitrary indentation on the comments themselves.

> Another small suggestion would be to support rules names instead of code to improve readability
> 
> def check():
>     # ruff: disable[multiple-spaces-after-comma] 
>     [
>         {'line_id': 1,              'balance': 1000.0,  'amount': 2000.0},
>         {'line_id': 2,              'balance': -1000.0, 'amount': -2000.0},
>     ]

I believe this is on our roadmap in general, including for `#noqa` comments. We want to make sure we have consistency across all suppression comment styles.

> EDIT: I wasn't able to make it work by adding preview in the config, looks like it works only using the command line

Can you please open an issue for that with a repro?

> In any case the current version is already a game changer and enough for most needs so thanks a lot for adding this to ruff.

💜




---

_Comment by @amyreese on 2025-12-16 18:17_

> So one would ha to write something like:
> 
> ruff: disable[A]
> ruff: disable[B]
> ...
> ruff: enable[A]
> ...
> ruff: enable[B]

Yes.

> Is this supported?
> 
> ruff: disable[E200]
> ... 
> ruff: disable[E]
> ...
> ruff: enable[E]
> ...
> ruff: enable[E200]

No, we only support exact code matches, similar to `#noqa`.



---

_Comment by @MichaReiser on 2025-12-17 09:02_

I think not supporting within expression suppressions is okay. Just suppress the entire statement.

```py
[
    # ruff: disable[E241]
    {'line_id': 1,              'balance': 1000.0,  'amount': 2000.0},
    {'line_id': 2,              'balance': -1000.0, 'amount': -2000.0},
]
```

It otherwise raises questions whether the following is valid or not


```py
[
    {
	    # ruff: disable[E241]
		'line_id': 1,              'balance': 1000.0,  'amount': 2000.0},
	# ruff: enable[E241]
    {'line_id': 2,              'balance': -1000.0, 'amount': -2000.0},
]
```

or even without the enable, how far does the suppression go? Is it also supposed to work in f-strings, what about if I put it in front of a binary expression?


I think this is something where a `ruff: ignore` might be more useful.

---

_Comment by @maltevesper on 2025-12-17 09:58_

I get false positives for ERA001: The comment is indistinguishable from defining a variable called `ruff` with generic typeannotation `disable[D102]`

```
class Test:
    # ruff: disable[D102]
    def test_method(self):
        pass
```

```
ERA001 Found commented-out code
 --> teste.py:2:5
  |
1 | class Test:
2 |     # ruff: disable[D102]
  |     ^^^^^^^^^^^^^^^^^^^^^
3 |     def test_method(self):
4 |         pass
  |
help: Remove commented-out code
```

Also it would be nice, if an enable is missmatched generated an error. I.e.:
```
class Test:
    # ruff: disable[D102]
    def test_method(self):
        pass

    # ruff: enable[D103]                    <- I would expect to get an error message for this line, since it was not disabled before
```

---

_Comment by @maltevesper on 2025-12-17 11:54_

I am excited for the new feature and gave it a bit more thought.
I am wondering if it is better to adjust the ERA001 lint or the syntax. However, I find the current syntax compelling, adding a space after the disable does not help (I am not sure why `x : list [int]` parses, but it does) and I did not come up with an alternative syntax I like

---

_Comment by @MichaReiser on 2025-12-17 11:56_

I think we should change `ERA001` to ignore this known false positives

---

_Comment by @ulgens on 2025-12-17 12:03_

+1 for excluding this from ERA001 but I missed why we are expecting ERA001 to handle an unimplemented (yet) feature, isn't it a bit early for that?

---

_Comment by @amyreese on 2025-12-17 18:34_

> +1 for excluding this from ERA001 but I missed why we are expecting ERA001 to handle an unimplemented (yet) feature, isn't it a bit early for that?

The new suppression system is already implemented and available when running Ruff with `--preview`.

---

_Comment by @amyreese on 2025-12-17 21:59_


> Also it would be nice, if an enable is missmatched generated an error. I.e.:
> 
> ```
> class Test:
>     # ruff: disable[D102]
>     def test_method(self):
>         pass
> 
>     # ruff: enable[D103]                    <- I would expect to get an error message for this line, since it was not disabled before
> ```

That is currently being added in #21908 

---
