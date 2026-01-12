```yaml
number: 1141
title: Support for pandas-vet
type: issue
state: closed
author: delulu52
labels:
  - rule
assignees: []
created_at: 2022-12-08T14:23:42Z
updated_at: 2022-12-15T19:01:03Z
url: https://github.com/astral-sh/ruff/issues/1141
synced_at: 2026-01-12T15:54:41Z
```

# Support for pandas-vet

---

_@delulu52_

Would be nice to have support for pandas-vet check, know have found them useful in my projects (this question likely interesting one where this perhaps has argument if should be included or not that of course fine in flake8 as has plugins but with ruff from my understanding plugins wouldn't really be possible let me know if that understanding is wrong)

https://github.com/deppen8/pandas-vet

---

_Label `rule` added by @charliermarsh on 2022-12-08 15:20_

---

_Comment by @charliermarsh on 2022-12-08 16:38_

@ksdaftari - I'm happy to include `pandas-vet` in Ruff, it's just a question of implementing and prioritizing it :) I try to ensure that the most popular plugins are implemented, and then for less-popular plugins, often look to contributors to tackle them.

---

_Comment by @edgarrmondragon on 2022-12-08 16:44_

At least `PD001` was covered by #1098 🙂. I might contribute this over the weekend, unless you want to take a stab at it @ksdaftari

---

_Comment by @delulu52 on 2022-12-08 17:04_

Thanks may take look at, am rust novice, so I may not be best but have been wanting to learn rust so perhaps could take stab (but again dont know if as novice if that would just be putting more work on your end). I did want to ask as if specific package items like this would think in scope for project like this. 

@charliermarsh Do you think there would be any type of plug-in system (even if something would need to be added with rust code). One thing that is nice about flake8 and such is ability to perhaps have some more opinionated/specific checks for team/org/project. Though perhaps that would be more out of scope and not goal project like this. 

Will say do like direction so far is going and the performance gains does seem like could definitely change how we develop as python developers (eg I know I personally in vscode turnoff many of my linting checks and just have run in pre-commit because of how slow and laggy can make my experience when editing, if this felt instantaneous that would be bit of game changer and just make coding feel so much faster and safer. So just to say thanks for taking this step within python community in project like this. 

I know this project is early in development, and was wondering if so you feel like prd ready? Have been keeping eye on and thinking about replacing it with my use of flake8 and such for me and my team. 

---

_Comment by @charliermarsh on 2022-12-08 18:46_

@ksdaftari - Yeah, we've had some discussion around a plugin system (#283). I haven't been prioritizing it so it doesn't have a definitive timeline, but my hope is that users can author plugins in Rust or Python (though we won't support existing Flake8 plugins, they'll have to be implemented for Ruff specifically).

We've got a bunch of major projects (and companies) using Ruff in production, and I do recommend it. The only real limitation is that Ruff doesn't support structural pattern matching (Python's `match` statement), which I'm hoping to resolve as part of an `0.1` release. It will definitely be fixed, but doesn't have a concrete timeline. Apart from that, the other reason _not_ to adopt Ruff right now is that there could be breaking changes between now and `0.1`. I will say that breaking changes have been minimal, and I've invested in keeping things compatible where I can, but it's a caveat to be aware of.


---

_Comment by @delulu52 on 2022-12-08 18:49_

@charliermarsh Awesome. Ok I may give ruff a try with some of repos especially larger ones, and iterate from there. And if end up adopting May end up taking this ticket. Thanks for all the info

---

_Comment by @charliermarsh on 2022-12-08 18:52_

@ksdaftari - Of course! Just let me know if you run into any issues. Can't promise that I'll fix or implement new features immediately but I will certainly try :)


---

_Comment by @delulu52 on 2022-12-14 13:19_

One thing wanted to note (more potential concern long term$, that thinking about this more assume we would want something like this to be more plug-in, or at least am wondering if any concerns around fact if have library specific linters like this, it may somewhat matter using correct longer based on what version of library user is using (perhaps not, it may just mean what codes you select/don't select, but do wonder if scenarios could get in that logic and such would matter on what version of library had installed)

---

_Comment by @lsorber on 2022-12-15 01:17_

FYI: pandas-vet’s purpose is to provide training wheels for newcomers to pandas. As a consequence, the pandas-vet rule set is not designed to validate idiomatic usage of pandas, but rather to encourage simplified usage.

Idiomatic usage and simplified usage don’t always agree with each other, unfortunately [1]. For instance, its PD008 rule tells you not to use the .at method for the sake of simplicity, even though .at is the most efficient and explicit way to select a scalar from a DataFrame [2].

[1] https://github.com/deppen8/pandas-vet/issues/105
[2] https://pandas.pydata.org/docs/user_guide/indexing.html#indexing-basics-get-value

---

_Closed by @charliermarsh on 2022-12-15 19:01_

---
