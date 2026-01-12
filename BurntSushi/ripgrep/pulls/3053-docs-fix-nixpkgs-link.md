```yaml
number: 3053
title: "docs: fix nixpkgs link"
type: pull_request
state: closed
author: nyurik
labels: []
assignees: []
base: master
head: patch-1
created_at: 2025-05-22T14:58:32Z
updated_at: 2025-08-17T21:34:44Z
url: https://github.com/BurntSushi/ripgrep/pull/3053
synced_at: 2026-01-12T18:23:15Z
```

# docs: fix nixpkgs link

---

_@nyurik_

At least I hope this is the right one

---

_Review comment by @RossSmyth on `README.md`:318 on 2025-06-09 19:32_

nix-env is not really recommended these days because it modifies global state. Either:
```suggestion
Add this to your NixOS or Home-Manager config.
```nix
  environment.systemPackages = [
    pkgs.ripgrep
  ];
```
or
```suggestion
To temporarily add it to your `$PATH`
```bash
$ nix-shell -p ripgrep
```

---

_@RossSmyth reviewed on 2025-06-09 19:33_

Linking to the source file is sort of meaningless. It would be best to link to something like 
https://search.nixos.org/packages?channel=unstable&show=ripgrep&from=0&size=50&sort=relevance&type=packages&query=ripgrep

---

_Review comment by @Ajlow2000 on `README.md`:318 on 2025-06-28 05:15_

Just commenting to support that this recommendation is more modern and agreed upon than nix-env. I would prefer to see these suggestions go in. 

---

_@Ajlow2000 reviewed on 2025-06-28 05:15_

---

_Comment by @BurntSushi on 2025-08-17 21:34_

Duplicate of #3006.

(I do see that this is more elaborate, but I prefer to keep linking to the source file. I also prefer the simplicity of the status quo. However, if an authoritative source wants to adjudicate a more idiomatic approach, I'd be happy to receive a PR for it.)

---

_Closed by @BurntSushi on 2025-08-17 21:34_

---
