```yaml
number: 16196
title: uvx does not invalidate cache when source code changes with --from flag
type: issue
state: closed
author: oxysoft
labels:
  - question
assignees: []
created_at: 2025-10-09T01:20:48Z
updated_at: 2025-11-01T20:01:23Z
url: https://github.com/astral-sh/uv/issues/16196
synced_at: 2026-01-12T16:02:26Z
```

# uvx does not invalidate cache when source code changes with --from flag

---

_@oxysoft_

  uvx --from <local-path> aggressively caches installed packages and does NOT invalidate the cache when source files change, leading to developers running stale code even after making edits. This makes uvx completely unreliable for
  local development workflows.

  Steps to Reproduce

  1. Create a Python package with an entry point (e.g., MCP server)
  2. Install and run it with: uvx --from /path/to/package my-command
  3. Observe initial output
  4. Edit the source code in /path/to/package/src/
  5. Run the same command again: uvx --from /path/to/package my-command
  6. BUG: The output is unchanged - the old cached version runs despite source code changes

  Expected Behavior

  When using --from with a local file path, uvx should:
  - Detect that the source is a local directory
  - Check if source files have been modified (via mtime or hash)
  - Invalidate the cache and reinstall if changes are detected
  - OR at minimum, provide a --no-cache or --reload flag

  Actual Behavior

  uvx caches the package installation and never checks if the source has changed. Developers must:
  - Manually find and delete cache directories
  - Use uv run instead (which defeats the purpose of uvx)
  - Restart their entire development environment
  - Waste hours debugging "phantom" bugs where their code changes don't appear

  Workaround

  Use uv run from within the package directory instead:
  cd /path/to/package && uv run my-command

  This is slower and more cumbersome but actually picks up source changes.

  Impact

  Critical for local development. This behavior makes uvx completely unusable for:
  - Developing CLI tools
  - Testing MCP servers during development
  - Any iterative development workflow where you need to test changes quickly

  The --from flag strongly implies "use this specific source location," but it actually means "install from here once, then ignore it forever."

  Environment

  - uv version: (check with uv --version)
  - Platform: Linux (but likely affects all platforms)
  - Use case: MCP server development with FastMCP

  Suggested Fix

  1. Option A (Best): Detect local paths and always check modification time
  2. Option B: Add --no-cache / --force-reinstall flag to uvx
  3. Option C: Document this behavior prominently in --from documentation
  4. Option D: Make uvx detect when --from points to a local path and emit a warning about caching

  Related Documentation Gaps

  The uvx documentation does not mention:
  - That it caches installations from local paths
  - How to clear the cache
  - When/why you should use uv run vs uvx --from
  - That code changes won't be reflected without cache invalidation

  This led to multiple hours wasted debugging "why aren't my changes working" when the code was fine - the tool was just lying about what it was running.

  Reproduction Context

  We were developing a Unity Profiler MCP server. Made changes to add a new tool (get_frames_overview), confirmed it existed in source, ran with uvx --from, tool didn't appear. Spent significant time debugging the MCP protocol,
  FastMCP internals, tool registration, etc. - all of which were working correctly. The only issue was uvx running stale cached code.

  Extremely frustrating user experience. The tool provided no indication it was using cached code.

---

_Comment by @konstin on 2025-10-09 07:30_

There are several ways to solve this with uvx:
* Use an editable install instead of a regular install (`--with-editable`). This links Python code instead of copying it, so changes are instant
* Use `--reinstall-package` (https://docs.astral.sh/uv/reference/cli/#uv-tool-run--reinstall-package)
* Set `cache-keys` in the project itself
* Use `--no-cache`

---

_Label `question` added by @konstin on 2025-10-09 07:30_

---

_Comment by @konstin on 2025-10-09 07:33_

> Suggested Fix
> 
>     1. Option A (Best): Detect local paths and always check modification time
> 
>     2. Option B: Add --no-cache / --force-reinstall flag to uvx
> 
>     3. Option C: Document this behavior prominently in --from documentation
> 
>     4. Option D: Make uvx detect when --from points to a local path and emit a warning about caching
> 
> 
> Related Documentation Gaps
> 
> The uvx documentation does not mention:
> 
>     * That it caches installations from local paths
> 
>     * How to clear the cache
> 
>     * When/why you should use uv run vs uvx --from
> 
>     * That code changes won't be reflected without cache invalidation
> 
> 
> This led to multiple hours wasted debugging "why aren't my changes working" when the code was fine - the tool was just lying about what it was running.

Please consult the uv documentation - These statements don't reflect what is actually implemented in `uvx`.

---

_Closed by @charliermarsh on 2025-11-01 20:01_

---
