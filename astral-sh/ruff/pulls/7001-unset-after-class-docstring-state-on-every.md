```yaml
number: 7001
title: "Unset `after_class_docstring` state on every iteration"
type: pull_request
state: merged
author: charliermarsh
labels:
  - formatter
assignees: []
merged: true
base: main
head: charlie/doc
created_at: 2023-08-30T03:17:00Z
updated_at: 2023-08-30T06:20:29Z
url: https://github.com/astral-sh/ruff/pull/7001
synced_at: 2026-01-12T02:45:38Z
```

# Unset `after_class_docstring` state on every iteration

---

_Pull request opened by @charliermarsh on 2023-08-30 03:17_

## Summary

This PR fixes a small deviation in Django. Given:

```python
class QuerySet(AltersData):
    """Represent a lazy database lookup for a set of objects."""

    def as_manager(cls):
        # Address the circular dependency between `Queryset` and `Manager`.
        from django.db.models.manager import Manager

        manager = Manager.from_queryset(cls)()
        manager._built_with_as_manager = True
        return manager

    as_manager.queryset_only = True
    as_manager = classmethod(as_manager)
```

We weren't setting `after_class_docstring = false` on all branches, and so we were incorrectly inserting a blank line between the two statements at the end, like:

```python
class QuerySet(AltersData):
    """Represent a lazy database lookup for a set of objects."""

    def as_manager(cls):
        # Address the circular dependency between `Queryset` and `Manager`.
        from django.db.models.manager import Manager

        manager = Manager.from_queryset(cls)()
        manager._built_with_as_manager = True
        return manager

    as_manager.queryset_only = True

    as_manager = classmethod(as_manager)
```

## Test Plan

`cargo test`


---

_Label `formatter` added by @charliermarsh on 2023-08-30 03:17_

---

_Review requested from @MichaReiser by @charliermarsh on 2023-08-30 03:35_

---

_Marked ready for review by @charliermarsh on 2023-08-30 03:35_

---

_@MichaReiser approved on 2023-08-30 06:19_

---

_Comment by @MichaReiser on 2023-08-30 06:20_

The similarity index remain unchanged but I manually verified that it fixes a few divergences for django without introducing any new ones. 

| project      | similarity index |
|--------------|------------------|
| cpython      | 0.76082          |
| django       | 0.99921          |
| transformers | 0.99854          |
| twine        | 0.99982          |
| typeshed     | 0.99953          |
| warehouse    | 0.99648          |
| zulip        | 0.99928          |


| project      | similarity index |
|--------------|------------------|
| cpython      | 0.76082          |
| django       | 0.99921          |
| transformers | 0.99854          |
| twine        | 0.99982          |
| typeshed     | 0.99953          |
| warehouse    | 0.99648          |
| zulip        | 0.99928          |


```diff
< index 6472412c14..958f037f6e 100644
---
> index 6472412c14..79c287202d 100644
764,772c764
< @@ -329,6 +329,7 @@ class QuerySet(AltersData):
<          return manager
<  
<      as_manager.queryset_only = True
< +
<      as_manager = classmethod(as_manager)
<  
<      ########################
< @@ -1388,9 +1389,7 @@ class QuerySet(AltersData):
---
> @@ -1388,9 +1388,7 @@ class QuerySet(AltersData):
783c775
< @@ -1795,9 +1794,8 @@ class QuerySet(AltersData):
---
> @@ -1795,9 +1793,8 @@ class QuerySet(AltersData):
1183,1194d1174
< diff --git a/django/utils/datastructures.py b/django/utils/datastructures.py
< index ded606d02a..e0f6401e1c 100644
< --- a/django/utils/datastructures.py
< +++ b/django/utils/datastructures.py
< @@ -240,6 +240,7 @@ class ImmutableList(tuple):
<  
<      # All list mutation functions complain.
<      __delitem__ = complain
< +
<      __delslice__ = complain
<      __iadd__ = complain
<      __imul__ = complain
1963,1974d1942
< diff --git a/tests/model_formsets_regress/tests.py b/tests/model_formsets_regress/tests.py
< index 0ccc2c0490..733cb962e0 100644
< --- a/tests/model_formsets_regress/tests.py
< +++ b/tests/model_formsets_regress/tests.py
< @@ -442,6 +442,7 @@ class FormfieldShouldDeleteFormTests(TestCase):
<      NormalFormset = modelformset_factory(
<          User, form=CustomDeleteUserForm, can_delete=True
<      )
< +
<      DeleteFormset = modelformset_factory(
<          User, form=CustomDeleteUserForm, formset=BaseCustomDeleteModelFormSet
<      )
2034c2002
< index 604136b5ac..972befcba4 100644
---
> index 604136b5ac..92c6a879fc 100644
2045,2052d2012
< @@ -55,6 +56,7 @@ class MyPerson(Person):
<          permissions = (("display_users", "May display users information"),)
<  
<      objects = SubManager()
< +
<      other = PersonManager()
<  
<      def has_special_name(self):
```

---

_Merged by @MichaReiser on 2023-08-30 06:20_

---

_Closed by @MichaReiser on 2023-08-30 06:20_

---

_Branch deleted on 2023-08-30 06:20_

---
