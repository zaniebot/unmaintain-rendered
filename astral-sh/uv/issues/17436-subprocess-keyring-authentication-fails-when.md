```yaml
number: 17436
title: "Subprocess Keyring authentication fails when using uv tool run with @latest qualifier"
type: issue
state: open
author: Vaeyrun
labels:
  - bug
assignees: []
created_at: 2026-01-13T10:47:39Z
updated_at: 2026-01-13T22:09:13Z
url: https://github.com/astral-sh/uv/issues/17436
synced_at: 2026-01-13T22:36:13Z
```

# Subprocess Keyring authentication fails when using uv tool run with @latest qualifier

---

_@Vaeyrun_

### Summary

This is a subtle bug, as it only effects calling `uv tool run` or `uvx` with the `@latest` version qualifier (e.g. `uvx ruff@latest` when your environment is configured to use keyring subprocess authentication for a private index on a non-windows (or in my case macOS) environment.

Given a user configuration `uv.toml` with the following structure (in our case using artifacts-keyring with Azure DevOps):
```
keyring-provider = "subprocess"
[[index]]
url = "https://VssSessionToken@pkgs.dev.azure.com/<org>/_packaging/<feed>/pypi/simple"
```

Running `uvx ruff` or `uvx ruff@0.14.11` will work as expected.
However, doing the same with `uvx ruff@latest` fails with a 401 unauthorized against the private index:
```
  × No solution found when resolving tool dependencies:
  ╰─▶ Because ruff was not found in the package registry and you require ruff, we can conclude that your requirements are unsatisfiable.

      hint: An index URL (https://pkgs.dev.azure.com/<org>/_packaging/<feed>/pypi/simple) could not be queried due to a lack of valid authentication credentials (401 Unauthorized).
```

I've already dug into the code, and I believe I've confirmed the root cause of the bug as well as a fix that seems to work when I build locally.

In `crates/uv/src/commands/tool/run.rs:920` we have the following code to create the RegistryClient to find the latest version of the package for the '@latest' scenario:
```
        // Build the registry client to fetch the latest version.
        let client = RegistryClientBuilder::new(client_builder.clone(), cache.clone())
            .index_locations(settings.resolver.index_locations.clone())
            .index_strategy(settings.resolver.index_strategy)
            .markers(interpreter.markers())
            .platform(interpreter.platform())
            .build();
```

Digging further, I've identified that in other scenarios where the code searches the indexes, it's using `resolve_names` as defined in `crates/uv/src/commands/project/mod.rs:1735` which has some additional config when creating a clone of the client_builder on line 1792:
```
let client_builder = client_builder.clone().keyring(*keyring_provider);
```

It looks like the injection of the `keyring` settings into the client_builder has been missed when searching for the `@latest` version when running `uvx`, which I've confirmed and tested locally by updating the code snippet at `crates/uv/src/commands/tool/run.rs:920` to be:
```
        // Build the registry client to fetch the latest version.
        let client = RegistryClientBuilder::new(client_builder.clone().keyring(settings.resolver.keyring_provider), cache.clone())
            .index_locations(settings.resolver.index_locations.clone())
            .index_strategy(settings.resolver.index_strategy)
            .markers(interpreter.markers())
            .platform(interpreter.platform())
            .build();
```

Very happy to raise a PR to fix this, although I'm not sure I have the time to work out how to expand the current test cases to try and cover this edge-case, but I wanted to get the Issue raised as the basis for tracking a fix first.

### Platform

macOS 15.7.3 M4

### Version

0.9.18 (Homebrew 2025-12-16)

### Python version

3.14.0

---

_Label `bug` added by @Vaeyrun on 2026-01-13 10:47_

---

_Comment by @zanieb on 2026-01-13 22:09_

Thank you for the investigation!

---
