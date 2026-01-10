```yaml
number: 6999
title: Treat empty-line separated comments as trailing statement comments
type: pull_request
state: merged
author: charliermarsh
labels:
  - formatter
assignees: []
merged: true
base: main
head: charlie/newlines
created_at: 2023-08-30T02:15:14Z
updated_at: 2025-10-27T10:25:27Z
url: https://github.com/astral-sh/ruff/pull/6999
synced_at: 2026-01-10T16:59:49Z
```

# Treat empty-line separated comments as trailing statement comments

---

_Pull request opened by @charliermarsh on 2023-08-30 02:15_

## Summary

This PR modifies our between-statement comment handling such that comments that are not separated by a statement by any newlines continue to be treated as leading comments on the statement, but comments that _are_ separated are instead formatted as trailing comments on the preceding statement.

See, e.g., the originating snippet:

```python
DEFAULT_TEMPLATE = "flatpages/default.html"

# This view is called from FlatpageFallbackMiddleware.process_response
# when a 404 is raised, which often means CsrfViewMiddleware.process_view
# has not been called even if CsrfViewMiddleware is installed. So we need
# to use @csrf_protect, in case the template needs {% csrf_token %}.
# However, we can't just wrap this view; if no matching flatpage exists,
# or a redirect is required for authentication, the 404 needs to be returned
# without any CSRF checks. Therefore, we only
# CSRF protect the internal implementation.


def flatpage(request, url):
    pass
```

Here, we need to ensure that the `def flatpage` is precede by two empty lines. However, we want those two empty lines to be enforced from the _end_ of the comment block, _unless_ the comments are directly atop the `def flatpage`.

I played with this a bit, and I think the simplest conceptual model and implementation is to instead treat those as trailing comments on the preceding node. The main difficulty with this approach is that, in order to be fully compatible with Black, we'd sometimes need to insert newlines _between_ the preceding node and its trailing comments. See, e.g.:

```python
def func():
    ...
# comment

x = 1
```

In this case, we'd need to insert two blank lines between `def func(): ...` and `# comment`, but `# comment` is trailing comment on `def func(): ...`. So, we'd need to take this case into account in the various nodes that _require_ newlines after them: functions, classes, and imports. After some discussion, we've opted _not_ to support this, and just treat these as trailing comments -- so we won't insert newlines there. This means our handling is still identical to Black's on Black-formatted code, but avoids moving such trailing comments on unformatted code.

I dislike that the empty handling is so complex, and that it's split between so many different nodes, but this is really tricky. Continuing to treat these as leading comments is very difficult too, since we'd need to do similar tricks for the leading comment handling in those nodes, and influencing leading comments is even harder, since they're all formatted _before_ the node itself.

Closes https://github.com/astral-sh/ruff/issues/6761.

## Test Plan

`cargo test`

Surprisingly, it doesn't change the similarity at all (apart from a 0.00001 change in CPython), but I manually confirmed that it did fix the originating issue in Django.

Before:

| project      | similarity index |
|--------------|------------------|
| cpython      | 0.76082          |
| django       | 0.99921          |
| transformers | 0.99854          |
| twine        | 0.99982          |
| typeshed     | 0.99953          |
| warehouse    | 0.99648          |
| zulip        | 0.99928          |


After:

| project      | similarity index |
|--------------|------------------|
| cpython      | 0.76081          |
| django       | 0.99921          |
| transformers | 0.99854          |
| twine        | 0.99982          |
| typeshed     | 0.99953          |
| warehouse    | 0.99648          |
| zulip        | 0.99928          |


---

_@charliermarsh reviewed on 2023-08-30 02:19_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/tests/snapshots/format@fmt_on_off__mixed_space_and_tab.py.snap`:50 on 2023-08-30 02:19_

This matches Black.

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/tests/snapshots/black_compatibility@simple_cases__fmtpass_imports.py.snap`:39 on 2023-08-30 02:19_

This deviates from Black, but I feel like Black is wrong here?

Given:
```python
import dataclasses
# fmt: off
import os
# fmt: on
```

Why omit the empty line after `import dataclasses`? (Black would insert an empty line for a regular comment.)

---

_@charliermarsh reviewed on 2023-08-30 02:19_

---

_Label `formatter` added by @charliermarsh on 2023-08-30 02:20_

---

_@charliermarsh reviewed on 2023-08-30 03:28_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/comments/placement.rs`:1907 on 2023-08-30 03:28_

This omission looks like a bug.

---

_Comment by @charliermarsh on 2023-08-30 03:33_

Here's the diff-of-diffs, of main-formatting vs. this branch on Django:

```diff
diff --git a/django/contrib/flatpages/views.py b/django/contrib/flatpages/views.py
index 105eb90ee4..776f1796a6 100644
--- a/django/contrib/flatpages/views.py
+++ b/django/contrib/flatpages/views.py
@@ -9,7 +9,6 @@ from django.views.decorators.csrf import csrf_protect
 
 DEFAULT_TEMPLATE = "flatpages/default.html"
 
-
 # This view is called from FlatpageFallbackMiddleware.process_response
 # when a 404 is raised, which often means CsrfViewMiddleware.process_view
 # has not been called even if CsrfViewMiddleware is installed. So we need
diff --git a/django/contrib/gis/db/backends/oracle/models.py b/django/contrib/gis/db/backends/oracle/models.py
index 27e08a37c8..289c8139b1 100644
--- a/django/contrib/gis/db/backends/oracle/models.py
+++ b/django/contrib/gis/db/backends/oracle/models.py
@@ -17,7 +17,6 @@ class OracleGeometryColumns(models.Model):
     table_name = models.CharField(max_length=32)
     column_name = models.CharField(max_length=1024)
     srid = models.IntegerField(primary_key=True)
-
     # TODO: Add support for `diminfo` column (type MDSYS.SDO_DIM_ARRAY).
 
     class Meta:
diff --git a/django/db/models/base.py b/django/db/models/base.py
index 5d60af5687..8c8f2a28fd 100644
--- a/django/db/models/base.py
+++ b/django/db/models/base.py
@@ -2557,7 +2557,6 @@ class Model(AltersData, metaclass=ModelBase):
 # HELPER FUNCTIONS (CURRIED MODEL METHODS) #
 ############################################
 
-
 # ORDERING METHODS #########################
 
 
diff --git a/django/db/models/sql/query.py b/django/db/models/sql/query.py
index 14adcaea88..87e1f58d1c 100644
--- a/django/db/models/sql/query.py
+++ b/django/db/models/sql/query.py
@@ -1835,7 +1835,6 @@ class Query(BaseExpression):
         key field for example).
         """
         joins = [alias]
-
         # The transform can't be applied yet, as joins must be trimmed later.
         # To avoid making every caller of this method look up transforms
         # directly, compute transforms here and create a partial that converts
diff --git a/django/forms/formsets.py b/django/forms/formsets.py
index cc5530f1e4..549f4e8626 100644
--- a/django/forms/formsets.py
+++ b/django/forms/formsets.py
@@ -321,7 +321,6 @@ class BaseFormSet(RenderableFormMixin):
                 if self.can_delete and self._should_delete_form(form):
                     continue
                 self._ordering.append((i, form.cleaned_data[ORDERING_FIELD_NAME]))
-
             # After we're done populating self._ordering, sort it.
             # A sort function to order things numerically ascending, but
             # None should be sorted below anything else. Allowing None as
diff --git a/django/forms/models.py b/django/forms/models.py
index 4db1717a61..edf8aa03fb 100644
--- a/django/forms/models.py
+++ b/django/forms/models.py
@@ -960,7 +960,6 @@ class BaseModelFormSet(BaseFormSet, AltersData):
         from django.db.models import AutoField, ForeignKey, OneToOneField
 
         self._pk_field = pk = self.model._meta.pk
-
         # If a pk isn't editable, then it won't be on the form, so we need to
         # add it here so we can tell which object is which when we get the
         # data back. Generally, pk.editable should be false, but for some
diff --git a/django/template/smartif.py b/django/template/smartif.py
index acbd6e408e..5b15a5a476 100644
--- a/django/template/smartif.py
+++ b/django/template/smartif.py
@@ -1,8 +1,6 @@
 """
 Parser and utilities for the smart 'if' tag
 """
-
-
 # Using a simple top down parser, as described here:
 #    http://effbot.org/zone/simple-top-down-parsing.htm.
 # 'led' = left denotation
diff --git a/django/utils/timezone.py b/django/utils/timezone.py
index 7c65a03d73..102562b254 100644
--- a/django/utils/timezone.py
+++ b/django/utils/timezone.py
@@ -81,7 +81,6 @@ def _get_timezone_name(timezone):
 
 # Timezone selection functions.
 
-
 # These functions don't change os.environ['TZ'] and call time.tzset()
 # because it isn't thread safe.
 
diff --git a/tests/base/models.py b/tests/base/models.py
index b179878704..70088bd96e 100644
--- a/tests/base/models.py
+++ b/tests/base/models.py
@@ -1,6 +1,5 @@
 from django.db import models
 
-
 # The models definitions below used to crash. Generating models dynamically
 # at runtime is a bad idea because it pollutes the app registry. This doesn't
 # integrate well with the test suite but at least it prevents regressions.
diff --git a/tests/delete_regress/tests.py b/tests/delete_regress/tests.py
index 7aea5948f6..71e3bcb405 100644
--- a/tests/delete_regress/tests.py
+++ b/tests/delete_regress/tests.py
@@ -162,7 +162,6 @@ class LargeDeleteTests(TestCase):
         """
         for x in range(300):
             Book.objects.create(pagecount=x + 100)
-
         # attach a signal to make sure we will not fast-delete
 
         def noop(*args, **kwargs):
diff --git a/tests/files/tests.py b/tests/files/tests.py
index 63022df75a..b3478d2732 100644
--- a/tests/files/tests.py
+++ b/tests/files/tests.py
@@ -295,7 +295,6 @@ class DimensionClosingBug(unittest.TestCase):
         """
         get_image_dimensions() called with a filename should closed the file.
         """
-
         # We need to inject a modified open() builtin into the images module
         # that checks if the file was closed properly if the function is
         # called with a filename instead of a file object.
diff --git a/tests/foreign_object/tests.py b/tests/foreign_object/tests.py
index 147e0d73ad..c9e8da5792 100644
--- a/tests/foreign_object/tests.py
+++ b/tests/foreign_object/tests.py
@@ -23,7 +23,6 @@ from .models import (
     Person,
 )
 
-
 # Note that these tests are testing internal implementation details.
 # ForeignObject is not part of public API.
 
diff --git a/tests/generic_inline_admin/tests.py b/tests/generic_inline_admin/tests.py
index 284e94d1cd..13819725d6 100644
--- a/tests/generic_inline_admin/tests.py
+++ b/tests/generic_inline_admin/tests.py
@@ -379,7 +379,6 @@ class GenericInlineModelAdminTest(SimpleTestCase):
         `ModelAdmin.exclude` or `GenericInlineModelAdmin.exclude` are defined.
         Refs #15907.
         """
-
         # First with `GenericInlineModelAdmin`  -----------------
 
         class MediaForm(ModelForm):
diff --git a/tests/generic_views/views.py b/tests/generic_views/views.py
index 2d1f314901..5348c67632 100644
--- a/tests/generic_views/views.py
+++ b/tests/generic_views/views.py
@@ -285,7 +285,6 @@ class CustomSingleObjectView(generic.detail.SingleObjectMixin, generic.View):
 class BookSigningConfig:
     model = BookSigning
     date_field = "event_date"
-
     # use the same templates as for books
 
     def get_template_names(self):
diff --git a/tests/model_inheritance/models.py b/tests/model_inheritance/models.py
index ec7640477a..47aae186e0 100644
--- a/tests/model_inheritance/models.py
+++ b/tests/model_inheritance/models.py
@@ -13,7 +13,6 @@ Both styles are demonstrated here.
 """
 from django.db import models
 
-
 #
 # Abstract base classes
 #
diff --git a/tests/model_options/models/tablespaces.py b/tests/model_options/models/tablespaces.py
index 2a908d900b..19bfd31890 100644
--- a/tests/model_options/models/tablespaces.py
+++ b/tests/model_options/models/tablespaces.py
@@ -1,6 +1,5 @@
 from django.db import models
 
-
 # Since the test database doesn't have tablespaces, it's impossible for Django
 # to create the tables for models where db_tablespace is set. To avoid this
 # problem, we mark the models as unmanaged, and temporarily revert them to
diff --git a/tests/proxy_models/models.py b/tests/proxy_models/models.py
index 972befcba4..5fcdf4de04 100644
--- a/tests/proxy_models/models.py
+++ b/tests/proxy_models/models.py
@@ -6,7 +6,6 @@ providing a modified interface to the data from the base class.
 """
 from django.db import models
 
-
 # A couple of managers for testing managing overriding in proxy model cases.
 
 
diff --git a/tests/schema/models.py b/tests/schema/models.py
index 56dbcea5be..75e32a0eab 100644
--- a/tests/schema/models.py
+++ b/tests/schema/models.py
@@ -75,7 +75,6 @@ class Book(models.Model):
     author = models.ForeignKey(Author, models.CASCADE)
     title = models.CharField(max_length=100, db_index=True)
     pub_date = models.DateTimeField()
-
     # tags = models.ManyToManyField("Tag", related_name="books")
 
     class Meta:
diff --git a/tests/select_related/models.py b/tests/select_related/models.py
index e229b2bdec..d407dbdb11 100644
--- a/tests/select_related/models.py
+++ b/tests/select_related/models.py
@@ -11,7 +11,6 @@ from django.contrib.contenttypes.fields import GenericForeignKey, GenericRelatio
 from django.contrib.contenttypes.models import ContentType
 from django.db import models
 
-
 # Who remembers high school biology?
 
 
diff --git a/tests/serializers/models/data.py b/tests/serializers/models/data.py
index 8cf93cebff..3d863a3fb2 100644
--- a/tests/serializers/models/data.py
+++ b/tests/serializers/models/data.py
@@ -252,7 +252,6 @@ class SmallPKData(models.Model):
 # class TextPKData(models.Model):
 #     data = models.TextField(primary_key=True)
 
-
 # class TimePKData(models.Model):
 #    data = models.TimeField(primary_key=True)
 
diff --git a/tests/serializers/test_data.py b/tests/serializers/test_data.py
index 183598d3c3..e1cb776d83 100644
--- a/tests/serializers/test_data.py
+++ b/tests/serializers/test_data.py
@@ -74,7 +74,6 @@ from .models import (
 )
 from .tests import register_tests
 
-
 # A set of functions that can be used to recreate
 # test data objects of various kinds.
 # The save method is a raw base model save, to make
diff --git a/tests/unmanaged_models/models.py b/tests/unmanaged_models/models.py
index 6979109669..0eefcafda2 100644
--- a/tests/unmanaged_models/models.py
+++ b/tests/unmanaged_models/models.py
@@ -5,7 +5,6 @@ is generated for the table on various manage.py operations.
 
 from django.db import models
 
-
 #  All of these models are created in the database by Django.
```

---

_Comment by @charliermarsh on 2023-08-30 03:35_

I spot-checked these and they all seem correct. Do empty line changes not impact the Jaccard index?

---

_Marked ready for review by @charliermarsh on 2023-08-30 03:35_

---

_Review requested from @MichaReiser by @charliermarsh on 2023-08-30 03:35_

---

_Comment by @MichaReiser on 2023-08-30 06:22_

> I spot-checked these and they all seem correct. Do empty line changes not impact the Jaccard index?

They should but probably less than other changes because the diff is only a single line. Changes that affect how we split code often results in many line differences for each case which is why they are more likely to affect the jaccard index. 

That's why we should add a number of files that differ. 

Edit: After rebasing on top of #7003

This PR 

| project      | similarity index  | total files       | changed files     |
|--------------|------------------:|------------------:|------------------:|
| cpython      |           0.76082 |              1789 |              1633 |
| django       |           0.99922 |              2760 |               100 |
| transformers |           0.99855 |              2587 |               565 |
| twine        |           0.99982 |                33 |                 1 |
| typeshed     |           0.99953 |              3496 |              2174 |
| warehouse    |           0.99648 |               648 |                27 |
| zulip        |           0.99928 |              1437 |                44 |

Base

| project      | similarity index  | total files       | changed files     |
|--------------|------------------:|------------------:|------------------:|
| cpython      |           0.76082 |              1789 |              1634 |
| django       |           0.99922 |              2760 |               116 |
| transformers |           0.99855 |              2587 |               586 |
| twine        |           0.99982 |                33 |                 2 |
| typeshed     |           0.99953 |              3496 |              2173 |
| warehouse    |           0.99648 |               648 |                41 |
| zulip        |           0.99928 |              1437 |                54 |

This PR reduces the number of changed files for every project.


---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/format.rs`:515 on 2023-08-30 06:44_

Can you add a test where a comment has like 10 empty lines before. I expect line 410 to panic because 10..2 is an invalid range.

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/placement.rs`:1907 on 2023-08-30 06:46_

Can you explain what you mean by it? What's a bug? Is it a bug in the implementation? Which omission? 

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/stmt_import.rs`:36 on 2023-08-30 06:49_

Can't import have up to two empty lines? 

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/tests/snapshots/black_compatibility@simple_cases__fmtpass_imports.py.snap`:39 on 2023-08-30 06:51_

This seems fine but could we add some more `fmt: off` tests to ensure suppression works well with your changes? 

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/stmt_class_def.rs`:141 on 2023-08-30 06:53_

NIt
```suggestion
        .fmt(f)
```

---

_@MichaReiser approved on 2023-08-30 07:02_

Looks good. A bit worrying that the newline rules are now spread across `suite.rs`, comment formatting, and individual statements.


I noticed that we often emit a hard line break + two empty lines which is a no-op (at least the hard line break) but that seems to have existed already before your change. 

https://play.ruff.rs/26a51663-123d-4f55-a57a-5b6e9820a15a

---

_Comment by @charliermarsh on 2023-08-30 13:34_

I woke up realizing that this won't work for:

```python
import os # foo
# bar

x =1
```

Since we'll insert the empty lines before formatting `# foo`, like:

```python
import os

  # foo
# bar

x = 1
```

---

_Comment by @charliermarsh on 2023-08-30 13:35_

I am wondering if I instead need to move this logic into `trailing_comments` somehow.

---

_@MichaReiser requested changes on 2023-08-30 14:05_

Putting back in your queue since you discovered a change :) This makes it easier for me to know when the PR is ready to re-review

---

_@charliermarsh reviewed on 2023-08-30 15:43_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/comments/format.rs`:515 on 2023-08-30 15:43_

Apparently that's actually fine? It doesn't panic.

---

_@charliermarsh reviewed on 2023-08-30 15:44_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/comments/placement.rs`:1907 on 2023-08-30 15:44_

If the _only_ tokens in the contents were newlines, we'd _never_ set `max_new_lines` -- we'd return 0!

---

_Comment by @charliermarsh on 2023-08-30 15:46_

Alright, this is a compromise solution...

We _do_ insert requisite newlines after functions or classes:

```python
def func():
    pass
# this comment gets two empty lines inserted _before_ it

x = 1
```

But we _don't_ insert any newlines for imports:
```python
import x
# this comment does _not_, it stays where it is

import y
```

This works because functions and classes can't have trailing end-of-line comments, so they don't suffer from the problem above. However, we still get improved compatibility. I think it's also a reasonable deviation: in the function and class case, the comment follows the indented body, so it's harder to argue that it's "part of" the function or class; for the import, it immediately follows the import, so it's reasonable to treat it as such.


---

_Review requested from @MichaReiser by @charliermarsh on 2023-08-30 15:47_

---

_Comment by @charliermarsh on 2023-08-30 15:49_

@MichaReiser - We can also just remove that behavior for classes and functions. I don't feel strongly. It will reduce compatibility for unformatted code (and thus add lines back to the test snapshots), but it's not unreasonable.

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/format.rs`:515 on 2023-08-30 15:50_

I love how confident you are. This puts me at ease.

---

_@MichaReiser reviewed on 2023-08-30 15:51_

> @MichaReiser - We can also just remove that behavior for classes and functions. I don't feel strongly. It will reduce compatibility for unformatted code (and thus add lines back to the test snapshots), but it's not unreasonable.

I think I would prefer that. It means fewer weird cases to reason about. Should we do it as a separate PR?

I"ll review the rest tomorrow. I don't feel up to it today 

---

_Comment by @charliermarsh on 2023-08-30 15:52_

I can do it in the same PR.

---

_Comment by @charliermarsh on 2023-08-30 15:53_

I'm very sad because it means `tests/snapshots/black_compatibility@simple_cases__empty_lines.py.snap` doesn't get deleted due to a one-line difference :sob:

---

_@charliermarsh reviewed on 2023-08-30 15:56_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/tests/snapshots/black_compatibility@simple_cases__empty_lines.py.snap`:166 on 2023-08-30 15:56_

The difference in this file is now just that we aren't adding the extra empty line before this comment (which trails a function).

---

_Review requested from @MichaReiser by @charliermarsh on 2023-08-30 15:57_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/comments/format.rs`:515 on 2023-08-30 15:58_

Haha. I confirmed that when `actual` is 4 and `self.expected` is 2, it doesn't panic!

---

_@charliermarsh reviewed on 2023-08-30 15:58_

---

_@MichaReiser reviewed on 2023-08-30 16:01_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/tests/snapshots/black_compatibility@simple_cases__empty_lines.py.snap`:166 on 2023-08-30 16:01_

This is an interesting example and is somewhat different from the example that we discussed

```python
class Test: ...
# bar

x =1
```

Because the comment is separated by one line from the class. I can see why black adds two empty because the comment is clearly separate from the preceding statement (not directly adjacent). But implementing this is probably even more complicated than what you had initially. 

---

_@charliermarsh reviewed on 2023-08-30 16:04_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/tests/snapshots/black_compatibility@simple_cases__empty_lines.py.snap`:166 on 2023-08-30 16:04_

Ah yeah, I was thinking of this as the same case throughout that discussion since my mind was focused on the implementation. This why I'm somewhat partial to inserting the newlines for these trailing comments (whether there's an existing empty line or not) on classes and functions at least.

---

_@charliermarsh reviewed on 2023-08-30 16:11_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/tests/snapshots/black_compatibility@simple_cases__empty_lines.py.snap`:166 on 2023-08-30 16:11_

I am leaning towards re-adding this behavior for classes and functions.

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/placement.rs`:1907 on 2023-08-31 06:19_

Ah yeah, I noticed that once too but It blew up all the tests when I tried to fix it

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/suite.rs`:150 on 2023-08-31 06:21_

Should the class always add the new lines rather than making it conditional on the trailing comments. It would allow removing this check and allows us to have the logic in a single place.

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/suite.rs`:199 on 2023-08-31 06:21_

Why is it necessary to check whether the next node has leading comments but is not for classes and functions?

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/suite.rs`:291 on 2023-08-31 06:24_

We now look up the comments of every node twice: Once on line 308 and once here. Would it make sense to cache the comments similar to `preceding` (`preceding_commens`)?

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/tests/snapshots/black_compatibility@simple_cases__empty_lines.py.snap`:166 on 2023-08-31 06:25_

Ideal solution: Only add newlines before comments with at least one empty line.
Okay solution: Reverting to what you had.

---

_@MichaReiser approved on 2023-08-31 06:26_

This looks good. Please add some more tests around `fmt:off`.

---

_@charliermarsh reviewed on 2023-08-31 15:20_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/statement/suite.rs`:150 on 2023-08-31 15:20_

Would that not require us to know if the class is the last statement in the suite, to avoid adding unnecessary trailing newlines?

---

_@MichaReiser reviewed on 2023-08-31 15:33_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/suite.rs`:150 on 2023-08-31 15:33_

I see. Could be worth explaining this as part of the comment. 

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/tests/snapshots/black_compatibility@simple_cases__empty_lines.py.snap`:166 on 2023-08-31 20:44_

Alright, I'm reverting to what I had, but I'm going to look into a followup PR to move the comment handling into `suite.rs` and also enable the behavior we discussed here.

---

_@charliermarsh reviewed on 2023-08-31 20:44_

---

_Merged by @charliermarsh on 2023-08-31 20:55_

---

_Closed by @charliermarsh on 2023-08-31 20:55_

---

_Branch deleted on 2023-08-31 20:55_

---

_@MichaReiser reviewed on 2025-10-27 10:25_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/resources/test/fixtures/ruff/fmt_on_off/indent.py`:1 on 2025-10-27 10:25_

Huh, why did we delete the content of this test?

---
