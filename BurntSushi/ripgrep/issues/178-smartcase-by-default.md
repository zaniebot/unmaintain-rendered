```yaml
number: 178
title: Smartcase by default.
type: issue
state: closed
author: ticki
labels: []
assignees: []
created_at: 2016-10-14T17:08:06Z
updated_at: 2020-04-14T12:43:50Z
url: https://github.com/BurntSushi/ripgrep/issues/178
synced_at: 2026-01-12T16:13:21Z
```

# Smartcase by default.

---

_@ticki_

This is a really convinient default, which can be found in both Vim, Firefox, and Silver Searcher has.

It's one of the few things I've missed from `ag`.


---

_Renamed from "Ag-like smartcase" to "Smartcase by default." by @ticki on 2016-10-14 17:11_

---

_Comment by @kaushalmodi on 2016-10-14 17:25_

I also missed that. But that void is filled by aliasing `rg` to `rg --smart-case` (and few other flags I like).


---

_Comment by @BurntSushi on 2016-10-14 17:35_

It's not happening. Use an alias if you want it by default. :-)


---

_Closed by @BurntSushi on 2016-10-14 17:35_

---

_Comment by @ticki on 2016-10-14 17:39_

@BurntSushi thanks for the quick response.

Could you elaborate on why not? Do you strive for grep compat?


---

_Comment by @BurntSushi on 2016-10-14 17:43_

> Could you elaborate on why not?

Because I personally hate `--smart-case` and think it's a bad default. I don't think there's really that much more depth to it. I find its behavior (by default) very surprising. I recognize that reasonable people can disagree, but we still have to pick something, and I'd rather it not enabled by default.

> Do you strive for grep compat?

If you compare the CLI for `grep` and `ripgrep`, you will see that there is clearly a lot of overlap, but they are not identical. I'm willing to do things differently. I don't feel like `grep` compat factors much into this for me personally.


---

_Comment by @ticki on 2016-10-14 17:44_

Fair enough. I agree it is not obvious, but I find it convinient. I guess it's just a matter of taste and it's easier to pick the obvious default and let the user override it.


---

_Comment by @ad-si on 2019-01-07 13:47_

I always assume that search functions are smartcase and fuzzy per default. I find anything else really surprising ^^.

---

_Comment by @mversic on 2020-04-14 11:48_

@BurntSushi Looking at default behavior of `rg` vs `fd-find` I see that they handle `smart-case` differently. 
I would say that next-generation CLI tools such as these two should converge on the same set of defaults unless a strong case can be made to have different behavior. Someone could argue that these two are not correlated, but I feel that there is a strong correlation between the two. The fact is that it is more likely you will use one if you used the other which suggests they are categorized (rationally or subconsciously) as the **same set of tools** by the users. I am not providing any disambiguation of this categorization, whether it be functionality, time of creation, or anything else, I am just pointing out that this categorization exists.

I don't care what the default is, whether it's `smart-case` or not. What I believe would make most sense is that the defaults are the same across different CLI tools of the same group unless the most prominent use cases strongly favor differentiation of defaults.

Of course, both `rg` and `fd` can say that I'm late to the party and conform to being backwards compatible. If so, it's a pity, but at least have in mind this reasoning in future development and have a bigger picture of interconnectedness of the set of modern Rust based CLI tools being made. These tools will probably be a part of a larger ecosystem that is coming into existence in the following years.

---

_Comment by @BurntSushi on 2020-04-14 12:07_

@mversic I am 100% unwilling to change to smart case by default. I find it extremely surprising and unintuitive behavior, sorry. That ship has sailed and I'd really rather not re-litigate it. The kind of cohesion you're after just isn't possible. While I do generally try to be consistent with other tooling, like grep (there are far more similarities than differences), it is not a design objective to approach cohesion with any other tool. Consistency with other tools is just one axis and it must be weighed against other choices.

Generally speaking, I overall find your framing here pretty unconvincing.

---

_Comment by @mversic on 2020-04-14 12:43_

 > I find it extremely surprising and unintuitive behavior, sorry

I too personally find `smart-case` as a default behavior to be surprising, although whatever community decides, I'm fine 'with. It wasn't my intention to convince you to make this change. I wanted to emphasize the importance of consistent expectations among different tools

> Consistency with other tools is just one axis and it must be weighed against other choices

I've said the same thing here: `unless the most prominent use cases strongly favor differentiation of defaults`

> it is not a design objective to approach cohesion with any other tool

Of course, you are free to take myopic view regarding your tool. I can't help but dreaming how things might add up together to form a larger picture. `smart-case` is, of course, a minute nuisance in this regard. To my mind there is no question that `rg` will form a larger ecosystem with other tools (this trend I find quite obvious), but it seems it will happen rather organically than planned.

---
