```yaml
number: 17611
title: Add Nix installation instructions to documentation
type: pull_request
state: open
author: WeepingDogel
labels: []
assignees: []
base: main
head: docs-nix-install
created_at: 2026-01-19T14:44:46Z
updated_at: 2026-01-19T19:33:49Z
url: https://github.com/astral-sh/uv/pull/17611
synced_at: 2026-01-19T20:31:15Z
```

# Add Nix installation instructions to documentation

---

_@WeepingDogel_

## Summary

Add a new section covering installation of uv via Nix/NixOS, including:
- Instructions for adding uv to system packages
- Common error troubleshooting for dynamically linked executables
- Solution using nix-ld to fix compatibility issues


---

_Comment by @WeepingDogel on 2026-01-19 14:48_

### Nix

uv is available via [nix packages](https://search.nixos.org/packages?channel=25.11&show=uv&query=uv).

You add `uv` to your system packages in `/etc/nixos/configuration.nix`.

```nix
environment.systemPackages = with pkgs; [
  uv
];
```

Then rebuild:

```console
$ sudo nixos-rebuild switch
```

#### Common Error

On NixOS, you may see this error:

```console
error: Querying Python at `/home/<user>/.local/share/uv/python/cpython-3.13.5-linux-x86_64-gnu/bin/python3.13` failed with exit status exit status: 127

[stderr]
Could not start dynamically linked executable: /home/<user>/.local/share/uv/python/cpython-3.13.5-linux-x86_64-gnu/bin/python3.13
NixOS cannot run dynamically linked executables intended for generic
linux environments out of the box. For more information, see:
https://nix.dev/permalink/stub-ld
```

This happens because ["NixOS cannot run dynamically linked executables intended for generic Linux environments out of the box."](https://nix.dev/guides/faq#how-to-run-non-nix-executables)

[To fix this](https://wiki.nixos.org/wiki/Python_quickstart_using_uv#Fix_with_nix-ld), you need to enable [nix-ld](https://wiki.nixos.org/wiki/Nix-ld) In your `/etc/nixos/configuration.nix`:

```nix
programs.nix-ld.enable = true;
```

---

_@zanieb reviewed on 2026-01-19 19:33_

---

_Review comment by @zanieb on `docs/getting-started/installation.md`:181 on 2026-01-19 19:33_

Should we be suggesting not using our managed Python versions? (this seems helpful to document either way though)

---
