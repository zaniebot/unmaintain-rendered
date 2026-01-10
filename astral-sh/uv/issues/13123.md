```yaml
number: 13123
title: Extend uv config Command to Support Viewing/Modifying/Resetting Configurations
type: issue
state: closed
author: willow-yang
labels:
  - enhancement
assignees: []
created_at: 2025-04-27T03:02:22Z
updated_at: 2025-12-31T08:37:19Z
url: https://github.com/astral-sh/uv/issues/13123
synced_at: 2026-01-10T03:11:34Z
```

# Extend uv config Command to Support Viewing/Modifying/Resetting Configurations

---

_Issue opened by @willow-yang on 2025-04-27 03:02_

### Summary


## Description
Currently, uv lacks a built-in way to view or manage runtime configurations, making it difficult for users to debug configuration conflicts or adjust settings efficiently. To improve usability and transparency, we propose adding a uv config command inspired by tools like git config, enabling users to view, modify, and reset configurations via the CLI.

## Proposed Functionality
1. View Configurations
+ List All Active Configurations
```bash
uv config list
```
Outputs all resolved configurations in key=value format, prioritized by source (CLI arguments > environment variables > config files > defaults).
+ Query a Specific Configuration Key
```bash
uv config get <key>
```
Returns the effective value of a key (e.g., uv config get cache-dir).
+ Show Configuration Sources (Optional)
```bash
uv config list --show-sources
```
Annotates the source of each configuration (e.g., cache-dir=/tmp/cache  # Environment: UV_CACHE_DIR).
2. Modify Configurations
+ Set a Configuration Key
```bash
uv config set <key> <value> [--scope]
```
Writes the key-value pair to a configuration file in the specified scope:
- --local (default): Project-level (uv.toml or pyproject.toml).
- --global: User-level (~/.uv/config.toml).
- --system: System-wide (/etc/uv/config.toml).
Example:
```bash
uv config set cache-dir /tmp/cache --local
```
3. Reset Configurations
+ Reset a Key to Default
```bash
uv config reset <key> [--scope]
```
+ Removes the key from the specified scope, restoring the default value or inheriting from a higher priority source. Example:
```bash
uv config reset cache-dir --local
```
## Use Cases
+ Debugging Configuration Conflicts: Identify why a configuration isn’t applied as expected (e.g., environment variables overriding file settings).
+ Rapid Configuration Adjustments: Modify settings in scripts or CI/CD pipelines without manual file edits.
+ Portable Configuration: Use --local to bind configurations to a project for team collaboration.
+ Recovery from Errors: Quickly reset misconfigured keys to defaults.
## Considerations
+ Priority Rules: Ensure set respects existing priority logic (e.g., CLI arguments override file-based configurations).
+ File Handling: Auto-create configuration files if missing, preserving TOML syntax compatibility.
## Security:
+ Redact sensitive fields (e.g., credentials) by default in uv config list output.
+ Discourage writing sensitive data via set; recommend environment variables instead.
+ Error Handling: Gracefully handle non-existent keys in reset (e.g., Key "cache-dir" not found in --local scope).
## Summary
This feature would align uv’s configuration management with developer expectations, offering a familiar and intuitive workflow akin to git config. By adopting a subcommand-based design (e.g., list, get, set, reset), the implementation ensures extensibility for future enhancements (e.g., JSON output, filtering).

We believe this addition will significantly improve uv's debuggability and user experience, especially in complex environments with layered configurations.

Feel free to copy and paste this directly into the uv GitHub repository! Let me know if further refinements are needed. 😊

### Example

_No response_

---

_Label `enhancement` added by @willow-yang on 2025-04-27 03:02_

---

_Comment by @edobez on 2025-09-04 09:01_

As a first step if would be good to have a subset of this feature: only showing the effective configuration, like `pip config debug`.

---

_Comment by @ubalklen on 2025-12-27 16:22_

Sounds like a dupe of #6042

---

_Closed by @willow-yang on 2025-12-31 08:35_

---

_Reopened by @willow-yang on 2025-12-31 08:35_

---

_Comment by @willow-yang on 2025-12-31 08:37_

Similar issue already exists #6042

---

_Closed by @willow-yang on 2025-12-31 08:37_

---
