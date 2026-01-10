```yaml
number: 5102
title: Configuration via JSON
type: issue
state: closed
author: tgross35
labels:
  - configuration
  - cli
assignees: []
created_at: 2023-06-14T21:58:35Z
updated_at: 2023-07-10T08:29:04Z
url: https://github.com/astral-sh/ruff/issues/5102
synced_at: 2026-01-10T11:09:47Z
```

# Configuration via JSON

---

_Issue opened by @tgross35 on 2023-06-14 21:58_

This is my second (and I think improved) take on https://github.com/astral-sh/ruff/issues/4970

What would thoughts be on accepting a parameter `--config-json` that takes identical configuration to `ruff.toml`? Example:

```sh
ruff . --config-json '{"per-file-ignores":{"__init__.py":["E402"],"path/to/file.py":["E402"]},"ruff":{"ignore":["E501"],"select":["E","F","B"],"unfixable":["B"]}}'
```

Reasoning:

- It would be possible to invoke `ruff` from tools that are unaware of ruff's CLI format or schema. For example, a `cargo-ruff` tool could (1) grab the `package.metadata.ruff` table from `Cargo.toml`, (2) naively reserialize it as JSON, no validation required, (3) invoke ruff with the CLI option. Same for a NPM tool that uses config in `package.json`, or something that uses legacy `tox.ini` or `setup.cfg`
- There is minimal maintenance overhead: since the schema is identical to the `toml` version, the same structs can be used with serde and no additional documentation is required
- If there is ever highly nested or advanced configuration that can be specified in `ruff.toml`/`pyproject.toml` but is difficult to supply via CLI arguments, this would provide a workaround for CI tools

---

_Comment by @charliermarsh on 2023-06-14 22:06_

I think this does make sense, and it's come up at least once before.

---

_Comment by @charliermarsh on 2023-06-14 22:09_

It would help with this too: https://github.com/astral-sh/ruff/issues/3694.

---

_Comment by @tgross35 on 2023-06-14 22:11_

Good point with #3694 - I can take a try at this sometime this week if you're OK with what I described

---

_Comment by @charliermarsh on 2023-06-14 22:13_

I'm supportive of it! You can probably look at how the `config` command-line argument is treated (which takes a path to a configuration file), and mostly follow the same pattern.

(Disclaimer that I'll want to get @MichaReiser's take prior to merging since he's thought about the future of configuration -- he's on vacation but will be back next week.)

---

_Label `configuration` added by @charliermarsh on 2023-06-14 22:14_

---

_Label `cli` added by @charliermarsh on 2023-06-14 22:14_

---

_Comment by @tgross35 on 2023-06-14 22:16_

Sounds great! I will jump on this

---

_Comment by @MichaReiser on 2023-06-19 08:37_

I think being able to set any configuration value via the CLI would have two benefits:

* Reduce the explicit need to add a CLI argument for every configuration option
* Allow other tools to *generate* the configuration on the fly and pass it to ruff. 

The downside I see of using `--config-json` is that:

* Writting JSON is cumbersome and some users may prefer YAML, meaning we would need to support different input formats
* It replaces any existing configuration. Thus, it doesn't help to reduce the amount of CLI arguments that ruff must support to allow customizing all configuration options via the CLI.

@tgross35 what would you think if we instead use an approach similar to `cargo` that allows you to override every key in the configuration:

```
--config <KEY=VALUE>      Override a configuration value
```

We would need to come up with a different name because ruff already uses `--config`. 

I think this should work for your use case too, because you can iterate over the toml and create one `--config` argument for each *terminal* key in the configuration. 

One issue that could potentially come up is that this approach fails for very large configurations where it would be necessary to pass a few thousands of lines of key value pairs. 


---

_Comment by @tgross35 on 2023-06-19 17:54_

> The downside I see of using `--config-json` is that:
> 
> * Writting JSON is cumbersome and some users may prefer YAML, meaning we would need to support different input formats
> * It replaces any existing configuration. Thus, it doesn't help to reduce the amount of CLI arguments that ruff must support to allow customizing all configuration options via the CLI.

I don't think there is any need to support YAML or anything else. As I see it, this option would be available for other tools to run `ruff` using their configuration - such as in #3694 where they want to configure via python (they could simply `json.dumps` a dict to get a valid `--config-json` CLI option). Or for other tools that use JSON, YAML, or TOML configuration - that tool would be responsible for converting YAML to JSON, but ruff has no need of burdening itself with anything other than a JSON representation (which comes for free with the `serde` structs used for TOML).

> @tgross35 what would you think if we instead use an approach similar to `cargo` that allows you to override every key in the configuration:
> 
> ```
> --config <KEY=VALUE>      Override a configuration value
> ```
> 
> We would need to come up with a different name because ruff already uses `--config`.

This might be a good idea if you are looking for ways to ease user configuration, but I think it is clunkier for other tools - because now you need to take configuration that is easily serializable to JSON (as is the case for TOML and YAML) and manually split it up, remove quotes, convert lists to CSV, recurse this for all nesting options, etc. And then do the opposite on the `ruff` side. Not that that's impossible, but it seems like more trouble than it's worth when the TOML<->JSON and YAML<->JSON conversion is already well defined.

---

_Comment by @MichaReiser on 2023-06-19 20:16_

Have you considered creating a (named) temp file and passing that path to Ruff? It seems that unlocks the functionality you want without requiring any changes on Ruff's side. 

The reason I'm heistant of adding `--config-json` is that it only solves a very particular use case and may not be useful to many other users. Each added option makes ruff more difficult to use (mental overload because you get hit by a wall of options). That's why I be more in favor of adding an option that uses multiple purposes like cargo's config. I can see how this is more difficult to use from a tooling perspective because you need to flatten lists (specify the same key multiple times) and correctly escape values as you pointed out. I wonder if it would be possible to use `jq` and `yq` to perform this conversation. 

---

_Comment by @MichaReiser on 2023-06-19 20:24_

I did some more research and the only tool that I found that supports something similar is Jest where `--config` either accepts a path or a JSON encoded value. We could probably support this but I'm still leaning towards this being a very specific use case.

---

_Comment by @tgross35 on 2023-06-22 07:15_

> Have you considered creating a (named) temp file and passing that path to Ruff? It seems that unlocks the functionality you want without requiring any changes on Ruff's side.

The named temp file is the current solution, but it is unfortunately quite cumbersome.

> Each added option makes ruff more difficult to use (mental overload because you get hit by a wall of options). That's why I be more in favor of adding an option that uses multiple purposes like cargo's config. 

Could the option be hidden from the `--help` output?

In any case, I don't think `--config-json` option would be any more of a mental overload than `--config KEY=VALUE`. It's a well defined & understood schema that uses the identical config to `ruff.toml` (just put a `ruff.toml` into https://pseitz.github.io/toml-to-json-online-converter/ and you would have the `--config-json` argument). For any sort of `KEY=VALUE` configuration, the user needs to read the docs to figure out how to do the things like lists or nested objects - not a bad thing to have, but not as intuitive when you're trying to run from another program.

Samples of the use cases I am imagining, which could be documented examples:

```rust
//! A `cargo-ruff` tool that would run ruff upon running `cargo ruff`, with config in
//! `[package.metadata.ruff]` in `Cargo.toml`

fn main() {
    let manifest_path = // ... use cargo_metadata to extract
    let contents = std::fs::read_to_string(manifest_path)?;
    let v: Value = serde_json::from_str(contents)?;
    let cfg = v["package"]["metadata"]["ruff"];
    Command::new("ruff").arg("--config-json").arg(cfg.to_str()).status();
}
```

```make
# Makefile for C+Py projects where some configuration exists in a YAML file

lint:
    ruff --config-json $(yq '.ruff-config' project-config.yaml)
```

```json5
// A NPM project with a few python files (very similar to above)
{
  "name": "demo",
  // ...
  "scripts": {
    "lint:py": "ruff $(jq '.ruff-config' package.json)"
  },
  "ruff-config":
    "per-file-ignores": {
      "__init__.py": ["E402"],
      "path/to/file.py": ["E402"]
    }
}
```

```py
# Python just needs to construct a dict to run `ruff`

cfg = {"extend-exclude": [f for f in skip_files]}
subprocess.run(["ruff", "--config-json", json.dumps(cfg)])
```

```rust
// The ruff changes would only be a few dozen lines: more or
// less just the below

if let Some(cfg) = args.config_json {
    return serde_json::from_str::<Options>()?;
}
```

All of those are simple yet powerful use cases in 1-3 lines -  super easy to add to any project. Any sort of custom `KEY=VALUE` schema turns any of the above into multiple lines, not something you could easily put in a `package.json` script.

---

_Comment by @MichaReiser on 2023-07-10 08:29_

Thank you for adding the additional detail and I see why a temporary file doesn't work well for a script in `package.json`. 

We discussed this further internally. We do see the value of it and may decide to support it eventually, but we aren't sure if adding a new CLI option accepting JSON is the right way to go. I'll close this issue in favor of https://github.com/astral-sh/ruff/issues/3694 which asks for a similar feature. 

I, unfortunately, can't suggest a better solution for the time being other than autogenerating the `ruff.toml` and committing it. You can also put the file in a subfolder and pass it with `--config <path>`

---

_Closed by @MichaReiser on 2023-07-10 08:29_

---
