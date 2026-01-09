---
number: 17079
title: "flake8-django: Ensure either `related_name` for each FK-based field or `default_related_name` per model is set"
type: issue
state: open
author: GitRon
labels:
  - rule
assignees: []
created_at: 2025-03-31T07:30:05Z
updated_at: 2025-04-01T07:44:39Z
url: https://github.com/astral-sh/ruff/issues/17079
synced_at: 2026-01-07T13:12:16-06:00
---

# flake8-django: Ensure either `related_name` for each FK-based field or `default_related_name` per model is set

---

_Issue opened by @GitRon on 2025-03-31 07:30_

### Summary

Django's default related names with `*_set` are quite unintuitive. Therefore, all foreign keys (also One2One and M2M of course) should implement a custom related name, either per field or in the model meta options.

Since you can do quite magic things when following related names, it should be explicit what you are "querying" for.

I've tried adding it to the original package but it seems to be abandoned: https://github.com/rocioar/flake8-django/pull/53

If you need more info or sparring, just leave me a note 🙂 

---

_Label `rule` added by @MichaReiser on 2025-03-31 08:48_

---

_Comment by @UnknownPlatypus on 2025-03-31 21:53_

I personally would like the opposite because guessing what `related_name` another developer chose is actually not that easy and while the `*_set` might seem unintuitive at first, it's predictable.
A flag would probably do it to accommodate for both

---

_Comment by @GitRon on 2025-04-01 06:10_

Hi @UnknownPlatypus!

Thx for your thoughts.

> I personally would like the opposite because guessing what related_name another developer chose is actually not that easy and while the *_set might seem unintuitive at first, it's predictable.

Generally, I'd be 100% with you. But since models represent data, it's highly semantic. I should really know what my data does and stands for. Therefore, I believe that - explicit is better than implicit - having to actively think about what my relationship means is worth something. Even if it's just the plural of the model in 95% of all cases.

> A flag would probably do it to accommodate for both

What are you suggesting exactly? Can't follow your thought, sorry 😔 


---

_Comment by @UnknownPlatypus on 2025-04-01 07:44_

> What are you suggesting exactly? Can't follow your thought, sorry 😔

That this rule could either enforce that **you never use** `related_name` (hence relying on the `*_set` behavior) **or always use it**.

To me consistency is key, and my experience working on a rather large django code base showed me that people do the plural thing in many ways. For exemple, to reference a model named `MySuperModel`, you will end up with:
- `related_name="my_super_models"`
- `related_name="mysupermodels"`
- `related_name="my_super_model_set"`

So you end up trying to enforce a rule to make it consistent again and end up really close to the initial `*_set` that is not that bad overall.

>  believe that - explicit is better than implicit 

I overall agree with you but in practice, it's really exhausting to be always searching for which plural variant another developer used and you end up losing quite some time. I think it's fine to expect a Django developer to know this, he will learn it quite fast anyway.

---
