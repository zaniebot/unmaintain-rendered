```yaml
number: 2450
title: Update default_types.rs for elixir
type: pull_request
state: closed
author: angrycandy
labels:
  - rollup
assignees: []
base: master
head: patch-1
created_at: 2023-03-13T15:47:56Z
updated_at: 2023-07-08T22:52:52Z
url: https://github.com/BurntSushi/ripgrep/pull/2450
synced_at: 2026-01-12T18:23:14Z
```

# Update default_types.rs for elixir

---

_@angrycandy_

Elixir https://hexdocs.pm/phoenix_live_view/assigns-eex.html:

Phoenix template language is called HEEx (HTML+EEx). Those templates are either files with the .heex extension...

---

_Comment by @Stratus3D on 2023-04-17 15:16_

There is also `leex`. but both of these extensions are for Phoenix, not core Elixir.

---

_Comment by @angrycandy on 2023-04-17 16:18_

> There is also `leex`. but both of these extensions are for Phoenix, not core Elixir.

@Stratus3D do you think supporting `heex` & `leex` degrades the non-Phoenix Elixir programming experience?

I don't think so, and I think it enhances the Phoenix experience.

BTW, [Upgrading and deprecations]:(https://github.com/phoenixframework/phoenix_live_view/blob/main/CHANGELOG.md#upgrading-and-deprecations)
`
The main deprecation in this release is that the ~L sigil and the .leex extension are now soft-deprecated. The docs have been updated to discourage them and using them will emit warnings in future releases. We recommend using the ~H sigil and the .heex extension for all future templates in your application. You should also plan to migrate the old templates accordingly using the recommendations below.
`

I think I should update the PR to include `leex`.

---

_Comment by @Stratus3D on 2023-04-17 16:39_

I don't think it's a negative to treat `heex` as Elixir no, but I figured I'd point out the distinction in case it mattered.

---

_Comment by @rodrigues on 2023-07-03 13:39_

It would be nice also to support [Livebook](https://livebook.dev) (`*.livemd` - Elixir code notebooks) 👌

Glad the CLI allows type customisation in the meantime:

```fish
function au
  rg --type-add "elixir:*.heex" --type-add "elixir:*.livemd" -telixir $argv
end

```

---

_Label `rollup` added by @BurntSushi on 2023-07-07 15:54_

---

_Closed by @BurntSushi on 2023-07-08 22:52_

---
