```yaml
number: 8465
title: Indent lambda parameters if parameters wrap
type: pull_request
state: closed
author: MichaReiser
labels:
  - formatter
  - preview
assignees: []
draft: true
base: main
head: indent-lambda-params
created_at: 2023-11-03T05:20:29Z
updated_at: 2024-04-08T07:03:37Z
url: https://github.com/astral-sh/ruff/pull/8465
synced_at: 2026-01-12T15:55:26Z
```

# Indent lambda parameters if parameters wrap

---

_@MichaReiser_

## Summary

A non-black compatible approach to https://github.com/astral-sh/ruff/issues/8179

The black-compatible formatting would enforce spaces when formatting parameters with the `ParameterParentheses::Never`. However, this doesn't work when using comments (which black doesn't support [playground](https://black.vercel.app/?version=stable&state=_Td6WFoAAATm1rRGAgAhARYAAAB0L-Wj4ACOAGBdAD2IimZxl1N_WhQ8R5sgERcjE4CA79XBQnYhkNIGX378uPpzJVCf3opcwNZ5N4_s5GR-d6S0M12O66BmJ9d2BIpKmRsn-5XXsdd5l9RAxvUQiFV4AOsopTANF-mxwYxCAADlTL2qgfqd_wABfI8BAAAAQVm_NLHEZ_sCAAAAAARZWg==)?)

This PR implements two changes (we may want to split it in two):

a) It indents the lambda parameters if they don't fit on a single line: 

```python
def a():
    return b(
        c,
        d,
        e,
        f=lambda 
            self,
            *args,
            **kwargs
        : aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa(*args, **kwargs),
        b=d,
        e=f,
    )

```

b) Preview style that parenthesizes the lambda body if it expands. 

```diff
         return {
             **super().get_event_triggers(),
             EventTriggers.ON_CHANGE: (
-                lambda e0: [
-                    Var.create_safe(f"{e0}.map(e => e.value)", _var_is_local=True)
-                ]
-                if self.is_multi.equals(Var.create_safe(True))
-                else lambda e0: [e0]
+                lambda e0: (
+                    [Var.create_safe(f"{e0}.map(e => e.value)", _var_is_local=True)]
+                    if self.is_multi.equals(Var.create_safe(True))
+                    else lambda e0: [e0]
+                )
             ),
         }
```


## Alternatives

Favor black compatibility and enforce using `space` in the parameters formatting when the `ParameterParentheses::Never` is used. Requires finding a comment placement that produces stable results. 

Unclear how we want to support 

```python
(
    lambda
    x,
    # comment
    y:
    z
)
```

## Notes

It's probably worth to split out the body changes. They seem to generally improve readability

## Test Plan

The non-preview changes don't seem to change the similarity index (at least after only indenting if there are 2 or more parameters). 

The preview changes regress the similarity index across the board

Main
| project        | similarity index  | total files       | changed files     |
|----------------|------------------:|------------------:|------------------:|
| cpython        |           0.75804 |              1799 |              1648 |
| django         |           0.99984 |              2772 |                34 |
| home-assistant |           0.99963 |             10596 |               143 |
| poetry         |           0.99925 |               317 |                12 |
| transformers   |           0.99967 |              2657 |               322 |
| twine          |           1.00000 |                33 |                 0 |
| typeshed       |           0.99980 |              3669 |                18 |
| warehouse      |           0.99977 |               654 |                13 |
| zulip          |           0.99970 |              1459 |                21 |



Preview

| project        | similarity index  | total files       | changed files     |
|----------------|------------------:|------------------:|------------------:|
| cpython        |           0.75804 |              1799 |              1648 |
| django         |           0.99978 |              2772 |                40 |
| home-assistant |           0.99957 |             10596 |               174 |
| poetry         |           0.99925 |               317 |                12 |
| transformers   |           0.99965 |              2657 |               328 |
| twine          |           1.00000 |                33 |                 0 |
| typeshed       |           0.99980 |              3669 |                18 |
| warehouse      |           0.99968 |               654 |                16 |
| zulip          |           0.99970 |              1459 |                21 |

Here a few examples

```diff
index 9aa78a281f..499bc0dfb9 100644
--- a/django/db/models/expressions.py
+++ b/django/db/models/expressions.py
@@ -366,21 +366,21 @@ class BaseExpression:
         internal_type = field.get_internal_type()
         if internal_type == "FloatField":
             return (
-                lambda value, expression, connection: None
-                if value is None
-                else float(value)
+                lambda value, expression, connection: (
+                    None if value is None else float(value)
+                )
             )
         elif internal_type.endswith("IntegerField"):
             return (
-                lambda value, expression, connection: None
-                if value is None
-                else int(value)
+                lambda value, expression, connection: (
+                    None if value is None else int(value)
+                )
             )
         elif internal_type == "DecimalField":
             return (
-                lambda value, expression, connection: None
-                if value is None
-                else Decimal(value)
+                lambda value, expression, connection: (
+                    None if value is None else Decimal(value)
+                )
             )
         return self._convert_value_noop
 
index 889c1cfe84..8555f08cb3 100644
--- a/django/contrib/gis/db/models/fields.py
+++ b/django/contrib/gis/db/models/fields.py
@@ -48,9 +48,9 @@ def get_srid_info(srid, connection):
     alias, get_srs = (
         (
             connection.alias,
-            lambda srid: SpatialRefSys.objects.using(connection.alias)
-            .get(srid=srid)
-            .srs,
+            lambda srid: (
+                SpatialRefSys.objects.using(connection.alias).get(srid=srid).srs
+            ),
         )
         if SpatialRefSys
         else (None, SpatialReference)


index dc857055b1..b45a8bb10e 100644
--- a/django/contrib/admin/tests.py
+++ b/django/contrib/admin/tests.py
@@ -110,8 +110,9 @@ class AdminSeleniumTestCase(SeleniumTestCase, StaticLiveServerTestCase):
         Block until the  page is ready.
         """
         self.wait_until(
-            lambda driver: driver.execute_script("return document.readyState;")
-            == "complete",
+            lambda driver: (
+                driver.execute_script("return document.readyState;") == "complete"
+            ),
             timeout,
         )
 
@@ -196,8 +197,8 @@ class AdminSeleniumTestCase(SeleniumTestCase, StaticLiveServerTestCase):
             # to be the case.
             with self.disable_implicit_wait():
                 self.wait_until(
-                    lambda driver: not driver.find_elements(
-                        By.CSS_SELECTOR, options_selector
+                    lambda driver: (
+                        not driver.find_elements(By.CSS_SELECTOR, options_selector)
                     )
                 )

index 9855c318be..fd7770eb2e 100644
--- a/django/test/testcases.py
+++ b/django/test/testcases.py
@@ -1470,8 +1470,10 @@ def skipIfDBFeature(*features):
 def skipUnlessDBFeature(*features):
     """Skip a test unless a database has all the named features."""
     return _deferredSkip(
-        lambda: not all(
-            getattr(connection.features, feature, False) for feature in features
+        lambda: (
+            not all(
+                getattr(connection.features, feature, False) for feature in features
+            )
         ),
         "Database doesn't support feature(s): %s" % ", ".join(features),
         "skipUnlessDBFeature",
@@ -1481,8 +1483,10 @@ def skipUnlessDBFeature(*features):
 def skipUnlessAnyDBFeature(*features):
     """Skip a test unless a database has any of the named features."""
     return _deferredSkip(
-        lambda: not any(
-            getattr(connection.features, feature, False) for feature in features
+        lambda: (
+            not any(
+                getattr(connection.features, feature, False) for feature in features
+            )
         ),
         "Database doesn't support any of the feature(s): %s" % ", ".join(features),
         "skipUnlessAnyDBFeature",

index b9a7e4cd64..a69ebd31cb 100644
--- a/tests/absolute_url_overrides/tests.py
+++ b/tests/absolute_url_overrides/tests.py
@@ -29,8 +29,9 @@ class AbsoluteUrlOverrideTests(SimpleTestCase):
 
         with self.settings(
             ABSOLUTE_URL_OVERRIDES={
-                "absolute_url_overrides.testb": lambda o: "/overridden-test-b/%s/"
-                % o.pk,
+                "absolute_url_overrides.testb": lambda o: (
+                    "/overridden-test-b/%s/" % o.pk
+                ),
             },
         ):

index 4d85d15065..9aec0a68e8 100644
--- a/tests/contenttypes_tests/test_views.py
+++ b/tests/contenttypes_tests/test_views.py
@@ -151,9 +151,9 @@ class ContentTypesViewsSiteRelTests(TestCase):
         The shortcut view works if a model's ForeignKey to site is None.
         """
         get_model.side_effect = (
-            lambda *args, **kwargs: MockSite
-            if args[0] == "sites.Site"
-            else ModelWithNullFKToSite
+            lambda *args, **kwargs: (
+                MockSite if args[0] == "sites.Site" else ModelWithNullFKToSite
+            )
         )
 
         obj = ModelWithNullFKToSite.objects.create(title="title")
@@ -173,9 +173,9 @@ class ContentTypesViewsSiteRelTests(TestCase):
         found in the m2m relationship.
         """
         get_model.side_effect = (
-            lambda *args, **kwargs: MockSite
-            if args[0] == "sites.Site"
-            else ModelWithM2MToSite
+            lambda *args, **kwargs: (
+                MockSite if args[0] == "sites.Site" else ModelWithM2MToSite
+            )
         )
```

Most of these seem clear improvements to me. 

---

_Comment by @MichaReiser on 2023-11-03 05:20_

Current dependencies on/for this PR:
* **#8465** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/8465?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 👈
* **#8466** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/8466?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a>
* `main`

This [stack of pull requests](https://stacking.dev/?utm_source=stack-comment) is managed by [Graphite](https://app.graphite.dev/github/pr/astral-sh/ruff/8465?utm_source=stack-comment).

---

_@MichaReiser reviewed on 2023-11-03 07:42_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/tests/snapshots/format@expression__lambda.py.snap`:359 on 2023-11-03 07:42_

This was better before

---

_Comment by @github-actions[bot] on 2023-11-03 07:55_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
ℹ️ ecosystem check **detected format changes**. (+4 -4 lines in 1 file in 41 projects)

<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+4 -4 lines across 1 file)</summary>
<p>

<a href='https://github.com/rotki/rotki/blob/b7e3655e090b1d31d99078618e1a864430fe672c/rotkehlchen/data_import/importers/binance.py#L272'>rotkehlchen/data_import/importers/binance.py~L272</a>
```diff
 
         for rows_group in rows_grouped_by_fee.values():
             rows_group.sort(
-                key=lambda x: x["Change"]
-                if same_assets
-                else x["Change"] * price_at_timestamp[x["Coin"]],
+                key=lambda x: (
+                    x["Change"] if same_assets else x["Change"] * price_at_timestamp[x["Coin"]]
+                ),
                 reverse=True,
             )  # noqa: E501
 
```

</p>
</details>

### Formatter (preview)
ℹ️ ecosystem check **detected format changes**. (+91 -71 lines in 15 files in 41 projects)

<details><summary><a href="https://github.com/RasaHQ/rasa">RasaHQ/rasa</a> (+4 -4 lines across 1 file)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/RasaHQ/rasa/blob/0dd893a1b343b069cbd237c74ded10ffa984afb4/rasa/nlu/featurizers/sparse_featurizer/lexical_syntactic_featurizer.py#L86'>rasa/nlu/featurizers/sparse_featurizer/lexical_syntactic_featurizer.py~L86</a>
```diff
         "suffix2": lambda token: token.text[-2:],
         "suffix1": lambda token: token.text[-1:],
         "pos": lambda token: token.data.get(POS_TAG_KEY, None),
-        "pos2": lambda token: token.data.get(POS_TAG_KEY, [])[:2]
-        if POS_TAG_KEY in token.data
-        else None,
+        "pos2": lambda token: (
+            token.data.get(POS_TAG_KEY, [])[:2] if POS_TAG_KEY in token.data else None
+        ),
         "upper": lambda token: token.text.isupper(),
         "digit": lambda token: token.text.isdigit(),
     }
```

</p>
</details>
<details><summary><a href="https://github.com/aws/aws-sam-cli">aws/aws-sam-cli</a> (+16 -13 lines across 1 file)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/aws/aws-sam-cli/blob/e4c61da9d873bbad7c17056efdd41601511ea86f/samcli/lib/cli_validation/image_repository_validation.py#L72'>samcli/lib/cli_validation/image_repository_validation.py~L72</a>
```diff
 
             validators = [
                 Validator(
-                    validation_function=lambda: bool(image_repository)
-                    + bool(image_repositories)
-                    + bool(resolve_image_repos)
-                    > 1,
+                    validation_function=lambda: (
+                        bool(image_repository) + bool(image_repositories) + bool(resolve_image_repos) > 1
+                    ),
                     exception=click.BadOptionUsage(
                         option_name="--image-repositories",
                         ctx=ctx,
```
<a href='https://github.com/aws/aws-sam-cli/blob/e4c61da9d873bbad7c17056efdd41601511ea86f/samcli/lib/cli_validation/image_repository_validation.py#L84'>samcli/lib/cli_validation/image_repository_validation.py~L84</a>
```diff
                     ),
                 ),
                 Validator(
-                    validation_function=lambda: not guided
-                    and not (image_repository or image_repositories or resolve_image_repos)
-                    and required,
+                    validation_function=lambda: (
+                        not guided and not (image_repository or image_repositories or resolve_image_repos) and required
+                    ),
                     exception=click.BadOptionUsage(
                         option_name="--image-repositories",
                         ctx=ctx,
```
<a href='https://github.com/aws/aws-sam-cli/blob/e4c61da9d873bbad7c17056efdd41601511ea86f/samcli/lib/cli_validation/image_repository_validation.py#L94'>samcli/lib/cli_validation/image_repository_validation.py~L94</a>
```diff
                     ),
                 ),
                 Validator(
-                    validation_function=lambda: not guided
-                    and (
-                        image_repositories
-                        and not resolve_image_repos
-                        and not _is_all_image_funcs_provided(template_file, image_repositories, parameters_overrides)
+                    validation_function=lambda: (
+                        not guided
+                        and (
+                            image_repositories
+                            and not resolve_image_repos
+                            and not _is_all_image_funcs_provided(
+                                template_file, image_repositories, parameters_overrides
+                            )
+                        )
                     ),
                     exception=click.BadOptionUsage(
                         option_name="--image-repositories", ctx=ctx, message=image_repos_error_msg
```

</p>
</details>
<details><summary><a href="https://github.com/demisto/content">demisto/content</a> (+37 -27 lines across 6 files)</summary>
<p>
<pre>ruff format --exclude Packs/ThreatQ/Integrations/ThreatQ/ThreatQ.py --preview</pre>
</p>
<p>

<a href='https://github.com/demisto/content/blob/8ba55546fd9af9dec4d217788682349544fd5418/Packs/FiltersAndTransformers/Scripts/IfElif/IfElif.py#L74'>Packs/FiltersAndTransformers/Scripts/IfElif/IfElif.py~L74</a>
```diff
         if "list_compare" in flags:
 
             def to_deep_search(func):
-                return lambda x, y: (func(x, y) or (any(func(x, i) for i in y) if isinstance(y, list) else False))
+                return lambda x, y: func(x, y) or (any(func(x, i) for i in y) if isinstance(y, list) else False)
 
             self.comparison_operators = {k: to_deep_search(v) for k, v in self.comparison_operators.items()}
 
```
<a href='https://github.com/demisto/content/blob/8ba55546fd9af9dec4d217788682349544fd5418/Packs/FireEyeETP/Integrations/FireEyeETPEventCollector/FireEyeETPEventCollector.py#L315'>Packs/FireEyeETP/Integrations/FireEyeETPEventCollector/FireEyeETPEventCollector.py~L315</a>
```diff
                 last_run_ids: set[str] = {
                     item.get("id", "")
                     for item in filter(
-                        lambda item: datetime.fromisoformat(demisto.get(item, "attributes.meta.last_modified_on"))
-                        == last_run_time,
+                        lambda item: (
+                            datetime.fromisoformat(demisto.get(item, "attributes.meta.last_modified_on")) == last_run_time
+                        ),
                         events,
                     )
                 }
```
<a href='https://github.com/demisto/content/blob/8ba55546fd9af9dec4d217788682349544fd5418/Packs/FireEyeETP/Integrations/FireEyeETPEventCollector/FireEyeETPEventCollector.py#L329'>Packs/FireEyeETP/Integrations/FireEyeETPEventCollector/FireEyeETPEventCollector.py~L329</a>
```diff
                 last_run_ids = {
                     item.get("id", "")
                     for item in filter(
-                        lambda item: datetime.fromisoformat(demisto.get(item, "attributes.lastModifiedDateTime"))
-                        == last_run_time,
+                        lambda item: (
+                            datetime.fromisoformat(demisto.get(item, "attributes.lastModifiedDateTime")) == last_run_time
+                        ),
                         events,
                     )
                 }
```
<a href='https://github.com/demisto/content/blob/8ba55546fd9af9dec4d217788682349544fd5418/Packs/GigamonThreatINSIGHT/Integrations/GigamonThreatINSIGHT/GigamonThreatINSIGHT.py#L756'>Packs/GigamonThreatINSIGHT/Integrations/GigamonThreatINSIGHT/GigamonThreatINSIGHT.py~L756</a>
```diff
             result = getDetectionsInc(detectionClient, result, args)
 
     # filter out training detections
-    result["detections"] = list(filter(lambda detection: (detection["account_uuid"] != TRAINING_ACC), result["detections"]))
+    result["detections"] = list(filter(lambda detection: detection["account_uuid"] != TRAINING_ACC, result["detections"]))
 
     # Include the rules if they need to be included
     if "include" in args and "rules" in args["include"].split(","):
```
<a href='https://github.com/demisto/content/blob/8ba55546fd9af9dec4d217788682349544fd5418/Packs/MailListener/Integrations/MailListenerV2/MailListenerV2.py#L524'>Packs/MailListener/Integrations/MailListenerV2/MailListenerV2.py~L524</a>
```diff
 
     return re.sub(
         r"(?P<lseps>\s*)(?P<begin>-----BEGIN(.*?)-----)(?P<body>.*?)(?P<end>-----END(.*?)-----)(?P<tseps>\s*)",
-        lambda m: m.group("lseps").replace(" ", "\n")
-        + m.group("begin")
-        + m.group("body").replace(" ", "\n")
-        + m.group("end")
-        + m.group("tseps").replace(" ", "\n"),
+        lambda m: (
+            m.group("lseps").replace(" ", "\n")
+            + m.group("begin")
+            + m.group("body").replace(" ", "\n")
+            + m.group("end")
+            + m.group("tseps").replace(" ", "\n")
+        ),
         credentials,
         flags=re.DOTALL,
     )
```
<a href='https://github.com/demisto/content/blob/8ba55546fd9af9dec4d217788682349544fd5418/Packs/Slack/Integrations/Slack/Slack.py#L125'>Packs/Slack/Integrations/Slack/Slack.py~L125</a>
```diff
         users = json.loads(integration_context["users"])
         users_filter = list(
             filter(
-                lambda u: u.get("name", "").lower() == user_to_search
-                or u.get("profile", {}).get("email", "").lower() == user_to_search
-                or u.get("real_name", "").lower() == user_to_search,
+                lambda u: (
+                    u.get("name", "").lower() == user_to_search
+                    or u.get("profile", {}).get("email", "").lower() == user_to_search
+                    or u.get("real_name", "").lower() == user_to_search
+                ),
                 users,
             )
         )
```
<a href='https://github.com/demisto/content/blob/8ba55546fd9af9dec4d217788682349544fd5418/Packs/Slack/Integrations/Slack/Slack.py#L141'>Packs/Slack/Integrations/Slack/Slack.py~L141</a>
```diff
             cursor = response.get("response_metadata", {}).get("next_cursor")
             users_filter = list(
                 filter(
-                    lambda u: u.get("name", "").lower() == user_to_search
-                    or u.get("profile", {}).get("email", "").lower() == user_to_search
-                    or u.get("real_name", "").lower() == user_to_search,
+                    lambda u: (
+                        u.get("name", "").lower() == user_to_search
+                        or u.get("profile", {}).get("email", "").lower() == user_to_search
+                        or u.get("real_name", "").lower() == user_to_search
+                    ),
                     workspace_users,
                 )
             )
```
<a href='https://github.com/demisto/content/blob/8ba55546fd9af9dec4d217788682349544fd5418/Packs/Slack/Integrations/SlackV3/SlackV3.py#L173'>Packs/Slack/Integrations/SlackV3/SlackV3.py~L173</a>
```diff
     """
     users_filter = list(
         filter(
-            lambda u: u.get("name", "").lower() == user_to_search
-            or u.get("profile", {}).get("display_name", "").lower() == user_to_search
-            or u.get("profile", {}).get("email", "").lower() == user_to_search
-            or u.get("profile", {}).get("real_name", "").lower() == user_to_search,
+            lambda u: (
+                u.get("name", "").lower() == user_to_search
+                or u.get("profile", {}).get("display_name", "").lower() == user_to_search
+                or u.get("profile", {}).get("email", "").lower() == user_to_search
+                or u.get("profile", {}).get("real_name", "").lower() == user_to_search
+            ),
             users_list,
         )
     )
```

</p>
</details>
<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (+6 -4 lines across 1 file)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/ibis-project/ibis/blob/6538b70c5fd0c53df79b9454275b0d1aaa79bf84/ibis/backends/base/sql/alchemy/__init__.py#L979'>ibis/backends/base/sql/alchemy/__init__.py~L979</a>
```diff
     return (
         sg.parse_one(element.fullname, into=sg.exp.Table, read=dialect)
         .transform(
-            lambda node: node.__class__(this=node.this, quoted=True)
-            if isinstance(node, sg.exp.Identifier)
-            else node
+            lambda node: (
+                node.__class__(this=node.this, quoted=True)
+                if isinstance(node, sg.exp.Identifier)
+                else node
+            )
         )
         .sql(dialect)
     )
```

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+18 -13 lines across 4 files)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/pandas-dev/pandas/blob/0cdb37c445fc0a0a2144f981fd1acbf4f07d88ee/asv_bench/benchmarks/io/style.py#L66'>asv_bench/benchmarks/io/style.py~L66</a>
```diff
         self.st = self.df.style.apply(_apply_func, axis=1)
 
     def _style_classes(self):
-        classes = self.df.map(lambda v: ("cls-1" if v > 0 else ""))
+        classes = self.df.map(lambda v: "cls-1" if v > 0 else "")
         classes.index, classes.columns = self.df.index, self.df.columns
         self.st = self.df.style.set_td_classes(classes)
 
```
<a href='https://github.com/pandas-dev/pandas/blob/0cdb37c445fc0a0a2144f981fd1acbf4f07d88ee/pandas/core/array_algos/replace.py#L87'>pandas/core/array_algos/replace.py~L87</a>
```diff
         op = lambda x: operator.eq(x, b)
     else:
         op = np.vectorize(
-            lambda x: bool(re.search(b, x))
-            if isinstance(x, str) and isinstance(b, (str, Pattern))
-            else False
+            lambda x: (
+                bool(re.search(b, x))
+                if isinstance(x, str) and isinstance(b, (str, Pattern))
+                else False
+            )
         )
 
     # GH#32621 use mask to avoid comparing to NAs
```
<a href='https://github.com/pandas-dev/pandas/blob/0cdb37c445fc0a0a2144f981fd1acbf4f07d88ee/pandas/core/dtypes/common.py#L129'>pandas/core/dtypes/common.py~L129</a>
```diff
     Evaluate if the tipo is a subclass of the klasses
     and not a datetimelike.
     """
-    return lambda tipo: (
-        issubclass(tipo, klasses)
-        and not issubclass(tipo, (np.datetime64, np.timedelta64))
+    return (
+        lambda tipo: (
+            issubclass(tipo, klasses)
+            and not issubclass(tipo, (np.datetime64, np.timedelta64))
+        )
     )
 
 
```
<a href='https://github.com/pandas-dev/pandas/blob/0cdb37c445fc0a0a2144f981fd1acbf4f07d88ee/pandas/core/internals/array_manager.py#L403'>pandas/core/internals/array_manager.py~L403</a>
```diff
             Whether to copy the blocks
         """
         return self._get_data_subset(
-            lambda arr: is_numeric_dtype(arr.dtype)
-            or getattr(arr.dtype, "_is_numeric", False)
+            lambda arr: (
+                is_numeric_dtype(arr.dtype) or getattr(arr.dtype, "_is_numeric", False)
+            )
         )
 
     def copy(self, deep: bool | Literal["all"] | None = True) -> Self:
```

</p>
</details>
<details><summary><a href="https://github.com/reflex-dev/reflex">reflex-dev/reflex</a> (+6 -6 lines across 1 file)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/reflex-dev/reflex/blob/b313aaf3ef322e54c76bdf9ef6175ca583e36762/reflex/components/forms/multiselect.py#L310'>reflex/components/forms/multiselect.py~L310</a>
```diff
         return {
             **super().get_event_triggers(),
             EventTriggers.ON_CHANGE: (
-                lambda e0: [
-                    Var.create_safe(f"{e0}.map(e => e.value)", _var_is_local=True)
-                ]
-                if self.is_multi.equals(Var.create_safe(True))
-                else lambda e0: [e0]
+                lambda e0: (
+                    [Var.create_safe(f"{e0}.map(e => e.value)", _var_is_local=True)]
+                    if self.is_multi.equals(Var.create_safe(True))
+                    else lambda e0: [e0]
+                )
             ),
         }
 
```

</p>
</details>
<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+4 -4 lines across 1 file)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/rotki/rotki/blob/b7e3655e090b1d31d99078618e1a864430fe672c/rotkehlchen/data_import/importers/binance.py#L272'>rotkehlchen/data_import/importers/binance.py~L272</a>
```diff
 
         for rows_group in rows_grouped_by_fee.values():
             rows_group.sort(
-                key=lambda x: x["Change"]
-                if same_assets
-                else x["Change"] * price_at_timestamp[x["Coin"]],
+                key=lambda x: (
+                    x["Change"] if same_assets else x["Change"] * price_at_timestamp[x["Coin"]]
+                ),
                 reverse=True,
             )  # noqa: E501
 
```

</p>
</details>




---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/tests/snapshots/format@expression__lambda.py.snap`:395 on 2023-11-03 08:20_

Undecided if this looked better before.

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/tests/snapshots/format@expression__lambda.py.snap`:953 on 2023-11-03 08:21_

This seems better to me

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/tests/snapshots/format@expression__lambda.py.snap`:983 on 2023-11-03 08:21_

This adds parentheses, but I don't mind them

---

_Label `formatter` added by @MichaReiser on 2023-11-03 08:24_

---

_Label `preview` added by @MichaReiser on 2023-11-03 08:24_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/tests/snapshots/format@expression__lambda.py.snap`:691 on 2023-11-03 08:27_

It looks okay.... Parentheses would make it look so much better

---

_@MichaReiser reviewed on 2023-11-03 08:27_

---

_Comment by @MichaReiser on 2023-11-03 09:24_

The main case that is not necessarily better in my view is 

```diff
index 9855c318be..fd7770eb2e 100644
--- a/django/test/testcases.py
+++ b/django/test/testcases.py
@@ -1470,8 +1470,10 @@ def skipIfDBFeature(*features):
 def skipUnlessDBFeature(*features):
     """Skip a test unless a database has all the named features."""
     return _deferredSkip(
-        lambda: not all(
-            getattr(connection.features, feature, False) for feature in features
+        lambda: (
+            not all(
+                getattr(connection.features, feature, False) for feature in features
+            )
         ),
```

🤷 

---

_Marked ready for review by @MichaReiser on 2023-11-03 09:25_

---

_Review requested from @charliermarsh by @MichaReiser on 2023-11-03 09:27_

---

_Comment by @MichaReiser on 2023-12-07 04:38_

Parenthesizing the body is probably less controversial. It might be worth avoiding to parenthesize the body if it has parentheses itself. But parenthesizing the body otherwise fits with the formatting that copilot would choose.

---

_Converted to draft by @MichaReiser on 2024-02-29 14:01_

---

_Closed by @MichaReiser on 2024-04-08 07:03_

---
