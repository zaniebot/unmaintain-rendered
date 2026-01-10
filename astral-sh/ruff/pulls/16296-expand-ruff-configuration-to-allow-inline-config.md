```yaml
number: 16296
title: "Expand `ruff.configuration` to allow inline config"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - server
assignees: []
merged: true
base: main
head: dhruv/server-configuration
created_at: 2025-02-21T08:35:25Z
updated_at: 2025-02-26T04:49:01Z
url: https://github.com/astral-sh/ruff/pull/16296
synced_at: 2026-01-10T19:49:01Z
```

# Expand `ruff.configuration` to allow inline config

---

_Pull request opened by @dhruvmanila on 2025-02-21 08:35_

## Summary

[Internal design document](https://www.notion.so/astral-sh/In-editor-settings-19e48797e1ca807fa8c2c91b689d9070?pvs=4)

This PR expands `ruff.configuration` to allow inline configuration directly in the editor. For example:

```json
{
	"ruff.configuration": {
		"line-length": 100,
		"lint": {
			"unfixable": ["F401"],
			"flake8-tidy-imports": {
				"banned-api": {
					"typing.TypedDict": {
						"msg": "Use `typing_extensions.TypedDict` instead"
					}
				}
			}
		},
		"format": {
			"quote-style": "single"
		}
	}
}
```

This means that now `ruff.configuration` accepts either a path to configuration file or the raw config itself. It's _mostly_ similar to `--config` with one difference that's highlighted in the following section. So, it can be said that the format of `ruff.configuration` when provided the config map is same as the one on the [playground] [^1].

## Limitations

<details><summary><b>Casing (<code>kebab-case</code> v/s/ <code>camelCase</code>)</b></summary>
<p>


The config keys needs to be in `kebab-case` instead of `camelCase` which is being used for other settings in the editor.

This could be a bit confusing. For example, the `line-length` option can be set directly via an editor setting or can be configured via `ruff.configuration`:

```json
{
	"ruff.configuration": {
        "line-length": 100
    },
    "ruff.lineLength": 120
}
```

#### Possible solution

We could use feature flag with [conditional compilation](https://doc.rust-lang.org/reference/conditional-compilation.html#the-cfg_attr-attribute) to indicate that when used in `ruff_server`, we need the `Options` fields to be renamed as `camelCase` while for other crates it needs to be renamed as `kebab-case`. But, this might not work very easily because it will require wrapping the `Options` struct and create two structs in which we'll have to add `#[cfg_attr(...)]` because otherwise `serde` will complain:

```
error: duplicate serde attribute `rename_all`
  --> crates/ruff_workspace/src/options.rs:43:38
   |
43 | #[cfg_attr(feature = "editor", serde(rename_all = "camelCase"))]
   |                                      ^^^^^^^^^^
```

</p>
</details> 

<details><summary><b>Nesting (flat v/s nested keys)</b></summary>
<p>

This is the major difference between `--config` flag on the command-line v/s `ruff.configuration` and it makes it such that `ruff.configuration` has same value format as [playground] [^1].

The config keys needs to be split up into keys which can result in nested structure instead of flat structure:

So, the following **won't work**:

```json
{
	"ruff.configuration": {
		"format.quote-style": "single",
		"lint.flake8-tidy-imports.banned-api.\"typing.TypedDict\".msg": "Use `typing_extensions.TypedDict` instead"
	}
}
```

But, instead it would need to be split up like the following:
```json
{
	"ruff.configuration": {
		"format": {
			"quote-style": "single"
		},
		"lint": {
			"flake8-tidy-imports": {
				"banned-api": {
					"typing.TypedDict": {
						"msg": "Use `typing_extensions.TypedDict` instead"
					}
				}
			}
		}
	}
}
```

#### Possible solution (1)

The way we could solve this and make it same as `--config` would be to add a manual logic of converting the JSON map into an equivalent TOML string which would be then parsed into `Options`.

So, the following JSON map:
```json
{ "lint.flake8-tidy-imports": { "banned-api": {"\"typing.TypedDict\".msg": "Use typing_extensions.TypedDict instead"}}}
```

would need to be converted into the following TOML string:
```toml
lint.flake8-tidy-imports = { banned-api = { "typing.TypedDict".msg = "Use typing_extensions.TypedDict instead" } }
```

by recursively convering `"key": value` into `key = value` which is to remove the quotes from key and replacing `:` with `=`.

#### Possible solution (2)

Another would be to just accept `Map<String, String>` strictly and convert it into `key = value` and then parse it as a TOML string. This would also match `--config` but quotes might become a nuisance because JSON only allows double quotes and so it'll require escaping any inner quotes or use single quotes.

</p>
</details> 

## Test Plan

### VS Code

**Requires https://github.com/astral-sh/ruff-vscode/pull/702**

**`settings.json`**:
```json
{
  "ruff.lint.extendSelect": ["TID"],
  "ruff.configuration": {
    "line-length": 50,
    "format": {
      "quote-style": "single"
    },
    "lint": {
      "unfixable": ["F401"],
      "flake8-tidy-imports": {
        "banned-api": {
          "typing.TypedDict": {
            "msg": "Use `typing_extensions.TypedDict` instead"
          }
        }
      }
    }
  }
}
```

Following video showcases me doing the following:
1. Check diagnostics that it includes `TID`
2. Run `Ruff: Fix all auto-fixable problems` to test `unfixable`
3. Run `Format: Document` to test `line-length` and `quote-style`

https://github.com/user-attachments/assets/0a38176f-3fb0-4960-a213-73b2ea5b1180

### Neovim

**`init.lua`**:
```lua
require('lspconfig').ruff.setup {
  init_options = {
    settings = {
      lint = {
        extendSelect = { 'TID' },
      },
      configuration = {
        ['line-length'] = 50,
        format = {
          ['quote-style'] = 'single',
        },
        lint = {
          unfixable = { 'F401' },
          ['flake8-tidy-imports'] = {
            ['banned-api'] = {
              ['typing.TypedDict'] = {
                msg = 'Use typing_extensions.TypedDict instead',
              },
            },
          },
        },
      },
    },
  },
}
```

Same steps as in the VS Code test:


https://github.com/user-attachments/assets/cfe49a9b-9a89-43d7-94f2-7f565d6e3c9d

## Documentation Preview


https://github.com/user-attachments/assets/e0062f58-6ec8-4e01-889d-fac76fd8b3c7



[playground]: https://play.ruff.rs

[^1]: This has one advantage that the value can be copy-pasted directly into the playground

---

_Label `server` added by @dhruvmanila on 2025-02-21 08:35_

---

_Comment by @github-actions[bot] on 2025-02-21 08:45_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Renamed from "Expand `ruff.configuration` to allow all config" to "Expand `ruff.configuration` to allow inline config" by @dhruvmanila on 2025-02-21 08:58_

---

_Comment by @InSyncWithFoo on 2025-02-21 10:27_

Does this mean the following existing settings will be deprecated?

* [`exclude`](https://docs.astral.sh/ruff/editors/settings/#exclude)
* [`lineLength`](https://docs.astral.sh/ruff/editors/settings/#linelength)
* [`lint.preview`](https://docs.astral.sh/ruff/editors/settings/#lint_preview)
* [`lint.select`](https://docs.astral.sh/ruff/editors/settings/#select)
* [`lint.extendSelect`](https://docs.astral.sh/ruff/editors/settings/#extendselect)
* [`lint.ignore`](https://docs.astral.sh/ruff/editors/settings/#ignore)
* [`format.preview`](https://docs.astral.sh/ruff/editors/settings/#format_preview)

---

_Comment by @dhruvmanila on 2025-02-24 04:50_

> Does this mean the following existing settings will be deprecated?

No, I don't think so we'll deprecate them. It's similar to how there's `ruff format --line-length 100 .` but the same can be set via `ruff format --config='line-length = 100' .`

---

_@dhruvmanila reviewed on 2025-02-24 05:12_

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/session/settings.rs`:373 on 2025-02-24 05:12_

I'm planning to open a follow-up PR to notify the user that resolving the client settings failed. We currently don't log or notify the user if it fails. For example, `lint.select = ["RUF07", "I001"]` will see that `RUF07` fails and thus ignores this config value which means that `I001` is not selected either.

---

_Comment by @dhruvmanila on 2025-02-24 06:15_

I need to update the documentation but I'll wait to first make sure that this is the approach that we want to take. I'd appreciate getting an initial feedback for the approach, I've documented the limitations along with potential solutions that could be implemented to resolve them.

---

_Marked ready for review by @dhruvmanila on 2025-02-24 06:16_

---

_Review requested from @MichaReiser by @dhruvmanila on 2025-02-24 06:16_

---

_Comment by @MichaReiser on 2025-02-24 12:41_

I'll reply to some of the design questions before taking a look at the code itself. 

> This means that now ruff.configuration accepts either a path to configuration file or the raw config itself. It's mostly similar to --config with one difference that's highlighted in the following section.

One consequence of this is that it won't be possible to specify both a configuration file and individual configuration options, which is different from the CLI. I mainly want to call this out but I think this is an okay limitation because we could always introduce a `configuration-file` option and deprecate `configuration: <path>` in favor of that option.


The casing difference is interesting. I don't think we should change the casing of configuration options because they're then different from what we list in the documentation. Again, I think this limitation is fine. 



> Another would be to just accept Map<String, String> strictly and convert it into key = value and then parse it as a TOML string. This would also match --config but quotes might become a nuisance because JSON only allows double quotes and so it'll require escaping any inner quotes or use single quotes.

I'm not sure I fully understand this limitation. I believe all field and table identifiers have to be string. It's only the values that can be numbers, strings, or entire tables. Could we, therefore, deserialize the settings to `Map<String, Value>` where `Value` is any acceptable [TOML value](https://docs.rs/toml/latest/toml/enum.Value.html)? Doing so would allow writing `"format": { "line-length": 100 }` but also `"format.line-length": 100`


---

_Comment by @dhruvmanila on 2025-02-24 14:54_

> One consequence of this is that it won't be possible to specify both a configuration file and individual configuration options, which is different from the CLI. I mainly want to call this out but I think this is an okay limitation because we could always introduce a `configuration-file` option and deprecate `configuration: <path>` in favor of that option.

Yes, I'm aware of this limitation and as you mention, it wouldn't be too much of an effort to add `configurationFile` option.

> The casing difference is interesting. I don't think we should change the casing of configuration options because they're then different from what we list in the documentation. Again, I think this limitation is fine.

👍 

> I'm not sure I fully understand this limitation. I believe all field and table identifiers have to be string. It's only the values that can be numbers, strings, or entire tables. Could we, therefore, deserialize the settings to `Map<String, Value>` where `Value` is any acceptable [TOML value](https://docs.rs/toml/latest/toml/enum.Value.html)? Doing so would allow writing `"format": { "line-length": 100 }` but also `"format.line-length": 100`

That won't work. So, currently this is what happens:
```
InitializeParams (JSON string) 
  -> Deserialize into json::Map<String, json::Value> (ClientSettings)
    -> Deserialize into toml Table
      -> Deserialize into Options (ResolvedClientSettings)
```

The JSON string in your example would be:
```
"{\"format.quote-style\": \"single\"}"
```
which gets deserialized into a JSON map:
```
{"format.quote-style": "single"}
```
Remember that in JSON all keys are **strings**.

Then, we create a TOML table and deserialize it into `Options`:
```rs
toml::Table::try_from(map)?.try_into::<Options>()?;
```
Here, the `format.quote-style` is still interpreted as `format.quote-style` and not `format = {quote-style = ... }` which then fails stating that there's no key like `format.quote-style` in `Options`.

---

_Comment by @MichaReiser on 2025-02-24 14:59_

> Then, we create a TOML table and deserialize it into Options:

Yeah, I think we would have to do the same as on the CLI where we parse the setting name as well. Simply deserializing into `Options` wont do

---

_Comment by @MichaReiser on 2025-02-24 15:08_

I took a look at Ruff's CLI and it seems to directly deserialize into the Table. I still think that it should be possible, but it may require some post/pre processing on our side. I'd have look closer into how TOML deserializes tables (which are thin wrappers around `HashMap<String. Value>` but it seems to me that it should be possible to automatically do the string unwrapping for the key. 

---

_Comment by @dhruvmanila on 2025-02-24 15:10_

> Yeah, I think we would have to do the same as on the CLI where we parse the setting name as well. Simply deserializing into `Options` wont do

There's a small difference between the CLI and what we have here. The command-line value if already a TOML string which means it can just use `toml::Table::from_str` but here we have a JSON string.

What you're describing is then what I've laid down with "Nesting > Possible solution (1)" section where we'll have to create this TOML string ourselves from the JSON value. Does that make sense? I know it's a bit confusing 😅 

---

_Comment by @dhruvmanila on 2025-02-24 15:18_

(Oops, I didn't see your comment)

> I still think that it should be possible, but it may require some post/pre processing on our side. I'd have look closer into how TOML deserializes tables (which are thin wrappers around `HashMap<String. Value>` but it seems to me that it should be possible to automatically do the string unwrapping for the key.

Fair enough. I tried a few things but that didn't work, I can time box it and look into how the library deserializes tables.

---

_Comment by @MichaReiser on 2025-02-24 15:54_

Okay. I played a bit with `toml` and the `toml-edit` crates and I now understand the problem. The issue is that `format.line-length` needs to deserialize to a nested hash map. I feel like it should be possible somehow to drive the toml `Table` deserializer to make this work but it's not obvious how. I don't think it's worth investing more time into this and using JSON is fine then. 

Do you want me to review the code as well or did you mainly want to get feedback on the config format?

---

_Comment by @dhruvmanila on 2025-02-24 16:58_

> Okay. I played a bit with `toml` and the `toml-edit` crates and I now understand the problem. The issue is that `format.line-length` needs to deserialize to a nested hash map. I feel like it should be possible somehow to drive the toml `Table` deserializer to make this work but it's not obvious how. I don't think it's worth investing more time into this and using JSON is fine then.

Thank you for spending time looking into it.

I also think this could possibly be done in the future if we get user feedback on this specific format issue because it would still be backwards compatible in that both nested and flat keys would be supported but right now it's only nested keys.

> Do you want me to review the code as well or did you mainly want to get feedback on the config format?

I mainly wanted to get feedback on the format. I'm planning to work on the documentation part tomorrow morning. I think it be would best to defer the review for tomorrow to save review cycle.

---

_Review request for @MichaReiser removed by @dhruvmanila on 2025-02-25 04:28_

---

_Comment by @MichaReiser on 2025-02-25 07:39_

I hope you don't mind if I put this back into draft. It makes it easier for me to see when I'm supposed to review it.

---

_Converted to draft by @MichaReiser on 2025-02-25 07:39_

---

_Marked ready for review by @dhruvmanila on 2025-02-25 08:00_

---

_Review requested from @MichaReiser by @dhruvmanila on 2025-02-25 08:00_

---

_Review comment by @MichaReiser on `docs/editors/settings.md`:88 on 2025-02-25 09:46_

(directly-) nested tabs feels a bit odd to me. I'm leaning towards using two sections here instead, one for inline configuration and one with an external file. It does use more space but I do find it less confusing. 

---

_Review comment by @MichaReiser on `crates/ruff_server/src/session/index/ruff_settings.rs`:398 on 2025-02-25 09:48_

What's the reason for using `context` + `unwrap_err`? Does it give us nicer diagnostic rendering? Could we use `tracing::error!("Unable to load editor-specified inline configuration: {:?}")` instead?

---

_Review comment by @MichaReiser on `crates/ruff_server/src/session/settings.rs`:82 on 2025-02-25 09:52_

It's unclear to me where we serialize the value to TOML. I understand deserialization but I don't see where we perform any serialization. 

I worry that this error message is also not very helpful for users because it's not clear what's going wrong. Can we use a more specific error message? *Failed to load configuration from `ruff.configurations`*:

---

_Review comment by @MichaReiser on `crates/ruff_server/src/session/settings.rs`:86 on 2025-02-25 09:53_

```suggestion
    #[error("using `extend` is unsupported in `ruff.configuration`")]
```

---

_Review comment by @MichaReiser on `crates/ruff_server/src/session/settings.rs`:369 on 2025-02-25 09:54_

I think we should mention `ruff.configuration` here to guide users towards how to fix this error. It's otherwise unclear what configuration the LSP is referring to and how it can be fixed.

---

_Review comment by @MichaReiser on `docs/editors/settings.md`:14 on 2025-02-25 09:55_

```suggestion
The `configuration` setting allows you to configure editor-specific Ruff.
```

---

_Review comment by @MichaReiser on `docs/editors/settings.md`:21 on 2025-02-25 10:01_

I'm not sure if referencing the playground is useful for our users. Most people have never used the playground before. It's very likely that they've never heard about it before. 

That's why I'd suggest removing the reference for now. We can add a more detailed documentation explaining how our configuration in JSON if this proves to be a problem.

---

_Review comment by @MichaReiser on `docs/editors/settings.md`:26 on 2025-02-25 10:02_

```suggestion
    The **Inline JSON configuration** option was introduced in Ruff `0.9.8`.
```

I think this is enough, considering that it was explained right above

---

_Review comment by @MichaReiser on `docs/editors/settings.md`:29 on 2025-02-25 10:09_

I find this explanation a bit confusing for two reasons:

* You say **Ruff** will automatically detect.. consistent with the behavior of the *Ruff CLI*. What I find confusing about it is that it suggests that the editor integration is the "core" Ruff and the Ruff CLI is some form of an extension
* It's unclear to me what the project's filesystem is. 

The second item seems an issue inherent to the naming of our options. Naming the option `projectFirst` might have been more intuitive

Maybe;

The default behavior, if `configuration` is unset, is to load the settings from the project's configuration (a `ruff.toml` or `pyproject.toml` in the project's directory), consistent with when running Ruff on the CLI. 

---

_Review comment by @MichaReiser on `docs/editors/settings.md`:33 on 2025-02-25 10:11_

```suggestion
When both an editor-provided configuration and a local configuration file are present, you can
control how Ruff resolves conflicts between them using the
The [`configurationPreference`](#configurationpreference) controls the precedence if both an editor-provided configuration (`ruff.configuration`) and a project level configuration are present.
```

---

_Review comment by @MichaReiser on `docs/editors/settings.md`:46 on 2025-02-25 10:11_

```suggestion
    project's directory (if present)
```

---

_Review comment by @MichaReiser on `docs/editors/settings.md`:49 on 2025-02-25 10:12_

```suggestion
For example, if the line length is specified in all three sources, Ruff will use the value from the [`lineLength`](#linelength) setting.
```

---

_@MichaReiser approved on 2025-02-25 10:12_

Thanks. This looks great. I've mainly some documentation and error handling suggestions.

---

_Comment by @MichaReiser on 2025-02-25 10:16_

One noteworthy drawback of having a single option for `configuration` is that setting the path is no longer possible from the Settings UI. 

---

_Comment by @dhruvmanila on 2025-02-25 11:45_

> One noteworthy drawback of having a single option for `configuration` is that setting the path is no longer possible from the Settings UI. 

Yeah, that's a good point. Maybe it might be worth adding the `configurationFile` option along with this change. Let me think about it.

I was also wondering if something like `ruff.experimental` is worth using such that we can first add `ruff.experimental.configuration` to allow initial feedback and then later merge it. It does raise questions on when can we promote a feature out of experimental phase because it's going to be tied up with the Ruff version. It might just be too complicated and not worth it.

---

_@dhruvmanila reviewed on 2025-02-25 12:26_

---

_Review comment by @dhruvmanila on `docs/editors/settings.md`:88 on 2025-02-25 12:26_

Yeah, it does seem simpler. Thanks for the suggestion!

<img width="2560" alt="Screenshot 2025-02-25 at 5 56 30 PM" src="https://github.com/user-attachments/assets/8a609655-7e7e-4edd-8366-3b9f562bf03c" />



---

_@dhruvmanila reviewed on 2025-02-25 12:34_

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/session/index/ruff_settings.rs`:398 on 2025-02-25 12:34_

I mainly used it to avoid having long messages on a single line. So, for an invalid configuration when provided the path.

With `context`:

```
   0.024785292s ERROR ThreadId(11) ruff_server::session::index::ruff_settings: Unable to load editor-specified configuration file

Caused by:
    0: Failed to load configuration `/tmp/ruff-config-test/pyproject.toml`
    1: Failed to parse /tmp/ruff-config-test/pyproject.toml
    2: TOML parse error at line 2, column 15
         |
       2 | line-length = "90"
         |               ^^^^
       invalid type: string "90", expected a nonzero u16
```

Without `context`:
```
   0.040345750s ERROR ThreadId(09) ruff_server::session::index::ruff_settings: Unable to load editor-specified configuration file: Failed to load configuration `/tmp/ruff-config-test/pyproject.toml`

Caused by:
    0: Failed to parse /tmp/ruff-config-test/pyproject.toml
    1: TOML parse error at line 2, column 15
         |
       2 | line-length = "90"
         |               ^^^^
       invalid type: string "90", expected a nonzero u16
```

It might be that the additional text isn't required given it already starts out with `Failed to load configuration ...:`

---

_@dhruvmanila reviewed on 2025-02-25 12:36_

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/session/index/ruff_settings.rs`:398 on 2025-02-25 12:36_

Similarly, for inline configuration:

With `context`:
```
   0.011989375s ERROR ThreadId(10) ruff_server::session::index::ruff_settings: Unable to load editor-specified inline configuration

Caused by:
    Invalid `cache-dir` value: error looking key 'RANDOM' up: environment variable not found
```

Without `context`:
```
 0.018654333s ERROR ThreadId(09) ruff_server::session::index::ruff_settings: Unable to load editor-specified inline configuration: Invalid `cache-dir` value: error looking key 'RANDOM' up: environment variable not found
```

---

_@dhruvmanila reviewed on 2025-02-25 12:38_

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/session/index/ruff_settings.rs`:398 on 2025-02-25 12:38_

I'll avoid using `context` for inline configuration but will keep it for file configuration for now.

---

_@dhruvmanila reviewed on 2025-02-25 12:39_

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/session/settings.rs`:82 on 2025-02-25 12:39_

The serialization of `map` to TOML happens here (`try_from`):

https://github.com/astral-sh/ruff/blob/c60924d52e41ea897e03fe9db8d1cefdae94fd96/crates/ruff_server/src/session/settings.rs#L65-L65

I'll update the error message

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/session/settings.rs`:86 on 2025-02-25 12:39_

I think `ruff.configuration` is specific to VS Code, the server setting is just `configuration` so will use that instead.

---

_@dhruvmanila reviewed on 2025-02-25 12:39_

---

_Comment by @MichaReiser on 2025-02-25 12:51_

> Yeah, that's a good point. Maybe it might be worth adding the configurationFile option along with this change. Let me think about it.

I'm fine deferring it for now. I just thought it worth calling out. I expect that many users will want to use the inline config anyway

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/session/settings.rs`:82 on 2025-02-25 15:21_

I'm going to keep this message for now. The "Failed to load ..." message that you've suggested is already going to be appended before this. The reason I want to keep this is that for something like:
```json
  "ruff.configuration": {
    "line-length": null
  },
```
We'll get:
```
Failed to load settings from `configuration`: unsupported unit type
```
But, with the above message:
```
Failed to load settings from `configuration`: error serializing configuration to TOML: unsupported unit type
```
We'll atleast know that the issue is with TOML deserialization

---

_@dhruvmanila reviewed on 2025-02-25 15:21_

---

_@dhruvmanila reviewed on 2025-02-25 15:22_

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/session/settings.rs`:86 on 2025-02-25 15:22_

Actually, this is only relevant for inline configuration specifically as `extend` is allowed when providing file path. I'll leave this as is.

---

_@dhruvmanila reviewed on 2025-02-25 15:23_

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/session/settings.rs`:369 on 2025-02-25 15:23_

I've updated this to use "Failed to load settings from `configuration`: {err}"

---

_@dhruvmanila reviewed on 2025-02-25 15:24_

---

_Review comment by @dhruvmanila on `docs/editors/settings.md`:14 on 2025-02-25 15:24_

I don't think I follow with what you mean with "... to configure editor-specific Ruff." ?

---

_@dhruvmanila reviewed on 2025-02-25 15:36_

---

_Review comment by @dhruvmanila on `docs/editors/settings.md`:29 on 2025-02-25 15:36_

I've incorporated this suggestion, thanks

---

_@MichaReiser reviewed on 2025-02-25 16:31_

---

_Review comment by @MichaReiser on `crates/ruff_server/src/session/settings.rs`:82 on 2025-02-25 16:31_

> Failed to load settings from `configuration`: error serializing configuration to TOML: unsupported unit type

I don't think I would understand this message or even know what I have to do.

---

_@MichaReiser reviewed on 2025-02-25 16:32_

---

_Review comment by @MichaReiser on `docs/editors/settings.md`:14 on 2025-02-25 16:32_

oh, there's a missing *behavior* at the end.

---

_@dhruvmanila reviewed on 2025-02-26 04:20_

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/session/settings.rs`:82 on 2025-02-26 04:20_

I think one solution would be to handle these `null` cases because that's the only case where I'm able to get this serialization error. I checked the spec as well (https://toml.io/en/v1.0.0#keys) but can't get the `InvalidToml` error

---

_@dhruvmanila reviewed on 2025-02-26 04:25_

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/session/settings.rs`:82 on 2025-02-26 04:25_

We could skip the intermediate JSON deserialization but that would yield to even worse experience:
```
error: Failed to deserialize initialization options: data did not match any variant of untagged enum InitializationOptions. Falling back to default client settings...
```

---

_Merged by @dhruvmanila on 2025-02-26 04:47_

---

_Closed by @dhruvmanila on 2025-02-26 04:47_

---

_Branch deleted on 2025-02-26 04:47_

---
