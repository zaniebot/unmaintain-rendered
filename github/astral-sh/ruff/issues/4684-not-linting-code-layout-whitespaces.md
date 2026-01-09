---
number: 4684
title: Not linting code layout/whitespaces
type: issue
state: closed
author: piotr-rarus
labels: []
assignees: []
created_at: 2023-05-27T18:04:08Z
updated_at: 2023-05-27T19:17:04Z
url: https://github.com/astral-sh/ruff/issues/4684
synced_at: 2026-01-07T13:12:14-06:00
---

# Not linting code layout/whitespaces

---

_Issue opened by @piotr-rarus on 2023-05-27 18:04_

Hello folks,
consider following crap code:

```py
import numpy as np


def estimate_coef (x , 
                   y):
        # number of observations/points
        n = np.size( x)    
    
        # mean of x and y vector
        m_x = np.mean(x )
        m_y = np.mean(y)   
    
        # calculating cross-deviation and deviation about x
        SS_xy = np.sum(y*x) - n*m_y*m_x  
        SS_xx = np.sum(x*x) - n*m_x*m_x   
    
        # calculating regression coefficients
        b_1 = SS_xy/SS_xx   
        b_0 = m_y - b_1*m_x  
    
        return b_0 ,  b_1    
```

Let's run `flake8` against it.

```sh
$ poetry run flake8 lr.py
> lr.py:4:18: E211 whitespace before '('
lr.py:4:21: E203 whitespace before ','
lr.py:4:23: W291 trailing whitespace
lr.py:6:9: E117 over-indented (comment)
lr.py:10:24: E202 whitespace before ')'
lr.py:11:25: W291 trailing whitespace
lr.py:12:1: W293 blank line contains whitespace
lr.py:14:40: W291 trailing whitespace
lr.py:15:40: W291 trailing whitespace
lr.py:16:1: W293 blank line contains whitespace
lr.py:18:26: W291 trailing whitespace
lr.py:19:28: W291 trailing whitespace
lr.py:20:1: W293 blank line contains whitespace
lr.py:21:19: E203 whitespace before ','
lr.py:21:26: W291 trailing whitespace
```

According to `PEP8` code layout and where to put whitespaces is pretty important stuff and clearly defined. 
`Ruff` doesn't detect anything here whatsoever.  I've been pretty excited to check out this hot new thingy `ruff` hoping to happily migrate from `flake8`. I have no idea if it's by design or still not implemented but saying `ruff` can be put in `flake8` place in your pipeline and everything will be ok is a bit far reach now.

> ⚖️ [Near-parity](https://beta.ruff.rs/docs/faq/#how-does-ruff-compare-to-flake8) with the built-in Flake8 rule set

Guys, that's not near-parity. I've spent last 2 hours looking through docs, bugs thinking I've screwed up something since `ruff` wasn't showing anything.

Please don't read it as a rant. That's just my first experience with `ruff` as an idiot senior dev with 10 years of experience.
It was pretty rough. I'd be pretty happy to contribute though as I have to start learning rust anyway ;p



---

_Comment by @piotr-rarus on 2023-05-27 18:15_

> By default, Ruff enables Flake8's E and F rules. Ruff supports all rules from the F category, and a [subset](https://beta.ruff.rs/docs/rules/#error-e) of the E category, omitting those stylistic rules made obsolete by the use of an autoformatter, like [Black](https://github.com/psf/black).

Guys, no check can be made obsolete. Not everyone is using black or any other formatter. This is just hidden coupling. Even if I'm using auto-formatter you actually put linters after them in your pipeline. You need definitive check against PEP8 in your pipeline, no matter other tools you might be using.

You made pretty excessive assumption here which contradicts general knowledge how we use linters in CI.

---

_Comment by @JonathanPlasse on 2023-05-27 18:39_

Some of the rules are available with 0.0.269 https://github.com/charliermarsh/ruff/releases/tag/v0.0.269

---

_Comment by @piotr-rarus on 2023-05-27 19:04_

I'm using the latest v0.0.270. I'm aware what is to be expected of v0.0.x but I have issue with this design mindset.
All of Python folks already solved that in the past and showed us how to walk it. It's clearly violating PEP20.
I just want `ruff` to become awesome linter and safe to use by juniors.

> Errors should never pass silently.
> Unless explicitly silenced.

---

_Closed by @piotr-rarus on 2023-05-27 19:04_

---

_Comment by @charliermarsh on 2023-05-27 19:17_

I'm sorry that you had a rough experience. Candidly, it doesn't feel super productive to me to debate some of the questions raised above since you have strong opinions on them, and I'm not sure what we can do to address your concerns, other than the things we're already doing: these limitations _are_ called out in the docs, and we actually _are_ in the process of adding those rules (we've added most of them as experimental rules in the last few releases; soon, they will be part of the default `E` rule set, just as they are in Flake8, and so will be on-by-default for all users).

For context, a project of this scope requires constant prioritization, and the stylistic rules just weren't heavily prioritized since, empirically, that weren't blockers to enabling projects of all shapes and sizes to migrate to Ruff.

---
