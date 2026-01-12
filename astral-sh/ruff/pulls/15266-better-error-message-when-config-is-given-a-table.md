```yaml
number: 15266
title: "Better error message when `--config` is given a table key and a non-inline-table value"
type: pull_request
state: merged
author: InSyncWithFoo
labels:
  - cli
assignees: []
merged: true
base: main
head: cli-config
created_at: 2025-01-05T08:57:29Z
updated_at: 2025-01-06T13:23:49Z
url: https://github.com/astral-sh/ruff/pull/15266
synced_at: 2026-01-12T15:55:50Z
```

# Better error message when `--config` is given a table key and a non-inline-table value

---

_@InSyncWithFoo_

## Summary

Resolves #13995.

## Test Plan

`cargo nextest run` and `cargo insta test`.

---

_Comment by @github-actions[bot] on 2025-01-05 09:03_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review requested from @AlexWaygood by @AlexWaygood on 2025-01-05 11:15_

---

_Comment by @AlexWaygood on 2025-01-05 12:49_

This is great! I think we can do even better with a few tweaks, though. With your branch currently, this is the error message I get:

```
~/dev/ruff (cli-config)⚡ % cargo run -- check . --config='lint.flake8-pytest-style="csv"'
   Compiling ruff v0.8.6 (/Users/alexw/dev/ruff/crates/ruff)
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 2.51s
     Running `target/debug/ruff check . '--config=lint.flake8-pytest-style="csv"'`
error: invalid value 'lint.flake8-pytest-style="csv"' for '--config <CONFIG_OPTION>'

  tip: A `--config` flag must either be a path to a `.toml` configuration file
       or a TOML `<KEY> = <VALUE>` pair overriding a specific configuration
       option

`lint.flake8-pytest-style` is a table.
Did you mean to use one of its subkeys instead? Possible choices:
lint.flake8-pytest-style.fixture-parentheses
lint.flake8-pytest-style.parametrize-names-type
lint.flake8-pytest-style.parametrize-values-type
lint.flake8-pytest-style.parametrize-values-row-type
lint.flake8-pytest-style.raises-require-match-for
lint.flake8-pytest-style.raises-extend-require-match-for
lint.flake8-pytest-style.mark-parentheses

Could not parse the supplied argument as a `ruff.toml` configuration option:

invalid type: string "csv", expected struct Flake8PytestStyleOptions
in `lint`

For more information, try '--help'.
```

<details>
<summary>Colorized screenshot</summary>

![image](https://github.com/user-attachments/assets/d3e19543-ce22-4c19-a7df-b289651b5f5a)

</details>

But if I apply this diff to your branch:

```diff
--- a/crates/ruff/src/args.rs
+++ b/crates/ruff/src/args.rs
@@ -978,23 +978,25 @@ The path `{value}` does not point to a configuration file"
                     let prefixed_subkeys = format!("{set}")
                         .trim_ascii()
                         .split('\n')
-                        .map(|child| format!("{key}.{child}"))
+                        .map(|child| format!(" - `{key}.{child}`"))
                         .join("\n");
 
                     tip.push_str(&format!(
                         "
 
-`{key}` is a table.
-Did you mean to use one of its subkeys instead? Possible choices:
+`{key}` is a table of configuration options, but only a single option can be
+overridden with `--config`. Did you want to override one of the table's subkeys?
+
+Possible choices:
 {prefixed_subkeys}"
                     ));
                 }
-            };
-
-            tip.push_str(&format!(
-                "\n\n{}:\n\n{underlying_error}",
-                config_parse_error.description()
-            ));
+            } else {
+                tip.push_str(&format!(
+                    "\n\n{}:\n\n{underlying_error}",
+                    config_parse_error.description()
+                ));
+            }
         }
```

Then it becomes:

```
~/dev/ruff (cli-config)⚡ [2] % cargo run -- check . --config='lint.flake8-pytest-style="csv"'
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.36s
     Running `target/debug/ruff check . '--config=lint.flake8-pytest-style="csv"'`
error: invalid value 'lint.flake8-pytest-style="csv"' for '--config <CONFIG_OPTION>'

  tip: A `--config` flag must either be a path to a `.toml` configuration file
       or a TOML `<KEY> = <VALUE>` pair overriding a specific configuration
       option

`lint.flake8-pytest-style` is a table of configuration options, but only a single option can be
overridden with `--config`. Did you want to override one of the table's subkeys?

Possible choices:
 - `lint.flake8-pytest-style.fixture-parentheses`
 - `lint.flake8-pytest-style.parametrize-names-type`
 - `lint.flake8-pytest-style.parametrize-values-type`
 - `lint.flake8-pytest-style.parametrize-values-row-type`
 - `lint.flake8-pytest-style.raises-require-match-for`
 - `lint.flake8-pytest-style.raises-extend-require-match-for`
 - `lint.flake8-pytest-style.mark-parentheses`

For more information, try '--help'.
```

<details>
<summary>Colorized screenshot</summary>

![image](https://github.com/user-attachments/assets/c49133ab-9506-4b42-aa9f-27634d0bb0ea)

</details>

which feels significantly more readable to me. WDYT?

---

_Comment by @InSyncWithFoo on 2025-01-05 13:40_

@AlexWaygood I was actually aiming for copyability, but readability is fine too.

> [...] but only a single option can be overridden with `--config`.

This is rather misleading, because you <em>can</em> pass an inline table as the value:

```shell
# Same as "lint.select = ['I001']"
$ ruff check --isolated --config="lint = { select = ['I001'] }"
```

---

_Comment by @AlexWaygood on 2025-01-05 13:43_

> This is rather misleading, because you _can_ pass an inline table as the value:

Oh, great point! I forgot how my own feature worked :-) We can scrap that clause, then.

---

_@AlexWaygood approved on 2025-01-05 14:03_

LGTM, thank you!

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-01-05 14:03_

---

_Label `cli` added by @MichaReiser on 2025-01-06 08:43_

---

_@MichaReiser reviewed on 2025-01-06 08:57_

---

_Review comment by @MichaReiser on `crates/ruff/src/args.rs`:984 on 2025-01-06 08:57_

This will not only list fields but also sub-tables, which I think is confusing (at least with how the message is phrased because setting `lint.isort` wouldn't be a correct option either):

```
`lint` is a table of configuration options.
Did you want to override one of the table's subkeys?

Possible choices:

- `lint.allowed-confusables`
- `lint.dummy-variable-rgx`
- `lint.extend-ignore (deprecated)`
- `lint.extend-select`
- `lint.extend-fixable`
- `lint.external`
- `lint.fixable`
- `lint.ignore`
- `lint.extend-safe-fixes`
- `lint.extend-unsafe-fixes`
- `lint.ignore-init-module-imports (deprecated)`
- `lint.logger-objects`
- `lint.select`
- `lint.explicit-preview-rules`
- `lint.task-tags`
- `lint.typing-modules`
- `lint.unfixable`
- `lint.flake8-annotations`
- `lint.flake8-bandit`
- `lint.flake8-boolean-trap`
- `lint.flake8-bugbear`
- `lint.flake8-builtins`
- `lint.flake8-comprehensions`
- `lint.flake8-copyright`
- `lint.flake8-errmsg`
- `lint.flake8-quotes`
- `lint.flake8-self`
- `lint.flake8-tidy-imports`
- `lint.flake8-type-checking`
- `lint.flake8-gettext`
- `lint.flake8-implicit-str-concat`
- `lint.flake8-import-conventions`
- `lint.flake8-pytest-style`
- `lint.flake8-unused-arguments`
- `lint.isort`
- `lint.mccabe`
- `lint.pep8-naming`
- `lint.pycodestyle`
- `lint.pydocstyle`
- `lint.pyflakes`
- `lint.pylint`
- `lint.pyupgrade`
- `lint.per-file-ignores`
- `lint.extend-per-file-ignores`
- `lint.exclude`
- `lint.ruff`
- `lint.preview`

For more information, try '--help'.
```


Using the `Display` output and then reparsing it also seems fragile, considering that we have the structured metadata. 

I'd suggest to add a `collect_fields` method to `OptionSet` that collects the direct children of `set` and then map over the field names. 

---

_@MichaReiser reviewed on 2025-01-06 08:57_

This looks great. I'm a bit worried about the output parsing and I think we can improve the handling if `set` itself has sub-table entries.

---

_@MichaReiser approved on 2025-01-06 13:11_

---

_Review comment by @AlexWaygood on `crates/ruff/tests/lint.rs`:789 on 2025-01-06 13:14_

this comment is irrelevant to this test; I think it was just copied and pasted from the test immediately above

```suggestion
```

---

_Review comment by @AlexWaygood on `crates/ruff/tests/lint.rs`:826 on 2025-01-06 13:14_

```suggestion
```

---

_@AlexWaygood reviewed on 2025-01-06 13:14_

---

_Merged by @AlexWaygood on 2025-01-06 13:20_

---

_Closed by @AlexWaygood on 2025-01-06 13:20_

---

_Branch deleted on 2025-01-06 13:23_

---
