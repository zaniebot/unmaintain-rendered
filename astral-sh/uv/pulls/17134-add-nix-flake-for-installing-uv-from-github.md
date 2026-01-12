```yaml
number: 17134
title: Add Nix flake for installing uv from GitHub
type: pull_request
state: closed
author: deadlovelll
labels: []
assignees: []
base: main
head: main
created_at: 2025-12-15T15:17:53Z
updated_at: 2025-12-18T13:08:28Z
url: https://github.com/astral-sh/uv/pull/17134
synced_at: 2026-01-12T16:12:37Z
```

# Add Nix flake for installing uv from GitHub

---

_@deadlovelll_

## Summary

Added a Nix flake for uv that builds the package directly from GitHub. 
This allows developers to install and run uv in an isolated environment 
without manually using curl. The flake provides `packages.default` 
and `apps.uv` for easy usage via `nix develop`.

## Example

**Before:**

```nix
{
  description = "super-project";

  inputs = {
    nixpkgs.url = "github:NixOS/nixpkgs/nixpkgs-unstable";
    flake-utils.url = "github:numtide/flake-utils";
  };

  outputs = { self, nixpkgs, flake-utils }:

    flake-utils.lib.eachDefaultSystem (system:
      let
        pkgs = import nixpkgs { inherit system; };
        python = pkgs.python313;
      in
      {
        devShell = pkgs.mkShell {
          buildInputs = [
            python
            pkgs.curl
            pkgs.git
          ];

          shellHook = ''
            echo "Setting up Python ${python.version} environment"

            if ! command -v uv &>/dev/null; then
              echo "Installing uv..."
              curl -LsSf https://astral.sh/uv/install.sh | sh
            fi

            .........................

            echo "Dev environment ready"
          '';
        };
      }
    );
}
```

**After:**

```bash
{
  description = "super-project";

  inputs = {
    nixpkgs.url = "github:NixOS/nixpkgs/nixpkgs-unstable";
    flake-utils.url = "github:numtide/flake-utils";
    uv.url = "github:deadlovelll/uv-flake";
  };

  outputs = { self, nixpkgs, flake-utils, uv }:

    flake-utils.lib.eachDefaultSystem (system:
      let
        pkgs = import nixpkgs { inherit system; };
        python = pkgs.python313;
      in
      {
        devShell = pkgs.mkShell {
          buildInputs = [
            python
            pkgs.curl
            pkgs.git
            uv.packages.${system}.default 
          ];

          shellHook = ''
            echo "Setting up Python ${python.version} environment"

            ..............

            echo "Dev environment ready"
          '';
        };
      }
    );
}
```

You just need to add uv to inputs instead of writing curl script

---

_Comment by @zanieb on 2025-12-15 15:37_

Isn't it a nix anti-pattern to copy a pre-built binary from GitHub like this?

---

_Closed by @zanieb on 2025-12-18 13:08_

---
