```yaml
number: 5385
title: Ruff complains about a too long line which black does not format
type: issue
state: closed
author: nbro10
labels:
  - question
assignees: []
created_at: 2023-06-27T10:26:03Z
updated_at: 2023-06-30T00:18:29Z
url: https://github.com/astral-sh/ruff/issues/5385
synced_at: 2026-01-10T11:09:47Z
```

# Ruff complains about a too long line which black does not format

---

_Issue opened by @nbro10 on 2023-06-27 10:26_

Ruff complains about this (in file `issue.py`), although black doesn't change this code
```

class A:
    def my_func(self, constants):
        headers = {
            constants.HEADERS.CONTENT_TYPE.value: constants.CONTENT_TYPE.URL_ENCODED.value
        }

```

The line is ofc `            constants.HEADERS.CONTENT_TYPE.value: constants.CONTENT_TYPE.URL_ENCODED.value`, which has 90 chars

Ruff version: 0.0.275
Invoked command: `ruff check issue.py`
No configuration added to pyproject.toml for either ruff or black (so both should assume a max line-length of 88)

---

_Label `question` added by @charliermarsh on 2023-06-27 13:49_

---

_Comment by @charliermarsh on 2023-06-27 15:15_

Ah yeah, this is by-design and consistent with Flake8 and other linters. In general, the line-length check is just looking at the length of the physical line, and not whether a formatter has the ability to truncate it. If you want to rely on Black to enforce line length, you might consider disabling E501 altogether.

---

_Closed by @charliermarsh on 2023-06-27 15:15_

---

_Comment by @nbro10 on 2023-06-27 15:27_

Thanks. But disabling  E501 globally doesn't seem to be a good idea but locally may be an option. I was asking this question mostly because I've read somewhere that ruff is supposed to be compatible with black. That apparently doesn't mean that, if you run black and then ruff, you get no errors like this

---

_Comment by @charliermarsh on 2023-06-27 15:29_

It's compatible with Black in that it won't make any changes that conflict with Black, and there aren't any rules that will be triggered by way of running Black on your codebase. (This isn't true of Flake8 or Pycodestyle, which have rules that disagree with Black's formatting.)


---

_Comment by @nbro10 on 2023-06-27 15:31_

Well, ruff has a rule that will be triggered after running black. This is exactly what I'm reporting in this issue. Some rule in ruff is incompatible with how black runs.

---

_Comment by @charliermarsh on 2023-06-27 15:47_

I think we're using different definitions of compatibility. Let me provide you with a different example. If you use `isort` out of the box, it's incompatible with Black. If you run Black, then isort, then Black, then isort, they will continue to disagree infinitely, and change the code back and forth to fit their divergent rules.

On the other hand, running Black and Ruff is intended to be a stable operation: if you run Ruff, then Black, you shouldn't expect Ruff to flag any new rules after running Black. This doesn't mean that Ruff won't flag _any_ violations after running Black. But running Black won't _introduce_ any new violations.

As an aside, you can fix the line-length violation, like:

```py
class A:
    def my_func(self, constants):
        headers = {
            constants.HEADERS.CONTENT_TYPE.value: (
                constants.CONTENT_TYPE.URL_ENCODED.value
            )
        }
```

And Ruff will respect it, and Black won't undo it.

But, we may just be on the same page, and you may just disagree in some sense, which is fine too.


---

_Comment by @nbro10 on 2023-06-27 16:01_

If you're saying that Ruff will not report anything after running black, provided that we first fix all issues reported by ruff before running black itself, this may not be true, because our fix may be undone by black? 

You're saying that running black won't introduce new violations, but it might introduce violations. 

Anyway, this specific case is not a big problem for me personally, but I just don't get in which way we can really say that ruff is really compatible with black if it's not happy with something that black does or not

---

_Comment by @charliermarsh on 2023-06-27 16:23_

Can I ask a slightly different question here: what is your ideal behavior?


---

_Comment by @nbro10 on 2023-06-27 16:36_

Ideally, if one wants to follow the black conventions, he/she probably doesn't want ruff to report anything black does or doesn't do. In other words, `ruff` should not report issues because of black (after you run black) and I'd like to avoid to change the style, after I run black, in order to make `ruff` happy. But maybe I don't have the full picture and this may also not be ideal. Maybe this is more a black issue, because black should format that line, if the max length is supposed to be 88

---

_Comment by @dimaqq on 2023-06-29 12:31_

> Ah yeah, this is by-design and consistent with Flake8 and other linters. In general, the line-length check is just looking at the length of the physical line, and not whether a formatter has the ability to truncate it. If you want to rely on Black to enforce line length, you might consider disabling E501 altogether.

Sorry to chime in, as a non-user of `ruff`, rather someone who is trying it out, it seems like the tool is taking on too much... I'd be very happy to leave formatting to `black` entirely and have this tool focus on all the other fluff.

---

_Comment by @dimaqq on 2023-06-29 12:36_

Here's another example:

```py
async def frob(foo):                                                                                                                                    
    match foo.bar:                                                                        
        case 42:    
            try:            
                httpx.do.something()                                                    
            except httpx.HTTPStatusError as e:                                          
                logging.exception(                                                      
                    f"Error response {e.response.status_code!r} while requesting {e.request.url!r}"
                )                      
```

This kind of line could only be split using C-style string literal concatenation:
```
                logging.exception(                                                      
                    "Error response "
                    f"{e.response.status_code!r} while requesting {e.request.url!r}"
                )  
```

This has been discussed at `black` a while back and ultimately a sensible decision was taken not to split user literals.

Which leaves long literals a valid exception to the line length rule.

I think long type annotations and complex inline dicts are the same.

Please consider re-opening this ticket.

---

_Comment by @charliermarsh on 2023-06-29 12:37_

We enable only a very few stylistic rules by default. You’re welcome to turn off the line-length rule if you find it unhelpful! Though note that Black alone does not solve line lengths — you can still have overlong comments, strings, lines that Black doesn’t know how to split, and so on. I think it’s very reasonable to have a linter that checks for overlong lines (_not_ supporting a line length check would strike me as unusual, Flake8 has one too, etc.), but again, you’re welcome to turn it off. 

---

_Comment by @charliermarsh on 2023-06-29 12:38_

What I don’t quite follow is: if you want Black to be the determiner of what’s a valid line length, why enable E501 at all?

---

_Comment by @charliermarsh on 2023-06-29 12:38_

(To be clear, we don’t attempt to autofix or modify line lengths at all.)

---

_Comment by @nbro10 on 2023-06-29 12:41_

Like I said in the comment above, maybe this is a black problem, but also a ruff's documentation problem, in my view. It's a black problem because I expect black to format the code so that no line has more than the specified length. It's a ruff's docs problem because ruff is not really compatible with what black does (otherwise it wouldn't report something that black didn't do). But this is not a big issue because this is more an exception than the rule

---

_Comment by @dimaqq on 2023-06-29 12:45_

I'd like to have good tools work together out of the box.

In fact, I've removed `flake8` from a few repos merely because the tool required custom config to be compatible with `black`.

It is true, too, that another, more pedantic developer brought `flake8` with the correct set of disabled E's and W's, so not all is lost.

My humble 2c: a young, lesser known project must be compatible from get go to gain traction.

---

_Comment by @zanieb on 2023-06-29 16:27_

@nbro10 I think this is clearly documented at https://beta.ruff.rs/docs/faq/#is-ruff-compatible-with-black — is there somewhere else you think this information should be highlighted?

Black cannot safely automatically split some long lines. In these cases, it is valuable to many users to have a linter highlight the line length violation so it can be split manually.

---

_Comment by @eli-schwartz on 2023-06-30 00:15_

> If you're saying that Ruff will not report anything after running black, provided that we first fix all issues reported by ruff before running black itself, this may not be true, because our fix may be undone by black?

Are you saying that running `black` caused two shorter lines to be merged into one longer (90 characters) line?

i.e.:
- run `ruff`: everything is good, lines are all short enough
- run `black`: lines become longer
- run `ruff`: errors now occur

---
