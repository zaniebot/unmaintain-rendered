```yaml
number: 14224
title: "[`ruff`] Add implementation for `flake8-markupsafe` via `RUF035`"
type: pull_request
state: merged
author: Daverball
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: feat/flake8-markupsafe
created_at: 2024-11-09T12:35:44Z
updated_at: 2024-11-15T10:36:13Z
url: https://github.com/astral-sh/ruff/pull/14224
synced_at: 2026-01-12T15:55:47Z
```

# [`ruff`] Add implementation for `flake8-markupsafe` via `RUF035`

---

_@Daverball_

Closes #14124

## Summary

This adds an implementation for [flake8-markupsafe](https://github.com/vmagamedov/flake8-markupsafe), minus the questionable exception for i18n and mako support, but with the ability to specify further aliases/subclasses/functionally equivalent for `markupsafe.Markup`. By default `flask.Markup` is also detected as a commonly used alias.

## Test Plan

`cargo nextest run`


---

_Comment by @github-actions[bot] on 2024-11-09 12:55_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+51 -0 violations, +0 -0 fixes in 4 projects; 50 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+17 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/bec090ceb8547df5564398066e73880875d00569/airflow/www/app.py#L94'>airflow/www/app.py:94:25:</a> RUF035 Unsafe use of `markupsafe.Markup` detected
+ <a href='https://github.com/apache/airflow/blob/bec090ceb8547df5564398066e73880875d00569/airflow/www/utils.py#L383'>airflow/www/utils.py:383:12:</a> RUF035 Unsafe use of `markupsafe.Markup` detected
+ <a href='https://github.com/apache/airflow/blob/bec090ceb8547df5564398066e73880875d00569/airflow/www/utils.py#L579'>airflow/www/utils.py:579:16:</a> RUF035 Unsafe use of `markupsafe.Markup` detected
+ <a href='https://github.com/apache/airflow/blob/bec090ceb8547df5564398066e73880875d00569/airflow/www/utils.py#L587'>airflow/www/utils.py:587:20:</a> RUF035 Unsafe use of `markupsafe.Markup` detected
+ <a href='https://github.com/apache/airflow/blob/bec090ceb8547df5564398066e73880875d00569/airflow/www/utils.py#L596'>airflow/www/utils.py:596:20:</a> RUF035 Unsafe use of `markupsafe.Markup` detected
+ <a href='https://github.com/apache/airflow/blob/bec090ceb8547df5564398066e73880875d00569/airflow/www/utils.py#L600'>airflow/www/utils.py:600:16:</a> RUF035 Unsafe use of `markupsafe.Markup` detected
+ <a href='https://github.com/apache/airflow/blob/bec090ceb8547df5564398066e73880875d00569/airflow/www/utils.py#L611'>airflow/www/utils.py:611:15:</a> RUF035 Unsafe use of `markupsafe.Markup` detected
+ <a href='https://github.com/apache/airflow/blob/bec090ceb8547df5564398066e73880875d00569/airflow/www/utils.py#L614'>airflow/www/utils.py:614:15:</a> RUF035 Unsafe use of `markupsafe.Markup` detected
+ <a href='https://github.com/apache/airflow/blob/bec090ceb8547df5564398066e73880875d00569/airflow/www/utils.py#L624'>airflow/www/utils.py:624:12:</a> RUF035 Unsafe use of `markupsafe.Markup` detected
+ <a href='https://github.com/apache/airflow/blob/bec090ceb8547df5564398066e73880875d00569/airflow/www/utils.py#L923'>airflow/www/utils.py:923:24:</a> RUF035 Unsafe use of `markupsafe.Markup` detected
+ <a href='https://github.com/apache/airflow/blob/bec090ceb8547df5564398066e73880875d00569/airflow/www/views.py#L1079'>airflow/www/views.py:1079:21:</a> RUF035 Unsafe use of `markupsafe.Markup` detected
+ <a href='https://github.com/apache/airflow/blob/bec090ceb8547df5564398066e73880875d00569/airflow/www/views.py#L1098'>airflow/www/views.py:1098:24:</a> RUF035 Unsafe use of `markupsafe.Markup` detected
+ <a href='https://github.com/apache/airflow/blob/bec090ceb8547df5564398066e73880875d00569/airflow/www/views.py#L4337'>airflow/www/views.py:4337:20:</a> RUF035 Unsafe use of `markupsafe.Markup` detected
+ <a href='https://github.com/apache/airflow/blob/bec090ceb8547df5564398066e73880875d00569/airflow/www/views.py#L4341'>airflow/www/views.py:4341:16:</a> RUF035 Unsafe use of `markupsafe.Markup` detected
+ <a href='https://github.com/apache/airflow/blob/bec090ceb8547df5564398066e73880875d00569/airflow/www/views.py#L4523'>airflow/www/views.py:4523:20:</a> RUF035 Unsafe use of `markupsafe.Markup` detected
+ <a href='https://github.com/apache/airflow/blob/bec090ceb8547df5564398066e73880875d00569/airflow/www/widgets.py#L50'>airflow/www/widgets.py:50:16:</a> RUF035 Unsafe use of `markupsafe.Markup` detected
... 1 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+8 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/24b8a94c2ce4d21c65caa076bdc54ba61635be85/superset/connectors/sqla/models.py#L1305'>superset/connectors/sqla/models.py:1305:16:</a> RUF035 Unsafe use of `markupsafe.Markup` detected
+ <a href='https://github.com/apache/superset/blob/24b8a94c2ce4d21c65caa076bdc54ba61635be85/superset/models/dashboard.py#L225'>superset/models/dashboard.py:225:16:</a> RUF035 Unsafe use of `markupsafe.Markup` detected
+ <a href='https://github.com/apache/superset/blob/24b8a94c2ce4d21c65caa076bdc54ba61635be85/superset/models/helpers.py#L533'>superset/models/helpers.py:533:16:</a> RUF035 Unsafe use of `markupsafe.Markup` detected
+ <a href='https://github.com/apache/superset/blob/24b8a94c2ce4d21c65caa076bdc54ba61635be85/superset/models/helpers.py#L562'>superset/models/helpers.py:562:16:</a> RUF035 Unsafe use of `markupsafe.Markup` detected
+ <a href='https://github.com/apache/superset/blob/24b8a94c2ce4d21c65caa076bdc54ba61635be85/superset/models/slice.py#L335'>superset/models/slice.py:335:16:</a> RUF035 Unsafe use of `markupsafe.Markup` detected
+ <a href='https://github.com/apache/superset/blob/24b8a94c2ce4d21c65caa076bdc54ba61635be85/superset/models/sql_lab.py#L445'>superset/models/sql_lab.py:445:16:</a> RUF035 Unsafe use of `markupsafe.Markup` detected
+ <a href='https://github.com/apache/superset/blob/24b8a94c2ce4d21c65caa076bdc54ba61635be85/superset/utils/core.py#L483'>superset/utils/core.py:483:16:</a> RUF035 Unsafe use of `markupsafe.Markup` detected
+ <a href='https://github.com/apache/superset/blob/24b8a94c2ce4d21c65caa076bdc54ba61635be85/superset/views/database/mixins.py#L249'>superset/views/database/mixins.py:249:17:</a> RUF035 Unsafe use of `markupsafe.Markup` detected
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/b3f18b805d2054a8e4e11f7ec363a7f2367c8c6c/zerver/views/documentation.py#L291'>zerver/views/documentation.py:291:35:</a> RUF035 Unsafe use of `markupsafe.Markup` detected
</pre>

</p>
</details>
<details><summary><a href="https://github.com/indico/indico">indico/indico</a> (+25 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/indico/indico/blob/1ba036df833737f4facc0a902c219dd474e4dd88/indico/modules/admin/notices.py#L93'>indico/modules/admin/notices.py:93:16:</a> RUF035 Unsafe use of `markupsafe.Markup` detected
+ <a href='https://github.com/indico/indico/blob/1ba036df833737f4facc0a902c219dd474e4dd88/indico/modules/auth/controllers.py#L351'>indico/modules/auth/controllers.py:351:15:</a> RUF035 Unsafe use of `markupsafe.Markup` detected
+ <a href='https://github.com/indico/indico/blob/1ba036df833737f4facc0a902c219dd474e4dd88/indico/modules/bootstrap/controllers.py#L77'>indico/modules/bootstrap/controllers.py:77:15:</a> RUF035 Unsafe use of `markupsafe.Markup` detected
+ <a href='https://github.com/indico/indico/blob/1ba036df833737f4facc0a902c219dd474e4dd88/indico/modules/bootstrap/controllers.py#L98'>indico/modules/bootstrap/controllers.py:98:19:</a> RUF035 Unsafe use of `markupsafe.Markup` detected
+ <a href='https://github.com/indico/indico/blob/1ba036df833737f4facc0a902c219dd474e4dd88/indico/modules/events/agreements/placeholders.py#L31'>indico/modules/events/agreements/placeholders.py:31:16:</a> RUF035 Unsafe use of `markupsafe.Markup` detected
+ <a href='https://github.com/indico/indico/blob/1ba036df833737f4facc0a902c219dd474e4dd88/indico/modules/events/controllers/creation.py#L151'>indico/modules/events/controllers/creation.py:151:27:</a> RUF035 Unsafe use of `markupsafe.Markup` detected
+ <a href='https://github.com/indico/indico/blob/1ba036df833737f4facc0a902c219dd474e4dd88/indico/modules/events/models/events.py#L818'>indico/modules/events/models/events.py:818:16:</a> RUF035 Unsafe use of `markupsafe.Markup` detected
+ <a href='https://github.com/indico/indico/blob/1ba036df833737f4facc0a902c219dd474e4dd88/indico/modules/events/persons/placeholders.py#L71'>indico/modules/events/persons/placeholders.py:71:16:</a> RUF035 Unsafe use of `markupsafe.Markup` detected
+ <a href='https://github.com/indico/indico/blob/1ba036df833737f4facc0a902c219dd474e4dd88/indico/modules/events/registration/placeholders/invitations.py#L50'>indico/modules/events/registration/placeholders/invitations.py:50:16:</a> RUF035 Unsafe use of `markupsafe.Markup` detected
+ <a href='https://github.com/indico/indico/blob/1ba036df833737f4facc0a902c219dd474e4dd88/indico/modules/events/surveys/placeholders.py#L32'>indico/modules/events/surveys/placeholders.py:32:16:</a> RUF035 Unsafe use of `markupsafe.Markup` detected
+ <a href='https://github.com/indico/indico/blob/1ba036df833737f4facc0a902c219dd474e4dd88/indico/modules/legal/forms.py#L47'>indico/modules/legal/forms.py:47:80:</a> RUF035 Unsafe use of `markupsafe.Markup` detected
+ <a href='https://github.com/indico/indico/blob/1ba036df833737f4facc0a902c219dd474e4dd88/indico/modules/users/controllers.py#L124'>indico/modules/users/controllers.py:124:31:</a> RUF035 Unsafe use of `markupsafe.Markup` detected
+ <a href='https://github.com/indico/indico/blob/1ba036df833737f4facc0a902c219dd474e4dd88/indico/util/placeholders.py#L246'>indico/util/placeholders.py:246:12:</a> RUF035 Unsafe use of `markupsafe.Markup` detected
+ <a href='https://github.com/indico/indico/blob/1ba036df833737f4facc0a902c219dd474e4dd88/indico/web/flask/app.py#L238'>indico/web/flask/app.py:238:39:</a> RUF035 Unsafe use of `markupsafe.Markup` detected
+ <a href='https://github.com/indico/indico/blob/1ba036df833737f4facc0a902c219dd474e4dd88/indico/web/flask/app.py#L251'>indico/web/flask/app.py:251:49:</a> RUF035 Unsafe use of `markupsafe.Markup` detected
+ <a href='https://github.com/indico/indico/blob/1ba036df833737f4facc0a902c219dd474e4dd88/indico/web/flask/templating.py#L214'>indico/web/flask/templating.py:214:21:</a> RUF035 Unsafe use of `markupsafe.Markup` detected
+ <a href='https://github.com/indico/indico/blob/1ba036df833737f4facc0a902c219dd474e4dd88/indico/web/flask/templating.py#L39'>indico/web/flask/templating.py:39:12:</a> RUF035 Unsafe use of `markupsafe.Markup` detected
+ <a href='https://github.com/indico/indico/blob/1ba036df833737f4facc0a902c219dd474e4dd88/indico/web/forms/fields/protection.py#L49'>indico/web/forms/fields/protection.py:49:16:</a> RUF035 Unsafe use of `markupsafe.Markup` detected
+ <a href='https://github.com/indico/indico/blob/1ba036df833737f4facc0a902c219dd474e4dd88/indico/web/forms/widgets.py#L101'>indico/web/forms/widgets.py:101:16:</a> RUF035 Unsafe use of `markupsafe.Markup` detected
+ <a href='https://github.com/indico/indico/blob/1ba036df833737f4facc0a902c219dd474e4dd88/indico/web/forms/widgets.py#L37'>indico/web/forms/widgets.py:37:16:</a> RUF035 Unsafe use of `markupsafe.Markup` detected
+ <a href='https://github.com/indico/indico/blob/1ba036df833737f4facc0a902c219dd474e4dd88/indico/web/forms/widgets.py#L47'>indico/web/forms/widgets.py:47:16:</a> RUF035 Unsafe use of `markupsafe.Markup` detected
+ <a href='https://github.com/indico/indico/blob/1ba036df833737f4facc0a902c219dd474e4dd88/indico/web/menu.py#L180'>indico/web/menu.py:180:12:</a> RUF035 Unsafe use of `markupsafe.Markup` detected
+ <a href='https://github.com/indico/indico/blob/1ba036df833737f4facc0a902c219dd474e4dd88/indico/web/util.py#L33'>indico/web/util.py:33:26:</a> RUF035 Unsafe use of `markupsafe.Markup` detected
+ <a href='https://github.com/indico/indico/blob/1ba036df833737f4facc0a902c219dd474e4dd88/indico/web/util.py#L360'>indico/web/util.py:360:23:</a> RUF035 Unsafe use of `markupsafe.Markup` detected
... 1 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF035 | 51 | 51 | 0 | 0 | 0 |

</p>
</details>




---

_Marked ready for review by @Daverball on 2024-11-09 13:01_

---

_@sbrugman reviewed on 2024-11-09 13:04_

---

_Review comment by @sbrugman on `crates/ruff_linter/src/rules/flake8_markupsafe/rules/unsafe_markup_use.rs`:61 on 2024-11-09 13:04_

To only consider this rule if `markupsafe` or `flask` is imported and `settings.flake8_markupsafe.extend...` is empty:

```suggestion
     if !(checker.semantic().seen_module(Modules::MARKUPSAFE)
        || checker.semantic().seen_module(Modules::FLASK))
    {
        return;
    }
    if let Some(name) = checker
```

(And add `MARKUPSAFE` and `FLASK` to `Modules`)

---

_@sbrugman reviewed on 2024-11-09 13:07_

---

_Review comment by @sbrugman on `crates/ruff_linter/src/rules/flake8_markupsafe/rules/unsafe_markup_use.rs`:38 on 2024-11-09 13:07_

Consider adding the `## Options` section here, example: https://docs.astral.sh/ruff/rules/ambiguous-unicode-character-comment/#options

---

_@Daverball reviewed on 2024-11-09 13:10_

---

_Review comment by @Daverball on `crates/ruff_linter/src/rules/flake8_markupsafe/rules/unsafe_markup_use.rs`:61 on 2024-11-09 13:10_

This fast-path only works if no additional names have been specified in the options, so at the very least I would have to first check `checker.settings.flake8_markupsafe.extend_markup_names.is_empty()`.

---

_@sbrugman reviewed on 2024-11-09 13:14_

---

_Review comment by @sbrugman on `crates/ruff_linter/src/rules/flake8_markupsafe/rules/unsafe_markup_use.rs`:65 on 2024-11-09 13:14_

If the `call` is not unsafe, for instance when the call arguments are empty, then we do not use the qualified name (which I assume requires more computation). We should first check `!is_unsafe_call` and otherwise set `None`, if safe, then resolve the qualified name and check the markup call.

---

_Review comment by @sbrugman on `crates/ruff_linter/resources/test/fixtures/flake8_markupsafe/MS001.py`:10 on 2024-11-09 13:19_

Could you please add a couple of more test cases?

Examples:
- f-string formatting
- old-style formatting, `Markup("<em>%s</em> " % ("foo & bar",))`
- Explicit keyword arguments, e.g. `Markup(object="hello world")`

https://markupsafe.palletsprojects.com/en/stable/escaping/#markupsafe.Markup

---

_@sbrugman reviewed on 2024-11-09 13:19_

---

_@sbrugman reviewed on 2024-11-09 13:24_

---

_Review comment by @sbrugman on `crates/ruff_linter/src/rules/flake8_markupsafe/rules/unsafe_markup_use.rs`:61 on 2024-11-09 13:24_

That's true. Such a guard would be helpful for users that do not work with `markupsafe` or `flask` but have this rule, but have it enabled.

---

_@sbrugman reviewed on 2024-11-09 13:29_

---

_Review comment by @sbrugman on `crates/ruff_linter/src/rules/flake8_markupsafe/rules/unsafe_markup_use.rs`:18 on 2024-11-09 13:29_

It's probably a good idea to mention the deviations from the upstream rule (mako, i18n)

---

_Comment by @sbrugman on 2024-11-09 13:31_

Nice contribution, thanks!

---

_@Daverball reviewed on 2024-11-09 13:35_

---

_Review comment by @Daverball on `crates/ruff_linter/resources/test/fixtures/flake8_markupsafe/MS001.py`:10 on 2024-11-09 13:35_

Detecting keyword arguments would only reliably work for `markupsafe.Markup` and `flask.Markup` but not for equivalent classes, so I'm not sure it's worth adding extra complexity until type inference can easily give us the first positional argument regardless of how the function was called. And even then it's somewhat questionable, since code searching for `Markup(object=` yields zero results, I don't expect anyone to ever call it this way.

On the other hand, it is a security rule, so we could consider emitting an error if there isn't at least one positional argument.

But I knew I forgot about something, and it was old-style formatting, f-strings do already have a test case (the very first one).

---

_@Daverball reviewed on 2024-11-09 13:36_

---

_Review comment by @Daverball on `crates/ruff_linter/src/rules/flake8_markupsafe/rules/unsafe_markup_use.rs`:18 on 2024-11-09 13:36_

Not sure it's worth mentioning mako support, since mako support was not part of flake8, but rather had to be invoked manually by directly calling the mako module within the plugin. But mentioning the i18n deviation makes sense to me.

---

_@sbrugman reviewed on 2024-11-09 13:58_

---

_Review comment by @sbrugman on `crates/ruff_linter/resources/test/fixtures/flake8_markupsafe/MS001.py`:10 on 2024-11-09 13:58_

> Detecting keyword arguments would only reliably work for markupsafe.Markup and flask.Markup but not for equivalent classes, so I'm not sure it's worth adding extra complexity until type inference can easily give us the first positional argument regardless of how the function was called. And even then it's somewhat questionable, since code searching for Markup(object= yields zero results, I don't expect anyone to ever call it this way.

I was only suggesting to add them as test cases, and agree that we do not have to support. I think it's a good idea to include known syntactially valid negative test cases ahead of time for posterity.

---

_@Daverball reviewed on 2024-11-09 14:00_

---

_Review comment by @Daverball on `crates/ruff_linter/resources/test/fixtures/flake8_markupsafe/MS001.py`:10 on 2024-11-09 14:00_

Yeah, that's fair. I did include corresponding test cases.

---

_@sbrugman reviewed on 2024-11-09 14:00_

---

_Review comment by @sbrugman on `crates/ruff_linter/resources/test/fixtures/flake8_markupsafe/MS001.py`:10 on 2024-11-09 14:00_

> But I knew I forgot about something, and it was old-style formatting, f-strings do already have a test case (the very first one).

Overlooked the f-string. For completeness we can consider other valid but less frequent cases, e.g. multi-line strings, implicit string concatenation,  with and without comments.

---

_@sbrugman reviewed on 2024-11-09 14:11_

---

_Review comment by @sbrugman on `crates/ruff_linter/src/rules/flake8_markupsafe/rules/unsafe_markup_use.rs`:80 on 2024-11-09 14:11_

Would something like this work? (equivalent but in the same return-early style for consistency)

```rs

    let Some(qualified_name) = checker
         .semantic()
         .resolve_qualified_name(&call.func) else{
        return;
    };

    if !is_markup_call(&qualified_name, checker.settings) {
        return;
    } 

    checker
        .diagnostics
        .push(Diagnostic::new(UnsafeMarkupUse { qualified_name.to_string() }, call.range()));
```

---

_@sbrugman reviewed on 2024-11-09 14:20_

---

_Review comment by @sbrugman on `crates/ruff_linter/resources/test/fixtures/flake8_markupsafe/MS001.py`:13 on 2024-11-09 14:20_

Found one more example. In the ecosystem results we can find this example:

```python
Markup("*" * 8)
```

There is no user input here - essentially it's just string formatting -, so there is no possibility of tampering and we could allow these cases. 

https://github.com/apache/airflow/blob/340a70bfe7289e01898ddd75f8edfaf7772e9d09/airflow/www/views.py#L4523

Another example:

```python
flask.Markup("hello {}".format("world"))
```

Would it make sense to check if any variable is used inside the call?
We then also would have to exclude non-string operations.

Probably it adds too much complexity for now. 
When the type inference is available this might become easier.

---

_@Daverball reviewed on 2024-11-09 14:33_

---

_Review comment by @Daverball on `crates/ruff_linter/resources/test/fixtures/flake8_markupsafe/MS001.py`:13 on 2024-11-09 14:33_

I don't think we should go too crazy for the initial implementation with detecting false negatives. It is a security rule, so some false negatives are to be expected.

Detecting constant expressions sounds like something that should live in red-knot. So we can be more accurate here when it does, anything else feels like a bandaid solution to me.

---

_@sbrugman reviewed on 2024-11-09 14:39_

---

_Review comment by @sbrugman on `crates/ruff_linter/resources/test/fixtures/flake8_markupsafe/MS001.py`:13 on 2024-11-09 14:39_

I agree, let's add these cases for when red-knot is available.

---

_Label `rule` added by @MichaReiser on 2024-11-11 10:30_

---

_Label `preview` added by @MichaReiser on 2024-11-11 10:30_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/codes.rs`:1085 on 2024-11-11 11:04_

I understand that flake8-markup-safe has only a single rule. If that's indeed the case, then I don't think it's worth for us to introduce a new rule group just for that. Instead, we should add it to the `RUF` group and mentioned it in the documentation that this rule is inherited/inspired by `flake8-markup-safe`

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_markupsafe/rules/unsafe_markup_use.rs`:119 on 2024-11-11 11:11_

Nit:
```suggestion
	matches!(&*call.arguments.args, [first] if !first.is_string_literal_expr() && !first.is_bytes_literal_expr())
```

---

_@MichaReiser approved on 2024-11-11 11:14_

This looks good to me. Thank you. The only change I would make is that I don't think it's worth introducing a new rule group for a single rule. 

I'd also like to hear @AlexWaygood's opinion on the rule itself.

---

_@Daverball reviewed on 2024-11-11 11:21_

---

_Review comment by @Daverball on `crates/ruff_linter/src/codes.rs`:1085 on 2024-11-11 11:21_

That's fair, I brought this up in the original issue as well. I'm fine with whatever you decide on.

---

_@MichaReiser reviewed on 2024-11-11 11:24_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/codes.rs`:1085 on 2024-11-11 11:24_

Oh sorry that I missed this in the original issue. Let's wait until we hear back from @AlexWaygood before making the change.

---

_@AlexWaygood reviewed on 2024-11-11 13:06_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/codes.rs`:1085 on 2024-11-11 13:06_

Huh. Considering the flake8 plugin was never published to PyPI and we can find no uses of it in the wild, I also think it probably makes more sense to add it to the `RUF` category rather than add a new category just for this. (If the plugin were more widely used, I think it would be a different question, since we already have several other categories that only have a single rule in them.)

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_markupsafe/rules/unsafe_markup_use.rs`:22 on 2024-11-11 13:07_

You don't say here what you mean by "the original rule". Many users might read this documentation having not known that this is a reimplementation of a pre-existing flake8 plugin

---

_@AlexWaygood reviewed on 2024-11-11 13:07_

---

_@AlexWaygood reviewed on 2024-11-11 13:10_

I've never used `markupsafe` but it makes sense to me that this would be an important security consideration. I think it's good for linters to catch this.

Long-term, we should be able to improve the accuracy of rules like this when we switch to using red-knot as a backend (the `LiteralString` type is explicitly designed for this use case). But I don't think that should stop us from implementing this rule now!

---

_@Daverball reviewed on 2024-11-11 13:17_

---

_Review comment by @Daverball on `crates/ruff_linter/src/rules/flake8_markupsafe/rules/unsafe_markup_use.rs`:22 on 2024-11-11 13:17_

Yeah if we move this to the RUF namespace I don't think we need this sentence at all. Just a new one to give credit to the plugin which originally inspired this rule.

---

_Renamed from "[flake8-markupsafe] Adds Implementation for MS001" to "[flake8-markupsafe] Adds Implementation for MS001 via RUF035" by @Daverball on 2024-11-11 14:08_

---

_Review requested from @MichaReiser by @Daverball on 2024-11-11 14:40_

---

_@MichaReiser approved on 2024-11-11 18:29_

Nice!

---

_Merged by @MichaReiser on 2024-11-11 18:30_

---

_Closed by @MichaReiser on 2024-11-11 18:30_

---

_Branch deleted on 2024-11-11 18:55_

---

_Renamed from "[flake8-markupsafe] Adds Implementation for MS001 via RUF035" to "[`flake8-markupsafe`] Add implementation for MS001 via RUF035" by @dhruvmanila on 2024-11-15 10:35_

---

_Renamed from "[`flake8-markupsafe`] Add implementation for MS001 via RUF035" to "[`ruff`] Add implementation for `flake8-markupsafe` via `RUF035`" by @dhruvmanila on 2024-11-15 10:36_

---
