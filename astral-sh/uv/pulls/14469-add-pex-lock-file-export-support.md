```yaml
number: 14469
title: Add PEX lock file export support
type: pull_request
state: closed
author: ademariag
labels: []
assignees: []
base: main
head: feat/pex-lock-export
created_at: 2025-07-05T23:06:25Z
updated_at: 2025-07-14T17:00:25Z
url: https://github.com/astral-sh/uv/pull/14469
synced_at: 2026-01-12T16:11:14Z
```

# Add PEX lock file export support

---

_@ademariag_

## Summary

  Adds support for exporting UV lock files to the PEX lock
  format, enabling seamless integration with PEX packaging
  tools and Pantsbuild workflows.

  ## Changes

  ### Core Implementation
  - **New export format**: Added `ExportFormat::PexLock`
  with format detection for `.pex.lock` and `.pex.json`
  files
  - **PEX lock structure**: Implemented complete PEX lock
  file schema with proper JSON serialization
  - **Platform compatibility**: Uses universal platform
  tags `["py", "none", "any"]` for cross-platform
  compatibility
  - **Hash security**: Skips artifacts without hashes and
  properly extracts algorithm from hash strings

  ### Format Features
  - **Correct artifact format**: Uses separate `algorithm`
  and `hash` fields (not nested `hashes` object)
  - **3-component platform tags**: Implements
  `[interpreter, abi, platform]` format required by PEX
  parser
  - **Comprehensive metadata**: Includes all required PEX
  fields (pex_version, build settings, etc.)
  - **Multi-artifact support**: Handles both wheels and 
  source distributions with proper URL handling

  ### Integration
  - **CLI support**: `uv export --format pex.lock` and 
  automatic format detection
  - **Error handling**: Graceful handling of missing 
  artifacts and invalid URLs
  - **Documentation**: Full module documentation explaining
   PEX format and usage

  ## Testing

  - Unit tests for PEX lock serialization and format 
  validation
  - Comprehensive artifact handling (wheels, source 
  distributions, local packages)
  - Hash extraction and algorithm detection

  ## Usage

  ```bash
  # Export to PEX format explicitly
  uv export --format pex.lock

  # Auto-detect from file extension
  uv export --output-file requirements.pex.lock

  Compatibility

  - Compatible with PEX 2.44.0+ and Pantsbuild workflows
  - Resolves parsing errors from incorrect artifact and
  platform tag formats
  - Maintains all dependency resolution integrity with
  proper hash verification

  Closes: [Issue number if applicable]

  ## Summary of PR-Ready Changes

  ✅ **Code Quality**
  - Fixed visibility issues (`pub(crate) mod export`)
  - Added comprehensive documentation
  - Improved error handling (skip artifacts without hashes)
  - Dynamic hash algorithm extraction
  - Proper platform tag format with explanatory comments

  ✅ **Architecture**
  - Clean module structure following existing patterns
  - Proper re-exports through lock module
  - Integration with existing export infrastructure

  ✅ **Security**
  - Skip artifacts without hashes for security
  - Proper hash verification preservation
  - Safe URL handling for different source types

  ✅ **Robustness**
  - Handles missing URLs/filenames gracefully
  - Support for multiple hash algorithms
  - Cross-platform compatibility

---

_Marked ready for review by @ademariag on 2025-07-06 18:27_

---

_Comment by @jsirois on 2025-07-10 18:21_

@ademariag as the maintainer of Pex and a former Pants maintainer I'd like to suggest this is not a good approach for several reasons:
1. The PEX lock file format is not specified or documented; so taking up the responsibility of maintaining an export to the format is highly undesirable for any project. I assume uv heartily agrees with this.
2. There is, in fact, an appropriate export format that is specified in [PEP-751](https://peps.python.org/pep-0751/). This [pylock.toml format](https://packaging.python.org/en/latest/specifications/pylock-toml/#pylock-toml-spec) is supported for export by uv and for import by Pex. For Pex to be able to subset the lock (which Pants currently needs / leverages heavily), the optional dependencies field needs to be filled in however. This is tracked by uv here: https://github.com/astral-sh/uv/issues/13032.
3. IIUC you, as a ~3rd party, are attempting to work around Pants issues not by fixing Pants (for example to use uv directly), but by attempting to push fixes elsewhere. This might be an expedient tactic in general, but it is not usually a good long term approach.

I'd like to stress that your problem is over in Pants fundamentally and trying to back door a fix through uv or Pex is not optimal. If you were to maintain your tactical approach however, I'd suggest focusing on 2 above is the right way to go. Engage uv to re-affirm their stomach for https://github.com/astral-sh/uv/issues/13032 being fixed and offer help if they are positive on this direction. Although still tactical, at least you'd be improving inter-operation on a community standard in pylock.toml, which has wider benefits than your particular needs.

---

_Assigned to @charliermarsh by @charliermarsh on 2025-07-14 14:42_

---

_Comment by @charliermarsh on 2025-07-14 17:00_

> The PEX lock file format is not specified or documented; so taking up the responsibility of maintaining an export to the format is highly undesirable for any project. I assume uv heartily agrees with this.

:thumbsup: While I appreciate the PR, I was already a bit hesitant prior to @jsirois comment as I'm not super familiar with the format (and it hasn't been requested before), so incorporating it would require both significant upfront work and ongoing maintenance. But based on this, I'd prefer not to add support for the PEX lock file. It seems like PEP 751 is the better path forward here for interoperability.


---

_Closed by @charliermarsh on 2025-07-14 17:00_

---
