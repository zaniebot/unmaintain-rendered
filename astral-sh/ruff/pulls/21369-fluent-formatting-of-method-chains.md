```yaml
number: 21369
title: Fluent formatting of method chains
type: pull_request
state: merged
author: dylwil3
labels:
  - formatter
  - preview
assignees: []
merged: true
base: main
head: call-chains
created_at: 2025-11-10T21:35:33Z
updated_at: 2025-12-19T13:54:58Z
url: https://github.com/astral-sh/ruff/pull/21369
synced_at: 2026-01-12T15:57:21Z
```

# Fluent formatting of method chains

---

_@dylwil3_

This PR implements a modification (in preview) to fluent formatting for method chains: We break _at_ the first call instead of _after_.

For example, we have the following diff between `main` and this PR (with `line-length=8` so I don't have to stretch out the text):

```diff
 x = (
-    df.merge()
+    df
+    .merge()
     .groupby()
     .agg()
     .filter()
 )
```

## Explanation of current implementation

Recall that we traverse the AST to apply formatting. A method chain, while read left-to-right, is stored in the AST "in reverse". So if we start with something like

```python
a.b.c.d().e.f()
```

then the first syntax node we meet is essentially `.f()`. So we have to peek ahead. And we actually _already_ do this in our current fluent formatting logic: we peek ahead to count how many calls we have in the chain to see whether we should be using fluent formatting or now. 

In this implementation, we actually _record_ this number inside the enum for `CallChainLayout`. That is, we make the variant `Fluent` hold an `AttributeState`. This state can either be:

- The number of call-like attributes preceding the current attribute
- The state `FirstCallOrSubscript` which means we are at the first call-like attribute in the chain (reading from left to right)
- The state `BeforeFirstCallOrSubscript` which means we are in the "first group" of attributes, preceding that first call.

In our example, here's what it looks like at each attribute:

```
a.b.c.d().e.f @ Fluent(CallsOrSubscriptsPreceding(1))
a.b.c.d().e @ Fluent(CallsOrSubscriptsPreceding(1))
a.b.c.d @ Fluent(FirstCallOrSubscript)
a.b.c @ Fluent(BeforeFirstCallOrSubscript)
a.b @ Fluent(BeforeFirstCallOrSubscript)
```

Now, as we descend down from the parent expression, we pass along this little piece of state and modify it as we go to track where we are. This state doesn't do anything except when we are in `FirstCallOrSubscript`, in which case we add a soft line break.

Closes #8598 


---

_Label `formatter` added by @dylwil3 on 2025-11-10 21:35_

---

_Comment by @dylwil3 on 2025-11-10 23:16_

Not sure why the ecosystem check didn't post, but here it is attached: 
[ecosystem-result.md](https://github.com/user-attachments/files/23466180/ecosystem-result.md)


---

_Comment by @laundmo on 2025-12-04 17:34_

Wow the implementation for this is astonishingly simple!

Looking through the ecosystem results, I have basically no complaints. The only factor worth considering is if there should be an option/heuristic to skip breaking the first call to a new line for short combinations of name.method(), like pandas `pd` and similar:
<details><summary>Pandas diff excerpt</summary>
<p>

<a href='https://github.com/pandas-dev/pandas/blob/bcd9649419332a2a2c148e8feadff0e3da9f6669/pandas/core/generic.py#L9499'>pandas/core/generic.py~L9499</a>

```diff
 
         # reorder axis to keep things organized
         indices = (
-            np.arange(diff.shape[axis])
+            np
+            .arange(diff.shape[axis])
             .reshape([2, diff.shape[axis] // 2])
-            .T.reshape(-1)
+            .T
+            .reshape(-1)
         )
         diff = diff.take(indices, axis=axis)
 
```

<a href='https://github.com/pandas-dev/pandas/blob/bcd9649419332a2a2c148e8feadff0e3da9f6669/pandas/io/formats/style_render.py#L2553'>pandas/io/formats/style_render.py~L2553</a>

```diff
         Escaped string
     """
     return (
-        s.replace("\\", "ab2§=§8yz")  # rare string for final conversion: avoid \\ clash
+        s
+        .replace("\\", "ab2§=§8yz")  # rare string for final conversion: avoid \\ clash
         .replace("ab2§=§8yz ", "ab2§=§8yz\\space ")  # since \backslash gobbles spaces
         .replace("&", "\\&")
         .replace("%", "\\%")
```
<a href='https://github.com/pandas-dev/pandas/blob/bcd9649419332a2a2c148e8feadff0e3da9f6669/pandas/tests/arrays/test_datetimelike.py#L875'>pandas/tests/arrays/test_datetimelike.py~L875</a>

```diff
         b = pd.date_range("2000", periods=2, freq="h", tz="US/Central", unit=unit)._data
         result = DatetimeArray._concat_same_type([a, b])
         expected = (
-            pd.to_datetime([
+            pd
+            .to_datetime([
                 "2000-01-01 00:00:00",
                 "2000-01-02 00:00:00",
                 "2000-01-01 00:00:00",
```


</p>
</details>

If it isn't too configurable, i'm imagining a setting like `fluent-keep-first-length = 35` which would check if the first line, like `pd.to_datetime([` is longer than 35 unicode codepoints, and only break then. But i fully understand that might be too configurable for what ruff is trying to be, and i haven't explored the implications of this for various cases.

---

_Comment by @dylwil3 on 2025-12-04 19:52_

Thank you for looking at these! I was about to do the same 😄 

I'm conflicted about the issue of short identifiers at the start of these chains. Biome and Prettier don't put a break there if the identifier has length smaller than the tab width, but they also _indent_ method chains. I personally like the extra indentation, but it doesn't seem as common in the Python ecosystem so I didn't put it in this PR. Without it, it feels hard to justify this heuristic.

So at the moment I'm sort of leaning towards allowing the break after `np` and `pd`, etc. But it wouldn't be difficult to implement this exception if we wanted it. I don't think it should be configurable though (beyond the indirect configuration coming from tab style and line length).

---

_Comment by @alexreinking on 2025-12-04 21:34_

@laundmo -- looking at your examples, I think breaking after `np.` and `pd.` feels wrong, but breaking after `s.` seems okay. I think that's because `np.` and `pd.` are package names, so it feels like they're part of the function name itself: `pd.to_datetime` is _one thing_ that shouldn't be split, but `s` is its own thing and the code is acting on it by calling `.replace`.

Does ruff do any sort of semantic analysis for formatting? Does it know which names are package names?

---

_Comment by @dylwil3 on 2025-12-04 21:40_

> Does ruff do any sort of semantic analysis for formatting? Does it know which names are package names?

Nope, it doesn't use anything beyond syntactical information. I don't know of a formatter that uses semantic analysis - unless you count import sorting. 

(In Rust the syntax itself distinguishes between module member access/static methods (these use `::`) and methods that take `self` (uses `.`), but alas in Python it isn't so).

---

_Comment by @alexreinking on 2025-12-04 21:52_

> Nope, it doesn't use anything beyond syntactical information. I don't know of a formatter that uses semantic analysis - unless you count import sorting.

Well, in that case I'd rather lean towards seeing `s.replace` than breaking the module name away from its method. In particular, it's useful to `grep` for usages of things like `some_module.method` and this formatting rule would break that.

It should be possible to force a line break with a trailing comment, right?

```python
return (
    s  # <force line break>
    .replace(...)
    .replace(...)
    .replace(...)
)
```

---

_Comment by @dylwil3 on 2025-12-04 21:57_

> Well, in that case I'd rather lean towards seeing `s.replace` than breaking the module name away from its method. In particular, it's useful to `grep` for usages of things like `some_module.method` and this formatting rule would break that.

If I understand correctly, that's the current behavior. Would you like to weigh in at the [linked issue](https://github.com/astral-sh/ruff/issues/8598)? (Not your fault - I forgot to link the issue until now 😄 )

Also - wouldn't you still be able to `grep` but just put optional whitespace/multiline in your regex?

---

_Comment by @laundmo on 2025-12-04 22:03_

I do think the linked issue has a point, but at the same time i thought it would be useful to consider the nuance here, with a mind to implementation

---

_Comment by @alexreinking on 2025-12-04 22:07_

> Also - wouldn't you still be able to `grep` but just put optional whitespace/multiline in your regex?

Yes, but as you can see, I'm prone to forgetting to do that 😅 

---

_Comment by @dhirschfeld on 2025-12-04 22:59_

I'd prefer to not break `np.<name>` on the initial line, same goes for any `<module>.<name>` combo.

Given the lack of semantic information perhaps the best you can do is to say any identifier under 5 characters long on the first line won't be broken up. Importing modules with a 2-char identifier is idiomatic Python, and, for lesser known modules I've seen 3 and 4-char abbreviations.

It wouldn't be perfect, but it would capture most instances where I'd prefer the initial identifier wasn't split. Of course some won't like that - I don't think there's a way to satisfy everyone... unless you make it configurable how many characters are allowed before the first identifier is split on it's own line.

---

_Comment by @dylwil3 on 2025-12-04 23:55_

I've reviewed the ecosystem changes and here are my thoughts.

# Overall

I think that in almost all cases the new formatting is better than the old. There is one
situation where I think the new formatting is not quite as good, and the remaining
"ugly" formatting is not new to this PR but we might think about fixing it (unless it
greatly increases complexity) since we are overhauling call chains here anyway.

Finally, as brought up in other comments, there's a question of how we feel about
splitting methods when accessing "short" identifiers or modules.

I personally think it looks okay. I also don't really see a way to avoid it (beyond
_maybe_ adopting the tab-width heuristic, but it feels like it's trying to solve a
different problem.) A heuristic not tied to something like tab-width, but instead to a
sort of arbitrary but fixed constant of "small" seems brittle and possibly confusing to
users. A configuration option doesn't seem correct either - it's the sort of
bikeshedding that a formatter tries to remove.

But again: if we were in an alternative universe where we already had this new
formatting, and someone opened a PR with this universe's stable formatting, and I saw
all these diffs in reverse, I would vote _not_ to accept the "new" style. So I think
that means that I like the style of this PR.

## Handling of intermediate attribute access

The most egregious example of what I mean is this:

<a href='https://github.com/ibis-project/ibis/blob/9126733b38e1c92f6e787f92dc9954e88ab6400d/docs/posts/pydata-performance-part2/polars_native.py#L13'>docs/posts/pydata-performance-part2/polars_native.py~L13</a>

```diff
     ])
     .with_columns(
         month=pl.col("uploaded_on").dt.truncate("1mo"),
-        ext=pl.col("path")
-        .str.extract(pattern=r"\.([a-z0-9]+)$", group_index=1)
-        .str.replace_all(pattern=r"cxx|cpp|cc|c|hpp|h", value="C/C++")
-        .str.replace_all(pattern="^f.*$", value="Fortran")
-        .str.replace("rs", "Rust", literal=True)
-        .str.replace("go", "Go", literal=True)
-        .str.replace("asm", "Assembly", literal=True)
+        ext=pl
+        .col("path")
+        .str
+        .extract(pattern=r"\.([a-z0-9]+)$", group_index=1)
+        .str
+        .replace_all(pattern=r"cxx|cpp|cc|c|hpp|h", value="C/C++")
+        .str
+        .replace_all(pattern="^f.*$", value="Fortran")
+        .str
+        .replace("rs", "Rust", literal=True)
+        .str
+        .replace("go", "Go", literal=True)
+        .str
+        .replace("asm", "Assembly", literal=True)
         .replace({"": None}),
     )
     .group_by(["month", "ext"])
```

So there is a question of whether a different heuristic should be applied to
"intermediate" attributes within the method chain so that, in this example, the
`.str.replace(` calls would stay on the same line.

<details><summary>Other examples</summary>

<a href='https://github.com/binary-husky/gpt_academic/blob/0aa0472da44edc0a888c7f83b564c6d0d1089366/crazy_functions/doc_fns/read_fns/excel_reader.py#L133'>crazy_functions/doc_fns/read_fns/excel_reader.py~L133</a>

```diff
                 return "\n".join(rows)
             else:
                 flat_values = (
-                    chunk.astype(str)
+                    chunk
+                    .astype(str)
                     .replace({"nan": "", "None": "", "NaN": ""})
-                    .values.flatten()
+                    .values
+                    .flatten()
                 )
                 return " ".join(v for v in flat_values if v)

```

<a href='https://github.com/freedomofpress/securedrop/blob/6e3e49d7775c42698bf6e24843e29e08459d58da/securedrop/tests/functional/app_navigators/journalist_app_nav.py#L287'>securedrop/tests/functional/app_navigators/journalist_app_nav.py~L287</a>

```diff
         else:
             # We created a totp user
             otp_secret = (
-                self.driver.find_element(By.CSS_SELECTOR, "#shared-secret")
-                .text.strip()
+                self.driver
+                .find_element(By.CSS_SELECTOR, "#shared-secret")
+                .text
+                .strip()
                 .replace(" ", "")
             )
             totp = two_factor.TOTP(otp_secret)
```

<a href='https://github.com/reflex-dev/reflex/blob/e4300adbbec62630cb3ce5c0610400788557e9b9/reflex/event.py#L1106'>reflex/event.py~L1106</a>

```diff
     get_element_by_id = FunctionStringVar.create("document.getElementById")

     return run_script(
-        get_element_by_id.call(elem_id)
+        get_element_by_id
+        .call(elem_id)
         .to(ObjectVar)
-        .scrollIntoView.to(FunctionVar)
+        .scrollIntoView
+        .to(FunctionVar)
         .call(align_to_top),
     )

```

<a href='https://github.com/reflex-dev/reflex/blob/e4300adbbec62630cb3ce5c0610400788557e9b9/reflex/event.py#L1219'>reflex/event.py~L1219</a>

```diff
     return run_script(
         Var("navigator")
         .to(dict)
-        .clipboard.to(dict)
-        .writeText.to(FunctionVar)
+        .clipboard
+        .to(dict)
+        .writeText
+        .to(FunctionVar)
         .call(content)
     )

```

</details>

## Calls not aligning with initial identifier

In certain cases we align the method calls with some piece of text (e.g. a binary
operator, walrus operator, or `for` statement) that precedes the initial identifier.

I don't think we have to do this to avoid invalid syntax, but I haven't played around
with it yet. If we could avoid it, it would be nice.

In any case - this is an existing issue, so we may not want to deal with it here.

<details><summary>Examples</summary>
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/nlu/classifiers/test_diet_classifier.py#L874'>tests/nlu/classifiers/test_diet_classifier.py~L874</a>
```diff
                             # expected to change can be compared directly
                             assert (
                                 feature_signature.units
-                                == old_signature.get(attribute)
+                                == old_signature
+                                .get(attribute)
                                 .get(feature_type)[index]
                                 .units
                             )
```

<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/nlu/selectors/test_selectors.py#L798'>tests/nlu/selectors/test_selectors.py~L798</a>

```diff
                             # expected to change can be compared directly
                             assert (
                                 feature_signature.units
-                                == old_signature.get(attribute)
+                                == old_signature
+                                .get(attribute)
                                 .get(feature_type)[index]
                                 .units
                             )
```

<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/unit/accounting/test_misc.py#L431'>rotkehlchen/tests/unit/accounting/test_misc.py~L431</a>

```diff
     assert len(rotki.accountant.pots[0].cost_basis.missing_acquisitions) == 0
     assert (
         len(
-            used_acquisitions := rotki.accountant.pots[0]
-            .cost_basis.get_events(asset=A_ETH)
+            used_acquisitions := rotki.accountant
+            .pots[0]
+            .cost_basis
+            .get_events(asset=A_ETH)
             .used_acquisitions
         )
         == 1
```

<a href='https://github.com/apache/superset/blob/23b61b080ed9bdb27747bbb4c1d18efec8006808/superset/connectors/sqla/models.py#L257'>superset/connectors/sqla/models.py~L257</a>

```diff
         # Get all groupable column names for this datasource
         drillable_columns = {
             row[0]
-            for row in db.session.query(TableColumn.column_name)
+            for row in db.session
+            .query(TableColumn.column_name)
             .filter(TableColumn.table_id == self.id)
             .filter(TableColumn.groupby)
             .all()
```

<a href='https://github.com/apache/superset/blob/23b61b080ed9bdb27747bbb4c1d18efec8006808/superset/security/manager.py#L2451'>superset/security/manager.py~L2451</a>

```diff
                     form_data
                     and (dashboard_id := form_data.get("dashboardId"))
                     and (
-                        dashboard_ := self.session.query(Dashboard)
+                        dashboard_ := self.session
+                        .query(Dashboard)
                         .filter(Dashboard.id == dashboard_id)
                         .one_or_none()
                     )
```

<a href='https://github.com/apache/superset/blob/23b61b080ed9bdb27747bbb4c1d18efec8006808/superset/security/manager.py#L2484'>superset/security/manager.py~L2484</a>

```diff
                             form_data.get("type") != "NATIVE_FILTER"
                             and (slice_id := form_data.get("slice_id"))
                             and (
-                                slc := self.session.query(Slice)
+                                slc := self.session
+                                .query(Slice)
                                 .filter(Slice.id == slice_id)
                                 .one_or_none()
                             )
```

<a href='https://github.com/apache/superset/blob/23b61b080ed9bdb27747bbb4c1d18efec8006808/superset/security/manager.py#L2558'>superset/security/manager.py~L2558</a>

```diff
         need to be scoped
         """
         return (
-            self.session.query(self.user_model)
+            self.session
+            .query(self.user_model)
             .filter(self.user_model.username == username)
             .one_or_none()
         )
```

</details>


---

_Marked ready for review by @dylwil3 on 2025-12-04 23:55_

---

_Review requested from @MichaReiser by @dylwil3 on 2025-12-04 23:55_

---

_Label `preview` added by @dylwil3 on 2025-12-05 00:01_

---

_Comment by @dhirschfeld on 2025-12-05 00:33_

How about, if it's used as a namespace it doesn't get broken up? That might solve both cases.

A namespace is identified as not being a function call - e.g.
- `np.function()` doesn't get broken up because `np` isn't a function call
- `.str.extract(pattern=r"\.([a-z0-9]+)$", group_index=1)` doesn't get split because `str` is not a function call
- `.dt.truncate("1mo")` would stay together as `dt` is used as a namespace

---

_Comment by @alexreinking on 2025-12-05 01:00_

In all the cases you found pathological, a rule where an extra layer of parenthesis are introduced when the separation exceeds a tab width would fix the flow, IMHO.

<details>
<summary>Examples</summary>

Consider:

```diff
                             # expected to change can be compared directly
                             assert (
-                                feature_signature.units
-                                == old_signature.get(attribute)
+                                feature_signature.units == (
+                                    old_signature
+                                    .get(attribute)
+                                    .get(feature_type)[index]
+                                    .units
+                                )
                             )
```

Or alternatively,

```diff
                             form_data.get("type") != "NATIVE_FILTER"
                             and (slice_id := form_data.get("slice_id"))
                             and (
-                                slc := self.session.query(Slice)
+                                slc := (
+                                    self.session
+                                    .query(Slice)
+.                                   .filter(Slice.id == slice_id)
+                                    .one_or_none()
+                                )
                             )
```

</details>

---

_Comment by @alexreinking on 2025-12-05 01:04_

> So there is a question of whether a different heuristic should be applied to "intermediate" attributes within the method chain so that, in this example, the .str.replace( calls would stay on the same line.

I think it's totally fair to only break after `)` for the purposes of method chaining... a property access isn't a method, right? Maybe that's the syntactic rule that governs this?

---

_Comment by @dylwil3 on 2025-12-05 01:26_

> How about, if it's used as a namespace it doesn't get broken up? That might solve both cases.
> 
> A namespace is identified as not being a function call - e.g.
> 
> * `np.function()` doesn't get broken up because `np` isn't a function call
> * `.str.extract(pattern=r"\.([a-z0-9]+)$", group_index=1)` doesn't get split because `str` is not a function call
> * `.dt.truncate("1mo")` would stay together as `dt` is used as a namespace

If I understand correctly, this is just what the current, stable behavior is. The point is that we want to break up

```python
x = some_object_that_is_not_a_module.does_something().and_another_thing().and_something_else()
```

as

```python
x = (
    some_object_that_is_not_a_module
    .does_something()
    .and_another_thing()
    .and_something_else()
)
```

but currently we do:

```python
x = (
    some_object_that_is_not_a_module.does_something()
    .and_another_thing()
    .and_something_else()
)
```

Which is not what we want, in most cases. I don't really see a way to syntactically tell when something is a "namespace" or "module" as opposed to an object with some methods. (In Python, there also isn't really a _semantic_ distinction between those things.)

---

_Comment by @laundmo on 2025-12-05 02:10_

I think throughout this conversation i've become convinced that not breaking short identifiers shorter than tab length would only male sense if subsequent method calls were indented. That's actually something i personally prefer (indented calls) but it definitely doesn't have to be part of this PR if it will ever be added.

I think the main question left is what to do with intermediate attributes, and my personal tendency would be to not break after them. And honestly, if it was up to me, i'd make that dependant on the implementation complexity. If its doable in an hour or 2, do it, otherwise leave it as is. Since i assume this'll be a preview style anyways its not too bad to change it slightly later.

---

_Comment by @MichaReiser on 2025-12-05 07:10_

> The most egregious example of what I mean is this:

I think we should fix it because it feels inconsistent that we don't split the attribute on the first line, but do for subsequent calls. I strongly prefer **not-splitting** the attribute access (which is also what Prettier does).:

```diff
         # Query for starred sources, along with their unread
         # submission counts.
         starred = (
-            db.session.query(Source, unread_stmt.c.num_unread)
+            db.session
+            .query(Source, unread_stmt.c.num_unread)
             .filter_by(pending=False, deleted_at=None)
             .filter(Source.last_updated.isnot(None))
             .filter(SourceStar.starred.is_(True))
```

It also ensures the new formatting is consistent with the (non-method) call-chain layout:

```
assert (
    get_collection(COLLECTION_NAME)
    .config.params.sparse_vectors["sparse-text"]
    .modifier.call()
    .other.more.existing
)
```



> In certain cases we align the method calls with some piece of text (e.g. a binary
operator, walrus operator, or for statement) that precedes the initial identifier.

I'm fine with not addressing this in this PR, given that it's a pre-existing issue. I agree it's not great but it's also something that applies to all call-chains, not just method chains (and is related to https://github.com/astral-sh/ruff/issues/12856)



Not very common, but this instance also looks worse to me. We should at least add a test:

<a href='https://github.com/Snowflake-Labs/snowcli/blob/b3dfc9961faad853a2cd44d1d728134cd98f0335/tests/app/test_telemetry.py#L52'>tests/app/test_telemetry.py~L52</a>

```diff
     assert result.exit_code == 0, result.output
     # The method is called with a TelemetryData type, so we cast it to dict for simpler comparison
     usage_command_event = (
-        mock_conn.return_value._telemetry.try_add_log_to_batch.call_args_list[  # noqa: SLF001
+        mock_conn.return_value._telemetry.try_add_log_to_batch
+        .call_args_list[  # noqa: SLF001
             0
         ]
         .args[0]
```

---

_Comment by @dylwil3 on 2025-12-05 12:02_

> I think we should fix it because it feels inconsistent that we don't split the attribute on the first line, but do for subsequent calls. I strongly prefer **not-splitting** the attribute access (which is also what Prettier does).:

I agree with your conclusion - that we should fix cases like the "egregious example" - but I'm confused by your comment. The current PR _does_ treat the first line the same as all the rest. The prettier (and Prettier) formatting would treat the first line differently than all the rest because it would not split a call that follows an attribute access.

The rule in this PR (that applies to all lines) is that we keep chains of attribute access on one line, and split right before a call.   So, for example, the following is formatted in this PR:

```python
x = (
    some.very.nested.object
    .returns()
    .another.nested.object
    .and_another_thing()
    .and_something_else()
)
```

Notice that the same rule applies to the first line of the chain and to the third line of the chain.

On stable that becomes:

```python
x = (
    some.very.nested.object.returns()
    .another.nested.object.and_another_thing()
    .and_something_else()
)
```

which isn't that bad, but isn't the fluent formatting we're after. I believe we are both saying that we want:

```python
x = (
    some.very.nested.object
    .returns()
    .another.nested.object.and_another_thing()
    .and_something_else()
)
```

(which is what Prettier does). Notice that this treats the first line of the chain _differently_ than the third.

Anyway I will take a stab at implementing that - converting back to draft!

---

_Converted to draft by @dylwil3 on 2025-12-05 12:02_

---

_Comment by @dylwil3 on 2025-12-05 12:07_

> Not very common, but this instance also looks worse to me. We should at least add a test:

I agree - but I think it sort of looks bad either way and would be solved by fixing #13761 .

---

_Comment by @laundmo on 2025-12-05 12:32_

Hmm, what do you think about this?
```py
x = (
    some
    .very.nested.object.returns()
    .another.nested.object.and_another_thing()
    .and_something_else()
)
```

It seems somewhat strange at first, but i personally like the consistency between the first and second call.

What i imagine a bad case to look like, splitting like my idea:
```
x = (
    long_namespace
    .i.method()
    .pop()
)
```

---

_Comment by @MichaReiser on 2025-12-05 12:33_

I like this. Feels even more consistent

---

_Comment by @dylwil3 on 2025-12-05 21:33_

Okey dokey. As explained in the current version of the PR summary, the commit [4c1f9aa](https://github.com/astral-sh/ruff/commit/4c1f9aaa7f9498f9978aba085b16ea6a1774a866) introduces two simultaneous changes:

1. We implement the main change, which special cases the first method call in the chain and breaks right before it.
2. We also change the heuristic for _when_ to use fluent formatting

It's a huge ecosystem change, so I thought it would be easier to think about if we compared different versions. The versions in question are:

1. `main`
2. `just_criterion_change`: same as `main` except we modify the criterion for when to use fluent layout
3. `initial_attempt`: the first version of this PR, which was sorta okay, where we treated all attributes the same and broke before all calls in the chain
4. `just_major_change`: Use the same criterion as in `main` to determine _when_ to use fluent layout, but the fluent layout is the one where we break at the first method call and the rest is the same as in `main`
5. `both_changes`: Use fluent layout more often, and break at first method call

Here are the relevant ecosystem results:

[compare_initial_attempt__vs__both_changes.md](https://github.com/user-attachments/files/23970938/compare_initial_attempt__vs__both_changes.md)
[compare_initial_attempt__vs__just_major_change.md](https://github.com/user-attachments/files/23970940/compare_initial_attempt__vs__just_major_change.md)
[compare_just_criterion_change__vs__both_changes.md](https://github.com/user-attachments/files/23970941/compare_just_criterion_change__vs__both_changes.md)
[compare_main__vs__both_changes.md](https://github.com/user-attachments/files/23970942/compare_main__vs__both_changes.md)
[compare_main__vs__initial_attempt.md](https://github.com/user-attachments/files/23970943/compare_main__vs__initial_attempt.md)
[compare_main__vs__just_criterion_change.md](https://github.com/user-attachments/files/23970944/compare_main__vs__just_criterion_change.md)
[compare_main__vs__just_major_change.md](https://github.com/user-attachments/files/23970946/compare_main__vs__just_major_change.md)

With apologies for the spamming of everyone's inbox, I will also post the smaller of these separately below.


---

_Comment by @dylwil3 on 2025-12-05 21:34_

Here is the comparison of the initial attempt and just the major change (see https://github.com/astral-sh/ruff/pull/21369#issuecomment-3618670969)

ℹ️ ecosystem check **detected format changes**. (+26 -52 lines in 16 files in 7 projects; 48 projects unchanged)

<details><summary><a href="https://github.com/binary-husky/gpt_academic">binary-husky/gpt_academic</a> (+1 -2 lines across 1 file)</summary>
<p>

<a href='https://github.com/binary-husky/gpt_academic/blob/0aa0472da44edc0a888c7f83b564c6d0d1089366/crazy_functions/doc_fns/read_fns/excel_reader.py#L136'>crazy_functions/doc_fns/read_fns/excel_reader.py~L136</a>
```diff
                     chunk
                     .astype(str)
                     .replace({"nan": "", "None": "", "NaN": ""})
-                    .values
-                    .flatten()
+                    .values.flatten()
                 )
                 return " ".join(v for v in flat_values if v)
 
```

</p>
</details>
<details><summary><a href="https://github.com/freedomofpress/securedrop">freedomofpress/securedrop</a> (+1 -2 lines across 1 file)</summary>
<p>

<a href='https://github.com/freedomofpress/securedrop/blob/abee0e643ea1cb5d434fdd104196d9ab092005be/securedrop/tests/functional/app_navigators/journalist_app_nav.py#L289'>securedrop/tests/functional/app_navigators/journalist_app_nav.py~L289</a>
```diff
             otp_secret = (
                 self.driver
                 .find_element(By.CSS_SELECTOR, "#shared-secret")
-                .text
-                .strip()
+                .text.strip()
                 .replace(" ", "")
             )
             totp = two_factor.TOTP(otp_secret)
```

</p>
</details>
<details><summary><a href="https://github.com/qdrant/qdrant-client">qdrant/qdrant-client</a> (+2 -4 lines across 1 file)</summary>
<p>

<a href='https://github.com/qdrant/qdrant-client/blob/750d735b2917a30b0cbf468ddccbc47d12704e32/tests/congruence_tests/test_sparse_idf_search.py#L57'>tests/congruence_tests/test_sparse_idf_search.py~L57</a>
```diff
     assert (
         local_client
         .get_collection(COLLECTION_NAME)
-        .config.params
-        .sparse_vectors["sparse-text"]
+        .config.params.sparse_vectors["sparse-text"]
         .modifier
         == models.Modifier.IDF
     )
```
<a href='https://github.com/qdrant/qdrant-client/blob/750d735b2917a30b0cbf468ddccbc47d12704e32/tests/congruence_tests/test_sparse_idf_search.py#L85'>tests/congruence_tests/test_sparse_idf_search.py~L85</a>
```diff
     assert (
         local_client
         .get_collection(COLLECTION_NAME)
-        .config.params
-        .sparse_vectors["sparse-text"]
+        .config.params.sparse_vectors["sparse-text"]
         .modifier
         == models.Modifier.NONE
     )
```

</p>
</details>
<details><summary><a href="https://github.com/reflex-dev/reflex">reflex-dev/reflex</a> (+3 -6 lines across 1 file)</summary>
<p>

<a href='https://github.com/reflex-dev/reflex/blob/e4300adbbec62630cb3ce5c0610400788557e9b9/reflex/event.py#L1109'>reflex/event.py~L1109</a>
```diff
         get_element_by_id
         .call(elem_id)
         .to(ObjectVar)
-        .scrollIntoView
-        .to(FunctionVar)
+        .scrollIntoView.to(FunctionVar)
         .call(align_to_top),
     )
 
```
<a href='https://github.com/reflex-dev/reflex/blob/e4300adbbec62630cb3ce5c0610400788557e9b9/reflex/event.py#L1221'>reflex/event.py~L1221</a>
```diff
     return run_script(
         Var("navigator")
         .to(dict)
-        .clipboard
-        .to(dict)
-        .writeText
-        .to(FunctionVar)
+        .clipboard.to(dict)
+        .writeText.to(FunctionVar)
         .call(content)
     )
 
```

</p>
</details>
<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+6 -12 lines across 2 files)</summary>
<p>

<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/chain/ethereum/oracles/uniswap.py#L70'>rotkehlchen/chain/ethereum/oracles/uniswap.py~L70</a>
```diff
         self.uniswap_factories = {
             chain_id: Inquirer()
             .get_evm_manager(chain_id)
-            .node_inquirer.contracts
-            .contract(UNISWAP_FACTORY_ADDRESSES[version][chain_id])  # noqa: E501
+            .node_inquirer.contracts.contract(UNISWAP_FACTORY_ADDRESSES[version][chain_id])  # noqa: E501
             for chain_id in UNISWAP_SUPPORTED_CHAINS
         }
 
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/chain/ethereum/oracles/uniswap.py#L421'>rotkehlchen/chain/ethereum/oracles/uniswap.py~L421</a>
```diff
             return self.is_before_factory_deployment(
                 block_identifier=Inquirer()
                 .get_evm_manager(token.chain_id)
-                .node_inquirer
-                .get_blocknumber_by_time(timestamp),
+                .node_inquirer.get_blocknumber_by_time(timestamp),
                 chain_id=token.chain_id,
             )
         except RemoteError as e:  # can be raised by get_blocknumber_by_time
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/chain/ethereum/oracles/uniswap.py#L443'>rotkehlchen/chain/ethereum/oracles/uniswap.py~L443</a>
```diff
             to_token=to_token,
             block_identifier=Inquirer()
             .get_evm_manager(from_token.chain_id)
-            .node_inquirer
-            .get_blocknumber_by_time(timestamp),
+            .node_inquirer.get_blocknumber_by_time(timestamp),
             timestamp=timestamp,
         )
 
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/chain/ethereum/oracles/uniswap.py#L455'>rotkehlchen/chain/ethereum/oracles/uniswap.py~L455</a>
```diff
         self.uniswap_v3_pool_abi = (
             Inquirer()
             .get_evm_manager(ChainID.ETHEREUM)
-            .node_inquirer.contracts
-            .abi("UNISWAP_V3_POOL")
+            .node_inquirer.contracts.abi("UNISWAP_V3_POOL")
         )  # noqa: E501
 
     @cache_response_timewise()
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/chain/ethereum/oracles/uniswap.py#L582'>rotkehlchen/chain/ethereum/oracles/uniswap.py~L582</a>
```diff
         self.uniswap_v2_lp_abi = (
             Inquirer()
             .get_evm_manager(ChainID.ETHEREUM)
-            .node_inquirer.contracts
-            .abi("UNISWAP_V2_LP")
+            .node_inquirer.contracts.abi("UNISWAP_V2_LP")
         )  # noqa: E501
 
     @cache_response_timewise()
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/unit/accounting/test_misc.py#L433'>rotkehlchen/tests/unit/accounting/test_misc.py~L433</a>
```diff
         len(
             used_acquisitions := rotki.accountant
             .pots[0]
-            .cost_basis
-            .get_events(asset=A_ETH)
+            .cost_basis.get_events(asset=A_ETH)
             .used_acquisitions
         )
         == 1
```

</p>
</details>
<details><summary><a href="https://github.com/zanieb/huggingface-notebooks">zanieb/huggingface-notebooks</a> (+2 -4 lines across 2 files)</summary>
<p>

<a href='https://github.com/zanieb/huggingface-notebooks/blob/68cd6fa1a2831c5189f85257c13d691cb76292db/course/fr/chapter9/section4.ipynb#L129'>course/fr/chapter9/section4.ipynb~L129</a>
```diff
     "    .get(\n",
     "        \"https://huggingface.co/spaces/course-demos/Sketch-Recognition/raw/main/class_names.txt\"\n",
     "    )\n",
-    "    .text\n",
-    "    .replace(\"\\n\", \"\")\n",
+    "    .text.replace(\"\\n\", \"\")\n",
     "    .split(\"\\r\")\n",
     ")\n",
     "\n",
```
<a href='https://github.com/zanieb/huggingface-notebooks/blob/68cd6fa1a2831c5189f85257c13d691cb76292db/diffusers/sd_textual_inversion_training.ipynb#L988'>diffusers/sd_textual_inversion_training.ipynb~L988</a>
```diff
     "                latents = (\n",
     "                    vae\n",
     "                    .encode(batch[\"pixel_values\"].to(dtype=weight_dtype))\n",
-    "                    .latent_dist\n",
-    "                    .sample()\n",
+    "                    .latent_dist.sample()\n",
     "                    .detach()\n",
     "                )\n",
     "                latents = latents * 0.18215\n",
```

</p>
</details>
<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (+11 -22 lines across 8 files)</summary>
<p>
<pre>ruff format --no-preview --exclude examples/mcp/databricks_mcp_cookbook.ipynb,examples/chatgpt/gpt_actions_library/gpt_action_google_drive.ipynb,examples/chatgpt/gpt_actions_library/gpt_action_redshift.ipynb,examples/chatgpt/gpt_actions_library/gpt_action_salesforce.ipynb,</pre>
</p>
<p>

<a href='https://github.com/openai/openai-cookbook/blob/134d8a16f1c6b8a58351506e11cb96ada880f6bc/examples/Clustering.ipynb#L230'>examples/Clustering.ipynb~L230</a>
```diff
     "        df[df.Cluster == i]\n",
     "        .combined.str\n",
     "        .replace(\"Title: \", \"\")\n",
-    "        .str\n",
-    "        .replace(\"\\n\\nContent: \", \":  \")\n",
+    "        .str.replace(\"\\n\\nContent: \", \":  \")\n",
     "        .sample(rev_per_cluster, random_state=42)\n",
     "        .values\n",
     "    )\n",
```
<a href='https://github.com/openai/openai-cookbook/blob/134d8a16f1c6b8a58351506e11cb96ada880f6bc/examples/Clustering_for_transaction_classification.ipynb#L399'>examples/Clustering_for_transaction_classification.ipynb~L399</a>
```diff
     "        embedding_df[embedding_df.Cluster == i]\n",
     "        .combined.str\n",
     "        .replace(\"Supplier: \", \"\")\n",
-    "        .str\n",
-    "        .replace(\"Description: \", \":  \")\n",
-    "        .str\n",
-    "        .replace(\"Value: \", \":  \")\n",
+    "        .str.replace(\"Description: \", \":  \")\n",
+    "        .str.replace(\"Value: \", \":  \")\n",
     "        .sample(transactions_per_cluster, random_state=42)\n",
     "        .values\n",
     "    )\n",
```
<a href='https://github.com/openai/openai-cookbook/blob/134d8a16f1c6b8a58351506e11cb96ada880f6bc/examples/Developing_hallucination_guardrails.ipynb#L1044'>examples/Developing_hallucination_guardrails.ipynb~L1044</a>
```diff
     "        df[\"accurate\"] = (\n",
     "            df[\"accurate\"]\n",
     "            .astype(str)\n",
-    "            .str\n",
-    "            .strip()\n",
+    "            .str.strip()\n",
     "            .map(lambda x: 1 if x in [\"True\", \"true\"] else 0)\n",
     "        )\n",
     "        df[\"hallucination\"] = (\n",
```
<a href='https://github.com/openai/openai-cookbook/blob/134d8a16f1c6b8a58351506e11cb96ada880f6bc/examples/Leveraging_model_distillation_to_fine-tune_a_model.ipynb#L258'>examples/Leveraging_model_distillation_to_fine-tune_a_model.ipynb~L258</a>
```diff
     "varieties_less_than_five_list = (\n",
     "    df_france[\"variety\"]\n",
     "    .value_counts()[df_france[\"variety\"].value_counts() < 5]\n",
-    "    .index\n",
-    "    .tolist()\n",
+    "    .index.tolist()\n",
     ")\n",
     "df_france = df_france[~df_france[\"variety\"].isin(varieties_less_than_five_list)]\n",
     "\n",
```
<a href='https://github.com/openai/openai-cookbook/blob/134d8a16f1c6b8a58351506e11cb96ada880f6bc/examples/Reinforcement_Fine_Tuning.ipynb#L927'>examples/Reinforcement_Fine_Tuning.ipynb~L927</a>
```diff
     "    df_avg = (\n",
     "        pd\n",
     "        .DataFrame(model_metrics_avg)\n",
-    "        .T\n",
-    "        .reset_index()\n",
+    "        .T.reset_index()\n",
     "        .rename(columns={\"index\": \"Model\"})\n",
     "    )\n",
     "    df_std = (\n",
     "        pd\n",
     "        .DataFrame(model_metrics_std)\n",
-    "        .T\n",
-    "        .reset_index()\n",
+    "        .T.reset_index()\n",
     "        .rename(columns={\"index\": \"Model\"})\n",
     "    )\n",
     "\n",
```
<a href='https://github.com/openai/openai-cookbook/blob/134d8a16f1c6b8a58351506e11cb96ada880f6bc/examples/Semantic_text_search_using_embeddings.ipynb#L69'>examples/Semantic_text_search_using_embeddings.ipynb~L69</a>
```diff
     "        df\n",
     "        .sort_values(\"similarity\", ascending=False)\n",
     "        .head(n)\n",
-    "        .combined.str\n",
-    "        .replace(\"Title: \", \"\")\n",
-    "        .str\n",
-    "        .replace(\"; Content:\", \": \")\n",
+    "        .combined.str.replace(\"Title: \", \"\")\n",
+    "        .str.replace(\"; Content:\", \": \")\n",
     "    )\n",
     "    if pprint:\n",
     "        for r in results:\n",
```
<a href='https://github.com/openai/openai-cookbook/blob/134d8a16f1c6b8a58351506e11cb96ada880f6bc/examples/evaluation/How_to_eval_abstractive_summarization.ipynb#L311'>examples/evaluation/How_to_eval_abstractive_summarization.ipynb~L311</a>
```diff
     "    pd\n",
     "    .DataFrame(rouge_scores_out)\n",
     "    .set_index(\"Metric\")\n",
-    "    .style\n",
-    "    .apply(highlight_max, axis=1)\n",
+    "    .style.apply(highlight_max, axis=1)\n",
     ")\n",
     "\n",
     "rouge_scores_out"
```
<a href='https://github.com/openai/openai-cookbook/blob/134d8a16f1c6b8a58351506e11cb96ada880f6bc/examples/o1/Using_reasoning_for_data_validation.ipynb#L226'>examples/o1/Using_reasoning_for_data_validation.ipynb~L226</a>
```diff
     "    response_content = (\n",
     "        response\n",
     "        .choices[0]\n",
-    "        .message.content\n",
-    "        .replace(\"```json\", \"\")\n",
+    "        .message.content.replace(\"```json\", \"\")\n",
     "        .replace(\"```\", \"\")\n",
     "        .strip()\n",
     "    )\n",
```

</p>
</details>


---

_Comment by @dylwil3 on 2025-12-05 21:37_

Here is the comparison of `main` with just changing the criterion for fluent layout:

ℹ️ ecosystem check **detected format changes**. (+912 -613 lines in 110 files in 15 projects; 40 projects unchanged)

<details><summary><a href="https://github.com/RasaHQ/rasa">RasaHQ/rasa</a> (+8 -8 lines across 3 files)</summary>
<p>

<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/nlu/classifiers/diet_classifier.py#L358'>rasa/nlu/classifiers/diet_classifier.py~L358</a>
```diff
                 current_hidden_layer_sizes == first_hidden_layer_sizes
                 for current_hidden_layer_sizes in self.component_config[
                     HIDDEN_LAYERS_SIZES
-                ].values()
+                ]
+                .values()
             )
             if not identical_hidden_layer_sizes:
                 raise ValueError(
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/shared/core/domain.py#L1178'>rasa/shared/core/domain.py~L1178</a>
```diff
         # tuple because sub_state will be later transformed into a frozenset (so it can
         # be hashed for deduplication).
         entities = tuple(
-            self._get_featurized_entities(latest_message).intersection(
-                set(sub_state.get(ENTITIES, ()))
-            )
+            self._get_featurized_entities(latest_message)
+            .intersection(set(sub_state.get(ENTITIES, ())))
         )
         # Sort entities so that any derived state representation is consistent across
         # runs and invariant to the order in which the entities for an utterance are
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/shared/core/domain.py#L1312'>rasa/shared/core/domain.py~L1312</a>
```diff
         # remove active loop which only occur in rules but not in stories
         if (
             rule_only_loops
-            and state.get(rasa.shared.core.constants.ACTIVE_LOOP, {}).get(
-                rasa.shared.core.constants.LOOP_NAME
-            )
+            and state.get(rasa.shared.core.constants.ACTIVE_LOOP, {})
+            .get(rasa.shared.core.constants.LOOP_NAME)
             in rule_only_loops
         ):
             del state[rasa.shared.core.constants.ACTIVE_LOOP]
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/core/policies/test_unexpected_intent_policy.py#L810'>tests/core/policies/test_unexpected_intent_policy.py~L810</a>
```diff
                     all_label_embeddings[0],
                     all_label_embeddings[0],
                 ]
-            ).reshape((3, 2, 20)),
+            )
+            .reshape((3, 2, 20)),
             dtype=tf.float32,
         )
 
```

</p>
</details>
<details><summary><a href="https://github.com/binary-husky/gpt_academic">binary-husky/gpt_academic</a> (+17 -10 lines across 4 files)</summary>
<p>

<a href='https://github.com/binary-husky/gpt_academic/blob/0aa0472da44edc0a888c7f83b564c6d0d1089366/crazy_functions/agent_fns/pipe.py#L196'>crazy_functions/agent_fns/pipe.py~L196</a>
```diff
                     )
                 self.chatbot[-1] = [
                     self.chatbot[-1][0],
-                    self.chatbot[-1][1].replace(
-                        "[GPT-Academic] 等待中", "[GPT-Academic] 等待中."
-                    ),
+                    self.chatbot[-1][1]
+                    .replace("[GPT-Academic] 等待中", "[GPT-Academic] 等待中."),
                 ]
                 yield from update_ui(chatbot=self.chatbot, history=self.history)
                 # if time.time() - begin_waiting_time > patience:
```
<a href='https://github.com/binary-husky/gpt_academic/blob/0aa0472da44edc0a888c7f83b564c6d0d1089366/crazy_functions/crazy_utils.py#L524'>crazy_functions/crazy_utils.py~L524</a>
```diff
                             "".join([wtf["text"] for wtf in l["spans"]])
                             for l in t["lines"]
                         ]
-                    ).replace("- ", "")
+                    )
+                    .replace("- ", "")
                     for t in text_areas["blocks"]
                     if "lines" in t
                 ]
```
<a href='https://github.com/binary-husky/gpt_academic/blob/0aa0472da44edc0a888c7f83b564c6d0d1089366/crazy_functions/crazy_utils.py#L548'>crazy_functions/crazy_utils.py~L548</a>
```diff
                             "".join([wtf["text"] for wtf in l["spans"]])
                             for l in t["lines"]
                         ]
-                    ).replace("- ", "")
+                    )
+                    .replace("- ", "")
                     for t in text_areas["blocks"]
                     if "lines" in t
                 ]
```
<a href='https://github.com/binary-husky/gpt_academic/blob/0aa0472da44edc0a888c7f83b564c6d0d1089366/crazy_functions/review_fns/query_analyzer.py#L238'>crazy_functions/review_fns/query_analyzer.py~L238</a>
```diff
                         for cat in self._extract_tag(
                             extracted_responses[self.ARXIV_CATEGORIES_INDEX],
                             "categories",
-                        ).split(",")
+                        )
+                        .split(",")
                     ],
                     "sort_by": self._extract_tag(
                         extracted_responses[self.ARXIV_SORT_INDEX], "sort_by"
```
<a href='https://github.com/binary-husky/gpt_academic/blob/0aa0472da44edc0a888c7f83b564c6d0d1089366/crazy_functions/review_fns/query_analyzer.py#L276'>crazy_functions/review_fns/query_analyzer.py~L276</a>
```diff
                         field.strip()
                         for field in self._extract_tag(
                             extracted_responses[self.SEMANTIC_FIELDS_INDEX], "fields"
-                        ).split(",")
+                        )
+                        .split(",")
                     ],
                     "sort_by": "relevance",
                     "limit": 20,
```
<a href='https://github.com/binary-husky/gpt_academic/blob/0aa0472da44edc0a888c7f83b564c6d0d1089366/crazy_functions/review_fns/query_analyzer.py#L419'>crazy_functions/review_fns/query_analyzer.py~L419</a>
```diff
             is_latest_request = (
                 self._extract_tag(
                     extracted_responses[self.ARXIV_LATEST_INDEX], "is_latest_request"
-                ).lower()
+                )
+                .lower()
                 == "true"
             )
 
```
<a href='https://github.com/binary-husky/gpt_academic/blob/0aa0472da44edc0a888c7f83b564c6d0d1089366/shared_utils/cookie_manager.py#L201'>shared_utils/cookie_manager.py~L201</a>
```diff
     for name in output_name_list:
         call_args += f"""Data["{name}"],"""
     call_args = call_args.rstrip(",")
-    _js_callback = """
+    _js_callback = (
+        """
         (base64MiddleString)=>{
             console.log('hello')
             const stringData = atob(base64MiddleString);
```
<a href='https://github.com/binary-husky/gpt_academic/blob/0aa0472da44edc0a888c7f83b564c6d0d1089366/shared_utils/cookie_manager.py#L209'>shared_utils/cookie_manager.py~L209</a>
```diff
             call = JS_CALLBACK_GEN;
             call(CALL_ARGS);
         }
-    """.replace("JS_CALLBACK_GEN", js_callback).replace("CALL_ARGS", call_args)
+    """.replace("JS_CALLBACK_GEN", js_callback)
+        .replace("CALL_ARGS", call_args)
+    )
 
     btn.click(get_fn_wrap(), input_list, output_list + [middle_ware_component]).then(
         None, [middle_ware_component], None, _js=_js_callback
```

</p>
</details>
<details><summary><a href="https://github.com/docker/docker-py">docker/docker-py</a> (+44 -30 lines across 2 files)</summary>
<p>

<a href='https://github.com/docker/docker-py/blob/df3f8e2abc5a03de482e37214dddef9e0cee1bb1/tests/integration/api_build_test.py#L31'>tests/integration/api_build_test.py~L31</a>
```diff
                     'RUN env | grep "NO_PROXY=d"',
                     'RUN env | grep "no_proxy=d"',
                 ]
-            ).encode("ascii")
+            )
+            .encode("ascii")
         )
 
         self.client.build(fileobj=script, decode=True)
```
<a href='https://github.com/docker/docker-py/blob/df3f8e2abc5a03de482e37214dddef9e0cee1bb1/tests/integration/api_build_test.py#L54'>tests/integration/api_build_test.py~L54</a>
```diff
                     'RUN env | grep "NO_PROXY=d"',
                     'RUN env | grep "no_proxy=d"',
                 ]
-            ).encode("ascii")
+            )
+            .encode("ascii")
         )
 
         self.client.build(
```
<a href='https://github.com/docker/docker-py/blob/df3f8e2abc5a03de482e37214dddef9e0cee1bb1/tests/integration/api_build_test.py#L73'>tests/integration/api_build_test.py~L73</a>
```diff
                     "ADD https://dl.dropboxusercontent.com/u/20637798/silence.tar.gz"
                     " /tmp/silence.tar.gz",
                 ]
-            ).encode("ascii")
+            )
+            .encode("ascii")
         )
         stream = self.client.build(fileobj=script, decode=True)
         logs = []
```
<a href='https://github.com/docker/docker-py/blob/df3f8e2abc5a03de482e37214dddef9e0cee1bb1/tests/integration/api_build_test.py#L197'>tests/integration/api_build_test.py~L197</a>
```diff
                     "FROM scratch",
                     "CMD sh -c \"echo 'Hello, World!'\"",
                 ]
-            ).encode("ascii")
+            )
+            .encode("ascii")
         )
 
         tag = "shmsize"
```
<a href='https://github.com/docker/docker-py/blob/df3f8e2abc5a03de482e37214dddef9e0cee1bb1/tests/integration/api_build_test.py#L214'>tests/integration/api_build_test.py~L214</a>
```diff
     @requires_api_version("1.24")
     def test_build_isolation(self):
         script = io.BytesIO(
-            "\n".join(
-                ["FROM scratch", "CMD sh -c \"echo 'Deaf To All But The Song'"]
-            ).encode("ascii")
+            "\n".join(["FROM scratch", "CMD sh -c \"echo 'Deaf To All But The Song'"])
+            .encode("ascii")
         )
 
         stream = self.client.build(fileobj=script, tag="isolation", isolation="default")
```
<a href='https://github.com/docker/docker-py/blob/df3f8e2abc5a03de482e37214dddef9e0cee1bb1/tests/integration/api_build_test.py#L231'>tests/integration/api_build_test.py~L231</a>
```diff
                 [
                     "FROM scratch",
                 ]
-            ).encode("ascii")
+            )
+            .encode("ascii")
         )
 
         labels = {"test": "OK"}
```
<a href='https://github.com/docker/docker-py/blob/df3f8e2abc5a03de482e37214dddef9e0cee1bb1/tests/integration/api_build_test.py#L254'>tests/integration/api_build_test.py~L254</a>
```diff
                     "RUN touch baz",
                     "RUN touch bax",
                 ]
-            ).encode("ascii")
+            )
+            .encode("ascii")
         )
 
         stream = self.client.build(fileobj=script, tag="build1")
```
<a href='https://github.com/docker/docker-py/blob/df3f8e2abc5a03de482e37214dddef9e0cee1bb1/tests/integration/api_build_test.py#L294'>tests/integration/api_build_test.py~L294</a>
```diff
                     "WORKDIR /root/COPY --from=first /tmp/silence.tar.gz .",
                     'ONBUILD RUN echo "This should not be in the final image"',
                 ]
-            ).encode("ascii")
+            )
+            .encode("ascii")
         )
 
         stream = self.client.build(fileobj=script, target="first", tag="build1")
```
<a href='https://github.com/docker/docker-py/blob/df3f8e2abc5a03de482e37214dddef9e0cee1bb1/tests/integration/api_build_test.py#L318'>tests/integration/api_build_test.py~L318</a>
```diff
         )
 
         script = io.BytesIO(
-            "\n".join(["FROM busybox", "RUN ping -c1 pingtarget.docker"]).encode(
-                "ascii"
-            )
+            "\n".join(["FROM busybox", "RUN ping -c1 pingtarget.docker"])
+            .encode("ascii")
         )
 
         stream = self.client.build(
```
<a href='https://github.com/docker/docker-py/blob/df3f8e2abc5a03de482e37214dddef9e0cee1bb1/tests/integration/api_build_test.py#L363'>tests/integration/api_build_test.py~L363</a>
```diff
                     "RUN ping -c1 extrahost.local.test",
                     "RUN cp /etc/hosts /hosts-file",
                 ]
-            ).encode("ascii")
+            )
+            .encode("ascii")
         )
 
         stream = self.client.build(
```
<a href='https://github.com/docker/docker-py/blob/df3f8e2abc5a03de482e37214dddef9e0cee1bb1/tests/integration/api_build_test.py#L397'>tests/integration/api_build_test.py~L397</a>
```diff
                     "RUN echo blahblah > /file_2",
                     "RUN echo blahblahblah > /file_3",
                 ]
-            ).encode("ascii")
+            )
+            .encode("ascii")
         )
 
         def build_squashed(squash):
```
<a href='https://github.com/docker/docker-py/blob/df3f8e2abc5a03de482e37214dddef9e0cee1bb1/tests/ssh/api_build_test.py#L31'>tests/ssh/api_build_test.py~L31</a>
```diff
                     'RUN env | grep "NO_PROXY=d"',
                     'RUN env | grep "no_proxy=d"',
                 ]
-            ).encode("ascii")
+            )
+            .encode("ascii")
         )
 
         self.client.build(fileobj=script, decode=True)
```
<a href='https://github.com/docker/docker-py/blob/df3f8e2abc5a03de482e37214dddef9e0cee1bb1/tests/ssh/api_build_test.py#L54'>tests/ssh/api_build_test.py~L54</a>
```diff
                     'RUN env | grep "NO_PROXY=d"',
                     'RUN env | grep "no_proxy=d"',
                 ]
-            ).encode("ascii")
+            )
+            .encode("ascii")
         )
 
         self.client.build(
```
<a href='https://github.com/docker/docker-py/blob/df3f8e2abc5a03de482e37214dddef9e0cee1bb1/tests/ssh/api_build_test.py#L73'>tests/ssh/api_build_test.py~L73</a>
```diff
                     "ADD https://dl.dropboxusercontent.com/u/20637798/silence.tar.gz"
                     " /tmp/silence.tar.gz",
                 ]
-            ).encode("ascii")
+            )
+            .encode("ascii")
         )
         stream = self.client.build(fileobj=script, decode=True)
         logs = []
```
<a href='https://github.com/docker/docker-py/blob/df3f8e2abc5a03de482e37214dddef9e0cee1bb1/tests/ssh/api_build_test.py#L189'>tests/ssh/api_build_test.py~L189</a>
```diff
                     "FROM scratch",
                     "CMD sh -c \"echo 'Hello, World!'\"",
                 ]
-            ).encode("ascii")
+            )
+            .encode("ascii")
         )
 
         tag = "shmsize"
```
<a href='https://github.com/docker/docker-py/blob/df3f8e2abc5a03de482e37214dddef9e0cee1bb1/tests/ssh/api_build_test.py#L206'>tests/ssh/api_build_test.py~L206</a>
```diff
     @requires_api_version("1.24")
     def test_build_isolation(self):
         script = io.BytesIO(
-            "\n".join(
-                ["FROM scratch", "CMD sh -c \"echo 'Deaf To All But The Song'"]
-            ).encode("ascii")
+            "\n".join(["FROM scratch", "CMD sh -c \"echo 'Deaf To All But The Song'"])
+            .encode("ascii")
         )
 
         stream = self.client.build(fileobj=script, tag="isolation", isolation="default")
```
<a href='https://github.com/docker/docker-py/blob/df3f8e2abc5a03de482e37214dddef9e0cee1bb1/tests/ssh/api_build_test.py#L223'>tests/ssh/api_build_test.py~L223</a>
```diff
                 [
                     "FROM scratch",
                 ]
-            ).encode("ascii")
+            )
+            .encode("ascii")
         )
 
         labels = {"test": "OK"}
```
<a href='https://github.com/docker/docker-py/blob/df3f8e2abc5a03de482e37214dddef9e0cee1bb1/tests/ssh/api_build_test.py#L246'>tests/ssh/api_build_test.py~L246</a>
```diff
                     "RUN touch baz",
                     "RUN touch bax",
                 ]
-            ).encode("ascii")
+            )
+            .encode("ascii")
         )
 
         stream = self.client.build(fileobj=script, tag="build1")
```
<a href='https://github.com/docker/docker-py/blob/df3f8e2abc5a03de482e37214dddef9e0cee1bb1/tests/ssh/api_build_test.py#L286'>tests/ssh/api_build_test.py~L286</a>
```diff
                     "WORKDIR /root/COPY --from=first /tmp/silence.tar.gz .",
                     'ONBUILD RUN echo "This should not be in the final image"',
                 ]
-            ).encode("ascii")
+            )
+            .encode("ascii")
         )
 
         stream = self.client.build(fileobj=script, target="first", tag="build1")
```
<a href='https://github.com/docker/docker-py/blob/df3f8e2abc5a03de482e37214dddef9e0cee1bb1/tests/ssh/api_build_test.py#L310'>tests/ssh/api_build_test.py~L310</a>
```diff
         )
 
         script = io.BytesIO(
-            "\n".join(["FROM busybox", "RUN ping -c1 pingtarget.docker"]).encode(
-                "ascii"
-            )
+            "\n".join(["FROM busybox", "RUN ping -c1 pingtarget.docker"])
+            .encode("ascii")
         )
 
         stream = self.client.build(
```
<a href='https://github.com/docker/docker-py/blob/df3f8e2abc5a03de482e37214dddef9e0cee1bb1/tests/ssh/api_build_test.py#L355'>tests/ssh/api_build_test.py~L355</a>
```diff
                     "RUN ping -c1 extrahost.local.test",
                     "RUN cp /etc/hosts /hosts-file",
                 ]
-            ).encode("ascii")
+            )
+            .encode("ascii")
         )
 
         stream = self.client.build(
```
<a href='https://github.com/docker/docker-py/blob/df3f8e2abc5a03de482e37214dddef9e0cee1bb1/tests/ssh/api_build_test.py#L389'>tests/ssh/api_build_test.py~L389</a>
```diff
                     "RUN echo blahblah > /file_2",
                     "RUN echo blahblahblah > /file_3",
                 ]
-            ).encode("ascii")
+            )
+            .encode("ascii")
         )
 
         def build_squashed(squash):
```

</p>
</details>
<details><summary><a href="https://github.com/facebookresearch/chameleon">facebookresearch/chameleon</a> (+2 -1 lines across 1 file)</summary>
<p>

<a href='https://github.com/facebookresearch/chameleon/blob/dbbe47fdbc7a3cc4982ae931fea75e227262e70a/chameleon/inference/chameleon.py#L131'>chameleon/inference/chameleon.py~L131</a>
```diff
             [self.vocab.begin_image]
             + self.translation.convert_img2bp2(
                 self.image_tokenizer.img_tokens_from_pil(img)
-            ).tolist()
+            )
+            .tolist()
             + [self.vocab.end_image]
         )
 
```

</p>
</details>
<details><summary><a href="https://github.com/freedomofpress/securedrop">freedomofpress/securedrop</a> (+8 -6 lines across 2 files)</summary>
<p>

<a href='https://github.com/freedomofpress/securedrop/blob/abee0e643ea1cb5d434fdd104196d9ab092005be/securedrop/alembic/versions/d9d36b6f4d1e_remove_partial_index_on_valid_until_set_.py#L28'>securedrop/alembic/versions/d9d36b6f4d1e_remove_partial_index_on_valid_until_set_.py~L28</a>
```diff
     op.execute(
         sa.text(
             "UPDATE OR IGNORE instance_config SET valid_until=:epoch WHERE valid_until IS NULL;"
-        ).bindparams(epoch=datetime.fromtimestamp(0))
+        )
+        .bindparams(epoch=datetime.fromtimestamp(0))
     )
 
     # add new columns with default values
```
<a href='https://github.com/freedomofpress/securedrop/blob/abee0e643ea1cb5d434fdd104196d9ab092005be/securedrop/alembic/versions/d9d36b6f4d1e_remove_partial_index_on_valid_until_set_.py#L58'>securedrop/alembic/versions/d9d36b6f4d1e_remove_partial_index_on_valid_until_set_.py~L58</a>
```diff
 
     # valid_until is nullable again, set entries with unix epoch datetime value to NULL
     op.execute(
-        sa.text(
-            "UPDATE OR IGNORE instance_config SET valid_until = NULL WHERE valid_until=:epoch;"
-        ).bindparams(epoch=datetime.fromtimestamp(0))
+        sa.text("UPDATE OR IGNORE instance_config SET valid_until = NULL WHERE valid_until=:epoch;")
+        .bindparams(epoch=datetime.fromtimestamp(0))
     )
 
     # manually restore the partial index
```
<a href='https://github.com/freedomofpress/securedrop/blob/abee0e643ea1cb5d434fdd104196d9ab092005be/securedrop/source_app/main.py#L273'>securedrop/source_app/main.py~L273</a>
```diff
         if msg:
             logged_in_source_in_db.interaction_count += 1
             fnames.append(
-                Storage.get_default().save_message_submission(
+                Storage.get_default()
+                .save_message_submission(
                     logged_in_source_in_db.filesystem_id,
                     logged_in_source_in_db.interaction_count,
                     logged_in_source_in_db.journalist_filename,
```
<a href='https://github.com/freedomofpress/securedrop/blob/abee0e643ea1cb5d434fdd104196d9ab092005be/securedrop/source_app/main.py#L283'>securedrop/source_app/main.py~L283</a>
```diff
         if fh:
             logged_in_source_in_db.interaction_count += 1
             fnames.append(
-                Storage.get_default().save_file_submission(
+                Storage.get_default()
+                .save_file_submission(
                     logged_in_source_in_db.filesystem_id,
                     logged_in_source_in_db.interaction_count,
                     logged_in_source_in_db.journalist_filename,
```

</p>
</details>
<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+2 -1 lines across 1 file)</summary>
<p>

<a href='https://github.com/latchbio/latch/blob/dc91e5b22db76d1dcab1acf91c3afbcc597c0c3a/src/latch_cli/nextflow/config.py#L63'>src/latch_cli/nextflow/config.py~L63</a>
```diff
         default_value = gpjson.MessageToDict(
             TypeEngine.to_literal(
                 ctx, python_val, python_typ, TypeEngine.to_literal_type(python_typ)
-            ).to_flyte_idl()
+            )
+            .to_flyte_idl()
         )
 
     annotated = Annotated[
```

</p>
</details>
<details><summary><a href="https://github.com/milvus-io/pymilvus">milvus-io/pymilvus</a> (+4 -2 lines across 2 files)</summary>
<p>

<a href='https://github.com/milvus-io/pymilvus/blob/f01620ee42018d8d4f39e02ee771a78e8bd6e3d6/examples/bulk_import/bulk_writer_all_types.py#L206'>examples/bulk_import/bulk_writer_all_types.py~L206</a>
```diff
         "json": {"dummy": i},
         "timestamp": shanghai_tz.localize(
             datetime.datetime(2025, 1, 1, 0, 0, 0) + datetime.timedelta(days=i)
-        ).isoformat(),
+        )
+        .isoformat(),
         "geometry": f"POINT ({i} {i})",
         "array_bool": [True if (i + k) % 2 == 0 else False for k in range(4)],
         "array_int8": [(i + k) % 128 for k in range(4)],
```
<a href='https://github.com/milvus-io/pymilvus/blob/f01620ee42018d8d4f39e02ee771a78e8bd6e3d6/examples/timestamptz.py#L49'>examples/timestamptz.py~L49</a>
```diff
             "id": i + 1,
             "tsz": shanghai_tz.localize(
                 datetime.datetime(2025, 1, 1, 0, 0, 0) + datetime.timedelta(days=i)
-            ).isoformat(),
+            )
+            .isoformat(),
             "vec": [float(i) / 10 for i in range(4)],
         }
         for i in range(data_size)
```

</p>
</details>
<details><summary><a href="https://github.com/pypa/pip">pypa/pip</a> (+8 -4 lines across 2 files)</summary>
<p>

<a href='https://github.com/pypa/pip/blob/db8121d6c3bb1543c6c4fca48de010814d1ddcb2/tests/functional/test_install_reqs.py#L277'>tests/functional/test_install_reqs.py~L277</a>
```diff
             -e {}@10#egg=INITools
             -r {}-req.txt
         """
-        ).format(
+        )
+        .format(
             local_checkout("svn+http://svn.colorstudy.com/INITools", tmpdir),
             other_lib_name,
         ),
```
<a href='https://github.com/pypa/pip/blob/db8121d6c3bb1543c6c4fca48de010814d1ddcb2/tests/functional/test_install_reqs.py#L833'>tests/functional/test_install_reqs.py~L833</a>
```diff
             """\
             {url}; {req}
         """
-        ).format(
+        )
+        .format(
             url="https://github.com/a/b/c/asdf-1.5.2-cp27-none-xyz.whl",
             req='sys_platform == "xyz"',
         )
```
<a href='https://github.com/pypa/pip/blob/db8121d6c3bb1543c6c4fca48de010814d1ddcb2/tests/functional/test_uninstall.py#L466'>tests/functional/test_uninstall.py~L466</a>
```diff
             # and something else to test out:
             PyLogo<0.4
         """
-        ).format(url=local_svn_url)
+        )
+        .format(url=local_svn_url)
     )
     result = script.pip("install", "-r", "test-req.txt")
     script.scratch_path.joinpath("test-req.txt").write_text(
```
<a href='https://github.com/pypa/pip/blob/db8121d6c3bb1543c6c4fca48de010814d1ddcb2/tests/functional/test_uninstall.py#L481'>tests/functional/test_uninstall.py~L481</a>
```diff
             # and something else to test out:
             PyLogo<0.4
         """
-        ).format(url=local_svn_url)
+        )
+        .format(url=local_svn_url)
     )
     result2 = script.pip("uninstall", "-r", "test-req.txt", "-y")
     assert_all_changes(
```

</p>
</details>
<details><summary><a href="https://github.com/qdrant/qdrant-client">qdrant/qdrant-client</a> (+20 -30 lines across 2 files)</summary>
<p>

<a href='https://github.com/qdrant/qdrant-client/blob/750d735b2917a30b0cbf468ddccbc47d12704e32/tests/conversions/test_validate_conversions.py#L43'>tests/conversions/test_validate_conversions.py~L43</a>
```diff
 
             try:
                 result = list(
-                    inspect.signature(
-                        grpc_to_rest_convert[convert_function_name]
-                    ).parameters.keys()
+                    inspect.signature(grpc_to_rest_convert[convert_function_name])
+                    .parameters.keys()
                 )
                 if "collection_name" in result:
                     rest_fixture = grpc_to_rest_convert[convert_function_name](
```
<a href='https://github.com/qdrant/qdrant-client/blob/750d735b2917a30b0cbf468ddccbc47d12704e32/tests/conversions/test_validate_conversions.py#L61'>tests/conversions/test_validate_conversions.py~L61</a>
```diff
                 )
 
                 result = list(
-                    inspect.signature(
-                        rest_to_grpc_convert[back_convert_function_name]
-                    ).parameters.keys()
+                    inspect.signature(rest_to_grpc_convert[back_convert_function_name])
+                    .parameters.keys()
                 )
                 if "collection_name" in result:
                     grpc_fixture = rest_to_grpc_convert[back_convert_function_name](
```
<a href='https://github.com/qdrant/qdrant-client/blob/750d735b2917a30b0cbf468ddccbc47d12704e32/tests/embed_tests/test_local_inference.py#L387'>tests/embed_tests/test_local_inference.py~L387</a>
```diff
     remote_client.upload_points(COLLECTION_NAME, points, wait=True)
     assert remote_client.count(COLLECTION_NAME).count == len(points)
     assert isinstance(
-        remote_client.retrieve(COLLECTION_NAME, ids=[1], with_vectors=True)[0].vector[
-            "text"
-        ],
+        remote_client.retrieve(COLLECTION_NAME, ids=[1], with_vectors=True)[0]
+        .vector["text"],
         list,
     )  # assert doc has been substituted with its embedding
 
```
<a href='https://github.com/qdrant/qdrant-client/blob/750d735b2917a30b0cbf468ddccbc47d12704e32/tests/embed_tests/test_local_inference.py#L422'>tests/embed_tests/test_local_inference.py~L422</a>
```diff
 
     assert remote_client.count(COLLECTION_NAME).count == len(vectors)
     assert isinstance(
-        remote_client.retrieve(COLLECTION_NAME, ids=[1], with_vectors=True)[0].vector[
-            "text"
-        ],
+        remote_client.retrieve(COLLECTION_NAME, ids=[1], with_vectors=True)[0]
+        .vector["text"],
         list,
     )  # assert doc has been substituted with its embedding
 
```
<a href='https://github.com/qdrant/qdrant-client/blob/750d735b2917a30b0cbf468ddccbc47d12704e32/tests/embed_tests/test_local_inference.py#L435'>tests/embed_tests/test_local_inference.py~L435</a>
```diff
     )
     assert remote_client.count(COLLECTION_NAME).count == len(points)
     assert isinstance(
-        remote_client.retrieve(COLLECTION_NAME, ids=[1], with_vectors=True)[0].vector[
-            "text"
-        ],
+        remote_client.retrieve(COLLECTION_NAME, ids=[1], with_vectors=True)[0]
+        .vector["text"],
         list,
     )  # assert doc has been substituted with its embedding
 
```
<a href='https://github.com/qdrant/qdrant-client/blob/750d735b2917a30b0cbf468ddccbc47d12704e32/tests/embed_tests/test_local_inference.py#L449'>tests/embed_tests/test_local_inference.py~L449</a>
```diff
 
     assert remote_client.count(COLLECTION_NAME).count == len(vectors)
     assert isinstance(
-        remote_client.retrieve(COLLECTION_NAME, ids=[1], with_vectors=True)[0].vector[
-            "text"
-        ],
+        remote_client.retrieve(COLLECTION_NAME, ids=[1], with_vectors=True)[0]
+        .vector["text"],
         list,
     )  # assert doc has been substituted with its embedding
 
```
<a href='https://github.com/qdrant/qdrant-client/blob/750d735b2917a30b0cbf468ddccbc47d12704e32/tests/embed_tests/test_local_inference.py#L465'>tests/embed_tests/test_local_inference.py~L465</a>
```diff
 
     assert remote_client.count(COLLECTION_NAME).count == len(points)
     assert isinstance(
-        remote_client.retrieve(COLLECTION_NAME, ids=[1], with_vectors=True)[0].vector[
-            "text"
-        ],
+        remote_client.retrieve(COLLECTION_NAME, ids=[1], with_vectors=True)[0]
+        .vector["text"],
         list,
     )  # assert doc has been substituted with its embedding
 
```
<a href='https://github.com/qdrant/qdrant-client/blob/750d735b2917a30b0cbf468ddccbc47d12704e32/tests/embed_tests/test_local_inference.py#L486'>tests/embed_tests/test_local_inference.py~L486</a>
```diff
 
     assert remote_client.count(COLLECTION_NAME).count == len(vectors)
     assert isinstance(
-        remote_client.retrieve(COLLECTION_NAME, ids=[1], with_vectors=True)[0].vector[
-            "text"
-        ],
+        remote_client.retrieve(COLLECTION_NAME, ids=[1], with_vectors=True)[0]
+        .vector["text"],
         list,
     )  # assert doc has been substituted with its embedding
 
```
<a href='https://github.com/qdrant/qdrant-client/blob/750d735b2917a30b0cbf468ddccbc47d12704e32/tests/embed_tests/test_local_inference.py#L1541'>tests/embed_tests/test_local_inference.py~L1541</a>
```diff
 
     assert remote_client.count(COLLECTION_NAME).count == len(points)
     assert np.allclose(
-        remote_client.retrieve(COLLECTION_NAME, ids=[2], with_vectors=True)[0].vector[
-            "plain"
-        ],
+        remote_client.retrieve(COLLECTION_NAME, ids=[2], with_vectors=True)[0]
+        .vector["plain"],
         norm_ref_vector,
     )
     remote_client.delete_collection(COLLECTION_NAME)
```
<a href='https://github.com/qdrant/qdrant-client/blob/750d735b2917a30b0cbf468ddccbc47d12704e32/tests/embed_tests/test_local_inference.py#L1675'>tests/embed_tests/test_local_inference.py~L1675</a>
```diff
 
     assert remote_client.count(COLLECTION_NAME).count == len(vectors)
     assert np.allclose(
-        remote_client.retrieve(COLLECTION_NAME, ids=[1], with_vectors=True)[0].vector[
-            "plain"
-        ],
+        remote_client.retrieve(COLLECTION_NAME, ids=[1], with_vectors=True)[0]
+        .vector["plain"],
         norm_ref_vector,
     )
 
```

</p>
</details>
<details><summary><a href="https://github.com/reflex-dev/reflex">reflex-dev/reflex</a> (+20 -15 lines across 7 files)</summary>
<p>

<a href='https://github.com/reflex-dev/reflex/blob/e4300adbbec62630cb3ce5c0610400788557e9b9/reflex/components/component.py#L1955'>reflex/components/component.py~L1955</a>
```diff
                 else (
                     annotation_args[1]
                     if get_origin(
-                        annotation := inspect.getfullargspec(component_fn).annotations[
-                            key
-                        ]
+                        annotation := inspect.getfullargspec(component_fn)
+                        .annotations[key]
                     )
                     is typing.Annotated
                     and (annotation_args := get_args(annotation))
```
<a href='https://github.com/reflex-dev/reflex/blob/e4300adbbec62630cb3ce5c0610400788557e9b9/reflex/components/markdown/markdown.py#L290'>reflex/components/markdown/markdown.py~L290</a>
```diff
             *(
                 [inline_code_var_data.old_school_imports()]
                 if (
-                    inline_code_var_data
-                    := self._get_inline_code_fn_var()._get_all_var_data()
+                    inline_code_var_data := self._get_inline_code_fn_var()
+                    ._get_all_var_data()
                 )
                 is not None
                 else []
```
<a href='https://github.com/reflex-dev/reflex/blob/e4300adbbec62630cb3ce5c0610400788557e9b9/reflex/event.py#L687'>reflex/event.py~L687</a>
```diff
                 "meta_key": e.metaKey,
                 "shift_key": e.shiftKey,
             },
-        ).to(KeyInputInfo),
+        )
+        .to(KeyInputInfo),
     )
 
 
```
<a href='https://github.com/reflex-dev/reflex/blob/e4300adbbec62630cb3ce5c0610400788557e9b9/reflex/event.py#L755'>reflex/event.py~L755</a>
```diff
                 "meta_key": e.metaKey,
                 "shift_key": e.shiftKey,
             },
-        ).to(PointerEventInfo),
+        )
+        .to(PointerEventInfo),
     )
 
 
```
<a href='https://github.com/reflex-dev/reflex/blob/e4300adbbec62630cb3ce5c0610400788557e9b9/reflex/experimental/client_state.py#L152'>reflex/experimental/client_state.py~L152</a>
```diff
                                 f"(value) => {{ {_client_state_ref(var_name)} = value; }}"
                             )
                         ]
-                    ).to(list),
+                    )
+                    .to(list),
                     ArgsFunctionOperationBuilder.create(
                         args_names=("setter",),
                         return_expr=Var("setter").to(FunctionVar).call(Var(arg_name)),
```
<a href='https://github.com/reflex-dev/reflex/blob/e4300adbbec62630cb3ce5c0610400788557e9b9/reflex/vars/base.py#L3253'>reflex/vars/base.py~L3253</a>
```diff
                 _var_type=fn_return_type,
                 _var_data=var_data,
                 _js_expr=field_name,
-            ).guess_type()
+            )
+            .guess_type()
         )
 
         return fn(var)
```
<a href='https://github.com/reflex-dev/reflex/blob/e4300adbbec62630cb3ce5c0610400788557e9b9/tests/units/components/test_component.py#L1066'>tests/units/components/test_component.py~L1066</a>
```diff
             component3.create(),
             component4.create(),
             component3.create(),
-        )._get_all_hooks()
+        )
+        ._get_all_hooks()
         == exp_hooks
     )
 
```
<a href='https://github.com/reflex-dev/reflex/blob/e4300adbbec62630cb3ce5c0610400788557e9b9/tests/units/components/test_component.py#L1961'>tests/units/components/test_component.py~L1961</a>
```diff
         BaseComponent.create(
             GrandchildComponent1.create(GreatGrandchildComponent2.create()),
             GreatGrandchildComponent1.create(),
-        )._get_all_hooks(),
+        )
+        ._get_all_hooks(),
     ) == [
         "const hook1 = 42",
         "const hook2 = 43",
```
<a href='https://github.com/reflex-dev/reflex/blob/e4300adbbec62630cb3ce5c0610400788557e9b9/tests/units/components/test_component.py#L1974'>tests/units/components/test_component.py~L1974</a>
```diff
         Fragment.create(
             GreatGrandchildComponent2.create(),
             GreatGrandchildComponent1.create(),
-        )._get_all_hooks()
+        )
+        ._get_all_hooks()
     ) == [
         "const hook5 = 46",
         "const hook2 = 43",
```
<a href='https://github.com/reflex-dev/reflex/blob/e4300adbbec62630cb3ce5c0610400788557e9b9/tests/units/test_state.py#L2421'>tests/units/test_state.py~L2421</a>
```diff
 
     first_ws_message = emit_mock.mock_calls[0].args[1]  # pyright: ignore [reportAttributeAccessIssue]
     assert (
-        first_ws_message.delta[BackgroundTaskState.get_full_name()].pop(
-            "router" + FIELD_MARKER
-        )
+        first_ws_message.delta[BackgroundTaskState.get_full_name()]
+        .pop("router" + FIELD_MARKER)
         is not None
     )
     assert first_ws_message == StateUpdate(
```

</p>
</details>
<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+718 -465 lines across 66 files)</summary>
<p>

<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/package.py#L330'>package.py~L330</a>
```diff
                 'python -c "import os, sys; print(os.path.dirname(sys.executable))"',
                 encoding="utf8",
                 shell=True,
-            ).strip(),
+            )
+            .strip(),
         )
 
         if python_dir.name != "Scripts":
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/api/rest.py#L1068'>rotkehlchen/api/rest.py~L1068</a>
```diff
                 cursor.execute(
                     f"SELECT COUNT(*) FROM {table} WHERE tx_hash=?",
                     (event.tx_ref,),
-                ).fetchone()[0]
+                )
+                .fetchone()[0]
                 != 0
             ):
                 return None
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/api/rest.py#L2728'>rotkehlchen/api/rest.py~L2728</a>
```diff
                 old_endpoint := cursor.execute(
                     "SELECT endpoint FROM rpc_nodes WHERE identifier=?",
                     (node.identifier,),
-                ).fetchone()
+                )
+                .fetchone()
             ) is None:
                 return api_response(
                     wrap_in_fail_result(
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/api/rest.py#L3430'>rotkehlchen/api/rest.py~L3430</a>
```diff
                 cursor.execute(
                     f"SELECT COUNT(*) FROM blockchain_accounts WHERE blockchain IN ({','.join(['?'] * len(EVM_CHAINS_WITH_TRANSACTIONS))})",  # noqa: E501
                     [blockchain.value for blockchain in EVM_CHAINS_WITH_TRANSACTIONS],
-                ).fetchone()[0]
+                )
+                .fetchone()[0]
                 > 0
             )
             exchanges_bindings_with_rotkehlchen = [
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/api/rest.py#L3440'>rotkehlchen/api/rest.py~L3440</a>
```diff
                 cursor.execute(
                     f"SELECT COUNT(*) FROM user_credentials WHERE location IN ({','.join(['?'] * len(SUPPORTED_EXCHANGES))}) AND name != ?",  # noqa: E501
                     exchanges_bindings_with_rotkehlchen,
-                ).fetchone()[0]
+                )
+                .fetchone()[0]
                 > 0
             )
 
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/api/rest.py#L3475'>rotkehlchen/api/rest.py~L3475</a>
```diff
                 cursor.execute(
                     f"SELECT COUNT(*) FROM blockchain_accounts WHERE blockchain IN ({','.join(['?'] * len(EVM_CHAINS_WITH_TRANSACTIONS))})",  # noqa: E501
                     [blockchain.value for blockchain in EVM_CHAINS_WITH_TRANSACTIONS],
-                ).fetchone()[0]
+                )
+                .fetchone()[0]
                 > 0
             )
 
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/api/rest.py#L3510'>rotkehlchen/api/rest.py~L3510</a>
```diff
             if (
                 undecoded_count := cursor.execute(
                     "SELECT COUNT(*) FROM zksynclite_transactions WHERE is_decoded=0",
-                ).fetchone()[0]
+                )
+                .fetchone()[0]
             ) != 0:
                 tx_info[chain_name := SupportedBlockchain.ZKSYNC_LITE.name.lower()][
                     "undecoded"
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/api/rest.py#L6541'>rotkehlchen/api/rest.py~L6541</a>
```diff
                 cursor.execute(
                     "SELECT COUNT(*) FROM user_added_solana_tokens WHERE identifier = ?",
                     (old_asset.identifier,),
-                ).fetchone()[0]
+                )
+                .fetchone()[0]
                 == 0
             ):
                 return wrap_in_fail_result(
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/api/rest.py#L6592'>rotkehlchen/api/rest.py~L6592</a>
```diff
 
             with GlobalDBHandler().conn.write_ctx() as write_cursor:
                 if (
-                    write_cursor.execute(
-                        "SELECT COUNT(*) FROM user_added_solana_tokens"
-                    ).fetchone()[0]
+                    write_cursor.execute("SELECT COUNT(*) FROM user_added_solana_tokens")
+                    .fetchone()[0]
                     == 0
                 ):  # noqa: E501
                     write_cursor.execute("DROP TABLE user_added_solana_tokens")
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/api/rest_helpers/history_events.py#L35'>rotkehlchen/api/rest_helpers/history_events.py~L35</a>
```diff
             := write_cursor.execute(  # Get the group_identifier, selecting by the identifier of the first event.  # noqa: E501
                 "SELECT group_identifier FROM history_events WHERE identifier=?",
                 (events[0].identifier,),
-            ).fetchone()
+            )
+            .fetchone()
         )
         is not None
     ):
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/api/rest_helpers/history_events.py#L129'>rotkehlchen/api/rest_helpers/history_events.py~L129</a>
```diff
             write_cursor.execute(  # Check if this event will hit a sequence_index that is already in use.  # noqa: E501
                 "SELECT COUNT(*) FROM history_events WHERE group_identifier=? AND sequence_index=?",
                 (group_identifier, event.sequence_index),
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             != 0
         ):
             raise InputError(
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/api/v1/schemas.py#L1168'>rotkehlchen/api/v1/schemas.py~L1168</a>
```diff
                     group_identifier=data["group_identifier"],
                     amount=data["amount"],
                     extra_data=extra_data,
-                    fee_identifier=CreateHistoryEventSchema.history_event_context.get()[
-                        "schema"
-                    ].get_grouped_event_identifier(
+                    fee_identifier=CreateHistoryEventSchema.history_event_context.get()["schema"]
+                    .get_grouped_event_identifier(
                         data=data,
                         subtype=HistoryEventSubType.FEE,
                         sequence_index_offset=1,
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/api/v1/schemas.py#L4535'>rotkehlchen/api/v1/schemas.py~L4535</a>
```diff
                     cursor.execute(
                         "SELECT COUNT(*) FROM blockchain_accounts WHERE account=? AND blockchain=?",
                         (address, (chain := data["chain"]).value),
-                    ).fetchone()[0]
+                    )
+                    .fetchone()[0]
                     == 0
                 ):
                     raise ValidationError(
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/chain/base/modules/basenames/decoder.py#L93'>rotkehlchen/chain/base/modules/basenames/decoder.py~L93</a>
```diff
         # Call the L2 Resolver contract's name method.
         try:
             if not (
-                name_to_show := self.node_inquirer.contracts.contract(BASENAMES_L2_RESOLVER).call(  # noqa: E501
+                name_to_show := self.node_inquirer.contracts.contract(BASENAMES_L2_RESOLVER)
+                .call(  # noqa: E501
                     node_inquirer=self.node_inquirer,
                     method_name="name",
                     arguments=[node],
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/chain/ethereum/oracles/uniswap.py#L88'>rotkehlchen/chain/ethereum/oracles/uniswap.py~L88</a>
```diff
                         self.routing_assets[chain_id].append(
                             get_or_create_evm_token(
                                 userdb=(
-                                    node_inquirer := Inquirer()
-                                    .get_evm_manager(chain_id)
-                                    .node_inquirer
+                                    node_inquirer := Inquirer().get_evm_manager(
+                                        chain_id
+                                    ).node_inquirer
                                 ).database,  # noqa: E501
                                 evm_address=string_to_evm_address(asset.identifier.split(":")[-1]),
                                 chain_id=chain_id,
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/chain/evm/active_management/manager.py#L36'>rotkehlchen/chain/evm/active_management/manager.py~L36</a>
```diff
             contract.functions.transfer(
                 to_address,
                 asset_raw_value(amount=amount, asset=token),
-            ).build_transaction(
+            )
+            .build_transaction(
                 {
                     "from": from_address,
                     "nonce": rpc_client.eth.get_transaction_count(from_address),
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/chain/evm/decoding/cowswap/decoder.py#L198'>rotkehlchen/chain/evm/decoding/cowswap/decoder.py~L198</a>
```diff
 
             trades.append(
                 CowswapSwapData(
-                    from_asset=self.base.exceptions_mappings.get(
-                        from_asset.identifier, from_asset
-                    ).resolve_to_crypto_asset(),  # noqa: E501
+                    from_asset=self.base.exceptions_mappings.get(from_asset.identifier, from_asset)
+                    .resolve_to_crypto_asset(),  # noqa: E501
                     from_amount=from_amount - fee_amount,  # fee is taken as part of from asset
-                    to_asset=self.base.exceptions_mappings.get(
-                        to_asset.identifier, to_asset
-                    ).resolve_to_crypto_asset(),  # noqa: E501
+                    to_asset=self.base.exceptions_mappings.get(to_asset.identifier, to_asset)
+                    .resolve_to_crypto_asset(),  # noqa: E501
                     to_amount=to_amount,
                     fee_amount=fee_amount,
                     order_uid=order_uid,
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/chain/evm/decoding/curve/curve_cache.py#L665'>rotkehlchen/chain/evm/decoding/curve/curve_cache.py~L665</a>
```diff
             key := cursor.execute(
                 "SELECT key FROM unique_cache WHERE value = ?",
                 (pool_address,),
-            ).fetchone()
+            )
+            .fetchone()
         ) is not None:
             addresses.add(key[0][-42:])  # 42 is the length of the EVM address
 
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/chain/evm/decoding/curve/lend/decoder.py#L301'>rotkehlchen/chain/evm/decoding/curve/lend/decoder.py~L301</a>
```diff
                 key := cursor.execute(
                     "SELECT key FROM unique_cache WHERE value = ?",
                     (controller_address,),
-                ).fetchone()
+                )
+                .fetchone()
             ) is None:
                 log.error(
                     "Failed to find Curve lending vault address for controller "
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/chain/evm/decoding/efp/decoder.py#L75'>rotkehlchen/chain/evm/decoding/efp/decoder.py~L75</a>
```diff
         try:
             if (
                 address := to_checksum_address(
-                    self.node_inquirer.contracts.contract(self.list_records_contract).call(  # noqa: E501
+                    self.node_inquirer.contracts.contract(self.list_records_contract)
+                    .call(  # noqa: E501
                         node_inquirer=self.node_inquirer,
                         method_name="getListUser",
                         arguments=[slot],
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/chain/evm/decoding/extrafi/balances.py#L97'>rotkehlchen/chain/evm/decoding/extrafi/balances.py~L97</a>
```diff
                     self.addresses_with_activity(
                         event_types=self.deposit_event_types,
                         assets=(self.extrafi_token,),
-                    ).keys()
+                    )
+                    .keys()
                 )
             )
             != 0
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/chain/evm/decoding/quickswap/utils.py#L95'>rotkehlchen/chain/evm/decoding/quickswap/utils.py~L95</a>
```diff
                         (token1_address := deserialize_evm_address(position_info["token1"])),
                     ],
                 )
-            ).hex(),
+            )
+            .hex(),
             init_code=pool_init_code_hash,
             is_init_code_hashed=True,
         )
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/chain/evm/transactions.py#L698'>rotkehlchen/chain/evm/transactions.py~L698</a>
```diff
             db_id=cursor.execute(
                 "SELECT identifier FROM evm_transactions WHERE tx_hash=? AND chain_id=?",
                 (tx_hash, transaction.chain_id.serialize_for_db()),
-            ).fetchone()[0],
+            )
+            .fetchone()[0],
             authorization_list=transaction.authorization_list,
         ), tx_receipt  # type: ignore  # tx_receipt can't be None here
 
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/chain/solana/transactions.py#L219'>rotkehlchen/chain/solana/transactions.py~L219</a>
```diff
                         "AND block_time <= COALESCE((SELECT MIN(block_time) FROM solana_transactions WHERE block_time > ?), ?) "  # noqa: E501
                         "ORDER BY block_time;",
                         (address, start_ts, start_ts, end_ts, end_ts),
-                    ).fetchall()
+                    )
+                    .fetchall()
                 )
                 != 0
             ):
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/data_import/importers/binance.py#L393'>rotkehlchen/data_import/importers/binance.py~L393</a>
```diff
                 "_".join(
                     hash_binance_csv_row(row)
                     for row in sorted(trade_rows, key=operator.itemgetter(INDEX))
-                ).encode()
+                )
+                .encode()
             ).hexdigest()  # noqa: E501
             swap_events.extend(
                 create_swap_events(
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/data_migrations/migrations/migration_20.py#L201'>rotkehlchen/data_migrations/migrations/migration_20.py~L201</a>
```diff
                 affected_events_count := cursor.execute(
                     f"SELECT COUNT(*) FROM history_events WHERE event_identifier IN ({placeholders})",
                     location_hashes,
-                ).fetchone()[0]
+                )
+                .fetchone()[0]
             ) == 0:
                 log.debug("no trades affected by v1.39.0 upgrade bug")
                 return
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/db/accounting_rules.py#L125'>rotkehlchen/db/accounting_rules.py~L125</a>
```diff
                             else NO_ACCOUNTING_COUNTERPARTY,
                             *rule.serialize_for_db(),
                         ),
-                    ).fetchone()
+                    )
+                    .fetchone()
                 ) is not None:  # add event IDs to the existing event-specific rule
                     write_cursor.executemany(
                         "INSERT INTO accounting_rule_events(rule_id, event_id) VALUES (?, ?)",
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/db/accounting_rules.py#L563'>rotkehlchen/db/accounting_rules.py~L563</a>
```diff
             "JOIN accounting_rule_events ON accounting_rules.identifier = rule_id "
             "WHERE event_id=?",
             (event.identifier,),
-        ).fetchone()
+        )
+        .fetchone()
     ) is not None:
         cache_identifier = event.get_accounting_rule_key()
     else:  # if no event-specific rule found, try type-based rules
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/db/addressbook.py#L103'>rotkehlchen/db/addressbook.py~L103</a>
```diff
                         entry.address,
                         entry.blockchain.value if entry.blockchain else entry.get_ecosystem_key(),
                     ),  # noqa: E501
-                ).fetchone()[0]
+                )
+                .fetchone()[0]
                 != 0
             ):
                 raise InputError(
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/db/addressbook.py#L263'>rotkehlchen/db/addressbook.py~L263</a>
```diff
                     names := cursor.execute(
                         "SELECT name FROM address_book WHERE address=?",
                         (address,),
-                    ).fetchall()
+                    )
+                    .fetchall()
                 )
             ) == 0 or len(set(names)) > 1:
                 return  # If no name, or different names on different chains, leave as is.
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/db/dbhandler.py#L452'>rotkehlchen/db/dbhandler.py~L452</a>
```diff
     ) -> int | Timestamp | bool | Asset | list["ExchangeLocationID"] | str | None:
         deserializer, default_value = self.setting_to_default_type[name]
         if (
-            result := cursor.execute(
-                "SELECT value FROM settings WHERE name=?;", (name,)
-            ).fetchone()
+            result := cursor.execute("SELECT value FROM settings WHERE name=?;", (name,))
+            .fetchone()
         ) is not None:  # noqa: E501
             return deserializer(result[0])  # type: ignore
 
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/db/dbhandler.py#L742'>rotkehlchen/db/dbhandler.py~L742</a>
```diff
             value := cursor.execute(
                 "SELECT value FROM key_value_cache WHERE name=?;",
                 (name.value,),
-            ).fetchone()
+            )
+            .fetchone()
         ) is None:
             return None
 
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/db/solanatx.py#L257'>rotkehlchen/db/solanatx.py~L257</a>
```diff
                     "WHERE M.address = ? AND M.tx_id NOT IN ("
                     "SELECT tx_id FROM solanatx_address_mappings WHERE address != ?)",
                     (address, address),
-                ).fetchall()
+                )
+                .fetchall()
             )
             == 0
         ):
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/db/unresolved_conflicts.py#L98'>rotkehlchen/db/unresolved_conflicts.py~L98</a>
```diff
                     "taxable": entry[4],
                     "count_entire_amount_spend": entry[5],
                     "count_cost_basis_pnl": entry[6],
-                    "accounting_treatment": TxAccountingTreatment.deserialize_from_db(
-                        entry[7]
-                    ).serialize()
+                    "accounting_treatment": TxAccountingTreatment.deserialize_from_db(entry[7])
+                    .serialize()
                     if entry[7] is not None
                     else None,  # noqa: E501
                 }
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/db/upgrades/v38_v39.py#L269'>rotkehlchen/db/upgrades/v38_v39.py~L269</a>
```diff
         table_exists = (
             write_cursor.execute(
                 "SELECT COUNT(*) FROM sqlite_master WHERE type='table' AND name='rpc_nodes'",
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == 1
         )
         if table_exists:
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/db/upgrades/v45_v46.py#L43'>rotkehlchen/db/upgrades/v45_v46.py~L43</a>
```diff
         if (
             active_modules_result := write_cursor.execute(
                 "SELECT value FROM settings where name='active_modules'"
-            ).fetchone()
+            )
+            .fetchone()
         ) is None:  # noqa: E501
             return None
 
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/db/upgrades/v48_v49.py#L31'>rotkehlchen/db/upgrades/v48_v49.py~L31</a>
```diff
         if (
             write_cursor.execute(
                 "SELECT name FROM sqlite_master WHERE type='table' AND name='zksynclite_swaps'"
-            ).fetchone()
+            )
+            .fetchone()
             is None
         ):  # noqa: E501
             log.debug("zksynclite_swaps table does not exist, skipping upgrade")
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/db/utils.py#L327'>rotkehlchen/db/utils.py~L327</a>
```diff
         cursor.execute(
             "SELECT COUNT(*) FROM sqlite_master WHERE type='table' AND name=?",
             (name,),
-        ).fetchone()[0]
+        )
+        .fetchone()[0]
         == 1
     )
     if exists and schema is not None:
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/exchanges/binance.py#L804'>rotkehlchen/exchanges/binance.py~L804</a>
```diff
                         continue
 
                     event = HistoryEvent(
-                        group_identifier=hashlib.sha256(
-                            str(entry).encode()
-                        ).hexdigest(),  # entry hash  # noqa: E501
+                        group_identifier=hashlib.sha256(str(entry).encode())
+                        .hexdigest(),  # entry hash  # noqa: E501
                         sequence_index=0,  # since group_identifier is always different
                         timestamp=timestamp,
                         location=self.location,
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/exchanges/utils.py#L91'>rotkehlchen/exchanges/utils.py~L91</a>
```diff
                 self.secret,
                 message,
                 digest_algorithm,
-            ).digest(),
+            )
+            .digest(),
         ).decode("utf-8")
 
     def generate_hmac_digest(
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/externalapis/defillama.py#L306'>rotkehlchen/externalapis/defillama.py~L306</a>
```diff
                 coin_id_mapping={coin_id: from_asset},
                 from_assets=[from_asset],
                 to_asset=to_asset,
-            ).get(from_asset, ZERO_PRICE)
+            )
+            .get(from_asset, ZERO_PRICE)
         ) == ZERO_PRICE:
             raise NoPriceForGivenTimestamp(
                 from_asset=from_asset,
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/externalapis/opensea.py#L71'>rotkehlchen/externalapis/opensea.py~L71</a>
```diff
                 continue
 
             with GlobalDBHandler().conn.read_ctx() as cursor:
-                asset_identifier = cursor.execute(  # we consider as correct to query only by address knowing that the same address could exist in different chains until opensea provides the correct chain value in the api  # noqa: E501
-                    "SELECT identifier FROM evm_tokens WHERE address=?",
-                    (address,),
-                ).fetchone()
+                asset_identifier = (
+                    cursor.execute(  # we consider as correct to query only by address knowing that the same address could exist in different chains until opensea provides the correct chain value in the api  # noqa: E501
+                        "SELECT identifier FROM evm_tokens WHERE address=?",
+                        (address,),
+                    )
+                    .fetchone()
+                )
                 if asset_identifier is not None:
                     return Asset(asset_identifier[0])
 
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/globaldb/asset_updates/manager.py#L80'>rotkehlchen/globaldb/asset_updates/manager.py~L80</a>
```diff
         (
             result := write_cursor.execute(
                 "SELECT value FROM other_db.settings WHERE name='version';"
-            ).fetchone()
+            )
+            .fetchone()
         )
         is not None  # noqa: E501
         and int(result[0]) >= FIRST_GLOBAL_DB_VERSION_WITH_SOLANA_TOKENS
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/globaldb/asset_updates/manager.py#L342'>rotkehlchen/globaldb/asset_updates/manager.py~L342</a>
```diff
                     and cursor.execute(
                         "SELECT COUNT(*) FROM assets WHERE identifier=?",
                         (remote_asset_data.identifier,),
-                    ).fetchone()[0]
+                    )
+                    .fetchone()[0]
                     == 0
                 ):
                     executeall(cursor, full_insert)  # we apply the full insert query
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/globaldb/cache.py#L111'>rotkehlchen/globaldb/cache.py~L111</a>
```diff
         cursor.execute(
             "SELECT COUNT(*) FROM general_cache WHERE key=? AND value=?",
             (compute_cache_key(key_parts), value),
-        ).fetchone()[0]
+        )
+        .fetchone()[0]
         == 1
     )
 
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/globaldb/handler.py#L376'>rotkehlchen/globaldb/handler.py~L376</a>
```diff
                     data.update(
                         {
                             "protocol": entry[11],
-                            "token_kind": TokenKind.deserialize_solana_from_db(
-                                entry[13]
-                            ).serialize(),
+                            "token_kind": TokenKind.deserialize_solana_from_db(entry[13])
+                            .serialize(),
                             "address": entry[2],
                             "decimals": entry[3],
                         }
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/globaldb/handler.py#L422'>rotkehlchen/globaldb/handler.py~L422</a>
```diff
                 if assets_info[entry[0]]["underlying_tokens"] is None:
                     assets_info[entry[0]]["underlying_tokens"] = []
                 assets_info[entry[0]]["underlying_tokens"].append(
-                    UnderlyingToken.deserialize_from_db(
-                        (entry[1], entry[2], entry[3])
-                    ).serialize(),  # noqa: E501
+                    UnderlyingToken.deserialize_from_db((entry[1], entry[2], entry[3]))
+                    .serialize(),  # noqa: E501
                 )
 
             # get `entries_found`. In the case of handling the ignored assets we need to manually
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/globaldb/handler.py#L1634'>rotkehlchen/globaldb/handler.py~L1634</a>
```diff
             for query_entry, (_, _, queried_timestamp) in zip(querylist, query_data, strict=True):
                 # to the params add the arguments to prioritize manual prices
                 result = (
-                    cursor.execute(
-                        querystr, query_entry + (priority_type, queried_timestamp)
-                    ).fetchone()
+                    cursor.execute(querystr, query_entry + (priority_type, queried_timestamp))
+                    .fetchone()
                 )  # below last index of the result tuple is ignored in deserialize  # noqa: E501
                 if result is None:
                     prices_results.append(None)
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/globaldb/handler.py#L2464'>rotkehlchen/globaldb/handler.py~L2464</a>
```diff
                 and cursor.execute(
                     "SELECT COUNT(*) FROM location_unsupported_assets WHERE location=? AND exchange_symbol=?",  # noqa: E501
                     (exchange.serialize_for_db(), symbol),
-                ).fetchone()[0]
+                )
+                .fetchone()[0]
                 > 0
             ):
                 raise UnsupportedAsset(symbol)
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/globaldb/handler.py#L2603'>rotkehlchen/globaldb/handler.py~L2603</a>
```diff
             and cursor.execute(
                 "SELECT COUNT(*) FROM location_asset_mappings WHERE location IS NULL AND local_id=? AND exchange_symbol=?",  # noqa: E501
                 entry.serialize_for_db()[:2],  # the asset and the exchange symbol.
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             > 0
         ):
             raise rsqlite.IntegrityError("Entry already exists in the DB")
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/globaldb/handler.py#L2711'>rotkehlchen/globaldb/handler.py~L2711</a>
```diff
                     protocol := cursor.execute(
                         "SELECT protocol FROM evm_tokens WHERE identifier=?;",
                         (asset_identifier,),
-                    ).fetchone()
+                    )
+                    .fetchone()
                 )
                 is not None
                 else None
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/globaldb/upgrades/v12_v13.py#L92'>rotkehlchen/globaldb/upgrades/v12_v13.py~L92</a>
```diff
                 user_tokens := write_cursor.execute(
                     "SELECT identifier FROM assets WHERE type = ? AND identifier NOT LIKE ?",
                     ("Y", "solana%"),
-                ).fetchall()
+                )
+                .fetchall()
             )
             == 0
         ):
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/globaldb/upgrades/v9_v10.py#L37'>rotkehlchen/globaldb/upgrades/v9_v10.py~L37</a>
```diff
                         write_cursor.execute(
                             "SELECT MIN(asset) FROM multiasset_mappings WHERE collection_id = ?",
                             (id_,),
-                        ).fetchone()[0]
+                        )
+                        .fetchone()[0]
                     )
                 )
                 is None
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/inquirer.py#L864'>rotkehlchen/inquirer.py~L864</a>
```diff
                 to_asset=to_asset,
                 ignore_cache=ignore_cache,
                 skip_onchain=skip_onchain,
-            ).items()
+            )
+            .items()
         }
 
     @staticmethod
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/rotkehlchen.py#L621'>rotkehlchen/rotkehlchen.py~L621</a>
```diff
                 ),
                 extra_check_callback=lambda: cursor.execute(
                     "SELECT COUNT(*) FROM user_added_solana_tokens"
-                ).fetchone()[0]
+                )
+                .fetchone()[0]
                 > 0,  # noqa: E501
             )
 
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/api/blockchain/test_base.py#L1736'>rotkehlchen/tests/api/blockchain/test_base.py~L1736</a>
```diff
             cursor.execute(
                 "SELECT COUNT(*) FROM used_query_ranges WHERE name=?",
                 (f"{BRIDGE_QUERIED_ADDRESS_PREFIX}{gnosis_accounts[0]}",),
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == 0
         )
 
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/api/test_accounting_rules.py#L802'>rotkehlchen/tests/api/test_accounting_rules.py~L802</a>
```diff
         assert (
             cursor.execute(
                 "SELECT * FROM accounting_rules WHERE identifier IN (1, 82);",
-            ).fetchall()
+            )
+            .fetchall()
             == [
                 (1, "deposit", "deposit asset", "aave-v1", 0, 0, 1, "A", 0),
                 (82, "deposit", "fee", NO_ACCOUNTING_COUNTERPARTY, 1, 0, 1, None, 0),
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/api/test_assets_updates.py#L112'>rotkehlchen/tests/api/test_assets_updates.py~L112</a>
```diff
         assert (
             cursor.execute(
                 "SELECT name FROM asset_collections WHERE id=33",
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == "STASIS EURS"
         )
         assert (
             cursor.execute(
                 "SELECT COUNT(*) FROM multiasset_mappings WHERE collection_id=314 AND "
                 "asset='eip155:10/erc20:0xC52D7F23a2e460248Db6eE192Cb23dD12bDDCbf6'",
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == 1
         )
 
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/api/test_assets_updates.py#L278'>rotkehlchen/tests/api/test_assets_updates.py~L278</a>
```diff
                 cursor.execute(
                     "SELECT COUNT(*) FROM multiasset_mappings WHERE collection_id=314 AND "
                     "asset='eip155:10/erc20:0xC52D7F23a2e460248Db6eE192Cb23dD12bDDCbf6'",
-                ).fetchone()[0]
+                )
+                .fetchone()[0]
                 == 0
             )
             assert (
                 cursor.execute(
                     "SELECT COUNT(*) FROM asset_collections WHERE id=39",
-                ).fetchone()[0]
+                )
+                .fetchone()[0]
                 == 0
             )
 
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/api/test_assets_updates.py#L548'>rotkehlchen/tests/api/test_assets_updates.py~L548</a>
```diff
             cursor.execute(
                 "SELECT COUNT(*) from evm_tokens WHERE address=?",
                 ("0x6B175474E89094C44Da98b954EedeAC495271d0F",),
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == 1
         )  # noqa: E501
         assert (
             cursor.execute(
                 "SELECT COUNT(*) from assets WHERE identifier=?",
                 ("eip155:1/erc20:0x6B175474E89094C44Da98b954EedeAC495271d0F",),
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == 1
         )  # noqa: E501
 
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/api/test_assets_updates.py#L572'>rotkehlchen/tests/api/test_assets_updates.py~L572</a>
```diff
         assert (
             cursor.execute(
                 "SELECT COUNT(*) from common_asset_details WHERE identifier=?", ("DASH",)
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == 1
         )  # noqa: E501
         assert (
-            cursor.execute("SELECT COUNT(*) from assets WHERE identifier=?", ("DASH",)).fetchone()[
-                0
-            ]
+            cursor.execute("SELECT COUNT(*) from assets WHERE identifier=?", ("DASH",))
+            .fetchone()[0]
             == 1
         )  # noqa: E501
 
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/api/test_assets_updates.py#L596'>rotkehlchen/tests/api/test_assets_updates.py~L596</a>
```diff
             cursor.execute(
                 "SELECT COUNT(*) from common_asset_details WHERE identifier=?",
                 ("121-ada-FADS-as",),
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == 1
         )  # noqa: E501
         assert (
-            cursor.execute(
-                "SELECT COUNT(*) from assets WHERE identifier=?", ("121-ada-FADS-as",)
-            ).fetchone()[0]
+            cursor.execute("SELECT COUNT(*) from assets WHERE identifier=?", ("121-ada-FADS-as",))
+            .fetchone()[0]
             == 1
         )  # noqa: E501
 
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/api/test_assets_updates.py#L622'>rotkehlchen/tests/api/test_assets_updates.py~L622</a>
```diff
             cursor.execute(
                 "SELECT COUNT(*) from evm_tokens WHERE address=?",
                 ("0x1B175474E89094C44Da98b954EedeAC495271d0F",),
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == 1
         )  # noqa: E501
         assert (
             cursor.execute(
                 "SELECT COUNT(*) from assets WHERE identifier=?",
                 ("eip155:1/erc20:0x1B175474E89094C44Da98b954EedeAC495271d0F",),
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == 1
         )  # noqa: E501
 
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/api/test_data_purging.py#L369'>rotkehlchen/tests/api/test_data_purging.py~L369</a>
```diff
                 cursor.execute(
                     "SELECT COUNT(*) FROM key_value_cache WHERE name LIKE ?;",
                     (f"{cache_key.value[0].format(address='')}%",),
-                ).fetchone()[0]
+                )
+                .fetchone()[0]
                 == 0
             )
 
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/api/test_data_purging.py#L604'>rotkehlchen/tests/api/test_data_purging.py~L604</a>
```diff
                 cursor.execute(
                     f"SELECT COUNT(*) FROM key_value_cache WHERE {' OR '.join(['name LIKE ?'] * len(cache_keys))}",  # noqa: E501
                     cache_keys,
-                ).fetchone()[0]
+                )
+                .fetchone()[0]
                 == 0
             )
             assert (
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/api/test_evm_transactions.py#L331'>rotkehlchen/tests/api/test_evm_transactions.py~L331</a>
```diff
             cursor.execute(
                 "SELECT COUNT(*) FROM solana_transactions WHERE signature=?",
                 (deserialize_tx_signature(solana_signature).to_bytes(),),
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == 1
         )  # noqa: E501
         assert (
             cursor.execute(
                 "SELECT COUNT(*) FROM history_events WHERE group_identifier=?", (solana_signature,)
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == 2
         )  # noqa: E501
 
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/api/test_evmlike.py#L683'>rotkehlchen/tests/api/test_evmlike.py~L683</a>
```diff
             cursor.execute(
                 "SELECT COUNT(*) FROM zksynclite_transactions WHERE tx_hash=?",
                 (deserialize_evm_tx_hash(entry["tx_ref"]),),
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == 1
         )
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/api/test_history_base_entry.py#L314'>rotkehlchen/tests/api/test_history_base_entry.py~L314</a>
```diff
             cursor.execute(
                 "SELECT COUNT(*) FROM evm_transactions WHERE tx_hash=?",
                 (entry.tx_ref,),
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == 1
         )
     entry.tx_ref = original_tx_hash
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/api/test_history_base_entry.py#L1097'>rotkehlchen/tests/api/test_history_base_entry.py~L1097</a>
```diff
             cursor.execute(
                 "SELECT group_identifier FROM history_events WHERE identifier=?",
                 (entries[0]["identifier"],),
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == "new_eventid1"
         )
 
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/api/test_migrate_solana_token.py#L130'>rotkehlchen/tests/api/test_migrate_solana_token.py~L130</a>
```diff
             cursor.execute(
                 "SELECT COUNT(*) FROM common_asset_details WHERE identifier = ?",
                 (TEST_OLD_ASSET_IDENTIFIER,),
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == 0
         )  # noqa: E501
         assert (
             cursor.execute(
                 "SELECT COUNT(*) FROM assets WHERE identifier = ?", (TEST_OLD_ASSET_IDENTIFIER,)
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == 0
         )  # noqa: E501
         # check that the new asset is present.
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/api/test_migrate_solana_token.py#L293'>rotkehlchen/tests/api/test_migrate_solana_token.py~L293</a>
```diff
         assert (
             cursor.execute(
                 "SELECT COUNT(*) FROM assets WHERE identifier = ?", (new_token_identifier,)
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == 0
         )  # noqa: E501
         assert (
             cursor.execute(
                 "SELECT COUNT(*) FROM common_asset_details WHERE identifier = ?",
                 (new_token_identifier,),
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == 0
         )  # noqa: E501
         # Original token should still exist in global database
         assert (
             cursor.execute(
                 "SELECT COUNT(*) FROM assets WHERE identifier = ?", (TEST_OLD_ASSET_IDENTIFIER,)
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == 1
         )  # noqa: E501
         assert (
             cursor.execute(
                 "SELECT COUNT(*) FROM common_asset_details WHERE identifier = ?",
                 (TEST_OLD_ASSET_IDENTIFIER,),
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == 1
         )  # noqa: E501
         # Migration table should still contain the old token
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/api/test_migrate_solana_token.py#L322'>rotkehlchen/tests/api/test_migrate_solana_token.py~L322</a>
```diff
             cursor.execute(
                 "SELECT COUNT(*) FROM user_added_solana_tokens WHERE identifier = ?",
                 (TEST_OLD_ASSET_IDENTIFIER,),
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == 1
         )  # noqa: E501
 
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/api/test_migrate_solana_token.py#L334'>rotkehlchen/tests/api/test_migrate_solana_token.py~L334</a>
```diff
                 cursor.execute(
                     f"SELECT COUNT(*) FROM {table} WHERE {column} = ?",
                     (TEST_OLD_ASSET_IDENTIFIER,),
-                ).fetchone()[0]
+                )
+                .fetchone()[0]
                 == 1
             )  # noqa: E501
             # New asset should not exist
             assert (
                 cursor.execute(
                     f"SELECT COUNT(*) FROM {table} WHERE {column} = ?", (new_token_identifier,)
-                ).fetchone()[0]
+                )
+                .fetchone()[0]
                 == 0
             )  # noqa: E501
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/api/test_nfts.py#L544'>rotkehlchen/tests/api/test_nfts.py~L544</a>
```diff
         with db.conn.read_ctx() as cursor:
             assert cursor.execute("SELECT COUNT(*) FROM nfts").fetchone()[0] == 1
             assert (
-                cursor.execute(
-                    "SELECT COUNT(*) FROM nfts WHERE owner_address=?", (TEST_ACC5,)
-                ).fetchone()[0]
+                cursor.execute("SELECT COUNT(*) FROM nfts WHERE owner_address=?", (TEST_ACC5,))
+                .fetchone()[0]
                 == 0
             )  # noqa: E501
 
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/api/test_snapshots.py#L741'>rotkehlchen/tests/api/test_snapshots.py~L741</a>
```diff
     cursor = rotkehlchen_api_server.rest_api.rotkehlchen.data.db.conn.cursor()
     assert (
         len(
-            cursor.execute(
-                "SELECT timestamp FROM timed_balances WHERE timestamp=?", (ts,)
-            ).fetchall()
+            cursor.execute("SELECT timestamp FROM timed_balances WHERE timestamp=?", (ts,))
+            .fetchall()
         )
         == 0
     )  # noqa: E501
     assert (
         len(
-            cursor.execute(
-                "SELECT timestamp FROM timed_location_data WHERE timestamp=?", (ts,)
-            ).fetchall()
+            cursor.execute("SELECT timestamp FROM timed_location_data WHERE timestamp=?", (ts,))
+            .fetchall()
         )
         == 0
     )  # noqa: E501
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/api/test_user_assets.py#L711'>rotkehlchen/tests/api/test_user_assets.py~L711</a>
```diff
             global_cursor.execute(
                 "SELECT COUNT(*) FROM user_owned_assets WHERE asset_id=?",
                 (user_asset1_id,),
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == 1
         )
         # check the user asset is in user db
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/api/test_user_assets.py#L719'>rotkehlchen/tests/api/test_user_assets.py~L719</a>
```diff
             cursor.execute(
                 "SELECT COUNT(*) FROM assets WHERE identifier=?",
                 (user_asset1_id,),
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == 1
         )
         # Check that the manual balance is returned
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/api/test_user_assets.py#L738'>rotkehlchen/tests/api/test_user_assets.py~L738</a>
```diff
             cursor.execute(
                 "SELECT COUNT(*) from manually_tracked_balances WHERE asset=?;",
                 (user_asset1_id,),
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == 1
         )
 
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/api/test_user_assets.py#L777'>rotkehlchen/tests/api/test_user_assets.py~L777</a>
```diff
             cursor.execute(
                 "SELECT COUNT(*) FROM assets WHERE identifier=?",
                 (user_asset1_id,),
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == 0
         )
         assert (
             cursor.execute(
                 "SELECT COUNT(*) FROM assets WHERE identifier=?",
                 ("ICP",),
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == 1
         )
         assert (
             cursor.execute(
                 "SELECT COUNT(*) from manually_tracked_balances WHERE asset=?;",
                 (user_asset1_id,),
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == 0
         )
         assert (
             cursor.execute(
                 "SELECT COUNT(*) from manually_tracked_balances WHERE asset=?;",
                 ("ICP",),
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == 1
         )
 
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/api/test_user_assets.py#L807'>rotkehlchen/tests/api/test_user_assets.py~L807</a>
```diff
         global_cursor.execute(
             "SELECT COUNT(*) FROM user_owned_assets WHERE asset_id=?",
             (user_asset1_id,),
-        ).fetchone()[0]
+        )
+        .fetchone()[0]
         == 0
     )
     # check the previous asset is not in globaldb
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/api/test_user_assets.py#L815'>rotkehlchen/tests/api/test_user_assets.py~L815</a>
```diff
         global_cursor.execute(
             "SELECT COUNT(*) FROM assets WHERE identifier=?",
             (user_asset1_id,),
-        ).fetchone()[0]
+        )
+        .fetchone()[0]
         == 0
     )
 
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/api/test_user_assets.py#L843'>rotkehlchen/tests/api/test_user_assets.py~L843</a>
```diff
         cursor.execute(
             "SELECT COUNT(*) FROM assets WHERE identifier=?",
             (unknown_id,),
-        ).fetchone()[0]
+        )
+        .fetchone()[0]
         == 1
     )
     # Check that the manual balance is there -- can't query normally due to unknown asset
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/api/test_user_assets.py#L851'>rotkehlchen/tests/api/test_user_assets.py~L851</a>
```diff
         cursor.execute(
             "SELECT COUNT(*) FROM manually_tracked_balances WHERE asset=?",
             (unknown_id,),
-        ).fetchone()[0]
+        )
+        .fetchone()[0]
         == 1
     )
 
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/api/test_user_assets.py#L893'>rotkehlchen/tests/api/test_user_assets.py~L893</a>
```diff
         global_cursor.execute(
             "SELECT COUNT(*) FROM user_owned_assets WHERE asset_id=?",
             (unknown_id,),
-        ).fetchone()[0]
+        )
+        .fetchone()[0]
         == 0
     )
     # check the previous asset is not in globaldb
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/api/test_user_assets.py#L901'>rotkehlchen/tests/api/test_user_assets.py~L901</a>
```diff
         global_cursor.execute(
             "SELECT COUNT(*) FROM assets WHERE identifier=?",
             (unknown_id,),
-        ).fetchone()[0]
+        )
+        .fetchone()[0]
         == 0
     )
     # check the previous asset is not in userdb anymore
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/api/test_user_assets.py#L909'>rotkehlchen/tests/api/test_user_assets.py~L909</a>
```diff
         cursor.execute(
             "SELECT COUNT(*) FROM assets WHERE identifier=?",
             (unknown_id,),
-        ).fetchone()[0]
+        )
+        .fetchone()[0]
         == 0
     )
 
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/api/test_user_assets.py#L983'>rotkehlchen/tests/api/test_user_assets.py~L983</a>
```diff
             global_cursor.execute(
                 "SELECT COUNT(*) FROM user_owned_assets WHERE asset_id=?",
                 (glm_id,),
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == 1
         )
         assert (
             global_cursor.execute(
                 "SELECT COUNT(*) FROM common_asset_details WHERE swapped_for=?",
                 (glm_id,),
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == 1
         )
         assert (
             cursor.execute(
                 "SELECT COUNT(*) FROM assets WHERE identifier=?",
                 (glm_id,),
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == 1
         )
 
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/api/test_users.py#L684'>rotkehlchen/tests/api/test_users.py~L684</a>
```diff
     assert (
         requests.get(
             api_url_for(rotkehlchen_api_server, "specific_async_tasks_resource", task_id=task_id),
-        ).json()["result"]["status"]
+        )
+        .json()["result"]["status"]
         == "not-found"
     )
     assert Inquirer._uniswapv2 is None
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/data_migrations/test_migration_20.py#L466'>rotkehlchen/tests/data_migrations/test_migration_20.py~L466</a>
```diff
             cursor.execute(  # the trigger events should be removed
                 "SELECT COUNT(*) FROM history_events WHERE event_identifier = ?",
                 (hash_id(str(Location.KRAKEN)),),
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == 0
         )
 
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/data_migrations/test_migration_21.py#L41'>rotkehlchen/tests/data_migrations/test_migration_21.py~L41</a>
```diff
         rows = list(
             cursor.execute(
                 "SELECT address, blockchain, name FROM address_book ORDER BY blockchain"
-            ).fetchall()
+            )
+            .fetchall()
         )  # noqa: E501
         assert rows == [
             (eth_addr, "ETH", "yabir.eth"),
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/data_migrations/test_migrations.py#L474'>rotkehlchen/tests/data_migrations/test_migrations.py~L474</a>
```diff
         assert (
             write_cursor.execute(
                 "SELECT COUNT(*) FROM default_rpc_nodes WHERE name='polygon etherscan'"
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == 1
         )  # noqa: E501
 
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/data_migrations/test_migrations.py#L509'>rotkehlchen/tests/data_migrations/test_migrations.py~L509</a>
```diff
         GlobalDBHandler().conn.read_ctx() as cursor
     ):  # check the global db for the polygon etherscan node  # noqa: E501
         assert (
-            cursor.execute(
-                "SELECT COUNT(*) FROM default_rpc_nodes WHERE name='polygon etherscan'"
-            ).fetchone()[0]
+            cursor.execute("SELECT COUNT(*) FROM default_rpc_nodes WHERE name='polygon etherscan'")
+            .fetchone()[0]
             == 0
         )  # noqa: E501
         assert (
             cursor.execute(
                 "SELECT COUNT(*) FROM default_rpc_nodes WHERE name='polygon pos etherscan'"
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == 1
         )  # noqa: E501
     with (
         rotki.data.db.conn.read_ctx() as cursor
     ):  # check the user db for the polygon etherscan node  # noqa: E501
         assert (
-            cursor.execute(
-                "SELECT COUNT(*) FROM rpc_nodes WHERE name='polygon etherscan'"
-            ).fetchone()[0]
+            cursor.execute("SELECT COUNT(*) FROM rpc_nodes WHERE name='polygon etherscan'")
+            .fetchone()[0]
             == 0
         )  # noqa: E501
 
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/data_migrations/test_migrations.py#L680'>rotkehlchen/tests/data_migrations/test_migrations.py~L680</a>
```diff
             cursor.execute(
                 "SELECT value FROM unique_cache WHERE key=?",
                 ("YEARN_VAULTS",),
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == "179"
         )
 
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/data_migrations/test_migrations.py#L738'>rotkehlchen/tests/data_migrations/test_migrations.py~L738</a>
```diff
         assert json.loads(
             cursor.execute(  # make sure manual oracle is there before the migration
                 "SELECT value from settings where name='current_price_oracles'",
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
         ) == ["coingecko", "cryptocompare", "manualcurrent", "defillama"]
 
     with rotki.data.db.conn.write_ctx() as write_cursor:
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/data_migrations/test_migrations.py#L839'>rotkehlchen/tests/data_migrations/test_migrations.py~L839</a>
```diff
             cursor.execute(  # now make sure they are no longer ignored
                 "SELECT COUNT(*) FROM multisettings WHERE name='ignored_asset' AND value IN (?, ?, ?, ?)",  # noqa: E501
                 [token.identifier for token in tokens],
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == 0
         )
 
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/data_migrations/test_migrations.py#L847'>rotkehlchen/tests/data_migrations/test_migrations.py~L847</a>
```diff
             cursor.execute(
                 "SELECT COUNT(*) FROM evm_tx_mappings WHERE tx_id=?",
                 (spam_transaction_id,),
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == 0
         )
         assert json.loads(
             cursor.execute(  # make sure manual oracle is gone after the migration
                 "SELECT value from settings where name='current_price_oracles'",
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
         ) == ["coingecko", "cryptocompare", "defillama"]
 
     for token in tokens:
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/data_migrations/test_migrations.py#L868'>rotkehlchen/tests/data_migrations/test_migrations.py~L868</a>
```diff
             cursor.execute(
                 "SELECT value FROM unique_cache WHERE key=?",
                 ("YEARN_VAULTS",),
-            ).fetchall()
+            )
+            .fetchall()
             == []
         )
 
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/db/test_db_upgrades.py#L184'>rotkehlchen/tests/db/test_db_upgrades.py~L184</a>
```diff
     assert (
         cursor.execute(
             "SELECT COUNT(*) from used_query_ranges WHERE name LIKE 'uniswap%';",
-        ).fetchone()[0]
+        )
+        .fetchone()[0]
         == 2
     )
     assert (
         cursor.execute(
             "SELECT COUNT(*) from used_query_ranges WHERE name LIKE 'balancer%';",
-        ).fetchone()[0]
+        )
+        .fetchone()[0]
         == 2
     )
     assert cursor.execute("SELECT COUNT(*) from used_query_ranges;").fetchone()[0] == 6
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/db/test_db_upgrades.py#L471'>rotkehlchen/tests/db/test_db_upgrades.py~L471</a>
```diff
     assert (
         cursor.execute(
             "SELECT address, xpub, derivation_path, account_index, derived_index FROM xpub_mappings;",
-        ).fetchall()
+        )
+        .fetchall()
         == expected_xpub_mappings
     )
     # Check transactions are migrated and internal ones removed
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/db/test_db_upgrades.py#L1233'>rotkehlchen/tests/db/test_db_upgrades.py~L1233</a>
```diff
             cursor.execute(
                 "SELECT COUNT(*) FROM sqlite_master WHERE type='view' AND name=?",
                 ("combined_trades_view",),  # noqa: E501
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == 1
         )
 
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/db/test_db_upgrades.py#L1395'>rotkehlchen/tests/db/test_db_upgrades.py~L1395</a>
```diff
             cursor.execute(
                 "SELECT COUNT(*) FROM sqlite_master WHERE type='view' AND name=?",
                 ("combined_trades_view",),  # noqa: E501
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == 0
         )
         cursor.execute(
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/db/test_db_upgrades.py#L2066'>rotkehlchen/tests/db/test_db_upgrades.py~L2066</a>
```diff
         cursor.execute(
             "SELECT COUNT(*) FROM sqlite_master WHERE type='table' AND name=?",
             ("eth2_deposits",),
-        ).fetchone()[0]
+        )
+        .fetchone()[0]
         == 1
     )
     # Check used query ranges
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/db/test_db_upgrades.py#L2150'>rotkehlchen/tests/db/test_db_upgrades.py~L2150</a>
```diff
     assert (
         cursor.execute(
             "SELECT value FROM settings WHERE name='non_syncing_exchanges'",
-        ).fetchone()[0]
+        )
+        .fetchone()[0]
         == '[{"name": "Kucoin 1", "location": "kucoin"}]'
     )
     assert (
         cursor.execute(
             "SELECT value FROM settings WHERE name='ssf_0graph_multiplier'",
-        ).fetchone()[0]
+        )
+        .fetchone()[0]
         == "42"
     )
     assert (
         cursor.execute(
             "SELECT value FROM settings WHERE name='ssf_graph_multiplier'",
-        ).fetchone()
+        )
+        .fetchone()
         is None
     )
 
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/db/test_db_upgrades.py#L2398'>rotkehlchen/tests/db/test_db_upgrades.py~L2398</a>
```diff
         cursor.execute(
             "SELECT COUNT(*) FROM sqlite_master WHERE type='table' AND name=?",
             ("eth2_deposits",),
-        ).fetchone()[0]
+        )
+        .fetchone()[0]
         == 0
     )
     assert cursor.execute("SELECT * FROM used_query_ranges").fetchall() == [
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/db/test_db_upgrades.py#L2426'>rotkehlchen/tests/db/test_db_upgrades.py~L2426</a>
```diff
     assert (
         cursor.execute(  # Check that FTX was deleted from non syncing exchanges
             "SELECT value FROM settings WHERE name='non_syncing_exchanges'",
-        ).fetchone()[0]
+        )
+        .fetchone()[0]
         == '[{"name": "Kucoin 1", "location": "kucoin"}]'
     )
     assert (
         cursor.execute(  # Check the 0graph multiplier setting got updated properly
             "SELECT value FROM settings WHERE name='ssf_0graph_multiplier'",
-        ).fetchone()
+        )
+        .fetchone()
         is None
     )
     assert (
         cursor.execute(
             "SELECT value FROM settings WHERE name='ssf_graph_multiplier'",
-        ).fetchone()[0]
+        )
+        .fetchone()[0]
         == "42"
     )
     db.logout()
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/db/test_db_upgrades.py#L2460'>rotkehlchen/tests/db/test_db_upgrades.py~L2460</a>
```diff
     assert cursor.execute("SELECT COUNT(*) FROM aave_events").fetchone()[0] == 2
     aave_range_key = "aave_events_0x026045B110b49183E78520460eBEcdC6B4538C85"
     assert (
-        cursor.execute(
-            "SELECT COUNT(*) FROM used_query_ranges WHERE name=?", (aave_range_key,)
-        ).fetchone()[0]
+        cursor.execute("SELECT COUNT(*) FROM used_query_ranges WHERE name=?", (aave_range_key,))
+        .fetchone()[0]
         == 1
     )  # noqa: E501
     assert (
         cursor.execute(
             "SELECT COUNT(*) FROM sqlite_master WHERE type='table' AND name='amm_events';"
-        ).fetchone()[0]
+        )
+        .fetchone()[0]
         == 1
     )  # noqa: E501
     nodes_before = cursor.execute("SELECT * FROM rpc_nodes").fetchall()
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/db/test_db_upgrades.py#L2691'>rotkehlchen/tests/db/test_db_upgrades.py~L2691</a>
```diff
         cursor.execute(  # Check that Polygon POS location was added
             "SELECT location FROM location WHERE seq=?",
             (Location.POLYGON_POS.value,),
-        ).fetchone()[0]
+        )
+        .fetchone()[0]
         == Location.POLYGON_POS.serialize_for_db()
     )
     nodes_after = cursor.execute("SELECT * FROM rpc_nodes").fetchall()
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/db/test_db_upgrades.py#L2708'>rotkehlchen/tests/db/test_db_upgrades.py~L2708</a>
```diff
         == expected_internal_txs
     )  # noqa: E501
     assert (
-        cursor.execute(
-            "SELECT COUNT(*) FROM used_query_ranges WHERE name=?", (aave_range_key,)
-        ).fetchone()[0]
+        cursor.execute("SELECT COUNT(*) FROM used_query_ranges WHERE name=?", (aave_range_key,))
+        .fetchone()[0]
         == 0
     )  # noqa: E501
     assert (
         cursor.execute(
             "SELECT COUNT(*) FROM sqlite_master WHERE type='table' AND name='aave_events';"
-        ).fetchone()[0]
+        )
+        .fetchone()[0]
         == 0
     )  # noqa: E501
     assert (
         cursor.execute(
             "SELECT COUNT(*) FROM sqlite_master WHERE type='table' AND name='amm_events';"
-        ).fetchone()[0]
+        )
+        .fetchone()[0]
         == 0
     )  # noqa: E501
     assert cursor.execute(
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/db/test_db_upgrades.py#L2771'>rotkehlchen/tests/db/test_db_upgrades.py~L2771</a>
```diff
     assert (
         cursor.execute(
             "SELECT COUNT(*) FROM sqlite_master WHERE type='table' AND name='optimism_transactions';"
-        ).fetchone()[0]
+        )
+        .fetchone()[0]
         == 0
     )  # noqa: E501
     withdrawal_events_before = {
         x[0]
         for x in cursor.execute(
             "SELECT identifier FROM history_events WHERE event_identifier LIKE 'eth2_withdrawal_%'",
-        ).fetchall()
+        )
+        .fetchall()
     }
     block_events_before = {
         x[0]
         for x in cursor.execute(
             "SELECT identifier FROM history_events WHERE event_identifier LIKE 'evm_1_block_%'",
-        ).fetchall()
+        )
+        .fetchall()
     }
     assert cursor.execute(
         "SELECT event_identifier FROM history_events WHERE event_identifier LIKE 'rotki_events_%'"
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/db/test_db_upgrades.py#L2877'>rotkehlchen/tests/db/test_db_upgrades.py~L2877</a>
```diff
     assert (
         cursor.execute(
             "SELECT COUNT(*) FROM sqlite_master WHERE type='table' AND name='optimism_transactions';"
-        ).fetchone()[0]
+        )
+        .fetchone()[0]
         == 1
     )  # noqa: E501
     assert (
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/db/test_db_upgrades.py#L2888'>rotkehlchen/tests/db/test_db_upgrades.py~L2888</a>
```diff
         x[0]
         for x in cursor.execute(
             "SELECT identifier FROM history_events WHERE event_identifier LIKE 'EW_%'",
-        ).fetchall()
+        )
+        .fetchall()
     }
     assert withdrawal_events_after == withdrawal_events_before
     block_events_after = {
         x[0]
         for x in cursor.execute(
             "SELECT identifier FROM history_events WHERE event_identifier LIKE 'BP1_%'",
-        ).fetchall()
+        )
+        .fetchall()
     }
     assert block_events_after == block_events_before
     assert (
         cursor.execute(
             "SELECT event_identifier FROM history_events WHERE event_identifier LIKE 'rotki_events_%'"
-        ).fetchall()
+        )
+        .fetchall()
         == []
     )  # noqa: E501
     assert set(
         cursor.execute(
             "SELECT event_identifier FROM history_events WHERE event_identifier LIKE 'RE%'"
-        ).fetchall()
+        )
+        .fetchall()
     ) == {  # noqa: E501
         ("RE_0x2123",),
         ("REBTX_0x4123",),
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/db/test_db_upgrades.py#L2989'>rotkehlchen/tests/db/test_db_upgrades.py~L2989</a>
```diff
         cursor.execute(  # Check that Arbitrum One location was added
             "SELECT location FROM location WHERE location=?",
             (Location.ARBITRUM_ONE.serialize_for_db(),),
-        ).fetchone()[0]
+        )
+        .fetchone()[0]
         == Location.ARBITRUM_ONE.serialize_for_db()
     )
 
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/db/test_db_upgrades.py#L3090'>rotkehlchen/tests/db/test_db_upgrades.py~L3090</a>
```diff
     ]
     # Check that ledger actions settings are there
     assert (
-        cursor.execute(
-            "SELECT COUNT(*) FROM settings WHERE name=?", ("taxable_ledger_actions",)
-        ).fetchone()[0]
+        cursor.execute("SELECT COUNT(*) FROM settings WHERE name=?", ("taxable_ledger_actions",))
+        .fetchone()[0]
         == 1
     )  # noqa: E501
     # Check that ignored ledger actions are there
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/db/test_db_upgrades.py#L3282'>rotkehlchen/tests/db/test_db_upgrades.py~L3282</a>
```diff
     ]
     # Check that ledger actions settings are removed
     assert (
-        cursor.execute(
-            "SELECT COUNT(*) FROM settings WHERE name=?", ("taxable_ledger_actions",)
-        ).fetchone()[0]
+        cursor.execute("SELECT COUNT(*) FROM settings WHERE name=?", ("taxable_ledger_actions",))
+        .fetchone()[0]
         == 0
     )  # noqa: E501
     # Check that ignored ledger actions are removed
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/db/test_db_upgrades.py#L3314'>rotkehlchen/tests/db/test_db_upgrades.py~L3314</a>
```diff
         cursor.execute(
             "SELECT COUNT(*) FROM assets WHERE identifier=?",
             ("VELO",),
-        ).fetchone()[0]
+        )
+        .fetchone()[0]
         == 0
     )
     assert (
         cursor.execute(
             "SELECT asset FROM history_events WHERE identifier IN (?, ?)",
             ("10000", "10001"),
-        ).fetchall()
+        )
+        .fetchall()
         == [(bnb_velo_asset_id,)] * 2
     )
     assert cursor.execute(
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/db/test_db_upgrades.py#L3439'>rotkehlchen/tests/db/test_db_upgrades.py~L3439</a>
```diff
             cursor.execute(
                 "SELECT COUNT(*) FROM user_credentials WHERE location=?",
                 ("D",),
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == 1
         )
         assert (
             cursor.execute(
                 "SELECT COUNT(*) FROM used_query_ranges WHERE name LIKE ? ESCAPE ?;",
                 (f"{Location.BITTREX!s}\\_%", "\\"),
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == 3
         )
 
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/db/test_db_upgrades.py#L3481'>rotkehlchen/tests/db/test_db_upgrades.py~L3481</a>
```diff
         # verify that the history_events exists before upgrade
         assert cursor.execute("SELECT COUNT(*) FROM history_events").fetchone()[0] == 10
         assert (
-            cursor.execute("SELECT COUNT(*) FROM history_events WHERE location = 'B'").fetchone()[
-                0
-            ]
+            cursor.execute("SELECT COUNT(*) FROM history_events WHERE location = 'B'")
+            .fetchone()[0]
             == 2
         )  # kraken events: one valid and one that needs to be removed # noqa: E501
         assert cursor.execute("SELECT COUNT(*) FROM evm_events_info").fetchone()[0] == 8
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/db/test_db_upgrades.py#L3491'>rotkehlchen/tests/db/test_db_upgrades.py~L3491</a>
```diff
             cursor.execute(
                 "SELECT COUNT(*) FROM history_events_mappings WHERE name=? AND value=?",
                 (HISTORY_MAPPING_KEY_STATE, HISTORY_MAPPING_STATE_CUSTOMIZED),
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == 1
         )  # one event is customized
 
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/db/test_db_upgrades.py#L3639'>rotkehlchen/tests/db/test_db_upgrades.py~L3639</a>
```diff
             cursor.execute(
                 "SELECT COUNT(*) FROM user_credentials WHERE location=?",
                 ("D",),
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == 0
         )
         assert (
             cursor.execute(
                 "SELECT COUNT(*) FROM used_query_ranges WHERE name LIKE ? ESCAPE ?;",
                 (f"{Location.BITTREX!s}\\_%", "\\"),
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == 0
         )
         assert json.loads(
             cursor.execute(
                 "SELECT value FROM settings WHERE name=?",
                 ("non_syncing_exchanges",),
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
         ) == [{"name": "Kraken 1", "location": "kraken"}]
 
         # verify that the new eth2 validators table and data is there
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/db/test_db_upgrades.py#L3673'>rotkehlchen/tests/db/test_db_upgrades.py~L3673</a>
```diff
         assert (
             cursor.execute(
                 "SELECT validator_index, public_key, ownership_proportion from eth2_validators"
-            ).fetchall()
+            )
+            .fetchall()
             == validators_data
         )  # noqa: E501
         assert cursor.execute("SELECT * from eth2_daily_staking_details").fetchall() == daily_stats
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/db/test_db_upgrades.py#L3702'>rotkehlchen/tests/db/test_db_upgrades.py~L3702</a>
```diff
         assert (
             cursor.execute(
                 "SELECT identifier FROM history_events ORDER BY identifier",
-            ).fetchall()
+            )
+            .fetchall()
             == expected_events_ids
         )
         assert (
             cursor.execute(
                 "SELECT identifier FROM evm_events_info",
-            ).fetchall()
+            )
+            .fetchall()
             == expected_events_ids
         )
         assert (
             cursor.execute(
                 "SELECT parent_identifier FROM history_events_mappings",
-            ).fetchall()
+            )
+            .fetchall()
             == expected_events_ids
         )
 
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/db/test_db_upgrades.py#L3797'>rotkehlchen/tests/db/test_db_upgrades.py~L3797</a>
```diff
                 cursor.execute(  # Check that new locations were added
                     "SELECT location FROM location WHERE seq=?",
                     (new_loc.value,),
-                ).fetchone()[0]
+                )
+                .fetchone()[0]
                 == new_loc.serialize_for_db()
             )
         raw_list = cursor.execute(
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/db/test_db_upgrades.py#L3848'>rotkehlchen/tests/db/test_db_upgrades.py~L3848</a>
```diff
         assert (
             cursor.execute(
                 "SELECT COUNT(*) from evm_events_info WHERE counterparty=?", ("hop-protocol",)
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == 1
         )  # noqa: E501
         assert (
-            cursor.execute(
-                "SELECT COUNT(*) from evm_events_info WHERE counterparty=?", ("hop",)
-            ).fetchone()[0]
+            cursor.execute("SELECT COUNT(*) from evm_events_info WHERE counterparty=?", ("hop",))
+            .fetchone()[0]
             == 0
         )  # noqa: E501
         assert (
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/db/test_db_upgrades.py#L3904'>rotkehlchen/tests/db/test_db_upgrades.py~L3904</a>
```diff
         assert (
             cursor.execute(
                 "SELECT COUNT(*) from evm_events_info WHERE counterparty=?", ("hop-protocol",)
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == 0
         )  # noqa: E501
         assert (
-            cursor.execute(
-                "SELECT COUNT(*) from evm_events_info WHERE counterparty=?", ("hop",)
-            ).fetchone()[0]
+            cursor.execute("SELECT COUNT(*) from evm_events_info WHERE counterparty=?", ("hop",))
+            .fetchone()[0]
             == 1
         )  # noqa: E501
 
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/db/test_db_upgrades.py#L3920'>rotkehlchen/tests/db/test_db_upgrades.py~L3920</a>
```diff
             cursor.execute(
                 "SELECT COUNT(*) FROM user_credentials WHERE location=?",
                 (Location.COINBASEPRO.serialize_for_db(),),
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == 0
         )
 
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/db/test_db_upgrades.py#L3980'>rotkehlchen/tests/db/test_db_upgrades.py~L3980</a>
```diff
             cursor.execute(
                 "SELECT * FROM address_book WHERE address IN (?, ?)",
                 (bad_address, tether_address),
-            ).fetchall()
+            )
+            .fetchall()
         ) == {
             (tether_address, None, "Black Tether"),
             (bad_address, None, "yabirgb.eth"),
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/db/test_db_upgrades.py#L3995'>rotkehlchen/tests/db/test_db_upgrades.py~L3995</a>
```diff
                     Location.POLKADOT.serialize_for_db(),
                     Location.KUSAMA.serialize_for_db(),
                 ),
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == 0
         )
 
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/db/test_db_upgrades.py#L4056'>rotkehlchen/tests/db/test_db_upgrades.py~L4056</a>
```diff
             cursor.execute(
                 "SELECT * FROM address_book WHERE address IN (?, ?)",
                 (bad_address, tether_address),
-            ).fetchall()
+            )
+            .fetchall()
         ) == {
             (tether_address, "NONE", "Black Tether"),
             (bad_address, "NONE", "yabirgb.eth"),
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/db/test_db_upgrades.py#L4064'>rotkehlchen/tests/db/test_db_upgrades.py~L4064</a>
```diff
         assert (
             cursor.execute(
                 "SELECT value FROM settings WHERE name='historical_price_oracles'",
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == '["manual", "cryptocompare", "coingecko", "defillama", "uniswapv3", "uniswapv2"]'
         )  # noqa: E501
         assert (
             cursor.execute(
                 "SELECT * FROM used_query_ranges WHERE name LIKE 'zksynclitetxs\\_%' ESCAPE '\\'",
-            ).fetchone()
+            )
+            .fetchone()
             is None
         )
         assert (
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/db/test_db_upgrades.py#L4082'>rotkehlchen/tests/db/test_db_upgrades.py~L4082</a>
```diff
                     Location.POLKADOT.serialize_for_db(),
                     Location.KUSAMA.serialize_for_db(),
                 ),
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == 4
         )
 
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/db/test_db_upgrades.py#L4145'>rotkehlchen/tests/db/test_db_upgrades.py~L4145</a>
```diff
         assert (
             cursor.execute(
                 "SELECT COUNT(*) FROM pragma_table_info('history_events') WHERE name='extra_data'",
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == 0
         )
         assert (
             cursor.execute(
                 "SELECT COUNT(*) FROM pragma_table_info('evm_events_info') WHERE name='extra_data'",
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == 1
         )
         existing_evm_event = cursor.execute(
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/db/test_db_upgrades.py#L4165'>rotkehlchen/tests/db/test_db_upgrades.py~L4165</a>
```diff
         assert (
             cursor.execute(
                 "SELECT COUNT(*) FROM history_events WHERE event_identifier=?", ("KRAKEN-XXX",)
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == 2
         )  # kraken history event that needs to be removed since it will be replaced  # noqa: E501
 
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/db/test_db_upgrades.py#L4220'>rotkehlchen/tests/db/test_db_upgrades.py~L4220</a>
```diff
         assert (
             cursor.execute(
                 "SELECT COUNT(*) FROM pragma_table_info('history_events') WHERE name='extra_data'",
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == 1
         )
         assert (
             cursor.execute(
                 "SELECT COUNT(*) FROM pragma_table_info('evm_events_info') WHERE name='extra_data'",
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == 0
         )
 
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/db/test_db_upgrades.py#L4242'>rotkehlchen/tests/db/test_db_upgrades.py~L4242</a>
```diff
                 "sequence_index=? AND timestamp=? AND location=? AND location_label=? AND asset=? AND "
                 "amount=? AND usd_value=? AND notes=? AND type=? AND subtype=? AND extra_data IS NULL",
                 history_event_bindings,
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == 1
         )
 
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/db/test_db_upgrades.py#L4327'>rotkehlchen/tests/db/test_db_upgrades.py~L4327</a>
```diff
         assert (
             cursor.execute(
                 "SELECT COUNT(*) FROM history_events WHERE event_identifier=?", ("KRAKEN-XXX",)
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == 0
         )  # noqa: E501
 
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/db/test_db_upgrades.py#L4337'>rotkehlchen/tests/db/test_db_upgrades.py~L4337</a>
```diff
         assert (
             cursor.execute(
                 "SELECT COUNT(*) FROM settings WHERE name='account_for_assets_movements'",
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == 0
         )
 
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/db/test_db_upgrades.py#L4361'>rotkehlchen/tests/db/test_db_upgrades.py~L4361</a>
```diff
                 user_db_cursor.execute(
                     f"SELECT COUNT(*) FROM {table} WHERE {column} IN (?, ?)",
                     (uniswap_erc20_token.identifier, uniswap_erc721_token.identifier),
-                ).fetchone()[0]
+                )
+                .fetchone()[0]
                 == 0
                 if expect_removed
                 else count
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/db/test_db_upgrades.py#L4376'>rotkehlchen/tests/db/test_db_upgrades.py~L4376</a>
```diff
                         uniswap_erc721_token.identifier,
                         basename_token.identifier,
                     ),  # noqa: E501
-                ).fetchone()[0]
+                )
+                .fetchone()[0]
                 == 0
                 if expect_removed
                 else 3
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/db/test_db_upgrades.py#L4385'>rotkehlchen/tests/db/test_db_upgrades.py~L4385</a>
```diff
                 global_db_cursor.execute(
                     "SELECT COUNT(*) FROM assets WHERE identifier IN (?, ?)",
                     (erc721_token_with_id.identifier, basename_token_with_id.identifier),
-                ).fetchone()[0]
+                )
+                .fetchone()[0]
                 == 2
             )  # tokens with collectible ids shouldn't be modified
 
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/db/test_db_upgrades.py#L4727'>rotkehlchen/tests/db/test_db_upgrades.py~L4727</a>
```diff
         assert "manual" in json.loads(
             write_cursor.execute(
                 "SELECT value FROM settings WHERE name='historical_price_oracles'"
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
         )  # noqa: E501
 
     with db_v46.conn.read_ctx() as cursor:  # assert block events state before upgrade
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/db/test_db_upgrades.py#L4861'>rotkehlchen/tests/db/test_db_upgrades.py~L4861</a>
```diff
         assert (
             cursor.execute(
                 "SELECT COUNT(*) FROM settings WHERE name='last_data_upload_ts'",
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == 1
         )
         assert json.loads(
             cursor.execute(
                 "SELECT value FROM settings WHERE name='active_modules'",
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
         ) == [
             "aave",
             "sushiswap",
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/db/test_db_upgrades.py#L4885'>rotkehlchen/tests/db/test_db_upgrades.py~L4885</a>
```diff
         assert set(
             cursor.execute(
                 "SELECT * FROM multisettings WHERE name LIKE 'queried\\_address\\_%' ESCAPE '\\'",
-            ).fetchall()
+            )
+            .fetchall()
         ) == {
             ("queried_address_aave", "0x2B888954421b424C5D3D9Ce9bB67c9bD47537d12"),
             ("queried_address_aave", "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045"),
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/db/test_db_upgrades.py#L4960'>rotkehlchen/tests/db/test_db_upgrades.py~L4960</a>
```diff
         assert (
             cursor.execute(
                 "SELECT COUNT(*) FROM multisettings WHERE name='ignored_asset' AND value IN ('BUX', 'TEDDY', 'BIDR')",  # noqa: E501
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == 3
         )
         assert (
             cursor.execute(
                 "SELECT COUNT(*) FROM multisettings WHERE name='ignored_asset' AND value IN ('eip155:56/erc20:0x211FfbE424b90e25a15531ca322adF1559779E45', 'eip155:43114/erc20:0x094bd7B2D99711A1486FB94d4395801C6d0fdDcC')",  # noqa: E501
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == 0
         )
         assert (
             cursor.execute(  # this is BIDR's actual evm token. Testing the already exists case for ignore asset  # noqa: E501
                 "SELECT COUNT(*) FROM multisettings WHERE name='ignored_asset' AND value ='eip155:56/erc20:0x9A2f5556e9A637e8fBcE886d8e3cf8b316a1D8a2'",  # noqa: E501
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == 1
         )
 
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/db/test_db_upgrades.py#L5019'>rotkehlchen/tests/db/test_db_upgrades.py~L5019</a>
```diff
             cursor.execute(  # events with the correct prefix for bybit, coinbase & gemini removed  # noqa: E501
                 "SELECT COUNT(*) FROM history_events WHERE location IN (?, ?, ?) AND event_identifier LIKE ?",  # noqa: E501
                 (bybit_location, coinbase_location, gemini_location, f"{CB_EVENTS_PREFIX}%"),
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == 0
         )
 
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/db/test_db_upgrades.py#L5032'>rotkehlchen/tests/db/test_db_upgrades.py~L5032</a>
```diff
                     f"{ROTKI_EVENT_PREFIX}_GEM_EVENT",
                     f"{ROTKI_EVENT_PREFIX}_BYBIT_EVENT",
                 ),  # noqa: E501
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == 4
         )
 
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/db/test_db_upgrades.py#L5043'>rotkehlchen/tests/db/test_db_upgrades.py~L5043</a>
```diff
                     f"{(coinbase_loc := Location.COINBASE.serialize())}_%_last_query_ts",
                     f"{coinbase_loc}_%_last_query_id",
                 ),  # noqa: E501
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == 0
         )
         assert (
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/db/test_db_upgrades.py#L5053'>rotkehlchen/tests/db/test_db_upgrades.py~L5053</a>
```diff
                     f"{(kraken_loc := Location.KRAKEN.serialize())}_%_last_query_ts",
                     f"{kraken_loc}_%_last_query_id",
                 ),  # noqa: E501
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == 1
         )
         assert (
             cursor.execute(  # ensure trades with bybit or Coinbase or Gemini location and a link are deleted  # noqa: E501
                 "SELECT COUNT(*) FROM trades WHERE location IN (?, ?, ?) AND link != ?",
                 (bybit_location, coinbase_location, gemini_location, ""),
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == 0
         )
         assert cursor.execute(
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/db/test_db_upgrades.py#L5080'>rotkehlchen/tests/db/test_db_upgrades.py~L5080</a>
```diff
                     f"{Location.GEMINI.serialize()}_%",
                     f"{Location.BYBIT.serialize()}_%",
                 ),  # noqa: E501
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == 0
         )
 
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/db/test_db_upgrades.py#L5164'>rotkehlchen/tests/db/test_db_upgrades.py~L5164</a>
```diff
             cursor.execute(  # verify that all old counterparty specific rules for these types are removed  # noqa: E501
                 "SELECT counterparty FROM accounting_rules WHERE (type=? AND subtype=?) OR (type=? AND subtype=?)",  # noqa: E501
                 ("deposit", "deposit asset", "withdrawal", "remove asset"),
-            ).fetchall()
+            )
+            .fetchall()
             == [("NONE",), ("NONE",)]
         )  # Keep the rules with null counterparty
 
         # ensure that manual historical price oracle is removed from settings.
         assert "manual" not in json.loads(
-            cursor.execute(
-                "SELECT value FROM settings WHERE name='historical_price_oracles'"
-            ).fetchone()[0]
+            cursor.execute("SELECT value FROM settings WHERE name='historical_price_oracles'")
+            .fetchone()[0]
         )  # noqa: E501
         assert (
             cursor.execute(
                 "SELECT COUNT(*) FROM settings WHERE name='last_data_upload_ts'",
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == 0
         )
         assert json.loads(
             cursor.execute(
                 "SELECT value FROM settings WHERE name='active_modules'",
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
         ) == [
             "sushiswap",
             "uniswap",
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/db/test_db_upgrades.py#L5197'>rotkehlchen/tests/db/test_db_upgrades.py~L5197</a>
```diff
         assert (
             cursor.execute(
                 "SELECT * FROM multisettings WHERE name LIKE 'queried\\_address\\_%' ESCAPE '\\'",
-            ).fetchall()
+            )
+            .fetchall()
             == []
         )
         # After tests for the avalanche/binance tokens deletion
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/db/test_db_upgrades.py#L5261'>rotkehlchen/tests/db/test_db_upgrades.py~L5261</a>
```diff
         assert (
             cursor.execute(
                 "SELECT COUNT(*) FROM multisettings WHERE name='ignored_asset' AND value IN ('BUX', 'TEDDY', 'BIDR')",  # noqa: E501
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == 0
         )
         assert (
             cursor.execute(
                 "SELECT COUNT(*) FROM multisettings WHERE name='ignored_asset' AND value IN ('eip155:56/erc20:0x211FfbE424b90e25a15531ca322adF1559779E45', 'eip155:43114/erc20:0x094bd7B2D99711A1486FB94d4395801C6d0fdDcC', 'eip155:56/erc20:0x9A2f5556e9A637e8fBcE886d8e3cf8b316a1D8a2')",  # noqa: E501
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == 3
         )
 
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/db/test_db_upgrades.py#L5534'>rotkehlchen/tests/db/test_db_upgrades.py~L5534</a>
```diff
             cursor.execute(
                 "SELECT value FROM settings where name=?",
                 ("use_unified_etherscan_api",),
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == "True"
         )
         assert not table_exists(cursor, "evm_transactions_authorizations")
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/db/test_db_upgrades.py#L5949'>rotkehlchen/tests/db/test_db_upgrades.py~L5949</a>
```diff
             cursor.execute(
                 "SELECT COUNT(*) from used_query_ranges WHERE name LIKE ? ESCAPE ?",
                 ("%\\_trades\\_%", "\\"),
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == 0
         )  # noqa: E501
         assert not table_exists(cursor, "action_type")
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/db/test_db_upgrades.py#L5961'>rotkehlchen/tests/db/test_db_upgrades.py~L5961</a>
```diff
             cursor.execute(
                 "SELECT value FROM settings where name=?",
                 ("use_unified_etherscan_api",),
-            ).fetchall()
+            )
+            .fetchall()
             == []
         )
         assert table_exists(cursor, "evm_transactions_authorizations")
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/db/test_db_upgrades.py#L5982'>rotkehlchen/tests/db/test_db_upgrades.py~L5982</a>
```diff
             cursor.execute(
                 "SELECT COUNT(*) FROM history_events WHERE asset=?",
                 (A_LTC.identifier,),
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == ignored_asset_event_count
         )
         assert (
             cursor.execute(
                 "SELECT COUNT(*) FROM history_events WHERE ignored=1",
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == ignored_asset_event_count
         )
 
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/db/test_db_upgrades.py#L6235'>rotkehlchen/tests/db/test_db_upgrades.py~L6235</a>
```diff
                 assert db.get_setting(cursor, "ongoing_upgrade_from_version") is None
                 # Check that the backup was used
                 assert (
-                    cursor.execute("SELECT value FROM settings WHERE name='is_backup'").fetchone()[
-                        0
-                    ]
+                    cursor.execute("SELECT value FROM settings WHERE name='is_backup'")
+                    .fetchone()[0]
                     == "33"
                 )  # noqa: E501
 
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/db/test_db_upgrades.py#L6279'>rotkehlchen/tests/db/test_db_upgrades.py~L6279</a>
```diff
         assert (
             cursor.execute(
                 "SELECT COUNT(*) FROM assets WHERE identifier IN ('$NAP', 'ACS', 'AI16Z', 'BONK', 'TRISIG', 'HODLSOL')"
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == 6
         )  # noqa: E501
         tokens_mapping = {  # define before/after mapping for token identifiers
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/db/test_db_upgrades.py#L6306'>rotkehlchen/tests/db/test_db_upgrades.py~L6306</a>
```diff
         assert (
             cursor.execute(
                 'SELECT currency, amount, usd_value FROM timed_balances WHERE currency IN ("$NAP", "ACS", "AI16Z")'
-            ).fetchall()
+            )
+            .fetchall()
             == expected_timed_balances_before
         )  # noqa: E501
         assert (
             cursor.execute(
                 'SELECT asset, amount, type FROM history_events WHERE asset IN ("$NAP", "ACS", "HODLSOL")'
-            ).fetchall()
+            )
+            .fetchall()
             == expected_history_events_before
         )  # noqa: E501
         assert (
             cursor.execute(
                 'SELECT pl_currency, profit_loss FROM margin_positions WHERE pl_currency IN ("AI16Z", "ACS")'
-            ).fetchall()
+            )
+            .fetchall()
             == expected_margin_positions_before
         )  # noqa: E501
         # it should not have any CAIPS format assets before upgrade
         assert (
             cursor.execute(
                 'SELECT COUNT(*) FROM assets WHERE identifier LIKE "solana:%" ORDER BY identifier'
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == 0
         )  # noqa: E501
         # user notes with empty string for global location
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/db/test_db_upgrades.py#L6337'>rotkehlchen/tests/db/test_db_upgrades.py~L6337</a>
```diff
         assert (
             cursor.execute(
                 solana_query := "SELECT COUNT(*) FROM location WHERE location='w' AND seq=55",
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == 0
         )
 
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/db/test_db_upgrades.py#L6369'>rotkehlchen/tests/db/test_db_upgrades.py~L6369</a>
```diff
         assert (
             cursor.execute(
                 'SELECT COUNT(*) FROM assets WHERE identifier IN ("$NAP", "ACS", "AI16Z", "BONK") ORDER BY identifier'
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == 0
         )  # noqa: E501
         # verify duplicates were deleted
         assert (
-            cursor.execute(
-                'SELECT COUNT(*) FROM assets WHERE identifier IN ("TRISIG", "HODLSOL")'
-            ).fetchone()[0]
+            cursor.execute('SELECT COUNT(*) FROM assets WHERE identifier IN ("TRISIG", "HODLSOL")')
+            .fetchone()[0]
             == 0
         )  # noqa: E501
         # Check that asset references were updated in other tables
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/db/test_db_upgrades.py#L6437'>rotkehlchen/tests/db/test_db_upgrades.py~L6437</a>
```diff
             cursor.execute(
                 "SELECT COUNT(*) FROM history_events_mappings WHERE parent_identifier=? AND name=? AND value=?",  # noqa: E501
                 (2, HISTORY_MAPPING_KEY_STATE, HISTORY_MAPPING_STATE_CUSTOMIZED),
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == 1
         )  # TEST_EVENT_2 is customized.
         assert not table_exists(cursor=cursor, name="accounting_rule_events")
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/db/test_db_upgrades.py#L6455'>rotkehlchen/tests/db/test_db_upgrades.py~L6455</a>
```diff
         assert (
             cursor.execute(
                 "SELECT COUNT(*) FROM assets WHERE identifier = ?", ((old_solana_id := "SOL-2"),)
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == 1
         )  # noqa: E501
         assert cursor.execute(
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/db/test_db_upgrades.py#L6482'>rotkehlchen/tests/db/test_db_upgrades.py~L6482</a>
```diff
         assert (
             cursor.execute(
                 "SELECT COUNT(*) FROM assets WHERE identifier = ?", ((new_solana_id := "SOL"),)
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == 0
         )  # noqa: E501
         assert cursor.execute(
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/db/test_db_upgrades.py#L6529'>rotkehlchen/tests/db/test_db_upgrades.py~L6529</a>
```diff
         assert (
             cursor.execute(
                 "SELECT identifier, event_identifier, location_label FROM history_events WHERE location = 'P'",  # noqa: E501
-            ).fetchall()
+            )
+            .fetchall()
             == [
                 (1, "TEST_EVENT_1", "Crypto.com App"),
                 (
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/db/test_db_upgrades.py#L6544'>rotkehlchen/tests/db/test_db_upgrades.py~L6544</a>
```diff
         assert (
             cursor.execute(
                 "SELECT type, subtype, counterparty FROM accounting_rules ORDER BY identifier"
-            ).fetchall()
+            )
+            .fetchall()
             == rules
         )  # noqa: E501
 
         # Check that SOL-2 no longer exists in assets table
         assert (
-            cursor.execute(
-                "SELECT COUNT(*) FROM assets WHERE identifier = ?", (old_solana_id,)
-            ).fetchone()[0]
+            cursor.execute("SELECT COUNT(*) FROM assets WHERE identifier = ?", (old_solana_id,))
+            .fetchone()[0]
             == 0
         )  # noqa: E501
         # Check that SOL now exists in assets table
         assert (
-            cursor.execute(
-                "SELECT COUNT(*) FROM assets WHERE identifier = ?", (new_solana_id,)
-            ).fetchone()[0]
+            cursor.execute("SELECT COUNT(*) FROM assets WHERE identifier = ?", (new_solana_id,))
+            .fetchone()[0]
             == 1
         )  # noqa: E501
         assert (
             cursor.execute(
                 "SELECT COUNT(*) FROM timed_balances WHERE currency = ?", (old_solana_id,)
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == 0
         )  # noqa: E501
         assert cursor.execute(
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/db/test_db_upgrades.py#L6576'>rotkehlchen/tests/db/test_db_upgrades.py~L6576</a>
```diff
         assert (
             cursor.execute(
                 "SELECT COUNT(*) FROM manually_tracked_balances WHERE asset = ?", (old_solana_id,)
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == 0
         )  # noqa: E501
         assert cursor.execute(
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/db/test_db_upgrades.py#L6587'>rotkehlchen/tests/db/test_db_upgrades.py~L6587</a>
```diff
         assert (
             cursor.execute(
                 "SELECT COUNT(*) FROM margin_positions WHERE pl_currency = ?", (old_solana_id,)
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == 0
         )  # noqa: E501
         assert cursor.execute(
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/db/test_db_upgrades.py#L6605'>rotkehlchen/tests/db/test_db_upgrades.py~L6605</a>
```diff
             cursor.execute(
                 "SELECT * FROM external_service_credentials WHERE name IN (?, ?)",
                 ("monerium", "gnosis_pay"),
-            ).fetchall()
+            )
+            .fetchall()
             == []
         )  # noqa: E501
         assert cursor.execute("SELECT * FROM gnosispay_data").fetchall() == [gnosispay_data[:-1]]
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/db/test_db_upgrades.py#L6663'>rotkehlchen/tests/db/test_db_upgrades.py~L6663</a>
```diff
         assert (
             cursor.execute(
                 "SELECT identifier, group_identifier, sequence_index, asset FROM history_events ORDER BY identifier"
-            ).fetchall()
+            )
+            .fetchall()
             == result
         )  # noqa: E501
         assert (
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/exchanges/test_binance.py#L1175'>rotkehlchen/tests/exchanges/test_binance.py~L1175</a>
```diff
                 cursor.execute(
                     'SELECT COUNT(*) FROM history_events WHERE subtype="reward" AND location=?;',
                     (location.serialize_for_db(),),
-                ).fetchone()[0]
+                )
+                .fetchone()[0]
                 == count
             )
 
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/exchanges/test_bitfinex.py#L210'>rotkehlchen/tests/exchanges/test_bitfinex.py~L210</a>
```diff
             cursor.execute(
                 "SELECT exchange_symbol, local_id FROM location_asset_mappings WHERE location=?;",
                 (bitfinex_db_serialized,),
-            ).fetchall()
+            )
+            .fetchall()
         )
 
     mock_bitfinex.first_connection()
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/exchanges/test_bitfinex.py#L220'>rotkehlchen/tests/exchanges/test_bitfinex.py~L220</a>
```diff
             cursor.execute(
                 "SELECT exchange_symbol, local_id FROM location_asset_mappings WHERE location=?;",
                 (bitfinex_db_serialized,),
-            ).fetchall()
+            )
+            .fetchall()
         )
 
     new_mappings = mappings_after - mappings_before
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/exchanges/test_bitfinex.py#L233'>rotkehlchen/tests/exchanges/test_bitfinex.py~L233</a>
```diff
                     "SELECT COUNT(*) FROM location_unsupported_assets "
                     "WHERE location=? AND exchange_symbol=?;",
                     (bitfinex_db_serialized, bfx_symbol),
-                ).fetchone()[0]
+                )
+                .fetchone()[0]
                 == 0
             )  # no new mapping added for unsupported assets
 
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/unit/decoders/test_ens.py#L1101'>rotkehlchen/tests/unit/decoders/test_ens.py~L1101</a>
```diff
             assert (
                 cursor.execute(
                     f"SELECT COUNT(*) FROM unique_cache WHERE key LIKE '{cache_key.serialize()}%'",
-                ).fetchone()[0]
+                )
+                .fetchone()[0]
                 == 0
             )
 
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/unit/globaldb/test_asset_updates.py#L430'>rotkehlchen/tests/unit/globaldb/test_asset_updates.py~L430</a>
```diff
             == "Ethereum"
         )  # noqa: E501
         assert (
-            cursor.execute(
-                "SELECT symbol FROM common_asset_details WHERE identifier='ETH'"
-            ).fetchone()[0]
+            cursor.execute("SELECT symbol FROM common_asset_details WHERE identifier='ETH'")
+            .fetchone()[0]
             == "ETH"
         )  # noqa: E501
         # BTC should have been updated since the insertion were correct
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/unit/globaldb/test_asset_updates.py#L441'>rotkehlchen/tests/unit/globaldb/test_asset_updates.py~L441</a>
```diff
             == "name2"
         )  # noqa: E501
         assert (
-            cursor.execute(
-                "SELECT symbol FROM common_asset_details WHERE identifier='BTC'"
-            ).fetchone()[0]
+            cursor.execute("SELECT symbol FROM common_asset_details WHERE identifier='BTC'")
+            .fetchone()[0]
             == "symbol2"
         )  # noqa: E501
         # NEW-ASSET-1 should have not been added since the insertions were incorrect
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/unit/globaldb/test_asset_updates.py#L459'>rotkehlchen/tests/unit/globaldb/test_asset_updates.py~L459</a>
```diff
         assert (
             cursor.execute(
                 "SELECT symbol FROM common_asset_details WHERE identifier='NEW-ASSET-2'"
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == "symbol4"
         )  # noqa: E501
 
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/unit/globaldb/test_asset_updates.py#L601'>rotkehlchen/tests/unit/globaldb/test_asset_updates.py~L601</a>
```diff
             write_cursor.execute(
                 "SELECT COUNT(*) FROM underlying_tokens_list WHERE parent_token_entry=?;",
                 ("eip155:42161/erc20:0xA5EDBDD9646f8dFF606d7448e414884C7d905dCA",),
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == 1
         )
         assert (
             write_cursor.execute(
                 "SELECT COUNT(*) FROM multiasset_mappings WHERE asset=?;",
                 ("eip155:42161/erc20:0xFF970A61A04b1cA14834A43f5dE4533eBDDB5CC8",),
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == 1
         )
 
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/unit/globaldb/test_asset_updates.py#L676'>rotkehlchen/tests/unit/globaldb/test_asset_updates.py~L676</a>
```diff
             cursor.execute(
                 "SELECT COUNT(*) FROM underlying_tokens_list WHERE parent_token_entry=?;",
                 ("eip155:42161/erc20:0xA5EDBDD9646f8dFF606d7448e414884C7d905dCA",),
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == 1
         )
         assert (
             cursor.execute(
                 "SELECT COUNT(*) FROM multiasset_mappings WHERE asset=?;",
                 ("eip155:42161/erc20:0xFF970A61A04b1cA14834A43f5dE4533eBDDB5CC8",),
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == 1
         )
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/unit/globaldb/test_globaldb_cache.py#L417'>rotkehlchen/tests/unit/globaldb/test_globaldb_cache.py~L417</a>
```diff
         assert (
             write_cursor.execute(
                 "SELECT COUNT(*) FROM unique_cache WHERE key LIKE 'CURVE_POOL_ADDRESS100x%'",
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             > 0
         )
 
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/unit/globaldb/test_globaldb_cache.py#L674'>rotkehlchen/tests/unit/globaldb/test_globaldb_cache.py~L674</a>
```diff
                         "0xBc2acf5E821c5c9f8667A36bB1131dAd26Ed64F9"
                     ),
                 ),
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == 0
         )  # noqa: E501
 
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/unit/globaldb/test_globaldb_cache.py#L689'>rotkehlchen/tests/unit/globaldb/test_globaldb_cache.py~L689</a>
```diff
 
     with GlobalDBHandler().conn.read_ctx() as cursor:
         assert (
-            cursor.execute(
-                "SELECT protocol FROM evm_tokens WHERE address=?", (pool_address,)
-            ).fetchone()[0]
+            cursor.execute("SELECT protocol FROM evm_tokens WHERE address=?", (pool_address,))
+            .fetchone()[0]
             == "balancer-v2"
         )  # noqa: E501
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/unit/globaldb/test_globaldb_consistency.py#L127'>rotkehlchen/tests/unit/globaldb/test_globaldb_consistency.py~L127</a>
```diff
         # - `apply_pending_compatible_updates` runs during create_globaldb() and pulls all compatible asset updates up to v32 and then v36 (max compatible)  # noqa: E501
         # - At this point we are sure that assets updates up until 36 are applied
         assert (
-            old_globaldb_cursor.execute(
-                "SELECT value FROM settings WHERE name='assets_version'"
-            ).fetchone()[0]
+            old_globaldb_cursor.execute("SELECT value FROM settings WHERE name='assets_version'")
+            .fetchone()[0]
             == "36"
         )  # noqa: E501
         assert (
-            packaged_db_cursor.execute(
-                "SELECT value FROM settings WHERE name='assets_version'"
-            ).fetchone()[0]
+            packaged_db_cursor.execute("SELECT value FROM settings WHERE name='assets_version'")
+            .fetchone()[0]
             == "38"
         )  # noqa: E501
 
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/unit/globaldb/test_globaldb_consistency.py#L175'>rotkehlchen/tests/unit/globaldb/test_globaldb_consistency.py~L175</a>
```diff
                 f"protocol IN ({','.join(['?'] * len(IGNORED_PROTOCOLS))}) OR "
                 f"address in (SELECT value FROM general_cache)",
                 list(IGNORED_PROTOCOLS),
-            ).fetchall()
+            )
+            .fetchall()
         }
 
         (
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/unit/globaldb/test_globaldb_consistency.py#L237'>rotkehlchen/tests/unit/globaldb/test_globaldb_consistency.py~L237</a>
```diff
                 },
                 cursor.execute(
                     "SELECT identifier, weight, parent_token_entry FROM underlying_tokens_list",
-                ).fetchall(),
+                )
+                .fetchall(),
                 {
                     collection_id: (name, symbol)
                     for (collection_id, name, symbol) in cursor.execute(
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/unit/globaldb/test_globaldb_consistency.py#L246'>rotkehlchen/tests/unit/globaldb/test_globaldb_consistency.py~L246</a>
```diff
                 },
                 cursor.execute(
                     "SELECT collection_id, asset FROM multiasset_mappings",
-                ).fetchall(),
+                )
+                .fetchall(),
             )
             for cursor in (old_db_cursor, packaged_db_cursor)
         )
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/unit/globaldb/test_globaldb_consistency.py#L628'>rotkehlchen/tests/unit/globaldb/test_globaldb_consistency.py~L628</a>
```diff
                 set(
                     cursor.execute(
                         "SELECT location, exchange_symbol FROM location_unsupported_assets",
-                    ).fetchall()
+                    )
+                    .fetchall()
                 ),
             )
             for cursor in (old_db_cursor, packaged_db_cursor)
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/unit/globaldb/test_migrations.py#L29'>rotkehlchen/tests/unit/globaldb/test_migrations.py~L29</a>
```diff
         assert (
             cursor.execute(
                 "SELECT COUNT(*) FROM unique_cache WHERE key LIKE 'MAKERDAO_VAULT_ILK%'"
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == 0
         )  # noqa: E501
 
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/unit/globaldb/test_migrations.py#L45'>rotkehlchen/tests/unit/globaldb/test_migrations.py~L45</a>
```diff
         assert (
             cursor.execute(
                 "SELECT COUNT(*) FROM unique_cache WHERE key LIKE 'MAKERDAO_VAULT_ILK%'"
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == 25
         )  # noqa: E501
 
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/unit/globaldb/test_migrations.py#L65'>rotkehlchen/tests/unit/globaldb/test_migrations.py~L65</a>
```diff
     # Check state before migration
     with globaldb.conn.read_ctx() as cursor:
         assert (
-            cursor.execute(
-                "SELECT value FROM unique_cache WHERE key=?", ("YEARN_VAULTS",)
-            ).fetchone()[0]
+            cursor.execute("SELECT value FROM unique_cache WHERE key=?", ("YEARN_VAULTS",))
+            .fetchone()[0]
             == "179"
         )  # noqa: E501
 
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/unit/globaldb/test_migrations.py#L77'>rotkehlchen/tests/unit/globaldb/test_migrations.py~L77</a>
```diff
 
     with globaldb.conn.read_ctx() as cursor:
         assert (
-            cursor.execute(
-                "SELECT value FROM unique_cache WHERE key=?", ("YEARN_VAULTS",)
-            ).fetchone()
+            cursor.execute("SELECT value FROM unique_cache WHERE key=?", ("YEARN_VAULTS",))
+            .fetchone()
             is None
         )  # noqa: E501
 
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/unit/globaldb/test_migrations.py#L119'>rotkehlchen/tests/unit/globaldb/test_migrations.py~L119</a>
```diff
         rows = list(
             cursor.execute(
                 "SELECT address, blockchain, name FROM address_book ORDER BY blockchain"
-            ).fetchall()
+            )
+            .fetchall()
         )  # noqa: E501
         assert rows == [
             (eth_addr, "ETH", "yabir.eth"),
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/unit/globaldb/test_upgrades.py#L318'>rotkehlchen/tests/unit/globaldb/test_upgrades.py~L318</a>
```diff
             cursor.execute(
                 "SELECT COUNT(*) from contract_abi WHERE name in (?, ?, ?)",
                 ("ETH_SCAN", "ETH_MULTICALL", "ETH_MULTICALL2"),
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == 0
         )
         assert (
             cursor.execute(
                 "SELECT COUNT(*) from contract_data WHERE name in (?, ?, ?)",
                 ("ETH_SCAN", "ETH_MULTICALL", "ETH_MULTICALL2"),
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == 0
         )
 
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/unit/globaldb/test_upgrades.py#L583'>rotkehlchen/tests/unit/globaldb/test_upgrades.py~L583</a>
```diff
             cursor.execute(
                 "SELECT COUNT(*) FROM assets WHERE identifier=?",
                 ("VELO",),
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == 0
         )
         assert (
             cursor.execute(
                 "SELECT COUNT(*) FROM common_asset_details WHERE identifier=?",
                 ("VELO",),
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == 0
         )
 
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/unit/globaldb/test_upgrades.py#L681'>rotkehlchen/tests/unit/globaldb/test_upgrades.py~L681</a>
```diff
                 cursor.execute(
                     "SELECT COUNT(*) FROM location_asset_mappings WHERE location IS ?",
                     (exchange,),
-                ).fetchone()[0]
+                )
+                .fetchone()[0]
                 == expected_mappings_count
             )
 
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/unit/globaldb/test_upgrades.py#L703'>rotkehlchen/tests/unit/globaldb/test_upgrades.py~L703</a>
```diff
                 cursor.execute(
                     "SELECT COUNT(*) FROM location_unsupported_assets WHERE location = ?",
                     (location,),
-                ).fetchone()[0]
+                )
+                .fetchone()[0]
                 == expected_mappings_count
             )
 
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/unit/globaldb/test_upgrades.py#L720'>rotkehlchen/tests/unit/globaldb/test_upgrades.py~L720</a>
```diff
             cursor.execute(
                 "SELECT protocol from evm_tokens where identifier=?",
                 ("eip155:1/erc20:0xA0b73E1Ff0B80914AB6fe0444E65848C4C34450b",),
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == "spam"
         )
         assert (
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/unit/globaldb/test_upgrades.py#L730'>rotkehlchen/tests/unit/globaldb/test_upgrades.py~L730</a>
```diff
                     "SPAM_ASSET_FALSE_POSITIVE",
                     "eip155:1/erc20:0xA0b73E1Ff0B80914AB6fe0444E65848C4C34450b",
                 ),  # noqa: E501
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == 0
         )
         assert (
             cursor.execute(
                 "SELECT COUNT(*) FROM contract_data WHERE address=?",
                 ("0xAB392016859663Ce1267f8f243f9F2C02d93bad8",),
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == 1
         )
 
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/unit/globaldb/test_upgrades.py#L757'>rotkehlchen/tests/unit/globaldb/test_upgrades.py~L757</a>
```diff
         ).fetchall()
 
         cached_lp_tokens = set(
-            cursor.execute(
-                "SELECT value FROM general_cache WHERE key LIKE 'CURVE_LP_TOKENS%'"
-            ).fetchall()
+            cursor.execute("SELECT value FROM general_cache WHERE key LIKE 'CURVE_LP_TOKENS%'")
+            .fetchall()
         )  # noqa: E501
         cached_pool_tokens = set(
-            cursor.execute(
-                "SELECT value FROM general_cache WHERE key LIKE 'CURVE_POOL_TOKENS%'"
-            ).fetchall()
+            cursor.execute("SELECT value FROM general_cache WHERE key LIKE 'CURVE_POOL_TOKENS%'")
+            .fetchall()
         )  # noqa: E501
         cached_underlying_tokens = set(
             cursor.execute(
                 "SELECT value FROM general_cache WHERE key LIKE 'CURVE_POOL_UNDERLYING_TOKENS%'"
-            ).fetchall()
+            )
+            .fetchall()
         )  # noqa: E501
         cached_gauges = set(
-            cursor.execute(
-                "SELECT value FROM unique_cache WHERE key LIKE 'CURVE_GAUGE_ADDRESS%'"
-            ).fetchall()
+            cursor.execute("SELECT value FROM unique_cache WHERE key LIKE 'CURVE_GAUGE_ADDRESS%'")
+            .fetchall()
         )  # noqa: E501
         cached_pools = set(
-            cursor.execute(
-                "SELECT value FROM unique_cache WHERE key LIKE 'CURVE_POOL_ADDRESS%'"
-            ).fetchall()
+            cursor.execute("SELECT value FROM unique_cache WHERE key LIKE 'CURVE_POOL_ADDRESS%'")
+            .fetchall()
         )  # noqa: E501
 
         assert len(cached_lp_tokens) == 973
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/unit/globaldb/test_upgrades.py#L790'>rotkehlchen/tests/unit/globaldb/test_upgrades.py~L790</a>
```diff
 
         # cache with new keys doesn't exist yet
         assert (
-            cursor.execute(
-                "SELECT COUNT(*) FROM general_cache WHERE key LIKE 'CURVE_LP_TOKENS1%'"
-            ).fetchone()[0]
+            cursor.execute("SELECT COUNT(*) FROM general_cache WHERE key LIKE 'CURVE_LP_TOKENS1%'")
+            .fetchone()[0]
             == 0
         )  # noqa: E501
         assert (
             cursor.execute(
                 "SELECT COUNT(*) FROM general_cache WHERE key LIKE 'CURVE_POOL_TOKENS1%'"
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == 0
         )  # noqa: E501
         assert (
             cursor.execute(
                 "SELECT COUNT(*) FROM unique_cache WHERE key LIKE 'CURVE_GAUGE_ADDRESS1%'"
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == 0
         )  # noqa: E501
         assert (
             cursor.execute(
                 "SELECT COUNT(*) FROM unique_cache WHERE key LIKE 'CURVE_POOL_ADDRESS1%'"
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == 0
         )  # noqa: E501
 
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/unit/globaldb/test_upgrades.py#L848'>rotkehlchen/tests/unit/globaldb/test_upgrades.py~L848</a>
```diff
             cursor.execute(
                 "SELECT protocol from evm_tokens where identifier=?",
                 ("eip155:1/erc20:0xA0b73E1Ff0B80914AB6fe0444E65848C4C34450b",),
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             is None
         )
         assert (
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/unit/globaldb/test_upgrades.py#L858'>rotkehlchen/tests/unit/globaldb/test_upgrades.py~L858</a>
```diff
                     "SPAM_ASSET_FALSE_POSITIVE",
                     "eip155:1/erc20:0xA0b73E1Ff0B80914AB6fe0444E65848C4C34450b",
                 ),  # noqa: E501
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == 1
         )
 
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/unit/globaldb/test_upgrades.py#L890'>rotkehlchen/tests/unit/globaldb/test_upgrades.py~L890</a>
```diff
                 cursor.execute(
                     "SELECT COUNT(*) FROM asset_collections WHERE id=? AND name=? AND symbol=?",
                     (collection_id, name, symbol),
-                ).fetchone()[0]
+                )
+                .fetchone()[0]
                 == 1
             )
 
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/unit/globaldb/test_upgrades.py#L899'>rotkehlchen/tests/unit/globaldb/test_upgrades.py~L899</a>
```diff
             v7_multiasset_mappings
             == cursor.execute(
                 "SELECT rowid, collection_id, asset FROM multiasset_mappings;",
-            ).fetchall()
+            )
+            .fetchall()
         )
 
         assert (
             cursor.execute(
                 "SELECT COUNT(*) FROM contract_data WHERE address=?",
                 ("0xAB392016859663Ce1267f8f243f9F2C02d93bad8",),
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == 0
         )
         assert (
             cursor.execute(
                 "SELECT COUNT(*) FROM contract_data WHERE address=?",
                 ("0xc97EE9490F4e3A3136A513DB38E3C7b47e69303B",),
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == 1
         )
 
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/unit/globaldb/test_upgrades.py#L921'>rotkehlchen/tests/unit/globaldb/test_upgrades.py~L921</a>
```diff
         assert (
             cursor.execute(
                 "SELECT COUNT(*) FROM general_cache WHERE key LIKE 'CURVE_LP_TOKENS0x%'"
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == 0
         )  # noqa: E501
         assert (
             cursor.execute(
                 "SELECT COUNT(*) FROM general_cache WHERE key LIKE 'CURVE_POOL_TOKENS0x%'"
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == 0
         )  # noqa: E501
         assert (
             cursor.execute(
                 "SELECT COUNT(*) FROM unique_cache WHERE key LIKE 'CURVE_GAUGE_ADDRESS0x%'"
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == 0
         )  # noqa: E501
         assert (
             cursor.execute(
                 "SELECT COUNT(*) FROM unique_cache WHERE key LIKE 'CURVE_POOL_ADDRESS0x%'"
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == 0
         )  # noqa: E501
 
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/unit/globaldb/test_upgrades.py#L948'>rotkehlchen/tests/unit/globaldb/test_upgrades.py~L948</a>
```diff
             set(
                 cursor.execute(
                     "SELECT value FROM general_cache WHERE key LIKE 'CURVE_LP_TOKENS1%'"
-                ).fetchall()
+                )
+                .fetchall()
             )
             == cached_lp_tokens
         )  # noqa: E501
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/unit/globaldb/test_upgrades.py#L956'>rotkehlchen/tests/unit/globaldb/test_upgrades.py~L956</a>
```diff
             set(
                 cursor.execute(
                     "SELECT value FROM general_cache WHERE key LIKE 'CURVE_POOL_TOKENS1%'"
-                ).fetchall()
+                )
+                .fetchall()
             )
             == cached_pool_tokens
         )  # noqa: E501
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/unit/globaldb/test_upgrades.py#L964'>rotkehlchen/tests/unit/globaldb/test_upgrades.py~L964</a>
```diff
             set(
                 cursor.execute(
                     "SELECT value FROM unique_cache WHERE key LIKE 'CURVE_GAUGE_ADDRESS1%'"
-                ).fetchall()
+                )
+                .fetchall()
             )
             == cached_gauges
         )  # noqa: E501
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/unit/globaldb/test_upgrades.py#L972'>rotkehlchen/tests/unit/globaldb/test_upgrades.py~L972</a>
```diff
             set(
                 cursor.execute(
                     "SELECT value FROM unique_cache WHERE key LIKE 'CURVE_POOL_ADDRESS1%'"
-                ).fetchall()
+                )
+                .fetchall()
             )
             == cached_pools
         )  # noqa: E501
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/unit/globaldb/test_upgrades.py#L981'>rotkehlchen/tests/unit/globaldb/test_upgrades.py~L981</a>
```diff
         assert (
             cursor.execute(
                 "SELECT COUNT(*) FROM general_cache WHERE key LIKE 'CURVE_POOL_UNDERLYING_TOKENS%'"
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == 0
         )  # noqa: E501
 
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/unit/globaldb/test_upgrades.py#L1042'>rotkehlchen/tests/unit/globaldb/test_upgrades.py~L1042</a>
```diff
         assert (
             cursor.execute(
                 "SELECT COUNT(*) FROM underlying_tokens_list WHERE identifier=parent_token_entry",
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == 2
         )
 
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/unit/globaldb/test_upgrades.py#L1080'>rotkehlchen/tests/unit/globaldb/test_upgrades.py~L1080</a>
```diff
             len(
                 cursor.execute(
                     "SELECT * FROM price_history_source_types WHERE type IN ('G', 'H') AND seq IN (7, 8);",
-                ).fetchall()
+                )
+                .fetchall()
             )
             == 2
         )
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/unit/globaldb/test_upgrades.py#L1091'>rotkehlchen/tests/unit/globaldb/test_upgrades.py~L1091</a>
```diff
         assert (
             cursor.execute(  # Check that the two underlying tokens that havethemselves as underlying have been removed  # noqa: E501
                 "SELECT COUNT(*) FROM underlying_tokens_list",
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == underlying_count_before - 2
         )
         assert (
             cursor.execute(
                 "SELECT COUNT(*) FROM underlying_tokens_list WHERE identifier=parent_token_entry",
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == 0
         )
 
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/unit/globaldb/test_upgrades.py#L1168'>rotkehlchen/tests/unit/globaldb/test_upgrades.py~L1168</a>
```diff
             len(
                 cursor.execute(
                     'SELECT name FROM sqlite_master WHERE type="index" and sql IS NOT NULL',
-                ).fetchall()
+                )
+                .fetchall()
             )
             == 0
         )
         assert (
-            cursor.execute("SELECT COUNT(*) FROM asset_types WHERE type IN ('S', 'X')").fetchone()[
-                0
-            ]
+            cursor.execute("SELECT COUNT(*) FROM asset_types WHERE type IN ('S', 'X')")
+            .fetchone()[0]
             == 2
         )  # noqa: E501
         assert (
-            cursor.execute(
-                "SELECT COUNT(*) FROM assets WHERE identifier IN ('BIDR', 'TEDDY')"
-            ).fetchone()[0]
+            cursor.execute("SELECT COUNT(*) FROM assets WHERE identifier IN ('BIDR', 'TEDDY')")
+            .fetchone()[0]
             == 2
         )  # noqa: E501
         assert (
             cursor.execute(
                 "SELECT COUNT(*) FROM common_asset_details WHERE identifier IN ('BIDR', 'TEDDY')"
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == 2
         )  # noqa: E501
         # Make sure that non movable binance assets have their type changed
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/unit/globaldb/test_upgrades.py#L1197'>rotkehlchen/tests/unit/globaldb/test_upgrades.py~L1197</a>
```diff
         assert (
             cursor.execute(
                 "SELECT COUNT(*) FROM assets WHERE identifier='eip155:56/erc20:0x0409633A72D846fc5BBe2f98D88564D35987904D'"
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == 0
         )  # noqa: E501
 
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/unit/globaldb/test_upgrades.py#L1230'>rotkehlchen/tests/unit/globaldb/test_upgrades.py~L1230</a>
```diff
             ("idx_multiasset_mappings_identifier",),
         ]
         assert (
-            cursor.execute("SELECT COUNT(*) FROM asset_types WHERE type IN ('S', 'X')").fetchone()[
-                0
-            ]
+            cursor.execute("SELECT COUNT(*) FROM asset_types WHERE type IN ('S', 'X')")
+            .fetchone()[0]
             == 0
         )  # noqa: E501
         assert (
-            cursor.execute(
-                "SELECT COUNT(*) FROM assets WHERE identifier IN ('BIDR', 'TEDDY')"
-            ).fetchone()[0]
+            cursor.execute("SELECT COUNT(*) FROM assets WHERE identifier IN ('BIDR', 'TEDDY')")
+            .fetchone()[0]
             == 0
         )  # noqa: E501
         assert (
             cursor.execute(
                 "SELECT COUNT(*) FROM common_asset_details WHERE identifier IN ('BIDR', 'TEDDY')"
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == 0
         )  # noqa: E501
         assert (
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/unit/globaldb/test_upgrades.py#L1253'>rotkehlchen/tests/unit/globaldb/test_upgrades.py~L1253</a>
```diff
         assert (
             cursor.execute(
                 "SELECT COUNT(*) FROM assets WHERE identifier='eip155:56/erc20:0x0409633A72D846fc5BBe2f98D88564D35987904D'"
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == 1
         )  # noqa: E501
         assert (
-            cursor.execute(
-                "SELECT swapped_for FROM common_asset_details WHERE identifier='PHX'"
-            ).fetchone()[0]
+            cursor.execute("SELECT swapped_for FROM common_asset_details WHERE identifier='PHX'")
+            .fetchone()[0]
             == "eip155:56/erc20:0x0409633A72D846fc5BBe2f98D88564D35987904D"
         )  # noqa: E501
 
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/unit/globaldb/test_upgrades.py#L1278'>rotkehlchen/tests/unit/globaldb/test_upgrades.py~L1278</a>
```diff
                     CacheType.AERODROME_POOL_ADDRESS.serialize(),
                     CacheType.AERODROME_GAUGE_ADDRESS.serialize(),
                 ),
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == 1914
         )
         assert (
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/unit/globaldb/test_upgrades.py#L1290'>rotkehlchen/tests/unit/globaldb/test_upgrades.py~L1290</a>
```diff
                     CacheType.AERODROME_GAUGE_BRIBE_ADDRESS.serialize(),
                     CacheType.AERODROME_GAUGE_FEE_ADDRESS.serialize(),
                 ),
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == 0
         )
         assert cursor.execute(
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/unit/globaldb/test_upgrades.py#L1301'>rotkehlchen/tests/unit/globaldb/test_upgrades.py~L1301</a>
```diff
             cursor.execute(
                 "SELECT COUNT(*) FROM unique_cache WHERE key LIKE ? ESCAPE ?",
                 ("CURVE\\_LENDING\\_VAULT\\_AMM%", "\\"),
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == 2
         )
         assert (
             cursor.execute(
                 "SELECT COUNT(*) FROM unique_cache WHERE key LIKE ? ESCAPE ?",
                 ("CURVE\\_LENDING\\_VAULT\\_COLLATERAL\\_TOKEN%", "\\"),
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == 2
         )
         assert table_exists(cursor=cursor, name="counterparty_asset_mappings") is False
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/unit/globaldb/test_upgrades.py#L1348'>rotkehlchen/tests/unit/globaldb/test_upgrades.py~L1348</a>
```diff
                     CacheType.AERODROME_POOL_ADDRESS.serialize(),
                     CacheType.AERODROME_GAUGE_ADDRESS.serialize(),
                 ),
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == 4420
         )
         assert (
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/unit/globaldb/test_upgrades.py#L1360'>rotkehlchen/tests/unit/globaldb/test_upgrades.py~L1360</a>
```diff
                     CacheType.AERODROME_GAUGE_BRIBE_ADDRESS.serialize(),
                     CacheType.AERODROME_GAUGE_FEE_ADDRESS.serialize(),
                 ),
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == 1194
         )
         assert cursor.execute(
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/unit/globaldb/test_upgrades.py#L1371'>rotkehlchen/tests/unit/globaldb/test_upgrades.py~L1371</a>
```diff
             cursor.execute(
                 "SELECT COUNT(*) FROM unique_cache WHERE key LIKE ? ESCAPE ?",
                 ("CURVE\\_LENDING\\_VAULT\\_AMM%", "\\"),
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == 0
         )
         assert (
             cursor.execute(
                 "SELECT COUNT(*) FROM unique_cache WHERE key LIKE ? ESCAPE ?",
                 ("CURVE\\_LENDING\\_VAULT\\_COLLATERAL\\_TOKEN%", "\\"),
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == 0
         )
         assert cursor.execute("SELECT COUNT(*) FROM default_rpc_nodes").fetchone()[0] == 33
         assert (
             cursor.execute(
                 'SELECT COUNT(*) FROM default_rpc_nodes WHERE endpoint=""',
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == 0
         )
 
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/unit/globaldb/test_upgrades.py#L1398'>rotkehlchen/tests/unit/globaldb/test_upgrades.py~L1398</a>
```diff
         assert table_exists(cursor=cursor, name="solana_tokens") is False
         assert index_exists(cursor=cursor, name="idx_solana_tokens_identifier") is False
         assert (
-            cursor.execute(
-                "SELECT COUNT(*) FROM token_kinds WHERE token_kind IN ('D', 'E')"
-            ).fetchone()[0]
+            cursor.execute("SELECT COUNT(*) FROM token_kinds WHERE token_kind IN ('D', 'E')")
+            .fetchone()[0]
             == 0
         )  # noqa: E501
         assert (
-            solana_tokens_count := cursor.execute(
-                "SELECT COUNT(*) FROM assets WHERE type='Y'"
-            ).fetchone()[0]
+            solana_tokens_count := cursor.execute("SELECT COUNT(*) FROM assets WHERE type='Y'")
+            .fetchone()[0]
         ) == 432  # noqa: E501
         assert cursor.execute(
             "SELECT identifier, name FROM assets WHERE type='Y' LIMIT 10"
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/unit/globaldb/test_upgrades.py#L1504'>rotkehlchen/tests/unit/globaldb/test_upgrades.py~L1504</a>
```diff
             protocol_tokens_count := cursor.execute(
                 f"SELECT COUNT(*) FROM evm_tokens WHERE protocol IN ({','.join(['?' for _ in protocol_mapping])})",  # noqa: E501
                 tuple(protocol_mapping),
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
         ) > 0
 
         balancer_cache_mapping = {  # define before/after mapping for cache keys
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/unit/globaldb/test_upgrades.py#L1521'>rotkehlchen/tests/unit/globaldb/test_upgrades.py~L1521</a>
```diff
             (
                 balancer_gauges := cursor.execute(
                     "SELECT key FROM general_cache WHERE key LIKE 'BALANCER_GAUGES%'"
-                ).fetchall()
+                )
+                .fetchall()
             )
             == [  # noqa: E501
                 ("BALANCER_GAUGES1001",),
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/unit/globaldb/test_upgrades.py#L1571'>rotkehlchen/tests/unit/globaldb/test_upgrades.py~L1571</a>
```diff
         assert table_exists(cursor=cursor, name="solana_tokens") is True
         assert index_exists(cursor=cursor, name="idx_solana_tokens_identifier") is True
         assert (
-            cursor.execute(
-                "SELECT COUNT(*) FROM token_kinds WHERE token_kind IN ('D', 'E')"
-            ).fetchone()[0]
+            cursor.execute("SELECT COUNT(*) FROM token_kinds WHERE token_kind IN ('D', 'E')")
+            .fetchone()[0]
             == 2
         )  # noqa: E501
         assert cursor.execute(
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/unit/globaldb/test_upgrades.py#L1600'>rotkehlchen/tests/unit/globaldb/test_upgrades.py~L1600</a>
```diff
             ("solana/token:ATLASXmbPQxBUYbxPsV97usA3fPQYEqzQBUHgiFCUsXx", "Star Atlas"),
         ]
         assert (
-            cursor.execute(
-                "SELECT COUNT(*) FROM assets WHERE identifier IN ('HODLSOL','TRISIG')"
-            ).fetchone()[0]
+            cursor.execute("SELECT COUNT(*) FROM assets WHERE identifier IN ('HODLSOL','TRISIG')")
+            .fetchone()[0]
             == 0
         )  # noqa: E501
         # solana_tokens count reduced by 5: 2 duplicates removed + 3 user-added tokens migrated to AssetType.OTHER i.e. 'W'  # noqa: E501
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/unit/globaldb/test_upgrades.py#L1620'>rotkehlchen/tests/unit/globaldb/test_upgrades.py~L1620</a>
```diff
         assert (
             cursor.execute(
                 "SELECT COUNT(*) FROM solana_tokens s LEFT JOIN assets a ON s.identifier = a.identifier WHERE a.identifier IS NULL"
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == 0
         )  # noqa: E501
         # Ensure all solana tokens have corresponding entries in the common_asset_details table
         assert (
             cursor.execute(
                 "SELECT COUNT(*) FROM solana_tokens s LEFT JOIN common_asset_details c ON s.identifier = c.identifier WHERE c.identifier IS NULL"
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == 0
         )  # noqa: E501
         assert (
             cursor.execute(
                 "SELECT COUNT(*) FROM assets WHERE type='Y' AND identifier NOT LIKE 'solana%'"
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == 0
         )  # noqa: E501
         assert cursor.execute(
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/unit/globaldb/test_upgrades.py#L1688'>rotkehlchen/tests/unit/globaldb/test_upgrades.py~L1688</a>
```diff
         assert (
             cursor.execute(
                 "SELECT COUNT(*) FROM assets WHERE type='Y' AND identifier IN ('HODLER','LAND','LEMON')"
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == 0
         )  # noqa: E501
         assert (
             cursor.execute(
                 "SELECT identifier, name FROM assets WHERE type='W' AND identifier IN ('HODLER','LAND','LEMON')"
-            ).fetchall()
+            )
+            .fetchall()
             == user_added_tokens_before
         )  # noqa: E501
 
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/unit/globaldb/test_upgrades.py#L1707'>rotkehlchen/tests/unit/globaldb/test_upgrades.py~L1707</a>
```diff
                 cursor.execute(
                     f"SELECT COUNT(*) FROM evm_tokens WHERE protocol IN ({','.join(['?' for _ in protocols])})",  # noqa: E501
                     tuple(protocols),
-                ).fetchone()[0]
+                )
+                .fetchone()[0]
                 == expected_count
             )
 
         # Verify balancer cache migration: old keys gone, new keys exist with same count
         assert (
-            cursor.execute(
-                'SELECT COUNT(*) FROM general_cache WHERE key LIKE "BALANCER_GAUGES%"'
-            ).fetchone()[0]
+            cursor.execute('SELECT COUNT(*) FROM general_cache WHERE key LIKE "BALANCER_GAUGES%"')
+            .fetchone()[0]
             == 0
         )  # noqa: E501
         # `balancer_gauges_count - 1` because a malformed/unexpected balancer gauge cache entry got deleted  # noqa: E501
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/unit/globaldb/test_upgrades.py#L1723'>rotkehlchen/tests/unit/globaldb/test_upgrades.py~L1723</a>
```diff
             cursor.execute(
                 "SELECT COUNT(*) FROM general_cache WHERE key IN (?, ?, ?, ?, ?, ?, ?)",
                 list(balancer_cache_mapping.values()),
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == len(balancer_gauges) - 1
         )  # noqa: E501
         # Check that the coinbaseprime mappings have been combined with the coinbase mappings.
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/unit/globaldb/test_upgrades.py#L1752'>rotkehlchen/tests/unit/globaldb/test_upgrades.py~L1752</a>
```diff
                 cursor.execute(
                     "SELECT COUNT(*) FROM location_asset_mappings WHERE location=?",
                     (new_location,),
-                ).fetchone()[0]
+                )
+                .fetchone()[0]
                 == expected_new_count
             )
             assert (
                 cursor.execute(
                     "SELECT COUNT(*) FROM location_asset_mappings WHERE location=?",
                     (old_location,),
-                ).fetchone()[0]
+                )
+                .fetchone()[0]
                 == 0
             )
 
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/unit/globaldb/test_upgrades.py#L1831'>rotkehlchen/tests/unit/globaldb/test_upgrades.py~L1831</a>
```diff
         assert (
             cursor.execute(
                 "SELECT COUNT(*) FROM assets WHERE identifier = ?", ((new_solana_id := "SOL"),)
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == 0
         )  # noqa: E501
         assert (
             cursor.execute(
                 "SELECT COUNT(*) FROM common_asset_details WHERE identifier = ?", (new_solana_id,)
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == 0
         )  # noqa: E501
         assert cursor.execute(
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/unit/globaldb/test_upgrades.py#L1941'>rotkehlchen/tests/unit/globaldb/test_upgrades.py~L1941</a>
```diff
     assert globaldb.get_setting_value("version", 0) == 14
     with globaldb.conn.read_ctx() as cursor:
         assert (
-            cursor.execute(
-                "SELECT COUNT(*) FROM assets WHERE identifier = ?", (old_solana_id,)
-            ).fetchone()[0]
+            cursor.execute("SELECT COUNT(*) FROM assets WHERE identifier = ?", (old_solana_id,))
+            .fetchone()[0]
             == 0
         )  # noqa: E501
         assert (
             cursor.execute(
                 "SELECT COUNT(*) FROM common_asset_details WHERE identifier = ?", (old_solana_id,)
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == 0
         )  # noqa: E501
         assert cursor.execute(
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/unit/globaldb/test_upgrades.py#L1966'>rotkehlchen/tests/unit/globaldb/test_upgrades.py~L1966</a>
```diff
         assert (
             cursor.execute(
                 "SELECT COUNT(*) FROM location_asset_mappings WHERE local_id = ?", (old_solana_id,)
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == 0
         )  # noqa: E501
         assert cursor.execute(
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/unit/globaldb/test_upgrades.py#L1980'>rotkehlchen/tests/unit/globaldb/test_upgrades.py~L1980</a>
```diff
         assert (
             cursor.execute(
                 "SELECT COUNT(*) FROM multiasset_mappings WHERE asset = ?", (old_solana_id,)
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == 0
         )  # noqa: E501
         assert cursor.execute(
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/unit/globaldb/test_upgrades.py#L1992'>rotkehlchen/tests/unit/globaldb/test_upgrades.py~L1992</a>
```diff
         assert (
             cursor.execute(
                 "SELECT COUNT(*) FROM asset_collections WHERE main_asset = ?", (old_solana_id,)
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == 0
         )  # noqa: E501
         assert cursor.execute(
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/unit/globaldb/test_upgrades.py#L2004'>rotkehlchen/tests/unit/globaldb/test_upgrades.py~L2004</a>
```diff
         assert (
             cursor.execute(
                 "SELECT COUNT(*) FROM user_owned_assets WHERE asset_id = ?", (old_solana_id,)
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == 0
         )  # noqa: E501
         assert (
             cursor.execute(
                 "SELECT COUNT(*) FROM user_owned_assets WHERE asset_id = ?", (new_solana_id,)
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == 1
         )  # noqa: E501
         assert (
             cursor.execute(
                 "SELECT COUNT(*) FROM price_history WHERE from_asset = ? OR to_asset = ?",
                 (old_solana_id, old_solana_id),
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == 0
         )  # noqa: E501
         assert cursor.execute(
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/unit/globaldb/test_upgrades.py#L2031'>rotkehlchen/tests/unit/globaldb/test_upgrades.py~L2031</a>
```diff
             cursor.execute(
                 "SELECT COUNT(*) FROM binance_pairs WHERE base_asset = ? OR quote_asset = ?",
                 (old_solana_id, old_solana_id),
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == 0
         )  # noqa: E501
         assert cursor.execute(
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/unit/globaldb/test_upgrades.py#L2045'>rotkehlchen/tests/unit/globaldb/test_upgrades.py~L2045</a>
```diff
             cursor.execute(
                 "SELECT COUNT(*) FROM counterparty_asset_mappings WHERE local_id = ?",
                 (old_solana_id,),
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == 0
         )  # noqa: E501
         assert cursor.execute(
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/unit/globaldb/test_upgrades.py#L2117'>rotkehlchen/tests/unit/globaldb/test_upgrades.py~L2117</a>
```diff
             cursor.execute(
                 "SELECT COUNT(*) FROM assets WHERE identifier IN (?, ?, ?)",
                 (rocket_pool_asset, compound_usdt_asset, morpho_asset),
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == 0
         )  # noqa: E501
 
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/unit/globaldb/test_upgrades.py#L2143'>rotkehlchen/tests/unit/globaldb/test_upgrades.py~L2143</a>
```diff
             cursor.execute(
                 "SELECT COUNT(*) FROM assets WHERE identifier IN (?, ?, ?)",
                 (rocket_pool_asset, compound_usdt_asset, morpho_asset),
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == 3
         )  # noqa: E501
 
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/unit/test_data_updates.py#L769'>rotkehlchen/tests/unit/test_data_updates.py~L769</a>
```diff
                     Location.deserialize(deletion["location"]).serialize_for_db(),
                     deletion["location_symbol"],
                 ),  # noqa: E501
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == 0
         )
         assert not_exists == after_upgrade
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/unit/test_data_updates.py#L828'>rotkehlchen/tests/unit/test_data_updates.py~L828</a>
```diff
             cursor.execute(
                 "SELECT COUNT(*) FROM counterparty_asset_mappings WHERE counterparty IS ? AND symbol IS ?",  # noqa: E501
                 (deletion["counterparty"], deletion["counterparty_symbol"]),
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == 0
         )
         assert not_exists == after_upgrade
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/unit/test_ethereum_airdrops.py#L389'>rotkehlchen/tests/unit/test_ethereum_airdrops.py~L389</a>
```diff
             cursor.execute(
                 "SELECT COUNT(*) FROM unique_cache WHERE key LIKE ?",
                 ("AIRDROPS_HASH%",),
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == 0
         )
 
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/unit/test_ethereum_airdrops.py#L416'>rotkehlchen/tests/unit/test_ethereum_airdrops.py~L416</a>
```diff
                 cursor.execute(
                     "SELECT COUNT(*) FROM assets WHERE identifier=?",
                     (new_asset_identifier,),
-                ).fetchone()[0]
+                )
+                .fetchone()[0]
                 == 0
             )  # asset not present before
         data = check_airdrops(
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/unit/test_ethereum_airdrops.py#L438'>rotkehlchen/tests/unit/test_ethereum_airdrops.py~L438</a>
```diff
             cursor.execute(
                 "SELECT COUNT(*) FROM unique_cache WHERE key LIKE ?",
                 ("AIRDROPS_HASH%",),
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == 13
         )
         assert (
             cursor.execute(
                 "SELECT value FROM unique_cache WHERE key=?",
                 ("AIRDROPS_HASHdiva.parquet",),
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == MOCK_AIRDROP_INDEX["airdrops"]["diva"]["file_hash"]
         )
 
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/unit/test_ethereum_airdrops.py#L588'>rotkehlchen/tests/unit/test_ethereum_airdrops.py~L588</a>
```diff
             cursor.execute(
                 "SELECT value FROM unique_cache WHERE key=?",
                 ("AIRDROPS_HASHdiva.parquet",),
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == "updated_hash"
         )
 
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/unit/test_tasks_manager.py#L1171'>rotkehlchen/tests/unit/test_tasks_manager.py~L1171</a>
```diff
                 write_cursor.execute(
                     "SELECT COUNT(*) FROM evm_tokens WHERE protocol = ?;",
                     (counterparty,),
-                ).fetchone()[0]
+                )
+                .fetchone()[0]
                 == 0
             )
 
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/unit/test_tasks_manager.py#L1197'>rotkehlchen/tests/unit/test_tasks_manager.py~L1197</a>
```diff
                 write_cursor.execute(
                     "SELECT COUNT(*) FROM evm_tokens WHERE protocol = ?;",
                     (counterparty,),
-                ).fetchone()[0]
+                )
+                .fetchone()[0]
                 > 0
             )
             assert (
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/unit/test_tasks_manager.py#L1205'>rotkehlchen/tests/unit/test_tasks_manager.py~L1205</a>
```diff
                     "SELECT COUNT(*) FROM underlying_tokens_list WHERE parent_token_entry IN "
                     "(SELECT identifier FROM evm_tokens WHERE protocol = ?);",
                     (counterparty,),
-                ).fetchone()[0]
+                )
+                .fetchone()[0]
                 > 0
             )
 
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/unit/test_tasks_manager.py#L1354'>rotkehlchen/tests/unit/test_tasks_manager.py~L1354</a>
```diff
         assert (
             cursor.execute(
                 "SELECT COUNT(*) FROM calendar_reminders WHERE event_id=2",
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             == 1
         )
 
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/utils/database.py#L236'>rotkehlchen/tests/utils/database.py~L236</a>
```diff
         cursor.execute(
             "SELECT COUNT(*) FROM sqlite_master WHERE type='index' AND name=?",
             (name,),
-        ).fetchone()[0]
+        )
+        .fetchone()[0]
         == 1
     )
     if exists and schema is not None:
```
<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/tests/utils/globaldb.py#L164'>rotkehlchen/tests/utils/globaldb.py~L164</a>
```diff
             cursor.execute(
                 "SELECT COUNT(*) FROM location_unsupported_assets WHERE location=? AND exchange_symbol=?",  # noqa: E501
                 (location.serialize_for_db(), asset_symbol),
-            ).fetchone()[0]
+            )
+            .fetchone()[0]
             > 0
         )
```

</p>
</details>
<details><summary><a href="https://github.com/indico/indico">indico/indico</a> (+26 -19 lines across 11 files)</summary>
<p>

<a href='https://github.com/indico/indico/blob/ec8a0095f00fa24bdbddd02a95fac4076d238596/indico/modules/events/management/controllers/base.py#L60'>indico/modules/events/management/controllers/base.py~L60</a>
```diff
         contribution_persons.extend(
             SubContributionPersonLink.query.filter(
                 SubContributionPersonLink.subcontribution.has(SubContribution.contribution.has(self._membership_filter))
-            ).all()
+            )
+            .all()
         )
 
         registered_persons = get_registered_event_persons(self.event)
```
<a href='https://github.com/indico/indico/blob/ec8a0095f00fa24bdbddd02a95fac4076d238596/indico/modules/events/models/events.py#L1067'>indico/modules/events/models/events.py~L1067</a>
```diff
     def session_block_count(self):
         from indico.modules.events.sessions.models.blocks import SessionBlock
 
-        return SessionBlock.query.filter(
-            SessionBlock.session.has(event=self, is_deleted=False), SessionBlock.timetable_entry != None
-        ).count()  # noqa: E711
+        return (
+            SessionBlock.query.filter(
+                SessionBlock.session.has(event=self, is_deleted=False), SessionBlock.timetable_entry != None
+            )  # noqa: E711
+            .count()
+        )
 
     def __repr__(self):
         return format_repr(
```
<a href='https://github.com/indico/indico/blob/ec8a0095f00fa24bdbddd02a95fac4076d238596/indico/modules/events/reminders/notifications_test.py#L141'>indico/modules/events/reminders/notifications_test.py~L141</a>
```diff
             <tr><td>Kitten</td><td>OVER 9000</td></tr>
             <tr><td>Molerat</td><td>WTF</td></tr>
         </table>
-    """).strip()
+    """)
+        .strip()
     )
     html_tpl, text_tpl = get_reminder_email_tpl(event, ReminderType.standard, RenderMode.html, False, False, None, note)
     snapshot.snapshot_dir = Path(__file__).parent / 'templates/emails/tests'
```
<a href='https://github.com/indico/indico/blob/ec8a0095f00fa24bdbddd02a95fac4076d238596/indico/modules/events/util.py#L756'>indico/modules/events/util.py~L756</a>
```diff
 def get_all_user_roles(event, user):
     event_roles = set(EventRole.query.with_parent(event).filter(EventRole.members.any(User.id == user.id)))
     category_roles = set(
-        CategoryRole.query.join(event.category.chain_query.subquery()).filter(
-            CategoryRole.members.any(User.id == user.id)
-        )
+        CategoryRole.query.join(event.category.chain_query.subquery())
+        .filter(CategoryRole.members.any(User.id == user.id))
     )
     return event_roles, category_roles
 
```
<a href='https://github.com/indico/indico/blob/ec8a0095f00fa24bdbddd02a95fac4076d238596/indico/modules/groups/core.py#L266'>indico/modules/groups/core.py~L266</a>
```diff
         if not identifiers and not emails:
             return set()
         return set(
-            User.query.outerjoin(Identity).filter(
+            User.query.outerjoin(Identity)
+            .filter(
                 ~User.is_deleted,
                 db.or_(
                     User.all_emails.in_(list(emails)),
```
<a href='https://github.com/indico/indico/blob/ec8a0095f00fa24bdbddd02a95fac4076d238596/indico/modules/rb/controllers/backend/bookings.py#L579'>indico/modules/rb/controllers/backend/bookings.py~L579</a>
```diff
     )
     def _process(self, room_ids, start_date, end_date, format):
         occurrences = (
-            ReservationOccurrence.query.join(ReservationOccurrence.reservation).filter(
+            ReservationOccurrence.query.join(ReservationOccurrence.reservation)
+            .filter(
                 Reservation.room_id.in_(room_ids),
                 ReservationOccurrence.is_valid,
                 db_dates_overlap(
```
<a href='https://github.com/indico/indico/blob/ec8a0095f00fa24bdbddd02a95fac4076d238596/indico/modules/rb/models/reservation_occurrences_test.py#L307'>indico/modules/rb/models/reservation_occurrences_test.py~L307</a>
```diff
         db_occ
         not in ReservationOccurrence.find_overlapping_with(
             room=db_occ.reservation.room, occurrences=[occ], skip_reservation_id=db_occ.reservation.id
-        ).all()
+        )
+        .all()
     )
 
 
```
<a href='https://github.com/indico/indico/blob/ec8a0095f00fa24bdbddd02a95fac4076d238596/indico/modules/rb/models/reservations.py#L297'>indico/modules/rb/models/reservations.py~L297</a>
```diff
         if occurs_on:
             query = query.filter(
                 Reservation.id.in_(
-                    db.session.query(ReservationOccurrence.reservation_id).filter(
-                        ReservationOccurrence.date.in_(occurs_on), ReservationOccurrence.is_valid
-                    )
+                    db.session.query(ReservationOccurrence.reservation_id)
+                    .filter(ReservationOccurrence.date.in_(occurs_on), ReservationOccurrence.is_valid)
                 )
             )
         if limit_per_room and (limit or offset):
```
<a href='https://github.com/indico/indico/blob/ec8a0095f00fa24bdbddd02a95fac4076d238596/indico/modules/rb/models/reservations_test.py#L165'>indico/modules/rb/models/reservations_test.py~L165</a>
```diff
         reservation
         not in Reservation.find_overlapping_with(
             room=reservation.room, occurrences=[occurrence], skip_reservation_id=reservation.id
-        ).all()
+        )
+        .all()
     )
 
 
```
<a href='https://github.com/indico/indico/blob/ec8a0095f00fa24bdbddd02a95fac4076d238596/indico/modules/users/controllers.py#L838'>indico/modules/users/controllers.py~L838</a>
```diff
 
     def _process(self):
         admins = set(
-            User.query.filter_by(is_admin=True, is_deleted=False).order_by(
-                db.func.lower(User.first_name), db.func.lower(User.last_name)
-            )
+            User.query.filter_by(is_admin=True, is_deleted=False)
+            .order_by(db.func.lower(User.first_name), db.func.lower(User.last_name))
         )
 
         form = AdminsForm(admins=admins)
```
<a href='https://github.com/indico/indico/blob/ec8a0095f00fa24bdbddd02a95fac4076d238596/indico/modules/users/util.py#L105'>indico/modules/users/util.py~L105</a>
```diff
                 )
             ),
             ~Category.is_deleted,
-        ).options(undefer('chain_titles'))
+        )
+        .options(undefer('chain_titles'))
     )
     if not detailed:
         return favorites | managed
```

</p>
</details>
<details><summary><a href="https://github.com/zanieb/huggingface-notebooks">zanieb/huggingface-notebooks</a> (+29 -19 lines across 4 files)</summary>
<p>

<a href='https://github.com/zanieb/huggingface-notebooks/blob/68cd6fa1a2831c5189f85257c13d691cb76292db/diffusers/sd_textual_inversion_training.ipynb#L930'>diffusers/sd_textual_inversion_training.ipynb~L930</a>
```diff
     "\n",
     "    # Initialize the optimizer\n",
     "    optimizer = torch.optim.AdamW(\n",
-    "        text_encoder.get_input_embeddings().parameters(),  # only optimize the embeddings\n",
+    "        text_encoder.get_input_embeddings()\n",
+    "        .parameters(),  # only optimize the embeddings\n",
     "        lr=learning_rate,\n",
     "    )\n",
     "\n",
```
<a href='https://github.com/zanieb/huggingface-notebooks/blob/68cd6fa1a2831c5189f85257c13d691cb76292db/diffusers_doc/en/custom_pipeline_examples.ipynb#L182'>diffusers_doc/en/custom_pipeline_examples.ipynb~L182</a>
```diff
     "from diffusers import DiffusionPipeline\n",
     "import torch\n",
     "\n",
-    "pipe = DiffusionPipeline.from_pretrained(\n",
-    "    \"CompVis/stable-diffusion-v1-4\",\n",
-    "    torch_dtype=torch.float16,\n",
-    "    safety_checker=None,  # Very important for videos...lots of false positives while interpolating\n",
-    "    custom_pipeline=\"interpolate_stable_diffusion\",\n",
-    ").to(\"cuda\")\n",
+    "pipe = (\n",
+    "    DiffusionPipeline.from_pretrained(\n",
+    "        \"CompVis/stable-diffusion-v1-4\",\n",
+    "        torch_dtype=torch.float16,\n",
+    "        safety_checker=None,  # Very important for videos...lots of false positives while interpolating\n",
+    "        custom_pipeline=\"interpolate_stable_diffusion\",\n",
+    "    )\n",
+    "    .to(\"cuda\")\n",
+    ")\n",
     "pipe.enable_attention_slicing()\n",
     "\n",
     "frame_filepaths = pipe.walk(\n",
```
<a href='https://github.com/zanieb/huggingface-notebooks/blob/68cd6fa1a2831c5189f85257c13d691cb76292db/diffusers_doc/en/pytorch/custom_pipeline_examples.ipynb#L182'>diffusers_doc/en/pytorch/custom_pipeline_examples.ipynb~L182</a>
```diff
     "from diffusers import DiffusionPipeline\n",
     "import torch\n",
     "\n",
-    "pipe = DiffusionPipeline.from_pretrained(\n",
-    "    \"CompVis/stable-diffusion-v1-4\",\n",
-    "    torch_dtype=torch.float16,\n",
-    "    safety_checker=None,  # Very important for videos...lots of false positives while interpolating\n",
-    "    custom_pipeline=\"interpolate_stable_diffusion\",\n",
-    ").to(\"cuda\")\n",
+    "pipe = (\n",
+    "    DiffusionPipeline.from_pretrained(\n",
+    "        \"CompVis/stable-diffusion-v1-4\",\n",
+    "        torch_dtype=torch.float16,\n",
+    "        safety_checker=None,  # Very important for videos...lots of false positives while interpolating\n",
+    "        custom_pipeline=\"interpolate_stable_diffusion\",\n",
+    "    )\n",
+    "    .to(\"cuda\")\n",
+    ")\n",
     "pipe.enable_attention_slicing()\n",
     "\n",
     "frame_filepaths = pipe.walk(\n",
```
<a href='https://github.com/zanieb/huggingface-notebooks/blob/68cd6fa1a2831c5189f85257c13d691cb76292db/diffusers_doc/en/tensorflow/custom_pipeline_examples.ipynb#L182'>diffusers_doc/en/tensorflow/custom_pipeline_examples.ipynb~L182</a>
```diff
     "from diffusers import DiffusionPipeline\n",
     "import torch\n",
     "\n",
-    "pipe = DiffusionPipeline.from_pretrained(\n",
-    "    \"CompVis/stable-diffusion-v1-4\",\n",
-    "    torch_dtype=torch.float16,\n",
-    "    safety_checker=None,  # Very important for videos...lots of false positives while interpolating\n",
-    "    custom_pipeline=\"interpolate_stable_diffusion\",\n",
-    ").to(\"cuda\")\n",
+    "pipe = (\n",
+    "    DiffusionPipeline.from_pretrained(\n",
+    "        \"CompVis/stable-diffusion-v1-4\",\n",
+    "        torch_dtype=torch.float16,\n",
+    "        safety_checker=None,  # Very important for videos...lots of false positives while interpolating\n",
+    "        custom_pipeline=\"interpolate_stable_diffusion\",\n",
+    "    )\n",
+    "    .to(\"cuda\")\n",
+    ")\n",
     "pipe.enable_attention_slicing()\n",
     "\n",
     "frame_filepaths = pipe.walk(\n",
```

</p>
</details>
<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (+2 -1 lines across 1 file)</summary>
<p>
<pre>ruff format --no-preview --exclude examples/mcp/databricks_mcp_cookbook.ipynb,examples/chatgpt/gpt_actions_library/gpt_action_google_drive.ipynb,examples/chatgpt/gpt_actions_library/gpt_action_redshift.ipynb,examples/chatgpt/gpt_actions_library/gpt_action_salesforce.ipynb,</pre>
</p>
<p>

<a href='https://github.com/openai/openai-cookbook/blob/134d8a16f1c6b8a58351506e11cb96ada880f6bc/examples/evaluation/use-cases/structured-outputs-evaluation.ipynb#L451'>examples/evaluation/use-cases/structured-outputs-evaluation.ipynb~L451</a>
```diff
     "                    f.write(\n",
     "                        client.evals.runs.output_items.list(\n",
     "                            run_id=run.id, eval_id=eval_id\n",
-    "                        ).model_dump_json(indent=4)\n",
+    "                        )\n",
+    "                        .model_dump_json(indent=4)\n",
     "                    )\n",
     "            break\n",
     "        time.sleep(5)\n",
```

</p>
</details>
<details><summary><a href="https://github.com/mesonbuild/meson-python">mesonbuild/meson-python</a> (+4 -2 lines across 2 files)</summary>
<p>
<pre>ruff format --no-preview --exclude tests/packages/symlinks/baz.py,tests/packages/symlinks/qux.py</pre>
</p>
<p>

<a href='https://github.com/mesonbuild/meson-python/blob/cfcfb3554800d42c859d04309da2c1973c9f97ce/mesonpy/__init__.py#L570'>mesonpy/__init__.py~L570</a>
```diff
                        {os.fspath(build_dir)!r},
                        {build_command!r},
                        {verbose!r},
-                   )""").encode('utf-8'),
+                   )""")
+                .encode('utf-8'),
             )
 
             # install .pth file
```
<a href='https://github.com/mesonbuild/meson-python/blob/cfcfb3554800d42c859d04309da2c1973c9f97ce/tests/test_wheel.py#L264'>tests/test_wheel.py~L264</a>
```diff
 
             [gui_scripts]
             example-gui = example:gui
-        """).strip()
+        """)
+            .strip()
         )
 
 
```

</p>
</details>


---

_Comment by @dylwil3 on 2025-12-05 21:41_

On the one hand, maybe the change in criterion brings in some things I'd rather it didn't. On the other hand, the current criterion seems very counterintuitive to me - why do we exclude the parent call if there is one?

I mean another possibility is to set the cutoff at 3 instead of 2, which would have the opposite effect of making fluent layout rarer.

I'm torn and open to feedback.

---

_Comment by @dylwil3 on 2025-12-05 21:43_

Also sorry @laundmo I decided, at least now, against this kind of thing:

```python
x = (
    some
    .very.nested.object.returns()
    .another.nested.object.and_another_thing()
    .and_something_else()
)
```

It feels like it's too different from Prettier, which is more battle-tested for this kind of formatting. I also tend to think of method chains as most often being of the form "perform a sequence of transformations on an object of interest". So I like to have `some.very.nested.object` on the first line since it's the "object of interest", as opposed to the namespace `some`.

---

_Marked ready for review by @dylwil3 on 2025-12-05 21:46_

---

_Comment by @dylwil3 on 2025-12-05 21:46_

Opening back up for one last review - of design, not implementation - before polishing. This is also the last time I spam your inboxes today, hooray!

---

_Comment by @MichaReiser on 2025-12-06 13:02_

I only looked at main vs final proposal because that seems what you want feedback on. 

Cases where I think the formatting looks worse:


<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/nlu/classifiers/diet_classifier.py#L356'>rasa/nlu/classifiers/diet_classifier.py~L356</a>
```diff
             # check that all hidden layer sizes are the same
             identical_hidden_layer_sizes = all(
                 current_hidden_layer_sizes == first_hidden_layer_sizes
-                for current_hidden_layer_sizes in self.component_config[
-                    HIDDEN_LAYERS_SIZES
-                ].values()
+                for current_hidden_layer_sizes in self
+                .component_config[HIDDEN_LAYERS_SIZES]
+                .values()
             )
             if not identical_hidden_layer_sizes:
                 raise ValueError(
```

This is where indenting the method chain would help. It's also surprisingly common. I think method-chaining makes this slightly worse than it is with call-chaining, but it is a pre-existing issue.

The same for 

<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/shared/core/domain.py#L1178'>rasa/shared/core/domain.py~L1178</a>
```diff
         # tuple because sub_state will be later transformed into a frozenset (so it can
         # be hashed for deduplication).
         entities = tuple(
-            self._get_featurized_entities(latest_message).intersection(
-                set(sub_state.get(ENTITIES, ()))
-            )
+            self
+            ._get_featurized_entities(latest_message)
+            .intersection(set(sub_state.get(ENTITIES, ())))
         )
         # Sort entities so that any derived state representation is consistent across
         # runs and invariant to the order in which the entities for an utterance are
```

Usage in named expressions is very common, and I think I prefer the old formatting. Again, this is a pre-existing issue and the solution is improved nested expression formatting but the new formatting makes this more obvious

<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/db/addressbook.py#L260'>rotkehlchen/db/addressbook.py~L260</a>
```diff
         with self.read_ctx(book_type) as cursor:
             if (
                 entry_count := len(
-                    names := cursor.execute(
+                    names := cursor
+                    .execute(
                         "SELECT name FROM address_book WHERE address=?",
                         (address,),
-                    ).fetchall()
+                    )
+                    .fetchall()
                 )
             ) == 0 or len(set(names)) > 1:
                 return  # If no name, or different names on different chains, leave as is.
```

Another instance

<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/db/unresolved_conflicts.py#L98'>rotkehlchen/db/unresolved_conflicts.py~L98</a>
```diff
                     "taxable": entry[4],
                     "count_entire_amount_spend": entry[5],
                     "count_cost_basis_pnl": entry[6],
-                    "accounting_treatment": TxAccountingTreatment.deserialize_from_db(
-                        entry[7]
-                    ).serialize()
+                    "accounting_treatment": TxAccountingTreatment
+                    .deserialize_from_db(entry[7])
+                    .serialize()
                     if entry[7] is not None
                     else None,  # noqa: E501
                 }
```


It looks a bit funny on literals but I don't think it's worth special casing:

<a href='https://github.com/docker/docker-py/blob/df3f8e2abc5a03de482e37214dddef9e0cee1bb1/tests/integration/api_build_test.py#L19'>tests/integration/api_build_test.py~L19</a>
```diff
         )
 
         script = io.BytesIO(
-            "\n".join(
+            "\n"
+            .join(
                 [
                     "FROM busybox",
                     'RUN env | grep "FTP_PROXY=a"',
```
<a href='https://github.com/docker/docker-py/blob/df3f8e2abc5a03de482e37214dddef9e0cee1bb1/tests/integration/api_build_test.py#L31'>tests/integration/api_build_test.py~L31</a>
```diff
                     'RUN env | grep "NO_PROXY=d"',
                     'RUN env | grep "no_proxy=d"',
                 ]
-            ).encode("ascii")
+            )
+            .encode("ascii")
         )
 
         self.client.build(fileobj=script, decode=True)
```

Keeping intermediate attribute expressions on a single line instead of putting them on their own line helps reduce the diff size:

```
<a href='https://github.com/reflex-dev/reflex/blob/e4300adbbec62630cb3ce5c0610400788557e9b9/reflex/event.py#L1106'>reflex/event.py~L1106</a>
```diff
     get_element_by_id = FunctionStringVar.create("document.getElementById")
 
     return run_script(
-        get_element_by_id.call(elem_id)
+        get_element_by_id
+        .call(elem_id)
         .to(ObjectVar)
         .scrollIntoView.to(FunctionVar)
         .call(align_to_top),
```


This diff is surprising:

<a href='https://github.com/rotki/rotki/blob/060a0009bc337350a2f82395897c0224ea7998c9/rotkehlchen/chain/ethereum/oracles/uniswap.py#L88'>rotkehlchen/chain/ethereum/oracles/uniswap.py~L88</a>
```diff
                         self.routing_assets[chain_id].append(
                             get_or_create_evm_token(
                                 userdb=(
-                                    node_inquirer := Inquirer()
-                                    .get_evm_manager(chain_id)
-                                    .node_inquirer
+                                    node_inquirer := Inquirer().get_evm_manager(
+                                        chain_id
+                                    ).node_inquirer
                                 ).database,  # noqa: E501
                                 evm_address=string_to_evm_address(asset.identifier.split(":")[-1]),
                                 chain_id=chain_id,
```


I overall like the changes. My only concern is that it makes the lack for good nested expression formatting more obvious but I don't think this should block landing this in preview (whether we should stabilize the change this year is a different question)


---

_Converted to draft by @dylwil3 on 2025-12-06 17:38_

---

_Comment by @alexreinking on 2025-12-06 19:10_

Yeah, that last diff is pretty gory... I would have much preferred to read

```diff
                         self.routing_assets[chain_id].append(
                             get_or_create_evm_token(
                                 userdb=(
-                                    node_inquirer := Inquirer()
-                                    .get_evm_manager(chain_id)
-                                    .node_inquirer
+                                    node_inquirer := (
+                                        Inquirer()
+                                        .get_evm_manager(chain_id)
+                                        .node_inquirer
+                                    )
                                 ).database,  # noqa: E501
                                 evm_address=string_to_evm_address(asset.identifier.split(":")[-1]),
                                 chain_id=chain_id,
```

But I gather this is a manifestation of #12856? I find it very hard to read call chains where the arguments to function calls or indexing expressions have been chopped down, but the chain components haven't.

---

_Comment by @dylwil3 on 2025-12-06 20:20_

> This diff is surprising:

Hmmm, this is a case where somehow my new logic for when to use fluent layout _didn't_ fire but the old one did. Thanks for catching that!

I thought my criterion was strictly weaker but maybe I did something wrong with whether we count the first call, now? I will look into it.

---

_Comment by @dylwil3 on 2025-12-08 17:09_

I found the stable logic for determining when we should use fluent formatting difficult to follow. So before making any of the preview changes in this PR, I am prepending the following patch. Below the patch I record the argument for why this is a pure refactor and has equivalent behavior to `main`.

This is sort of pedantic but I'm including it because the argument is somewhat subtle - the value of `attributes_after_parentheses` (defined below) is not the same after the refactor, but the result of the function nevertheless is.

(I have included a larger-than-necessary patch so that you can see the whole function body).

<details><summary>Unfold for the patch...</summary>

```diff
diff --git a/crates/ruff_python_formatter/src/expression/mod.rs b/crates/ruff_python_formatter/src/expression/mod.rs
index a320a1edf5..f314856cd0 100644
--- a/crates/ruff_python_formatter/src/expression/mod.rs
+++ b/crates/ruff_python_formatter/src/expression/mod.rs
@@ -895,6 +895,8 @@ impl CallChainLayout {
     pub(crate) fn from_expression(
         mut expr: ExprRef,
         comment_ranges: &CommentRanges,
         source: &str,
     ) -> Self {
+        let mut first_attr_value_parenthesized = false;
+
         let mut attributes_after_parentheses = 0;
         loop {
             match expr {
                 ExprRef::Attribute(ast::ExprAttribute { value, .. }) => {
                     // ```
                     // f().g
                     // ^^^ value
                     // data[:100].T
                     // ^^^^^^^^^^ value
                     // ```
                     if is_expression_parenthesized(value.into(), comment_ranges, source) {
                         // `(a).b`. We preserve these parentheses so don't recurse
-                        attributes_after_parentheses += 1;
+                        first_attr_value_parenthesized = true;
                         break;
                     } else if matches!(value.as_ref(), Expr::Call(_) | Expr::Subscript(_)) {
                         attributes_after_parentheses += 1;
                     }
 
                     expr = ExprRef::from(value.as_ref());
                 }
                 // ```
                 // f()
                 // ^^^ expr
                 // ^ func
                 // data[:100]
                 // ^^^^^^^^^^ expr
                 // ^^^^ value
                 // ```
                 ExprRef::Call(ast::ExprCall { func: inner, .. })
                 | ExprRef::Subscript(ast::ExprSubscript { value: inner, .. }) => {
+                    // We preserve these parentheses so don't recurse
+                    // e.g. (a)[0].x().y().z()
+                    //         ^stop here
+                    if is_expression_parenthesized(inner.into(), comment_ranges, source) {
+                        break;
+                    }
+
                     expr = ExprRef::from(inner.as_ref());
                 }
                 _ => {
-                    // We to format the following in fluent style:
-                    // ```
-                    // f2 = (a).w().t(1,)
-                    //       ^ expr
-                    // ```
-                    if is_expression_parenthesized(expr, comment_ranges, source) {
-                        attributes_after_parentheses += 1;
-                    }
-
                     break;
                 }
             }
-
-            // We preserve these parentheses so don't recurse
-            if is_expression_parenthesized(expr, comment_ranges, source) {
-                break;
-            }
         }
-        if attributes_after_parentheses < 2 {
+
+        if attributes_after_parentheses + u32::from(first_attr_value_parenthesized) < 2 {
             CallChainLayout::NonFluent
         } else {
             CallChainLayout::Fluent
         }
     }
```
</details>

<details><summary>Proof that this is a pure refactor</summary>

For brevity, let's pretend that this is just a function of the expression. We will denote the version on stable by `stable(expr)` and the version after the patch as `refactored(expr)`. We claim that `stable(expr) == refactored(expr)` in all cases. 

First let's observe that the following preliminary refactor is okay:

```diff
diff --git i/crates/ruff_python_formatter/src/expression/mod.rs w/crates/ruff_python_formatter/src/expression/mod.rs
index a320a1edf5..bfd7ac41c0 100644
--- i/crates/ruff_python_formatter/src/expression/mod.rs
+++ w/crates/ruff_python_formatter/src/expression/mod.rs
@@ -895,6 +895,7 @@ impl CallChainLayout {
         comment_ranges: &CommentRanges,
         source: &str,
     ) -> Self {
+        let mut first_attr_value_parenthesized = false;
         let mut attributes_after_parentheses = 0;
         loop {
             match expr {
@@ -907,7 +908,7 @@ impl CallChainLayout {
                     // ```
                     if is_expression_parenthesized(value.into(), comment_ranges, source) {
                         // `(a).b`. We preserve these parentheses so don't recurse
-                        attributes_after_parentheses += 1;
+                        first_attr_value_parenthesized = true;
                         break;
                     } else if matches!(value.as_ref(), Expr::Call(_) | Expr::Subscript(_)) {
                         attributes_after_parentheses += 1;
@@ -946,7 +947,7 @@ impl CallChainLayout {
                 break;
             }
         }
-        if attributes_after_parentheses < 2 {
+        if attributes_after_parentheses + u32::from(first_attr_value_parenthesized) < 2 {
             CallChainLayout::NonFluent
         } else {
             CallChainLayout::Fluent
```

Indeed: we break immediately after this accumulation, so we add that `1` if and only if `first_attr_value_parenthesized` is true at the end.

Next, consider the clause _after_ the match arm in `stable` that's given by:

```rust
            // We preserve these parentheses so don't recurse
            if is_expression_parenthesized(expr, comment_ranges, source) {
                break;
            }
```

I claim that this is only reachable from the `Call | Subscript` match arm. Indeed: the last match arm breaks at the end, so we must have come from one of the first two arms. But if we came from the first arm, then the `expr` we are evaluating in that clause is actually the `value` in an expression of the form `value.attr`. So our `expr` is parenthesized if and only if `value` is parenthesized. But then observe that we would have _already_ broken the loop at this line:

```rust
                     if is_expression_parenthesized(value.into(), comment_ranges, source) {
                         // `(a).b`. We preserve these parentheses so don't recurse
                         attributes_after_parentheses += 1;
                         break;
                     }
```

It is therefore a pure refactor to move the clause 

```rust
            // We preserve these parentheses so don't recurse
            if is_expression_parenthesized(expr, comment_ranges, source) {
                break;
            }
```

inside the match arm for `Call | Subscript`. 

It remains to show that this deletion is a pure refactor _given the previous refactors_:

```diff
                 _ => {
-                    // We to format the following in fluent style:
-                    // ```
-                    // f2 = (a).w().t(1,)
-                    //       ^ expr
-                    // ```
-                    if is_expression_parenthesized(expr, comment_ranges, source) {
-                        attributes_after_parentheses += 1;
-                    }
-
                     break;
                 }
             }
```

First suppose that `expr` is not an attribute, not a call, and not a subscript. Then the implementation for `stable` hits the last match arm, adds at most `1` to `attributes_after_parentheses`, and then breaks. So `stable(expr)` must be `NonFluent`. On the other hand, `refactored` immediately breaks with `attributes_after_parentheses == 0` and we also get `NonFluent` as the result. (Note that the clause is not actually _unreachable_ here, it's just that it doesn't change the outcome!)

In the remaining cases, we could only be hitting this last match arm _after_ having looped once. Thus the `expr` being checked for parentheses must be obtained as `inner` or `value` from the previous iteration (with notation as in the body of the function). But then, if `inner`/`value` has parentheses, we would have _already_ broken the loop by the end of the previous iteration. Thus, in these remaining cases, this clause is actually unreachable and can be deleted.
</details>


---

_Marked ready for review by @dylwil3 on 2025-12-08 19:35_

---

_Comment by @dylwil3 on 2025-12-08 19:37_

Ready for the actual implementation to be reviewed now. I also cleaned up the commit history so you should be able to review commit by commit.

---

_Review comment by @dylwil3 on `crates/ruff_python_formatter/src/expression/mod.rs`:1041 on 2025-12-08 19:38_

This inefficient break-down of the conditions is just so that it'll be very easy to edit the function when it comes time to stabilize.

---

_@dylwil3 reviewed on 2025-12-08 19:40_

---

_@dylwil3 reviewed on 2025-12-08 19:46_

---

_Review comment by @dylwil3 on `docs/formatter.md`:568 on 2025-12-08 19:46_

This is probably too much detail for user-facing docs? Maybe I can just add this to the PR summary and link to it here instead?

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/tests/snapshots/format@parentheses__call_chains.py.snap`:620 on 2025-12-08 21:20_

We should add dedicated tests for this new feature that also capture some of the tricky cases we've seen in the ecosystem results (e.g. the one regression I caught). We should also add tests for different comment placements (are there cases where the new layout should be skipped if comments are present?)

and we should also test *Why do we apply fluent formatting in a few more examples?* 

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/preview.rs`:68 on 2025-12-08 21:24_

I don't think we need two preview toggles unless you see a situation where we only stabilize one but not the other? 

However, given the large ecosystem change, I'd find it useful to split `is_fluent_layout_more_often_enabled` into its own PR so we can review the ecosystem impact separately from method chain formatting.

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/mod.rs`:924 on 2025-12-08 21:25_

Can we document what `u32` stores?

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/expr_attribute.rs`:202 on 2025-12-09 08:54_

I think i would change `from_expression` to take `context` to reduce the number of arguments. We almost always have access to context, and, if not, it's normally very easy to get access to `context`.

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/mod.rs`:894 on 2025-12-09 09:37_

Very likely a non native speaker issue but summand confused me, given that we aren't adding anything in the example right above this comment. I think we should either repeat the entire example or reduce the comment to only mention that this is the formatted output but expand the comment with what the tracked position is.

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/mod.rs`:934 on 2025-12-09 09:37_

Nit: From reading the call sites, I didn't expect this function to compute any new state.

Maybe: add_call_like_attribute(self). It might also be worth adding a #[must_use]. Same for after_attribute



---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/mod.rs`:955 on 2025-12-09 09:38_

Nit

```suggestion
                if let Some(x) = x.checked_sub(1) {
                    Self::Fluent(AttributeState::CallsOrSubscriptsPreceding(x))
                } else {
                    Self::Fluent(AttributeState::FirstCallOrSubscript)
                }
```

I don't think I understand why we subtract here. if so, I'd expected we add one. Would you mind adding some documentation explaining the logic here

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/mod.rs`:997 on 2025-12-09 09:44_

Why aren't we adding incrementing `call_like_count` in this branch? It might help to give `was_in_call_like` a more specific name or document its purpose when declaring the variable.

Is there even an instance where we don't increment `call_like_count`? To me, it seems we always increment it in the next iteration so that we could move the increment here instead of tracking `was_in_call_like` because the value only becomes `true` in this branch, we reset it in all other branches (by setting it to `false` or `break`ing), and setting `expr` to `inner` guarantees that we always do one additional round around the loop.


The only exception to this seems to be if `inner` is a `Subscript` or `Call` itself, in which case we don't increment. But the logic is easier to follow if we inline this check in this branch and add a comment of why we don't increment in that case.

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/mod.rs`:977 on 2025-12-09 09:50_

I wonder if we could move more logic into the `Call` / `Subscript` branch if we instead track `after_attribute` as a boolean, saying whether we're inspecting an attribute's value (which we always reset except if it is an attribute expression)

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/mod.rs`:1033 on 2025-12-09 09:55_

I'd find it useful to have some examples of when we're hitting those branches. What does `call_like_count + u32::from(first_attr_value_parenthesized) < 2)` mean?

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/mod.rs`:1041 on 2025-12-09 09:57_

I think a less awkward way of writing this would be:

```rust
let something_something_count = if is_fluent...() { call_like_count } else { attributes_after_parentheses };

if something_something_count + u32::from(first_attr_value_parenthesized) < 2 {
      CallChainLayout::NonFluent
  } else {
	CallChainLayout::Fluent(AttributeState::CallsOrSubscriptsPreceding(call_like_count))
  }
```

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/tests/snapshots/format@expression__split_empty_brackets.py.snap`:214 on 2025-12-09 09:59_

This is an instance where the new formatting looks worse.

---

_Review comment by @MichaReiser on `docs/formatter.md`:568 on 2025-12-09 10:00_

Agree. This might be a better fit for `formatter/black.md` where we document deviations

---

_@MichaReiser requested changes on 2025-12-09 10:09_

This is great. Indeed, the change is surprisingly simple. I'm left with some concern that the new formatting makes the lack for better nested expression formatting, specifically in binary expressions, more prevalant, making overall formatting look worse in some instances. Hopefully, we'll get some preview feedback to make up our minds. I think it would also be great to run the new version against pyx and open a PR in that repo to get some dogfooding feedback.

What I miss in this PR and why I'm requesting changes are dedicated tests that document the design decisions (e.g. that we split off `np.`), test edge cases, and regressions that we found in the ecosystem reports. E.g. how parenthesized expressions are handled, how we split subsequent attribute chains after the first call expression etc.


> The second modification fell out of the implementation, but it appears to fix some counterintuitive behavior present on main. If there is some justification for that choice, I can change it back. We could also make it a separate PR. It has to do with when we decide to use fluent formatting in the first place. A specific example would be this, again with line-length=8:

Did you mean to write `x = a().b().c()` because your example requires method-chaining, which isn't enabled on main?

I suggest splitting this change into its own PR because I find it impossible to trace any ecosystem changes back to this particular change, making it difficult for me to judge whether it is an improvement. We should also add dedicated tests for this change, to prevent future regressions (or having to run the ecosystem check to catch the regressions). 



---

_Comment by @astral-sh-bot[bot] on 2025-12-12 15:29_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
ℹ️ ecosystem check **detected format changes**. (+2948 -1526 lines in 441 files in 27 projects; 28 projects unchanged)

<details><summary><a href="https://github.com/DisnakeDev/disnake">DisnakeDev/disnake</a> (+2 -1 lines across 1 file)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/DisnakeDev/disnake/blob/394227ce57cc5245784c2125f5689cdc68a675c1/disnake/http.py#L1924'>disnake/http.py~L1924</a>
```diff
 
     def widget_image_url(self, guild_id: Snowflake, *, style: str) -> str:
         return str(
-            yarl.URL(Route.BASE)
+            yarl
+            .URL(Route.BASE)
             .with_path(f"/api/guilds/{guild_id}/widget.png")
             .with_query(style=style)
         )
```

</p>
</details>
<details><summary><a href="https://github.com/RasaHQ/rasa">RasaHQ/rasa</a> (+36 -18 lines across 7 files)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/cli/scaffold.py#L54'>rasa/cli/scaffold.py~L54</a>
```diff
     print_success("Finished creating project structure.")
 
     should_train = (
-        questionary.confirm("Do you want to train an initial model? 💪🏽")
+        questionary
+        .confirm("Do you want to train an initial model? 💪🏽")
         .skip_if(args.no_prompt, default=True)
         .ask()
     )
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/cli/scaffold.py#L83'>rasa/cli/scaffold.py~L83</a>
```diff
     import questionary
 
     should_run = (
-        questionary.confirm(
+        questionary
+        .confirm(
             "Do you want to speak to the trained assistant on the command line? 🤖"
         )
         .skip_if(args.no_prompt, default=False)
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/cli/scaffold.py#L206'>rasa/cli/scaffold.py~L206</a>
```diff
         path = args.init_dir
     else:
         path = (
-            questionary.text(
+            questionary
+            .text(
                 "Please enter a path where the project will be "
                 "created [default: current directory]"
             )
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/core/test.py#L500'>rasa/core/test.py~L500</a>
```diff
     # the response selector contains a "default" key
     if RESPONSE_SELECTOR_DEFAULT_INTENT in response_selector:
         full_retrieval_intent = (
-            response_selector.get(RESPONSE_SELECTOR_DEFAULT_INTENT, {})
+            response_selector
+            .get(RESPONSE_SELECTOR_DEFAULT_INTENT, {})
             .get(RESPONSE, {})
             .get(INTENT_RESPONSE_KEY)
         )
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/core/test.py#L508'>rasa/core/test.py~L508</a>
```diff
 
     # if specified, the response selector contains the base intent as key
     full_retrieval_intent = (
-        response_selector.get(base_intent, {})
+        response_selector
+        .get(base_intent, {})
         .get(RESPONSE, {})
         .get(INTENT_RESPONSE_KEY)
     )
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/core/tracker_store.py#L1013'>rasa/core/tracker_store.py~L1013</a>
```diff
 
     if is_postgresql_url(engine.url):
         query = sa.exists(
-            sa.select([(sa.text("schema_name"))])
+            sa
+            .select([(sa.text("schema_name"))])
             .select_from(sa.text("information_schema.schemata"))
             .where(sa.text(f"schema_name = '{schema_name}'"))
         )
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/core/tracker_store.py#L1199'>rasa/core/tracker_store.py~L1199</a>
```diff
         conn = engine.connect()
 
         matching_rows = (
-            conn.execution_options(isolation_level="AUTOCOMMIT")
+            conn
+            .execution_options(isolation_level="AUTOCOMMIT")
             .execute(
                 sa.text(
                     "SELECT 1 FROM pg_catalog.pg_database "
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/core/tracker_store.py#L1298'>rasa/core/tracker_store.py~L1298</a>
```diff
         """
         # Subquery to find the timestamp of the latest `SessionStarted` event
         session_start_sub_query = (
-            session.query(sa.func.max(self.SQLEvent.timestamp).label("session_start"))
+            session
+            .query(sa.func.max(self.SQLEvent.timestamp).label("session_start"))
             .filter(
                 self.SQLEvent.sender_id == sender_id,
                 self.SQLEvent.type_name == SessionStarted.type_name,
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/shared/utils/io.py#L347'>rasa/shared/utils/io.py~L347</a>
```diff
     if _is_ascii(content):
         # Required to make sure emojis are correctly parsed
         content = (
-            content.encode("utf-8")
+            content
+            .encode("utf-8")
             .decode("raw_unicode_escape")
             .encode("utf-16", "surrogatepass")
             .decode("utf-16")
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/integration_tests/core/test_tracker_store.py#L46'>tests/integration_tests/core/test_tracker_store.py~L46</a>
```diff
     )
 
     matching_rows = (
-        postgres_login_db_connection.execution_options(isolation_level="AUTOCOMMIT")
+        postgres_login_db_connection
+        .execution_options(isolation_level="AUTOCOMMIT")
         .execute(
             sa.text(
                 "SELECT 1 FROM pg_catalog.pg_database WHERE datname = :database_name"
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/integration_tests/core/test_tracker_store.py#L80'>tests/integration_tests/core/test_tracker_store.py~L80</a>
```diff
     )
 
     matching_rows = (
-        postgres_login_db_connection.execution_options(isolation_level="AUTOCOMMIT")
+        postgres_login_db_connection
+        .execution_options(isolation_level="AUTOCOMMIT")
         .execute(
             sa.text(
                 "SELECT 1 FROM pg_catalog.pg_database WHERE datname = :database_name"
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/integration_tests/core/test_tracker_store.py#L132'>tests/integration_tests/core/test_tracker_store.py~L132</a>
```diff
         for record in caplog.records
     ])
     matching_rows = (
-        postgres_login_db_connection.execution_options(isolation_level="AUTOCOMMIT")
+        postgres_login_db_connection
+        .execution_options(isolation_level="AUTOCOMMIT")
         .execute(
             sa.text(
                 "SELECT 1 FROM pg_catalog.pg_database WHERE datname = :database_name"
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/nlu/classifiers/test_diet_classifier.py#L879'>tests/nlu/classifiers/test_diet_classifier.py~L879</a>
```diff
                             # expected to change can be compared directly
                             assert (
                                 feature_signature.units
-                                == old_signature.get(attribute)
+                                == old_signature
+                                .get(attribute)
                                 .get(feature_type)[index]
                                 .units
                             )
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/nlu/selectors/test_selectors.py#L196'>tests/nlu/selectors/test_selectors.py~L196</a>
```diff
         classified_message.get("response_selector").get("all_retrieval_intents")
     ) == ["chitchat"]
     assert (
-        classified_message.get("response_selector")
+        classified_message
+        .get("response_selector")
         .get("default")
         .get("response")
         .get("intent_response_key")
     ) is not None
     assert (
-        classified_message.get("response_selector")
+        classified_message
+        .get("response_selector")
         .get("default")
         .get("response")
         .get("utter_action")
     ) is not None
     assert (
-        classified_message.get("response_selector")
+        classified_message
+        .get("response_selector")
         .get("default")
         .get("response")
         .get("responses")
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/nlu/selectors/test_selectors.py#L638'>tests/nlu/selectors/test_selectors.py~L638</a>
```diff
     unfeaturized_message = Message(data={TEXT: message_text})
     classified_message = response_selector.process([unfeaturized_message])[0]
     output = (
-        classified_message.get(RESPONSE_SELECTOR_PROPERTY_NAME)
+        classified_message
+        .get(RESPONSE_SELECTOR_PROPERTY_NAME)
         .get(RESPONSE_SELECTOR_DEFAULT_INTENT)
         .get(RESPONSE_SELECTOR_PREDICTION_KEY)
     )
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/nlu/selectors/test_selectors.py#L797'>tests/nlu/selectors/test_selectors.py~L797</a>
```diff
                             # expected to change can be compared directly
                             assert (
                                 feature_signature.units
-                                == old_signature.get(attribute)
+                                == old_signature
+                                .get(attribute)
                                 .get(feature_type)[index]
                                 .units
                             )
```

</p>
</details>
<details><summary><a href="https://github.com/Snowflake-Labs/snowcli">Snowflake-Labs/snowcli</a> (+12 -6 lines across 2 files)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/Snowflake-Labs/snowcli/blob/e552f2d07ba4d499e59092608e17e785f2a612a9/tests/app/test_telemetry.py#L52'>tests/app/test_telemetry.py~L52</a>
```diff
     assert result.exit_code == 0, result.output
     # The method is called with a TelemetryData type, so we cast it to dict for simpler comparison
     usage_command_event = (
-        mock_conn.return_value._telemetry.try_add_log_to_batch.call_args_list[  # noqa: SLF001
+        mock_conn.return_value._telemetry.try_add_log_to_batch
+        .call_args_list[  # noqa: SLF001
             0
         ]
         .args[0]
```
<a href='https://github.com/Snowflake-Labs/snowcli/blob/e552f2d07ba4d499e59092608e17e785f2a612a9/tests/app/test_telemetry.py#L109'>tests/app/test_telemetry.py~L109</a>
```diff
     assert result.exit_code == 0, result.output
     # The method is called with a TelemetryData type, so we cast it to dict for simpler comparison
     usage_command_event = (
-        mock_conn.return_value._telemetry.try_add_log_to_batch.call_args_list[  # noqa: SLF001
+        mock_conn.return_value._telemetry.try_add_log_to_batch
+        .call_args_list[  # noqa: SLF001
             0
         ]
         .args[0]
```
<a href='https://github.com/Snowflake-Labs/snowcli/blob/e552f2d07ba4d499e59092608e17e785f2a612a9/tests/app/test_telemetry.py#L140'>tests/app/test_telemetry.py~L140</a>
```diff
 
     # The method is called with a TelemetryData type, so we cast it to dict for simpler comparison
     result_command_event = (
-        mock_conn.return_value._telemetry.try_add_log_to_batch.call_args_list[  # noqa: SLF001
+        mock_conn.return_value._telemetry.try_add_log_to_batch
+        .call_args_list[  # noqa: SLF001
             1
         ]
         .args[0]
```
<a href='https://github.com/Snowflake-Labs/snowcli/blob/e552f2d07ba4d499e59092608e17e785f2a612a9/tests/app/test_telemetry.py#L181'>tests/app/test_telemetry.py~L181</a>
```diff
 
     # The method is called with a TelemetryData type, so we cast it to dict for simpler comparison
     result_command_event = (
-        mock_conn.return_value._telemetry.try_add_log_to_batch.call_args_list[  # noqa: SLF001
+        mock_conn.return_value._telemetry.try_add_log_to_batch
+        .call_args_list[  # noqa: SLF001
             1
         ]
         .args[0]
```
<a href='https://github.com/Snowflake-Labs/snowcli/blob/e552f2d07ba4d499e59092608e17e785f2a612a9/tests/app/test_telemetry.py#L233'>tests/app/test_telemetry.py~L233</a>
```diff
     assert result.exit_code == 0, result.output
 
     usage_command_event = (
-        mock_connect.return_value._telemetry.try_add_log_to_batch.call_args_list[  # noqa: SLF001
+        mock_connect.return_value._telemetry.try_add_log_to_batch
+        .call_args_list[  # noqa: SLF001
             0
         ]
         .args[0]
```
<a href='https://github.com/Snowflake-Labs/snowcli/blob/e552f2d07ba4d499e59092608e17e785f2a612a9/tests_e2e/conftest.py#L44'>tests_e2e/conftest.py~L44</a>
```diff
         return None
     text = "\n".join(line.rstrip() for line in text.splitlines())
     return (
-        text.replace("│", "|")
+        text
+        .replace("│", "|")
         .replace("─", "-")
         .replace("╭", "+")
         .replace("╰", "+")
```

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+236 -120 lines across 28 files)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/apache/airflow/blob/e1adf0b1eba8fb5e00046cf49f15da3de59ff705/airflow-core/src/airflow/api_fastapi/core_api/routes/public/xcom.py#L157'>airflow-core/src/airflow/api_fastapi/core_api/routes/public/xcom.py~L157</a>
```diff
     if dag_id != "~":
         query = query.where(XComModel.dag_id == dag_id)
     query = (
-        query.join(DR, and_(XComModel.dag_id == DR.dag_id, XComModel.run_id == DR.run_id))
+        query
+        .join(DR, and_(XComModel.dag_id == DR.dag_id, XComModel.run_id == DR.run_id))
         .join(DagModel, DR.dag_id == DagModel.dag_id)
         .options(joinedload(XComModel.task), joinedload(XComModel.dag_run).joinedload(DR.dag_model))
     )
```
<a href='https://github.com/apache/airflow/blob/e1adf0b1eba8fb5e00046cf49f15da3de59ff705/airflow-core/src/airflow/api_fastapi/core_api/routes/ui/dags.py#L168'>airflow-core/src/airflow/api_fastapi/core_api/routes/ui/dags.py~L168</a>
```diff
         select(
             DagRun.dag_id,
             DagRun.run_after,
-            func.rank()
+            func
+            .rank()
             .over(
                 partition_by=DagRun.dag_id,
                 order_by=DagRun.run_after.desc(),
```
<a href='https://github.com/apache/airflow/blob/e1adf0b1eba8fb5e00046cf49f15da3de59ff705/airflow-core/src/airflow/api_fastapi/core_api/services/public/task_instances.py#L431'>airflow-core/src/airflow/api_fastapi/core_api/services/public/task_instances.py~L431</a>
```diff
 
                 for dag_id, run_id, task_id, map_index in matched_task_keys:
                     ti = (
-                        self.session.execute(
+                        self.session
+                        .execute(
                             select(TI).where(
                                 TI.dag_id == dag_id,
                                 TI.run_id == run_id,
```
<a href='https://github.com/apache/airflow/blob/e1adf0b1eba8fb5e00046cf49f15da3de59ff705/airflow-core/src/airflow/jobs/scheduler_job_runner.py#L1761'>airflow-core/src/airflow/jobs/scheduler_job_runner.py~L1761</a>
```diff
         # This list is used to verify if the DagRun already exist so that we don't attempt to create
         # duplicate DagRuns
         existing_dagruns = (
-            session.execute(
+            session
+            .execute(
                 select(DagRun.dag_id, DagRun.logical_date).where(
                     tuple_(DagRun.dag_id, DagRun.logical_date).in_(
                         (dm.dag_id, dm.next_dagrun) for dm in dag_models
```
<a href='https://github.com/apache/airflow/blob/e1adf0b1eba8fb5e00046cf49f15da3de59ff705/airflow-core/src/airflow/jobs/scheduler_job_runner.py#L2443'>airflow-core/src/airflow/jobs/scheduler_job_runner.py~L2443</a>
```diff
     def _emit_ti_metrics(self, session: Session = NEW_SESSION) -> None:
         metric_states = {State.SCHEDULED, State.QUEUED, State.RUNNING, State.DEFERRED}
         all_states_metric = (
-            session.query(
+            session
+            .query(
                 TaskInstance.state,
                 TaskInstance.dag_id,
                 TaskInstance.task_id,
```
<a href='https://github.com/apache/airflow/blob/e1adf0b1eba8fb5e00046cf49f15da3de59ff705/airflow-core/src/airflow/jobs/triggerer_job_runner.py#L605'>airflow-core/src/airflow/jobs/triggerer_job_runner.py~L605</a>
```diff
         render_log_fname = log_filename_template_renderer()
 
         known_trigger_ids = (
-            self.running_triggers.union(x[0] for x in self.events)
+            self.running_triggers
+            .union(x[0] for x in self.events)
             .union(self.cancelling_triggers)
             .union(trigger[0] for trigger in self.failed_triggers)
             .union(trigger.id for trigger in self.creating_triggers)
```
<a href='https://github.com/apache/airflow/blob/e1adf0b1eba8fb5e00046cf49f15da3de59ff705/airflow-core/src/airflow/migrations/versions/0092_3_2_0_replace_deadline_inline_callback_with_fkey.py#L105'>airflow-core/src/airflow/migrations/versions/0092_3_2_0_replace_deadline_inline_callback_with_fkey.py~L105</a>
```diff
 
         conn.execute(callback_table.insert(), callback_inserts)
         conn.execute(
-            deadline_table.update()
+            deadline_table
+            .update()
             .where(deadline_table.c.id == sa.bindparam("deadline_id"))
             .values(callback_id=sa.bindparam("callback_id"), missed=sa.bindparam("missed")),
             deadline_updates,
```
<a href='https://github.com/apache/airflow/blob/e1adf0b1eba8fb5e00046cf49f15da3de59ff705/airflow-core/src/airflow/migrations/versions/0092_3_2_0_replace_deadline_inline_callback_with_fkey.py#L244'>airflow-core/src/airflow/migrations/versions/0092_3_2_0_replace_deadline_inline_callback_with_fkey.py~L244</a>
```diff
                 raise
 
         conn.execute(
-            deadline_table.update()
+            deadline_table
+            .update()
             .where(deadline_table.c.id == sa.bindparam("deadline_id"))
             .values(
                 callback=sa.bindparam("callback"),
```
<a href='https://github.com/apache/airflow/blob/e1adf0b1eba8fb5e00046cf49f15da3de59ff705/airflow-core/tests/unit/api_fastapi/core_api/routes/public/test_task_instances.py#L830'>airflow-core/tests/unit/api_fastapi/core_api/routes/public/test_task_instances.py~L830</a>
```diff
         self, test_client, session, params, expected_map_indexes, one_task_with_many_mapped_tis
     ):
         ti = (
-            session.query(TaskInstance)
+            session
+            .query(TaskInstance)
             .where(TaskInstance.task_id == "task_2", TaskInstance.map_index == 0)
             .first()
         )
```
<a href='https://github.com/apache/airflow/blob/e1adf0b1eba8fb5e00046cf49f15da3de59ff705/airflow-core/tests/unit/api_fastapi/core_api/routes/public/test_task_instances.py#L3461'>airflow-core/tests/unit/api_fastapi/core_api/routes/public/test_task_instances.py~L3461</a>
```diff
         )
 
         logs = (
-            session.query(Log)
+            session
+            .query(Log)
             .filter(Log.dag_id == dag_id, Log.run_id == dag_run_id, Log.event == event)
             .count()
         )
```
<a href='https://github.com/apache/airflow/blob/e1adf0b1eba8fb5e00046cf49f15da3de59ff705/airflow-core/tests/unit/dag_processing/test_collection.py#L658'>airflow-core/tests/unit/dag_processing/test_collection.py~L658</a>
```diff
         )
 
         import_error = (
-            session.query(ParseImportError)
+            session
+            .query(ParseImportError)
             .filter(ParseImportError.filename == filename, ParseImportError.bundle_name == bundle_name)
             .one()
         )
```
<a href='https://github.com/apache/airflow/blob/e1adf0b1eba8fb5e00046cf49f15da3de59ff705/airflow-core/tests/unit/dag_processing/test_manager.py#L482'>airflow-core/tests/unit/dag_processing/test_manager.py~L482</a>
```diff
         manager._file_stats[test_dag_path] = stat
 
         active_dag_count = (
-            session.query(func.count(DagModel.dag_id))
+            session
+            .query(func.count(DagModel.dag_id))
             .filter(
                 ~DagModel.is_stale,
                 DagModel.relative_fileloc == str(test_dag_path.rel_path),
```
<a href='https://github.com/apache/airflow/blob/e1adf0b1eba8fb5e00046cf49f15da3de59ff705/airflow-core/tests/unit/dag_processing/test_manager.py#L495'>airflow-core/tests/unit/dag_processing/test_manager.py~L495</a>
```diff
         manager._scan_stale_dags()
 
         active_dag_count = (
-            session.query(func.count(DagModel.dag_id))
+            session
+            .query(func.count(DagModel.dag_id))
             .filter(
                 ~DagModel.is_stale,
                 DagModel.relative_fileloc == str(test_dag_path.rel_path),
```
<a href='https://github.com/apache/airflow/blob/e1adf0b1eba8fb5e00046cf49f15da3de59ff705/airflow-core/tests/unit/dag_processing/test_manager.py#L506'>airflow-core/tests/unit/dag_processing/test_manager.py~L506</a>
```diff
         assert active_dag_count == 0
 
         serialized_dag_count = (
-            session.query(func.count(SerializedDagModel.dag_id))
+            session
+            .query(func.count(SerializedDagModel.dag_id))
             .filter(SerializedDagModel.dag_id == dag.dag_id)
             .scalar()
         )
```
<a href='https://github.com/apache/airflow/blob/e1adf0b1eba8fb5e00046cf49f15da3de59ff705/airflow-core/tests/unit/jobs/test_scheduler_job.py#L1398'>airflow-core/tests/unit/jobs/test_scheduler_job.py~L1398</a>
```diff
             )
 
         assert (
-            session.query(TaskInstance)
+            session
+            .query(TaskInstance)
             .filter(TaskInstance.dag_id == dag_id, TaskInstance.state == State.SCHEDULED)
             .count()
             == 1
         )
         assert (
-            session.query(TaskInstance)
+            session
+            .query(TaskInstance)
             .filter(TaskInstance.dag_id == dag_id, TaskInstance.state == State.QUEUED)
             .count()
             == 1
```
<a href='https://github.com/apache/airflow/blob/e1adf0b1eba8fb5e00046cf49f15da3de59ff705/airflow-core/tests/unit/jobs/test_scheduler_job.py#L3159'>airflow-core/tests/unit/jobs/test_scheduler_job.py~L3159</a>
```diff
             self.job_runner._do_scheduling(session)
 
         callback = (
-            session.query(DbCallbackRequest)
+            session
+            .query(DbCallbackRequest)
             .order_by(DbCallbackRequest.id.desc())
             .first()
             .get_callback_request()
```
<a href='https://github.com/apache/airflow/blob/e1adf0b1eba8fb5e00046cf49f15da3de59ff705/airflow-core/tests/unit/jobs/test_scheduler_job.py#L3341'>airflow-core/tests/unit/jobs/test_scheduler_job.py~L3341</a>
```diff
 
         # Verify the task instance was created
         initial_tis = (
-            session.query(TaskInstance)
+            session
+            .query(TaskInstance)
             .filter(TaskInstance.dag_id == dag_id, TaskInstance.task_id == "dummy")
             .all()
         )
```
<a href='https://github.com/apache/airflow/blob/e1adf0b1eba8fb5e00046cf49f15da3de59ff705/airflow-core/tests/unit/jobs/test_scheduler_job.py#L3370'>airflow-core/tests/unit/jobs/test_scheduler_job.py~L3370</a>
```diff
 
         # Verify no new task instances were created for the removed task in the new dagrun
         new_tis = (
-            session.query(TaskInstance)
+            session
+            .query(TaskInstance)
             .filter(
                 TaskInstance.dag_id == dag_id,
                 TaskInstance.task_id == "dummy",
```
<a href='https://github.com/apache/airflow/blob/e1adf0b1eba8fb5e00046cf49f15da3de59ff705/airflow-core/tests/unit/jobs/test_scheduler_job.py#L3791'>airflow-core/tests/unit/jobs/test_scheduler_job.py~L3791</a>
```diff
         assert len(task_instances_list) == 2
 
         ti0 = (
-            session.query(TaskInstance)
+            session
+            .query(TaskInstance)
             .filter(TaskInstance.task_id == "test_scheduler_verify_priority_and_slots_t0")
             .first()
         )
         assert ti0.state == State.SCHEDULED
 
         ti1 = (
-            session.query(TaskInstance)
+            session
+            .query(TaskInstance)
             .filter(TaskInstance.task_id == "test_scheduler_verify_priority_and_slots_t1")
             .first()
         )
         assert ti1.state == State.QUEUED
 
         ti2 = (
-            session.query(TaskInstance)
+            session
+            .query(TaskInstance)
             .filter(TaskInstance.task_id == "test_scheduler_verify_priority_and_slots_t2")
             .first()
         )
```
<a href='https://github.com/apache/airflow/blob/e1adf0b1eba8fb5e00046cf49f15da3de59ff705/airflow-core/tests/unit/jobs/test_scheduler_job.py#L3843'>airflow-core/tests/unit/jobs/test_scheduler_job.py~L3843</a>
```diff
         session.flush()
 
         tis_count = (
-            session.query(func.count(TaskInstance.task_id))
+            session
+            .query(func.count(TaskInstance.task_id))
             .filter(
                 TaskInstance.dag_id == dr.dag_id,
                 TaskInstance.logical_date == dr.logical_date,
```
<a href='https://github.com/apache/airflow/blob/e1adf0b1eba8fb5e00046cf49f15da3de59ff705/airflow-core/tests/unit/jobs/test_scheduler_job.py#L3913'>airflow-core/tests/unit/jobs/test_scheduler_job.py~L3913</a>
```diff
 
         if SQLALCHEMY_V_1_4:
             tis_count = (
-                session.query(func.count(TaskInstance.task_id))
+                session
+                .query(func.count(TaskInstance.task_id))
                 .filter(
                     TaskInstance.dag_id == dr.dag_id,
                     TaskInstance.logical_date == dr.logical_date,
```
<a href='https://github.com/apache/airflow/blob/e1adf0b1eba8fb5e00046cf49f15da3de59ff705/airflow-core/tests/unit/jobs/test_scheduler_job.py#L4012'>airflow-core/tests/unit/jobs/test_scheduler_job.py~L4012</a>
```diff
         do_schedule()
         with create_session() as session:
             ti = (
-                session.query(TaskInstance)
+                session
+                .query(TaskInstance)
                 .filter(
                     TaskInstance.dag_id == "test_retry_still_in_executor",
                     TaskInstance.task_id == "test_retry_handling_op",
```
<a href='https://github.com/apache/airflow/blob/e1adf0b1eba8fb5e00046cf49f15da3de59ff705/airflow-core/tests/unit/jobs/test_scheduler_job.py#L5016'>airflow-core/tests/unit/jobs/test_scheduler_job.py~L5016</a>
```diff
 
         def complete_one_dagrun():
             ti = (
-                session.query(TaskInstance)
+                session
+                .query(TaskInstance)
                 .join(TaskInstance.dag_run)
                 .filter(TaskInstance.state != State.SUCCESS)
                 .order_by(DagRun.logical_date)
```
<a href='https://github.com/apache/airflow/blob/e1adf0b1eba8fb5e00046cf49f15da3de59ff705/airflow-core/tests/unit/jobs/test_scheduler_job.py#L5156'>airflow-core/tests/unit/jobs/test_scheduler_job.py~L5156</a>
```diff
         session.flush()
 
         dag1_running_count = (
-            session.query(func.count(DagRun.id))
+            session
+            .query(func.count(DagRun.id))
             .filter(DagRun.dag_id == "test_dag1", DagRun.state == State.RUNNING)
             .scalar()
         )
```
<a href='https://github.com/apache/airflow/blob/e1adf0b1eba8fb5e00046cf49f15da3de59ff705/airflow-core/tests/unit/jobs/test_scheduler_job.py#L5199'>airflow-core/tests/unit/jobs/test_scheduler_job.py~L5199</a>
```diff
         session.flush()
 
         dag1_running_count = (
-            session.query(func.count(DagRun.id))
+            session
+            .query(func.count(DagRun.id))
             .filter(DagRun.dag_id == "test_dag1", DagRun.state == State.RUNNING)
             .scalar()
         )
```
<a href='https://github.com/apache/airflow/blob/e1adf0b1eba8fb5e00046cf49f15da3de59ff705/airflow-core/tests/unit/jobs/test_scheduler_job.py#L5219'>airflow-core/tests/unit/jobs/test_scheduler_job.py~L5219</a>
```diff
             dag_run_conf={},
         )
         dag1_running_count = (
-            session.query(func.count(DagRun.id))
+            session
+            .query(func.count(DagRun.id))
             .filter(DagRun.dag_id == "test_dag1", DagRun.state == State.RUNNING)
             .scalar()
         )
```
<a href='https://github.com/apache/airflow/blob/e1adf0b1eba8fb5e00046cf49f15da3de59ff705/airflow-core/tests/unit/jobs/test_scheduler_job.py#L5233'>airflow-core/tests/unit/jobs/test_scheduler_job.py~L5233</a>
```diff
         self.job_runner._start_queued_dagruns(session)
         session.flush()
         dag1_running_count = (
-            session.query(func.count(DagRun.id))
+            session
+            .query(func.count(DagRun.id))
             .filter(
                 DagRun.dag_id == dag1_dag_id,
                 DagRun.state == State.RUNNING,
```
<a href='https://github.com/apache/airflow/blob/e1adf0b1eba8fb5e00046cf49f15da3de59ff705/airflow-core/tests/unit/jobs/test_scheduler_job.py#L5250'>airflow-core/tests/unit/jobs/test_scheduler_job.py~L5250</a>
```diff
         self.job_runner._start_queued_dagruns(session)
         session.flush()
         dag1_running_count = (
-            session.query(func.count(DagRun.id))
+            session
+            .query(func.count(DagRun.id))
             .filter(
                 DagRun.dag_id == dag1_dag_id,
                 DagRun.state == State.RUNNING,
```
<a href='https://github.com/apache/airflow/blob/e1adf0b1eba8fb5e00046cf49f15da3de59ff705/airflow-core/tests/unit/jobs/test_scheduler_job.py#L5300'>airflow-core/tests/unit/jobs/test_scheduler_job.py~L5300</a>
```diff
         session.flush()
 
         dag1_running_count = (
-            session.query(func.count(DagRun.id))
+            session
+            .query(func.count(DagRun.id))
             .filter(DagRun.dag_id == "test_dag1", DagRun.state == State.RUNNING)
             .scalar()
         )
```
<a href='https://github.com/apache/airflow/blob/e1adf0b1eba8fb5e00046cf49f15da3de59ff705/airflow-core/tests/unit/jobs/test_scheduler_job.py#L5328'>airflow-core/tests/unit/jobs/test_scheduler_job.py~L5328</a>
```diff
         self.job_runner._start_queued_dagruns(session)
         session.flush()
         dag1_running_count = (
-            session.query(func.count(DagRun.id))
+            session
+            .query(func.count(DagRun.id))
             .filter(
                 DagRun.dag_id == dag1_dag_id,
                 DagRun.state == State.RUNNING,
```
<a href='https://github.com/apache/airflow/blob/e1adf0b1eba8fb5e00046cf49f15da3de59ff705/airflow-core/tests/unit/jobs/test_scheduler_job.py#L5363'>airflow-core/tests/unit/jobs/test_scheduler_job.py~L5363</a>
```diff
 
         def _running_counts():
             dag1_non_b_running = (
-                session.query(func.count(DagRun.id))
+                session
+                .query(func.count(DagRun.id))
                 .filter(
                     DagRun.dag_id == dag1_dag_id,
                     DagRun.state == State.RUNNING,
```
<a href='https://github.com/apache/airflow/blob/e1adf0b1eba8fb5e00046cf49f15da3de59ff705/airflow-core/tests/unit/jobs/test_scheduler_job.py#L5372'>airflow-core/tests/unit/jobs/test_scheduler_job.py~L5372</a>
```diff
                 .scalar()
             )
             dag1_b_running = (
-                session.query(func.count(DagRun.id))
+                session
+                .query(func.count(DagRun.id))
                 .filter(
                     DagRun.dag_id == dag1_dag_id,
                     DagRun.state == State.RUNNING,
```
<a href='https://github.com/apache/airflow/blob/e1adf0b1eba8fb5e00046cf49f15da3de59ff705/airflow-core/tests/unit/jobs/test_scheduler_job.py#L5479'>airflow-core/tests/unit/jobs/test_scheduler_job.py~L5479</a>
```diff
 
         def _running_counts():
             dag1_non_b_running = (
-                session.query(func.count(DagRun.id))
+                session
+                .query(func.count(DagRun.id))
                 .filter(
                     DagRun.dag_id == dag1_dag_id,
                     DagRun.state == State.RUNNING,
```
<a href='https://github.com/apache/airflow/blob/e1adf0b1eba8fb5e00046cf49f15da3de59ff705/airflow-core/tests/unit/jobs/test_scheduler_job.py#L5488'>airflow-core/tests/unit/jobs/test_scheduler_job.py~L5488</a>
```diff
                 .scalar()
             )
             dag1_b_running = (
-                session.query(func.count(DagRun.id))
+                session
+                .query(func.count(DagRun.id))
                 .filter(
                     DagRun.dag_id == dag1_dag_id,
                     DagRun.state == State.RUNNING,
```
<a href='https://github.com/apache/airflow/blob/e1adf0b1eba8fb5e00046cf49f15da3de59ff705/airflow-core/tests/unit/jobs/test_scheduler_job.py#L5586'>airflow-core/tests/unit/jobs/test_scheduler_job.py~L5586</a>
```diff
 
         def _running_counts():
             dag1_non_b_running = (
-                session.query(func.count(DagRun.id))
+                session
+                .query(func.count(DagRun.id))
                 .filter(
                     DagRun.dag_id == dag1_dag_id,
                     DagRun.state == State.RUNNING,
```
<a href='https://github.com/apache/airflow/blob/e1adf0b1eba8fb5e00046cf49f15da3de59ff705/airflow-core/tests/unit/jobs/test_scheduler_job.py#L5595'>airflow-core/tests/unit/jobs/test_scheduler_job.py~L5595</a>
```diff
                 .scalar()
             )
             dag1_b_running = (
-                session.query(func.count(DagRun.id))
+                session
+                .query(func.count(DagRun.id))
                 .filter(
                     DagRun.dag_id == dag1_dag_id,
                     DagRun.state == State.RUNNING,
```
<a href='https://github.com/apache/airflow/blob/e1adf0b1eba8fb5e00046cf49f15da3de59ff705/airflow-core/tests/unit/jobs/test_scheduler_job.py#L5735'>airflow-core/tests/unit/jobs/test_scheduler_job.py~L5735</a>
```diff
 
         def _running_counts():
             dag1_non_b_running = (
-                session.query(func.count(DagRun.id))
+                session
+                .query(func.count(DagRun.id))
                 .filter(
                     DagRun.dag_id == dag1_dag_id,
                     DagRun.state == State.RUNNING,
```
<a href='https://github.com/apache/airflow/blob/e1adf0b1eba8fb5e00046cf49f15da3de59ff705/airflow-core/tests/unit/jobs/test_scheduler_job.py#L5744'>airflow-core/tests/unit/jobs/test_scheduler_job.py~L5744</a>
```diff
                 .scalar()
             )
             dag1_b_running = (
-                session.query(func.count(DagRun.id))
+                session
+                .query(func.count(DagRun.id))
                 .filter(
                     DagRun.dag_id == dag1_dag_id,
                     DagRun.state == State.RUNNING,
```
<a href='https://github.com/apache/airflow/blob/e1adf0b1eba8fb5e00046cf49f15da3de59ff705/airflow-core/tests/unit/jobs/test_scheduler_job.py#L5826'>airflow-core/tests/unit/jobs/test_scheduler_job.py~L5826</a>
```diff
         )
 
         queued_count = (
-            session.query(func.count(DagRun.id))
+            session
+            .query(func.count(DagRun.id))
             .filter(
                 DagRun.dag_id == dag_id,
                 DagRun.state == State.QUEUED,
```
<a href='https://github.com/apache/airflow/blob/e1adf0b1eba8fb5e00046cf49f15da3de59ff705/airflow-core/tests/unit/jobs/test_scheduler_job.py#L5846'>airflow-core/tests/unit/jobs/test_scheduler_job.py~L5846</a>
```diff
 
         # No runs should be started because we couldn't acquire the lock
         running_count = (
-            session.query(func.count(DagRun.id))
+            session
+            .query(func.count(DagRun.id))
             .filter(
                 DagRun.dag_id == dag_id,
                 DagRun.state == State.RUNNING,
```
<a href='https://github.com/apache/airflow/blob/e1adf0b1eba8fb5e00046cf49f15da3de59ff705/airflow-core/tests/unit/jobs/test_scheduler_job.py#L5860'>airflow-core/tests/unit/jobs/test_scheduler_job.py~L5860</a>
```diff
         session.flush()
 
         running_count = (
-            session.query(func.count(DagRun.id))
+            session
+            .query(func.count(DagRun.id))
             .filter(
                 DagRun.dag_id == dag_id,
                 DagRun.state == State.RUNNING,
```
<a href='https://github.com/apache/airflow/blob/e1adf0b1eba8fb5e00046cf49f15da3de59ff705/airflow-core/tests/unit/jobs/test_scheduler_job.py#L5870'>airflow-core/tests/unit/jobs/test_scheduler_job.py~L5870</a>
```diff
         )
         assert running_count == backfill_max_active_runs
         queued_count = (
-            session.query(func.count(DagRun.id))
+            session
+            .query(func.count(DagRun.id))
             .filter(
                 DagRun.dag_id == dag_id,
                 DagRun.state == State.QUEUED,
```
<a href='https://github.com/apache/airflow/blob/e1adf0b1eba8fb5e00046cf49f15da3de59ff705/airflow-core/tests/unit/jobs/test_scheduler_job.py#L6208'>airflow-core/tests/unit/jobs/test_scheduler_job.py~L6208</a>
```diff
         self.job_runner._create_dag_runs([orm_dag], session)
 
         drs = (
-            session.query(DagRun)
+            session
+            .query(DagRun)
             .options(joinedload(DagRun.task_instances).joinedload(TaskInstance.dag_version))
             .all()
         )
```
<a href='https://github.com/apache/airflow/blob/e1adf0b1eba8fb5e00046cf49f15da3de59ff705/airflow-core/tests/unit/jobs/test_scheduler_job.py#L6732'>airflow-core/tests/unit/jobs/test_scheduler_job.py~L6732</a>
```diff
         # Check catchup worked correctly by ensuring logical_date is quite new
         # Our dag is a daily dag
         assert (
-            session.query(DagRun.logical_date)
+            session
+            .query(DagRun.logical_date)
             .filter(DagRun.logical_date != DEFAULT_DATE)  # exclude the first run
             .scalar()
         ) > (timezone.utcnow() - timedelta(days=2))
```
<a href='https://github.com/apache/airflow/blob/e1adf0b1eba8fb5e00046cf49f15da3de59ff705/airflow-core/tests/unit/jobs/test_scheduler_job.py#L7927'>airflow-core/tests/unit/jobs/test_scheduler_job.py~L7927</a>
```diff
 
         # Verify we have 1 running and 1 queued
         running_count = (
-            session.query(DagRun)
+            session
+            .query(DagRun)
             .filter(DagRun.dag_id == dag.dag_id, DagRun.state == DagRunState.RUNNING)
             .count()
         )
         queued_count = (
-            session.query(DagRun)
+            session
+            .query(DagRun)
             .filter(DagRun.dag_id == dag.dag_id, DagRun.state == DagRunState.QUEUED)
             .count()
         )
```
<a href='https://github.com/apache/airflow/blob/e1adf0b1eba8fb5e00046cf49f15da3de59ff705/airflow-core/tests/unit/jobs/test_scheduler_job.py#L7960'>airflow-core/tests/unit/jobs/test_scheduler_job.py~L7960</a>
```diff
 
         # Verify we now have 2 running dag runs
         running_count = (
-            session.query(DagRun)
+            session
+            .query(DagRun)
             .filter(DagRun.dag_id == dag.dag_id, DagRun.state == DagRunState.RUNNING)
             .count()
         )
```
<a href='https://github.com/apache/airflow/blob/e1adf0b1eba8fb5e00046cf49f15da3de59ff705/airflow-core/tests/unit/models/test_dag.py#L701'>airflow-core/tests/unit/models/test_dag.py~L701</a>
```diff
         assert [x.dag_id for x in asset1_orm.scheduled_dags] == [dag_id1]
         assert [(x.task_id, x.dag_id) for x in asset1_orm.producing_tasks] == [(task_id, dag_id2)]
         assert set(
-            session.query(
+            session
+            .query(
                 TaskOutletAssetReference.task_id,
                 TaskOutletAssetReference.dag_id,
                 TaskOutletAssetReference.asset_id,
```
<a href='https://github.com/apache/airflow/blob/e1adf0b1eba8fb5e00046cf49f15da3de59ff705/airflow-core/tests/unit/models/test_dag.py#L731'>airflow-core/tests/unit/models/test_dag.py~L731</a>
```diff
         asset2_orm = stored_assets[a2.uri]
         assert [x.dag_id for x in asset1_orm.scheduled_dags] == []
         assert set(
-            session.query(
+            session
+            .query(
                 TaskOutletAssetReference.task_id,
                 TaskOutletAssetReference.dag_id,
                 TaskOutletAssetReference.asset_id,
```
<a href='https://github.com/apache/airflow/blob/e1adf0b1eba8fb5e00046cf49f15da3de59ff705/airflow-core/tests/unit/models/test_dag.py#L2516'>airflow-core/tests/unit/models/test_dag.py~L2516</a>
```diff
 
     def get_ti_from_db(task):
         return (
-            session.query(TI)
+            session
+            .query(TI)
             .filter(
                 TI.dag_id == dag.dag_id,
                 TI.task_id == task.task_id,
```
<a href='https://github.com/apache/airflow/blob/e1adf0b1eba8fb5e00046cf49f15da3de59ff705/airflow-core/tests/unit/models/test_dag.py#L2600'>airflow-core/tests/unit/models/test_dag.py~L2600</a>
```diff
     session.query(TI).filter_by(dag_id=dag.dag_id).update({"state": TaskInstanceState.FAILED})
 
     ti_query = (
-        session.query(TI.task_id, TI.map_index, TI.run_id, TI.state)
+        session
+        .query(TI.task_id, TI.map_index, TI.run_id, TI.state)
         .filter(TI.dag_id == dag.dag_id, TI.task_id.in_([task_id, "downstream"]))
         .order_by(TI.run_id, TI.task_id, TI.map_index)
     )
```
<a href='https://github.com/apache/airflow/blob/e1adf0b1eba8fb5e00046cf49f15da3de59ff705/airflow-core/tests/unit/utils/test_log_handlers.py#L774'>airflow-core/tests/unit/utils/test_log_handlers.py~L774</a>
```diff
         TaskInstanceHistory.record_ti(ti, session=session)
         session.flush()
         tih = (
-            session.query(TaskInstanceHistory)
+            session
+            .query(TaskInstanceHistory)
             .filter_by(
                 dag_id=ti.dag_id,
                 task_id=ti.task_id,
```
<a href='https://github.com/apache/airflow/blob/e1adf0b1eba8fb5e00046cf49f15da3de59ff705/dev/breeze/src/airflow_breeze/utils/cdxgen.py#L426'>dev/breeze/src/airflow_breeze/utils/cdxgen.py~L426</a>
```diff
         )
 
         file_url = (
-            self.get_files_directory(self.application_root_path)
+            self
+            .get_files_directory(self.application_root_path)
             .relative_to(self.application_root_path)
             .as_posix()
         )
```
<a href='https://github.com/apache/airflow/blob/e1adf0b1eba8fb5e00046cf49f15da3de59ff705/dev/breeze/src/airflow_breeze/utils/constraints_version_check.py#L367'>dev/breeze/src/airflow_breeze/utils/constraints_version_check.py~L367</a>
```diff
     def get_release_dates(releases: dict, version: str) -> str:
         if releases.get(version):
             return (
-                datetime.fromisoformat(releases[version][0]["upload_time_iso_8601"].replace("Z", "+00:00"))
+                datetime
+                .fromisoformat(releases[version][0]["upload_time_iso_8601"].replace("Z", "+00:00"))
                 .replace(tzinfo=None)
                 .strftime("%Y-%m-%d")
             )
```
<a href='https://github.com/apache/airflow/blob/e1adf0b1eba8fb5e00046cf49f15da3de59ff705/devel-common/src/tests_common/test_utils/logs.py#L58'>devel-common/src/tests_common/test_utils/logs.py~L58</a>
```diff
 
 def check_last_log(session, dag_id, event, logical_date, expected_extra=None, check_masked=False):
     logs = (
-        session.query(
+        session
+        .query(
             Log.dag_id,
             Log.task_id,
             Log.event,
```
<a href='https://github.com/apache/airflow/blob/e1adf0b1eba8fb5e00046cf49f15da3de59ff705/providers/amazon/tests/system/amazon/aws/example_sagemaker_endpoint.py#L79'>providers/amazon/tests/system/amazon/aws/example_sagemaker_endpoint.py~L79</a>
```diff
 @task
 def call_endpoint(endpoint_name):
     response = (
-        boto3.Session()
+        boto3
+        .Session()
         .client("sagemaker-runtime")
         .invoke_endpoint(
             EndpointName=endpoint_name,
```
<a href='https://github.com/apache/airflow/blob/e1adf0b1eba8fb5e00046cf49f15da3de59ff705/providers/amazon/tests/unit/amazon/aws/hooks/test_base_aws.py#L517'>providers/amazon/tests/unit/amazon/aws/hooks/test_base_aws.py~L517</a>
```diff
             AwsBaseHook(aws_conn_id=aws_conn_id, client_type="s3").get_client_type()
             mocked_basic_session.assert_has_calls([
                 mock.call().client("sts", config=mock.ANY, endpoint_url=sts_endpoint),
-                mock.call()
+                mock
+                .call()
                 .client()
                 .assume_role(
                     RoleArn=role_arn,
```
<a href='https://github.com/apache/airflow/blob/e1adf0b1eba8fb5e00046cf49f15da3de59ff705/providers/amazon/tests/unit/amazon/aws/hooks/test_base_aws.py#L712'>providers/amazon/tests/unit/amazon/aws/hooks/test_base_aws.py~L712</a>
```diff
 
         mocked_basic_session.assert_has_calls = [
             mock.call().client("sts", config=mock.ANY, endpoint_url=sts_endpoint),
-            mock.call()
+            mock
+            .call()
             .client()
             .assume_role_with_saml(
                 DurationSeconds=duration_seconds,
```
<a href='https://github.com/apache/airflow/blob/e1adf0b1eba8fb5e00046cf49f15da3de59ff705/providers/edge3/src/airflow/providers/edge3/executors/edge_executor.py#L141'>providers/edge3/src/airflow/providers/edge3/executors/edge_executor.py~L141</a>
```diff
 
         # Check if job already exists with same dag_id, task_id, run_id, map_index, try_number
         existing_job = (
-            session.query(EdgeJobModel)
+            session
+            .query(EdgeJobModel)
             .filter_by(
                 dag_id=key.dag_id,
                 task_id=key.task_id,
```
<a href='https://github.com/apache/airflow/blob/e1adf0b1eba8fb5e00046cf49f15da3de59ff705/providers/edge3/src/airflow/providers/edge3/executors/edge_executor.py#L177'>providers/edge3/src/airflow/providers/edge3/executors/edge_executor.py~L177</a>
```diff
         changed = False
         heartbeat_interval: int = conf.getint("edge", "heartbeat_interval")
         lifeless_workers: list[EdgeWorkerModel] = (
-            session.query(EdgeWorkerModel)
+            session
+            .query(EdgeWorkerModel)
             .with_for_update(skip_locked=True)
             .filter(
                 EdgeWorkerModel.state.not_in([
```
<a href='https://github.com/apache/airflow/blob/e1adf0b1eba8fb5e00046cf49f15da3de59ff705/providers/edge3/src/airflow/providers/edge3/executors/edge_executor.py#L211'>providers/edge3/src/airflow/providers/edge3/executors/edge_executor.py~L211</a>
```diff
         """Update status ob jobs when workers die and don't update anymore."""
         heartbeat_interval: int = conf.getint("scheduler", "task_instance_heartbeat_timeout")
         lifeless_jobs: list[EdgeJobModel] = (
-            session.query(EdgeJobModel)
+            session
+            .query(EdgeJobModel)
             .with_for_update(skip_locked=True)
             .filter(
                 EdgeJobModel.state == TaskInstanceState.RUNNING,
```
<a href='https://github.com/apache/airflow/blob/e1adf0b1eba8fb5e00046cf49f15da3de59ff705/providers/edge3/src/airflow/providers/edge3/executors/edge_executor.py#L253'>providers/edge3/src/airflow/providers/edge3/executors/edge_executor.py~L253</a>
```diff
         job_success_purge = conf.getint("edge", "job_success_purge")
         job_fail_purge = conf.getint("edge", "job_fail_purge")
         jobs: list[EdgeJobModel] = (
-            session.query(EdgeJobModel)
+            session
+            .query(EdgeJobModel)
             .with_for_update(skip_locked=True)
             .filter(
                 EdgeJobModel.state.in_([
```
<a href='https://github.com/apache/airflow/blob/e1adf0b1eba8fb5e00046cf49f15da3de59ff705/providers/elasticsearch/src/airflow/providers/elasticsearch/log/es_task_handler.py#L100'>providers/elasticsearch/src/airflow/providers/elasticsearch/log/es_task_handler.py~L100</a>
```diff
     if not isinstance(ti, TaskInstanceKey):
         return ti
     val = (
-        session.query(TaskInstance)
+        session
+        .query(TaskInstance)
         .filter(
             TaskInstance.task_id == ti.task_id,
             TaskInstance.dag_id == ti.dag_id,
```
<a href='https://github.com/apache/airflow/blob/e1adf0b1eba8fb5e00046cf49f15da3de59ff705/providers/fab/src/airflow/providers/fab/auth_manager/security_manager/override.py#L1343'>providers/fab/src/airflow/providers/fab/auth_manager/security_manager/override.py~L1343</a>
```diff
 
     def get_public_role(self):
         return (
-            self.session.scalars(select(self.role_model).filter_by(name=self.auth_role_public))
+            self.session
+            .scalars(select(self.role_model).filter_by(name=self.auth_role_public))
             .unique()
             .one_or_none()
      

... (truncated 16493 lines) ...



---

_@dylwil3 reviewed on 2025-12-12 15:51_

---

_Review comment by @dylwil3 on `crates/ruff_python_formatter/src/expression/mod.rs`:894 on 2025-12-12 15:51_

The original example in the doc-comment has a sum of two things. Moved the wording up a bit so that "above example" unambiguously refers to one thing.

---

_@dylwil3 reviewed on 2025-12-12 15:52_

---

_Review comment by @dylwil3 on `crates/ruff_python_formatter/src/expression/mod.rs`:1033 on 2025-12-12 15:52_

Long explanation added in doc-comments in function

---

_@dylwil3 reviewed on 2025-12-12 15:54_

---

_Review comment by @dylwil3 on `crates/ruff_python_formatter/src/preview.rs`:68 on 2025-12-12 15:54_

I compared the various diffs between toggling different things. Adding "use fluent more often" on top of the stable style looks not great in a lot of examples, I think. I'd rather add _just_ the layout change, since it's a very understandable diff for users: it only changes two lines in every call chain. I think if we later want to "use fluent more often" on top of that, it wouldn't be too bad. But somehow doing it the other order or doing both results in a kind of surprising change in some examples. (It is more likely to highlight our lack of good nested expression formatting for some reason).

---

_Converted to draft by @dylwil3 on 2025-12-12 17:24_

---

_@dylwil3 reviewed on 2025-12-12 20:27_

---

_Review comment by @dylwil3 on `crates/ruff_python_formatter/src/expression/mod.rs`:955 on 2025-12-12 20:27_

The refactor is not equivalent, e.g. when x = 2. I added some doc comments - we subtract because we traverse from right to left and the "count" is of "remaining calls/subscripts".

---

_Comment by @dylwil3 on 2025-12-12 20:31_

(~~Edit: Hold that thought - didn't expect the ecosystem results to change, maybe I made a mistake in one of the million refactors or merges... sorry, looking into it~~)
(Update: False alarm! Looks like a couple repos were updated and a few things changed from the new lambda formatting. All looks good to me.)

Ok, added lots of doc-comments and some more fixtures.. The logic is still somewhat complex since it has to produce the correct behavior when, e.g., the root of the chain is either parenthesized or a call. 

(Actually I had to update the logic to handle correctly the case when the root of the chain is parenthesized but then we have a non-called attribute right after. I missed this before because it doesn't actually happen in the ecosystem or in our tests.)

I removed the "apply fluent more often" preview style - I think we can do a follow-up PR that does that if we want, but it looks strange sometimes when it's applied to the _stable_ fluent layout, so I think it should only be applied once the current PR lands. Also it was responsible for almost all of the examples you pointed to and said "this looks worse".

---

_Marked ready for review by @dylwil3 on 2025-12-12 20:31_

---

_Converted to draft by @dylwil3 on 2025-12-12 20:42_

---

_Marked ready for review by @dylwil3 on 2025-12-12 20:53_

---

_Review requested from @Copilot by @MichaReiser on 2025-12-12 22:21_

---

_Review requested from @ntBre by @MichaReiser on 2025-12-12 22:21_

---

_Comment by @MichaReiser on 2025-12-12 22:22_

Oh no, it wasn't my intention to request a copilot review and I don't know how to remove it again. I'm sorry 😪 

---

_Review comment by @Copilot on `crates/ruff_python_formatter/src/expression/mod.rs`:1020 on 2025-12-12 22:26_

The comment text is incomplete. It says "Similar to the above, but instead looks at all and subscripts" but appears to be missing the word "calls" between "all" and "and subscripts". The comment should read "Similar to the above, but instead looks at all calls and subscripts rather than looking only at those on _attribute values_."
```suggestion
        // Similar to the above, but instead looks at all calls
```

---

_Review comment by @Copilot on `crates/ruff_python_formatter/src/preview.rs`:64 on 2025-12-12 22:26_

The link in the doc comment is incomplete. It says [`fluent_layout_split_first_call`]() but the parentheses are empty. This should reference a GitHub issue or PR (e.g., `https://github.com/astral-sh/ruff/issues/8598`) similar to the other preview feature functions in this file.
```suggestion
/// [`fluent_layout_split_first_call`](https://github.com/astral-sh/ruff/issues/XXXXX) preview
```

---

_@copilot-pull-request-reviewer[bot] reviewed on 2025-12-12 22:26_

## Pull request overview

This PR implements a new preview-mode formatting style for method chains that breaks _at_ the first call or subscript instead of _after_ it. For example, `df.filter().agg()` now formats as `df\n.filter()\n.agg()` rather than `df.filter()\n.agg()` when lines need to be broken. This change applies to fluent-style method chains (those with 2+ called/subscripted attributes).

Key changes:
- Extended `CallChainLayout::Fluent` enum to track position within chains via a new `AttributeState` type
- Implemented state transitions to determine where to insert line breaks in preview mode
- Added preview feature flag `is_fluent_layout_split_first_call_enabled`

### Reviewed changes

Copilot reviewed 14 out of 14 changed files in this pull request and generated 3 comments.

<details>
<summary>Show a summary per file</summary>

| File | Description |
| ---- | ----------- |
| `docs/formatter.md` | Adds documentation explaining the new fluent layout preview style with examples |
| `crates/ruff_python_formatter/src/preview.rs` | Adds feature flag function for the new preview style |
| `crates/ruff_python_formatter/src/expression/mod.rs` | Core implementation: adds `AttributeState` enum and state transition logic to `CallChainLayout` |
| `crates/ruff_python_formatter/src/expression/expr_attribute.rs` | Updates attribute formatting to use state transitions and insert breaks at first call/subscript |
| `crates/ruff_python_formatter/src/expression/expr_call.rs` | Updates call expression formatting to decrement call-like count when traversing into attributes |
| `crates/ruff_python_formatter/src/expression/expr_subscript.rs` | Updates subscript formatting to decrement call-like count when traversing into attributes |
| `crates/ruff_python_formatter/src/expression/expr_lambda.rs` | Updates lambda formatting to match on new `Fluent` variant structure |
| `crates/ruff_python_formatter/tests/snapshots/format@parentheses__call_chains.py.snap` | Test snapshot showing preview formatting differences for method chains |
| `crates/ruff_python_formatter/tests/snapshots/format@expression__call.py.snap` | Test snapshot showing preview changes for call expressions |
| `crates/ruff_python_formatter/tests/snapshots/format@expression__await.py.snap` | Test snapshot showing preview changes for await expressions |
| `crates/ruff_python_formatter/tests/snapshots/format@expression__lambda.py.snap` | Test snapshot showing preview changes for lambda expressions with method chains |
| `crates/ruff_python_formatter/tests/snapshots/black_compatibility@cases__preview_long_dict_values.py.snap` | Test snapshot showing preview changes for dict values with method chains |
| `crates/ruff_python_formatter/tests/snapshots/format@expression__split_empty_brackets.py.snap` | Snapshot metadata update (removes `snapshot_kind`) |
| `crates/ruff_python_formatter/resources/test/fixtures/ruff/parentheses/call_chains.py` | Adds test cases for new preview behavior including polars examples and edge cases |
</details>






---

💡 <a href="/astral-sh/ruff/new/main/.github/instructions?filename=*.instructions.md" class="Link--inTextBlock" target="_blank" rel="noopener noreferrer">Add Copilot custom instructions</a> for smarter, more guided reviews. <a href="https://docs.github.com/en/copilot/customizing-copilot/adding-repository-custom-instructions-for-github-copilot" class="Link--inTextBlock" target="_blank" rel="noopener noreferrer">Learn how to get started</a>.

---

_Review comment by @Copilot on `crates/ruff_python_formatter/resources/test/fixtures/ruff/parentheses/call_chains.py`:266 on 2025-12-12 22:26_

Variable identical_hidden_layer_sizes is not used.
```suggestion
            all(
```

---

_Review comment by @ntBre on `crates/ruff_python_formatter/src/expression/mod.rs`:880 on 2025-12-12 23:49_

My idea in #21903 was to make myself update docs when stabilizing something by breaking my preview links. If you want to do the same, I think you could link this `preview` to your preview function with something like:


```suggestion
/// In [`preview`](crate::preview::is_fluent_layout_split_first_call_enabled), we also track the position of the leftmost call or
```

---

_Review comment by @ntBre on `crates/ruff_python_formatter/src/expression/mod.rs`:924 on 2025-12-13 00:38_

I guess we could use a smaller type here if we wanted to since the file size is bounded by a u32 and it takes a couple of characters to add a call or subscript, but I doubt it matters much either way.

---

_@ntBre reviewed on 2025-12-13 00:39_

I think this all makes sense to me, and the ecosystem changes look good! I only had a couple of very minor suggestions, but I'll leave it to Micha to approve.

---

_@dylwil3 reviewed on 2025-12-13 13:23_

---

_Review comment by @dylwil3 on `crates/ruff_python_formatter/src/expression/mod.rs`:924 on 2025-12-13 13:23_

I was wondering that, and probably my math is wrong, but isn't:

```
a().a().a().... # continue for u32::MAX characters
```

still much larger than `u16::MAX` many calls?

---

_@ntBre reviewed on 2025-12-13 18:11_

---

_Review comment by @ntBre on `crates/ruff_python_formatter/src/expression/mod.rs`:924 on 2025-12-13 18:11_

Oof, I was not thinking about this in the right way last night, I think you're right! Sorry for the noise!

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/mod.rs`:924 on 2025-12-15 07:46_

I think a `u32` is perfectly fine. It shouldn't matter much for performance (or storage). We could consider making it a `NonZeroU32` if it is guaranteed to always be non-zero.

---

_@MichaReiser reviewed on 2025-12-15 07:46_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/resources/test/fixtures/ruff/parentheses/call_chains.py`:221 on 2025-12-15 07:46_

It might be worth explaining why we do it regardless. I'm sure it will come up in a future issue and I would much appreciate if I came across a comment in code (or test) explaining why we do split after `np`

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/expr_attribute.rs`:51 on 2025-12-15 07:50_

Nit: It might be worth introducing a helper

```suggestion
            if call_chain_layout.is_fluent() {
```

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/mod.rs`:975 on 2025-12-15 07:53_

Nit: Align the method and variant name (I like `call_like` but I don't mind much either way)

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/mod.rs`:995 on 2025-12-15 07:54_

This comment is very helpful. Thank you

---

_@MichaReiser approved on 2025-12-15 07:58_

Thank you. 

The implementation looks good to me, but I think we're still a bit light in terms of tests (especially simple once, your examples are pretty massive :)).

I think it would be good to have a few, very simple method chains that document the intended breaking behavior. It might also be worth adding a few more tests with parenthesized roots (e.g. parenthesize the second element etc), and comments.

---

_Comment by @dylwil3 on 2025-12-15 15:29_

Thank you @MichaReiser and @ntBre for the reviews!

And thanks @laundmo , @alexreinking , and @dhirschfeld for the feedback - looking forward to any comments you have for improvement in the next iteration!

---

_Merged by @dylwil3 on 2025-12-15 15:29_

---

_Closed by @dylwil3 on 2025-12-15 15:29_

---

_Branch deleted on 2025-12-19 13:54_

---
