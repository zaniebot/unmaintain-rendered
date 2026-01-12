```yaml
number: 14499
title: "Added an automatic sourcing utility called \"Envy\" to CLI "
type: pull_request
state: open
author: DragonflyRobotics
labels: []
assignees: []
base: main
head: main
created_at: 2025-07-07T22:06:51Z
updated_at: 2025-08-02T00:18:46Z
url: https://github.com/astral-sh/uv/pull/14499
synced_at: 2026-01-12T16:11:15Z
```

# Added an automatic sourcing utility called "Envy" to CLI 

---

_@DragonflyRobotics_

## Summary
Firstly, I am a daily user of `uv` and enjoy its all encompassing features. I am a first time contributor to this project so please let me know of any modifications, improvements, or suggestions. I always struggled sourcing my environment since many projects had multiple environments, often with different names and locations. Sourcing them was always a struggle. I made "Envy" as a CLI tool to automatically find environments in the current directory (recursively) and auto source them. If there are multiple, it will open a FZF session to help the user quickly select one making the development process simpler. Considering that `uv` is a broad spectrum Python tool, I thought it would be best utilized as a component of `uv`. 

In addition to the software changes, this project will require users to have `fzf` installed and add ` eval "$(<PATH_TO_UV> envy init zsh)"` in their shell init (in this case `zsh`). This function will create a shell function called `envy` that can be used to auto source environments. 

I have only added functionality for `zsh` at the moment but if this seems like a viable contribution to `uv`, I can certainly add support for other POSIX and non-POSIX shells that are most common. This also applies to cross-platform support since this only supports Linux at the present. The modification isn't difficult, just tedious. 

This partially inspired from `zoxide` project that reducing typing and clutter in the terminal. I think this could also be expanded to help with other environment functions although the exact functions are still in progress. Any ideas on that regard would be appreciated.

### Usage
There are two modes of use:
`envy` - This mode will auto source if only a single environment is found in the current directory OR open `fzf` if multiple are found.
`envy -j` - This mode will auto source regardless of the number of environments to the one closest in file path to the user's working directory. 

The actual feature resides in `uv envy` and `uv envy -j` which will print anything that must be run in the current shell with `<ENVY> COMMAND`. But using the `envy` shell function, all `<ENVY>` commands will be run allowing the current shell to be sourced. 

Please let me know if you are interested in adding this handy feature to `uv`. I would be very grateful as would other Python users suffering from this inconvenience. If you would like to change any component of this project (including the name if it out-of-place from `uv` UX ideas) please let me know. 

## Test Plan
I have created test cases for the shell integration and environment search to the code. `cargo test -p uv-envy` will run these tests. The environment search is tested via `tempfile` to emulate directory structure. Unfortunately, there is no straight-forward way to test the sourcing functionality. Additionally, all tests under `uv` that pertain to my modifications (particularly the help menu) have been updated. 

## Note
Due to a slight typo on my end, my `uv-envy` crate files only appear in the latest commit. I truly apologize for the mistake and inconvenience. 
Also, for testing purposes, the path in the `init.rs` for the `zsh` template points to a hardcoded location on my system. That should be updated for testing to the debug binary location and eventually the `uv` installation directory (using `which uv`) but I haven't done that yet for ease of testing.

---
