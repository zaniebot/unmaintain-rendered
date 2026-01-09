---
number: 10461
title: Installation fails on macOS without fish
type: issue
state: open
author: TinsFox
labels:
  - bug
  - external
  - releases
assignees: []
created_at: 2025-01-10T08:09:15Z
updated_at: 2025-02-19T13:32:10Z
url: https://github.com/astral-sh/uv/issues/10461
synced_at: 2026-01-07T13:12:18-06:00
---

# Installation fails on macOS without fish

---

_Issue opened by @TinsFox on 2025-01-10 08:09_

I am trying to install uv. On a macOS M1 computer, I executed the command `curl -LsSf https://astral.sh/uv/install.sh | sh` but an error occurred.

<img width="708" alt="Image" src="https://github.com/user-attachments/assets/eca687ea-faeb-4ea2-b3cc-7ad957647cee" />

I don't have fish and I don't plan to use fish. Is this installation script bundled with fish?

---

_Comment by @TinsFox on 2025-01-10 08:12_

I also tried using sudo to execute the command, but it didn't work

---

_Comment by @chen1plus on 2025-01-12 01:37_

I think that removing below codes from the install script can solve the problem.

```sh
ensure mkdir -p "$HOME/.config/fish/conf.d"
exit4=$?
add_install_dir_to_path "$_install_dir_expr" "$_fish_env_script_path" "$_fish_env_script_path_expr" ".config/fish/conf.d/$APP_NAME.env.fish" "fish"
exit5=$?
```

It is located in the `install` function.

---

_Label `releases` added by @zanieb on 2025-01-12 06:11_

---

_Label `bug` added by @zanieb on 2025-01-12 06:11_

---

_Label `upstream` added by @zanieb on 2025-01-12 06:11_

---

_Comment by @AlanDuong07 on 2025-02-19 06:50_

This is affecting me too. I literally cannot use uv AT ALL. Can we please fix this bug? I am a normal, interested user of UV and I literally cannot use it because of this bug. @zanieb 

---

_Comment by @Gankra on 2025-02-19 13:32_

Ah, this is unfortunate and should definitely be fixed. In the meantime, does this expression from [the installer docs](https://docs.astral.sh/uv/configuration/installer/) fix it for you?

```
curl -LsSf https://astral.sh/uv/install.sh | env INSTALLER_NO_MODIFY_PATH=1 sh
```

---
