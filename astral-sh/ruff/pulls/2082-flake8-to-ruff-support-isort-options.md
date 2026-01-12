```yaml
number: 2082
title: "flake8_to_ruff: support `isort` options"
type: pull_request
state: merged
author: shannonrothe
labels: []
assignees: []
merged: true
base: main
head: flake8-to-ruff-isort
created_at: 2023-01-22T04:42:36Z
updated_at: 2023-01-22T18:18:20Z
url: https://github.com/astral-sh/ruff/pull/2082
synced_at: 2026-01-12T15:55:07Z
```

# flake8_to_ruff: support `isort` options

---

_@shannonrothe_

Hey @charliermarsh 👋🏼 I've been learning Rust for a little bit but haven't contributed any PRs until this one, so excuse me if it's a little rough. I mostly followed what was already in place for picking up `Black` options in the `flake8_to_ruff` converter.

For the `isort` options, I've chosen to support only `src_paths` for now as a simple starting point (this could be extended upon).

Admittedly, I'm not super familiar with the Python ecosystem and the associated tools (isort/Black), so let me know if I'm missing anything obvious 👌🏼 

See: https://github.com/charliermarsh/ruff/issues/1749

---

_@not-my-profile reviewed on 2023-01-22 04:58_

---

_Review comment by @not-my-profile on `src/flake8_to_ruff/isort.rs`:8 on 2023-01-22 04:58_

left over comment?

---

_@not-my-profile reviewed on 2023-01-22 04:59_

---

_Review comment by @not-my-profile on `src/flake8_to_ruff/isort.rs`:11 on 2023-01-22 04:59_

I think it would make sense to add a doc comment (a comment starting with `///`) linking the isort config documentation, so e.g.

```
/// The [isort configuration](https://pycqa.github.io/isort/docs/configuration/config_files.html).
```

---

_Review comment by @shannonrothe on `src/flake8_to_ruff/converter.rs`:28 on 2023-01-22 05:03_

Would you prefer if this was combined with the `black` argument above as a single argument? Maybe something like a `(Option<&Black>, Option<&ISort>)` tuple? Or maybe even a new struct:

---

_@shannonrothe reviewed on 2023-01-22 05:03_

---

_@shannonrothe reviewed on 2023-01-22 05:04_

---

_Review comment by @shannonrothe on `src/flake8_to_ruff/isort.rs`:8 on 2023-01-22 05:04_

Yep, copy-paste from `black.rs` and forgot the remove 👍🏼 

---

_Review comment by @charliermarsh on `src/flake8_to_ruff/isort.rs`:11 on 2023-01-22 05:10_

It's a nit but I think this can be `Isort` (rather than `ISort`).

---

_Review comment by @charliermarsh on `src/flake8_to_ruff/converter.rs`:28 on 2023-01-22 05:11_

I think what you have here is totally fine! A struct would also be reasonable, but honestly this is simple and clear so not sure that'd even be preferable.

---

_@not-my-profile reviewed on 2023-01-22 05:11_

---

_Review comment by @not-my-profile on `src/flake8_to_ruff/converter.rs`:28 on 2023-01-22 05:11_

Using tuples like that isn't really idiomatic in Rust. Introducing a struct with named fields would however probably be a good idea if we assume that more such config structs will be introduced in the future ... maybe `struct ToolConfigs { black: Option<&Black>, isort: Option<&ISort> }`?

---

_@charliermarsh reviewed on 2023-01-22 05:11_

Thanks for getting involved!

---

_Review comment by @shannonrothe on `src/flake8_to_ruff/converter.rs`:28 on 2023-01-22 05:11_

Alternatively, what about a new struct to hold "external" options:

```rust
pub struct ExternalOptions {
  pub black: Option<Black>,
  pub isort: Option<ISort>,
}
```

Then `convert` could take `Option<ExternalOptions>` instead of `Option<&Black>` and it would be easy to extend upon for future "external" tools.

**Edit**: beat me to it 🙈 happy to go with either option, what would you prefer @charliermarsh?

---

_@shannonrothe reviewed on 2023-01-22 05:11_

---

_@charliermarsh reviewed on 2023-01-22 05:13_

---

_Review comment by @charliermarsh on `src/flake8_to_ruff/converter.rs`:28 on 2023-01-22 05:13_

Yeah what @not-my-profile suggested is good too. Either way.

(I'm not sure what additional configuration options we'd respect in the future, though it's plausible that e.g. we could grab the target Python version.)

---

_@shannonrothe reviewed on 2023-01-22 05:17_

---

_Review comment by @shannonrothe on `src/flake8_to_ruff/converter.rs`:28 on 2023-01-22 05:17_

Great, pushed code with `ToolConfigs` option here: https://github.com/charliermarsh/ruff/pull/2082/commits/37a87df1ca5d96122db130509d34501a97f04b43

---

_@not-my-profile reviewed on 2023-01-22 05:17_

---

_Review comment by @not-my-profile on `src/flake8_to_ruff/converter.rs`:28 on 2023-01-22 05:17_

(`ExternalConfig` is probably a better name than `ToolConfigs`) ... I wouldn't name it `ExternalOptions` since it contains the `Option` type which could be a bit confusing^^

---

_@shannonrothe reviewed on 2023-01-22 05:17_

---

_Review comment by @shannonrothe on `src/flake8_to_ruff/tool_configs.rs`:9 on 2023-01-22 05:17_

Does it matter that the struct takes ownership here of `Black` and `Isort` respectively? Ideally these could be references but I had a hard time making that play nice in `parse_tool_configs` below 👇🏼 

---

_@charliermarsh reviewed on 2023-01-22 05:18_

---

_Review comment by @charliermarsh on `src/flake8_to_ruff/tool_configs.rs`:15 on 2023-01-22 05:18_

Should these both take `path.as_ref()`? (Does using `path` for both, without `.as_ref()`, lead to a borrowing error?)

---

_Comment by @charliermarsh on 2023-01-22 05:19_

For posterity: relevant issue is #1749.

---

_@shannonrothe reviewed on 2023-01-22 05:19_

---

_Review comment by @shannonrothe on `src/flake8_to_ruff/converter.rs`:28 on 2023-01-22 05:19_

Renamed here: https://github.com/charliermarsh/ruff/pull/2082/commits/aa15c6ad0361769938ae536cad5fb584a2054630

---

_Comment by @shannonrothe on 2023-01-22 05:20_

> For posterity: relevant issue is #1749.

Added to description 👌🏼 

---

_@shannonrothe reviewed on 2023-01-22 05:21_

---

_Review comment by @shannonrothe on `src/flake8_to_ruff/tool_configs.rs`:15 on 2023-01-22 05:21_

Potentially yes, I'm guessing if more external configurations were added here, we'd need to `.as_ref()` all except the last.

Without `.as_ref()` I get the following in `parse_external_config`:

<img width="833" alt="image" src="https://user-images.githubusercontent.com/803013/213901866-abbe98ef-7d43-450f-96ca-0c38b75e6e3e.png">


---

_Review comment by @charliermarsh on `src/flake8_to_ruff/tool_configs.rs`:9 on 2023-01-22 05:22_

Not a big deal. You could probably introduce a lifetime to do something like:

```rs
pub struct ToolConfigs<'a> {
    pub black: Option<&'a Black>,
    pub isort: Option<&'a Isort>,
}
```

But then you'd have to ensure that the values outlive this struct. So you'd need to parse the values at a level above `parse_tool_configs` (like, in `main.rs`), then pass them to `ToolConfigs` as references.

---

_@charliermarsh reviewed on 2023-01-22 05:22_

---

_@shannonrothe reviewed on 2023-01-22 05:38_

---

_Review comment by @shannonrothe on `src/flake8_to_ruff/tool_configs.rs`:9 on 2023-01-22 05:38_

Added lifetime in https://github.com/charliermarsh/ruff/pull/2082/commits/8c8c497b97b7eb87a8afe67d9d3876ae1559f6d9. Happy to revert that commit if the previous approach is preferable too 👍🏼 

---

_Comment by @not-my-profile on 2023-01-22 05:43_

I am not sure if this PR should close #1749 given that isort also has other settings not implemented by this PR.

---

_Comment by @shannonrothe on 2023-01-22 05:45_

> I am not sure if this PR should close #1749 given that isort also has other settings not implemented by this PR.

Fair enough, happy to remove from the description.

Another question - we could probably build off this PR to alleviate https://github.com/charliermarsh/ruff/issues/1567 right? Is there a counterpart option in Ruff for the `Line-Length` isort option?

---

_@shannonrothe reviewed on 2023-01-22 05:46_

---

_Review comment by @shannonrothe on `src/flake8_to_ruff/isort.rs`:23 on 2023-01-22 05:46_

Should we consolidate these with the same structs defined in `black.rs` too?

---

_Comment by @charliermarsh on 2023-01-22 05:47_

Ah yeah, sorry, it shouldn't close #1749 -- it should just reference it, like "See: #1749" or similar.

---

_@charliermarsh reviewed on 2023-01-22 05:48_

---

_Review comment by @charliermarsh on `src/flake8_to_ruff/isort.rs`:23 on 2023-01-22 05:48_

Yeah we might as well. We can have a single `Pyproject` and `Tools`, with `Isort` and `Black` structs defined in their respective files.

---

_@shannonrothe reviewed on 2023-01-22 05:50_

---

_Review comment by @shannonrothe on `src/flake8_to_ruff/isort.rs`:23 on 2023-01-22 05:50_

Consolidated here: https://github.com/charliermarsh/ruff/pull/2082/commits/0780fa0f4c66a6fc5fbb0bc0f1fd2f474db2273f

---

_Comment by @charliermarsh on 2023-01-22 05:52_

> Another question - we could probably build off this PR to alleviate https://github.com/charliermarsh/ruff/issues/1567 right? Is there a counterpart option in Ruff for the Line-Length isort option?

It's actually a bit different -- that issue references a setting that tells isort to sort the imports by their string length. isort _does_ have a [line-length](https://pycqa.github.io/isort/docs/configuration/options.html#line-length) setting, and we _should_ extract that and pass it along to Ruff (we have a top-level option for that), but it wouldn't close that issue :)

---

_Comment by @charliermarsh on 2023-01-22 05:53_

Relatedly, everything in [this section](https://github.com/charliermarsh/ruff#isort) of the configuration options can be extracted from the `isort` section in a `pyproject.toml`, like you're doing here for `src`.

---

_Review comment by @charliermarsh on `src/flake8_to_ruff/black.rs`:20 on 2023-01-22 05:56_

I think we should have one `parse` function that returns `ExternalConfig`. Right now, we're parsing the TOML twice into `Pyproject`, then extracting `tool.black` and `tool.isort` respectively, then merging them back into a single struct.

---

_@charliermarsh reviewed on 2023-01-22 05:56_

---

_@charliermarsh reviewed on 2023-01-22 05:57_

---

_Review comment by @charliermarsh on `src/flake8_to_ruff/black.rs`:20 on 2023-01-22 05:57_

(Or, one parse function that returns `Pyproject` to `main.rs`, then create `ExternalConfig` from `Pyproject`.)

---

_Review comment by @shannonrothe on `src/flake8_to_ruff/black.rs`:20 on 2023-01-22 06:23_

Ah yep that makes sense.

`Tools` has to own `black`/`isort` presumably for `serde` (de)serialization. I'm trying to do something like:

```rust
pub fn parse<'a, P: AsRef<Path>>(path: P) -> Result<ExternalConfig<'a>> {
    let contents = std::fs::read_to_string(path)?;
    let pyproject = toml_edit::easy::from_str::<Pyproject>(&contents)?;
    Ok(pyproject
        .tool
        .map(|tool| ExternalConfig {
            black: tool.black.as_ref(),
            isort: tool.isort.as_ref(),
        })
        .unwrap_or_default())
}
```

but the `tool.{black,isort}.as_ref()` lines complain because we're referencing `tool.{black,isort}` which is not owned by the current function. Any ideas? :)

Or, alternatively, if `flake8_to_ruff::parse` returns `Pyproject` and we try and produce an `ExternalConfig` from `main.rs`:

```rust
let pyproject = cli.pyproject.map(flake8_to_ruff::parse).transpose()?;
let external_config = pyproject
    .map(|pyproject| {
        pyproject
            .tool
            .map(|tool| ExternalConfig {
                // these lines aren't happy
                black: tool.black.as_ref(),
                isort: tool.isort.as_ref(),
            })
            .unwrap_or_default()
    })
    .unwrap_or_default();
```

---

_@shannonrothe reviewed on 2023-01-22 06:23_

---

_@not-my-profile reviewed on 2023-01-22 06:39_

---

_Review comment by @not-my-profile on `src/flake8_to_ruff/black.rs`:20 on 2023-01-22 06:39_

The signature doesn't make sense:

```rs
pub fn parse<'a, P: AsRef<Path>>(path: P) -> Result<ExternalConfig<'a>> {
```

The liftetime in the return type has to come from somewhere ... you can either change the input to `text: &'a str` or make the return type owned.

---

_@shannonrothe reviewed on 2023-01-22 06:52_

---

_Review comment by @shannonrothe on `src/flake8_to_ruff/black.rs`:20 on 2023-01-22 06:52_

Sorry @not-my-profile, I've changed `flake8_to_ruff::parse` to return `Pyproject` instead, which circumvents the above issue.

The current issue is mapping out of `Tools` (where `black`/`isort` are owned) into `ExternalConfig` (where `black`/`isort` are references).

---

_Review comment by @shannonrothe on `src/flake8_to_ruff/black.rs`:20 on 2023-01-22 07:00_

This seems to work, but is there a preferred (more idiomatic) way?

```rust
let mut external_config = ExternalConfig::default();
let pyproject = cli.pyproject.map(flake8_to_ruff::parse).transpose()?;
if let Some(pyproject) = &pyproject {
    if let Some(tool) = &pyproject.tool {
        external_config = ExternalConfig {
            black: tool.black.as_ref(),
            isort: tool.isort.as_ref(),
        };
    }
}
```

**Edit**: maybe this?

```rust
let external_config = pyproject
    .as_ref()
    .map(|pyproject| {
        pyproject
            .tool
            .as_ref()
            .map(|tool| ExternalConfig {
                black: tool.black.as_ref(),
                isort: tool.isort.as_ref(),
            })
            .unwrap_or_default()
    })
    .unwrap_or_default();
```

---

_@shannonrothe reviewed on 2023-01-22 07:00_

---

_@shannonrothe reviewed on 2023-01-22 07:04_

---

_Review comment by @shannonrothe on `src/flake8_to_ruff/black.rs`:20 on 2023-01-22 07:04_

Implemented suggestions here @charliermarsh: https://github.com/charliermarsh/ruff/pull/2082/commits/4127eb451877e3a687b4c74103dce99f11ceca69 👍🏼 

---

_@charliermarsh reviewed on 2023-01-22 16:51_

---

_Review comment by @charliermarsh on `src/flake8_to_ruff/black.rs`:20 on 2023-01-22 16:51_

Awesome -- will review in a bit.

---

_Merged by @charliermarsh on 2023-01-22 18:18_

---

_Closed by @charliermarsh on 2023-01-22 18:18_

---

_Review comment by @charliermarsh on `src/flake8_to_ruff/black.rs`:20 on 2023-01-22 18:18_

Thank you! I tweaked it a little bit to remove some levels of nesting by using `.and_then`.

---

_@charliermarsh reviewed on 2023-01-22 18:18_

---
