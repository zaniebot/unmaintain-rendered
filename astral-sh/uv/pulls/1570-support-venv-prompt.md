```yaml
number: 1570
title: "Support `venv --prompt`"
type: pull_request
state: merged
author: methane
labels:
  - enhancement
assignees: []
merged: true
base: main
head: support-prompt
created_at: 2024-02-17T04:24:43Z
updated_at: 2024-02-21T21:00:09Z
url: https://github.com/astral-sh/uv/pull/1570
synced_at: 2026-01-10T14:54:43Z
```

# Support `venv --prompt`

---

_Pull request opened by @methane on 2024-02-17 04:24_

## Summary

This PR adds the `--prompt` option to `venv` subcommand.

The default behavior for `uv venv` is to create a virtual environment in the current directory with `.venv` name. This is different from `venv` / `virtualenv` where a user always needs to provide the virtual environment path. This allows us to define our own behavior in the default scenario (`uv venv`). We've decided to use the current directory's name in that case.

Workflows:
| Command | Virtual Environment Name | Prompt |
|--------|--------|--------|
| `uv venv` | `.venv` (default) | Current directory name |
| `uv venv project` | `project` | `project` |
| `uv venv --prompt .` | `.venv` | Current directory name |
| `uv venv --prompt foobar` | `.venv` | `foobar` | 
| `uv venv project --prompt foobar` | `project` | `foobar` | 


Fixes #1445

## Test Plan

This is my first Rust code and I don't know how to write tests yet.
I just checked the behavior manually:

```
$ cargo build
$ mkdir t
$ cd t
$ ../target/debug/uv venv -p 3.11
$ rg -w t .venv/bin/acti*
.venv/bin/activate.csh
13:setenv VIRTUAL_ENV '/Users/inada-n/work/uv/t/.venv'
20:if ('t' != "") then
21:    setenv VIRTUAL_ENV_PROMPT 't'
23:    setenv VIRTUAL_ENV_PROMPT "$VIRTUAL_ENV:t:q"
38:    # in which case, $prompt is undefined and we wouldn't

.venv/bin/activate
48:VIRTUAL_ENV='/Users/inada-n/work/uv/t/.venv'
59:    VIRTUAL_ENV_PROMPT="t"

.venv/bin/activate.fish
61:set -gx VIRTUAL_ENV '/Users/inada-n/work/uv/t/.venv'
73:if test -n 't'
74:    set -gx VIRTUAL_ENV_PROMPT 't'

.venv/bin/activate.ps1
40:if ("t" -ne "") {
41:    $env:VIRTUAL_ENV_PROMPT = "t"

.venv/bin/activate.nu
6:# but then simply `deactivate` won't work because it is just an alias to hide
35:    let virtual_env = '/Users/inada-n/work/uv/t/.venv'
50:    let virtual_env_prompt = (if ('t' | is-empty) {
53:        't'
```

---

_@zanieb reviewed on 2024-02-17 04:27_

---

_Review comment by @zanieb on `crates/uv/src/main.rs`:1019 on 2024-02-17 04:27_

We should probably avoid getting the cwd if the prompt is not set to `.`

---

_@zanieb reviewed on 2024-02-17 04:28_

---

_Review comment by @zanieb on `crates/uv/src/main.rs`:1021 on 2024-02-17 04:28_

We try not to `unwrap()` in user-facing code, is there a safe way to get what you're looking for here?

Will this be a confusing user experience if this ends up being `None`?

---

_@zanieb had review dismissed on 2024-02-17 04:30_

Thanks for your pull request! I have some comments to start.

I'm not sure what the best way to test this is either, you can see our current `venv` tests at https://github.com/astral-sh/uv/blob/main/crates/uv/tests/venv.rs — I'd look into adding a snapshot there.

Here's a resource about our snapshotting tool: https://insta.rs/docs/cli/

---

_Label `enhancement` added by @zanieb on 2024-02-17 04:30_

---

_Comment by @zanieb on 2024-02-17 04:30_

Also congrats on your first Rust pull request ❤️ 

---

_@methane reviewed on 2024-02-17 05:00_

---

_Review comment by @methane on `crates/uv/src/main.rs`:1019 on 2024-02-17 05:00_

I am totally newbie to Rust so I don't know how to handle this lifetime issue.

```rust
            let prompt = if args.prompt == "." {
                match env::current_dir() {
                    Ok(cwd) => match cwd.file_name() {
                        Some(b) => b.to_str(),
                        None => None,
                    },
                    Err(_) => None,
                }
            } else {
                Some(args.prompt.as_str())
            };
```

```
error[E0597]: `cwd` does not live long enough
    --> crates/uv/src/main.rs:1021:38
     |
1019 |             let prompt = if args.prompt == "." {
     |                 ------ borrow later stored here
1020 |                 match env::current_dir() {
1021 |                     Ok(cwd) => match cwd.file_name() {
     |                        ---           ^^^ borrowed value does not live long enough
     |                        |
     |                        binding `cwd` declared here
...
1024 |                     },
     |                     - `cwd` dropped here while still borrowed
```

---

_Review comment by @dhruvmanila on `crates/uv/src/main.rs`:656 on 2024-02-17 05:18_

If I understand this correctly, then we would always default the prompt value to use the current directory name. This is different from the original behavior. What we want is:

1. If the `--prompt` flag isn't present, use the virtual environment name as the prompt value (what we currently do)
2. If the `--prompt` value is ".", use the current directory name
3. Otherwise, use whatever the value is provided by the user

This implementation is missing (1) behavior. This means that we would want to have an `Option<String>` here instead. If it's `None`, then we'd go with (1) otherwise we'd go with either (2) or (3). You will need to change other references accordingly.

```suggestion
    /// The prompt used in shells.
    #[clap(long)]
    prompt: Option<String>,
```

---

_Review comment by @dhruvmanila on `crates/uv/src/main.rs`:1033 on 2024-02-17 05:28_

I think this logic should reside in `gourgeist::create_bare_venv` so that the behavior remains the same through the API and CLI similar to how the [`venv.EnvBuilder`](https://docs.python.org/3/library/venv.html#venv.EnvBuilder) does in standard library.

---

_Review comment by @dhruvmanila on `crates/gourgeist/src/main.rs`:37 on 2024-02-17 05:29_

This isn't strictly required in this PR but I think it would be useful to provide the flag in the `gourgeist` CLI interface as well. You'll find the argument struct in this file itself which can then be passed here.

This would allow you to test your implementation using the `gourgeist` CLI:
```
cargo run --bin gourgeist -- ...
```

---

_Review comment by @dhruvmanila on `crates/gourgeist/src/bare.rs`:220 on 2024-02-17 05:31_

We don't really want to add the entry in the `pyvenv.cfg` file is the user pass in  the `--prompt` flag. That's the behavior of `venv` / `virtualenv`. I think we should match that for compatibility.

What you can do here is to add a new parameter to `write_cfg` below which would be an `Option` and then update the function to add the entry if it's `Some`.

---

_Review comment by @dhruvmanila on `crates/gourgeist/src/bare.rs`:168 on 2024-02-17 05:32_

nit: use `VIRTUAL_PROMPT` to match the original script. You'll need to update the references in all of the activation scripts.

https://github.com/pypa/virtualenv/blob/fa283474fd199e3836f8b2c99510190c6b77e2bc/src/virtualenv/activation/bash/activate.sh#L58

```suggestion
            .replace("{{ VIRTUAL_PROMPT }}", prompt)
```

---

_@dhruvmanila requested changes on 2024-02-17 05:37_

Thanks for creating this PR!

I was just playing around with `venv` / `virtualenv` when I saw this PR. That helped me understand what the original behavior is which means you don't need to fiddle around with other tools ;)

There are a few changes required to be compatible with the `--prompt` flag from other tools. 

From the implementation side, I'm not exactly sure but you might want to play around with either using `Option<&str>` or `Option<String>` type for the prompt parameter in the `gourgeist::create_bare_venv` function. This is because we need to get the prompt value from the current working directory if it's `Some(".")` which could lead into ownership problems. Feel free to ping me if you face any, I'm happy to help :)

---

_Assigned to @dhruvmanila by @zanieb on 2024-02-17 05:55_

---

_@methane reviewed on 2024-02-17 06:40_

---

_Review comment by @methane on `crates/uv/src/main.rs`:656 on 2024-02-17 06:40_

> * If the `--prompt` flag isn't present, use the virtual environment name as the prompt value (what we currently do)

There is an important difference between venv/virtualenv and uv venv.

venv/virtualenv was born before `.venv` become convention so they have mandatory ENV_DIR argument. It is natural to use ENV_DIR as the default prompt name.

On the other hand, uv venv uses `.venv` as ENV_DIR by default. But ".venv" is not good for default prompt.

How about changing default prompt by `name`? `prompt = if name == ".venv" { cwd.file_name() } else { name }`

---

_Comment by @methane on 2024-02-17 07:32_

### When name is specified

```
~/work/uv/t (git)-[support-prompt] % ../target/debug/uv venv -p 3.11 hoge
Using Python 3.11.7 interpreter at /opt/homebrew/opt/python@3.11/bin/python3.11
Creating virtualenv at: hoge
~/work/uv/t (git)-[support-prompt] % grep prompt hoge/pyvenv.cfg
prompt = hoge
```

### When name is omitted (.venv)

```
~/work/uv/t (git)-[support-prompt] % ../target/debug/uv venv -p 3.11
Using Python 3.11.7 interpreter at /opt/homebrew/opt/python@3.11/bin/python3.11
Creating virtualenv at: .venv
~/work/uv/t (git)-[support-prompt] % grep prompt .venv/pyvenv.cfg
prompt = t
```

---

_Review comment by @methane on `crates/uv/src/main.rs`:1033 on 2024-02-17 13:23_

I moved "." to basename(cwd) logic to create_bare_venv.

---

_@methane reviewed on 2024-02-17 13:23_

---

_@methane reviewed on 2024-02-17 13:31_

---

_Review comment by @methane on `crates/gourgeist/src/bare.rs`:220 on 2024-02-17 13:31_

I changed `write_cfg()` to:

```rust
        if !value.is_empty() {
            writeln!(f, "{key} = {value}")?;
        }
```

"prompt =" is not written when promt is None.

---

_@dhruvmanila reviewed on 2024-02-17 16:34_

---

_Review comment by @dhruvmanila on `crates/uv/src/main.rs`:656 on 2024-02-17 16:34_

> venv/virtualenv was born before `.venv` become convention so they have mandatory ENV_DIR argument. It is natural to use ENV_DIR as the default prompt name.

Good point. Thanks for giving me the context on your decision.

> On the other hand, uv venv uses `.venv` as ENV_DIR by default. But ".venv" is not good for default prompt.

This is why `--prompt` will exist ;)

What happens when a user provided a custom name for the virtual environment directory but the prompt still defaults to the current directory?

```console
$ cargo run --bin uv -- venv hello
    Finished dev [unoptimized + debuginfo] target(s) in 0.13s
     Running `target/debug/uv venv hello`
Using Python 3.12.1 interpreter at /Users/dhruv/.pyenv/versions/3.12.1/bin/python3.12
Creating virtualenv at: hello

$ source hello/bin/activate

$ bat .venv/pyvenv.cfg | grep prompt
prompt = uv

$ echo $VIRTUAL_ENV_PROMPT 
hello
```

I thought about what you've proposed but I still think we should stick with the original behavior for compatibility reason, not just with virtualenv / venv but with other tools which uses this information from either `VIRTUAL_ENV_PROMPT` or `pyvenv.cfg` file.

---

_@methane reviewed on 2024-02-17 17:35_

---

_Review comment by @methane on `crates/uv/src/main.rs`:656 on 2024-02-17 17:35_

> What happens when a user provided a custom name for the virtual environment directory but the prompt still defaults to the current directory?

Then, current PR doesn't set prompt. Activation script uses the venv directory name. This is same to venv/virtualenv.

```
$ ../target/debug/uv venv -p 3.11 hello
Using Python 3.11.7 interpreter at /opt/homebrew/opt/python@3.11/bin/python3.11
Creating virtualenv at: hello

$ grep prompt hello/pyvenv.cfg

$ source hello/bin/activate

$ echo $VIRTUAL_ENV_PROMPT
hello
```

---

_@methane reviewed on 2024-02-17 17:37_

---

_Review comment by @methane on `crates/uv/src/main.rs`:656 on 2024-02-17 17:37_

The only difference in this PR from original virtualenv/venv is:

* if target directory is `.venv` (default), and
* if prompt is not specified,
* uv assumes `--prompt .` is specified.

---

_Comment by @MichaReiser on 2024-02-18 09:40_

Nice. We now also support batch files. Could you add the same logic to the activation.bat file?

---

_@dhruvmanila reviewed on 2024-02-19 10:06_

---

_Review comment by @dhruvmanila on `crates/uv/src/main.rs`:656 on 2024-02-19 10:06_

Oh sorry, my example was incorrect. Yours is right.

I thought about this and I think what you proposed is a good default behavior:
* By default, use the current working directory as the prompt value
* If the virtual environment path is provided, then use that as the prompt value
* If `--prompt` is given
	* Value is `.`, use current working directory name
	* Otherwise, use the value provided

---

_Review comment by @dhruvmanila on `crates/gourgeist/src/bare.rs`:37 on 2024-02-19 10:09_

This is probably good enough but I think it would be safe to use an additional argument. This creates a clear separation that all the `data` values are always present and that only `prompt` is optional. This will also avoid any future bugs where we avoid adding an entry just because the value is empty.

```rs
fn write_cfg(f: &mut impl Write, data: &[(&str, String); 8, prompt: Option<String>]) -> io::Result<()> {
    for (key, value) in data {
        writeln!(f, "{key} = {value}")?;
	}
	if let Some(prompt) = prompt {
		writeln!(f, "prompt = {prompt}");
	}
}
```

---

_Review comment by @dhruvmanila on `crates/uv/src/main.rs`:654 on 2024-02-19 10:12_

nit: to make it clear that this is an alternative to the default value used.

```suggestion
    /// Provide an alternative prompt prefix for this environment
```

---

_Review comment by @dhruvmanila on `crates/uv/src/main.rs`:1022 on 2024-02-19 10:18_

Can we use a `const` value here so as to avoid updating the default environment name in multiple places (in `VenvArgs`, and here)?

```rs
const DEFAULT_VENV_NAME: &str = ".venv";
```

---

_Review comment by @dhruvmanila on `crates/gourgeist/src/bare.rs`:127 on 2024-02-19 10:55_

As `prompt` is now an `Option<String>`:

```suggestion
    let prompt = match prompt {
        Some(".") => env::current_dir()?
            .file_name()
            .map(|name| name.to_string_lossy().to_string()),
        Some(p) => Some(p.to_string()),
        None => None,
    };
```

---

_Review comment by @dhruvmanila on `crates/gourgeist/src/bare.rs`:185 on 2024-02-19 10:56_

Based on my suggestion of using `Option<String>`:

```suggestion
            .replace("{{ VIRTUAL_PROMPT }}", prompt.as_deref().unwrap_or(""))
```

---

_@dhruvmanila approved on 2024-02-19 11:00_

I like the default behavior that you proposed. With that in mind, this is the final set of review comments and then it should be good to go. 

It's mainly a simplification of using `Option` to avoid using `is_empty` for every entry in `pyvenv.cfg`. This is to keep the invariant that other entries are always going to be written to the file while `prompt` is optional.

Thanks for your patience!

---

_@MichaReiser reviewed on 2024-02-19 11:05_

---

_Review comment by @MichaReiser on `crates/uv/src/main.rs`:654 on 2024-02-19 11:05_

Can we add a description of what the promot is and what it is used for? 

It also seems that the prompt value `.` has a special meaning that it uses the current working directory which isn't clear otherwise. It also seems to default to `.` if you use `.venv` which might be surprising to users. 

Is defaulting to `.` to use the current directory name the same as in pip? I wonder if we could come up with a design that uses less "magical" values

---

_@MichaReiser reviewed on 2024-02-19 11:07_

---

_Review comment by @MichaReiser on `crates/gourgeist/src/bare.rs`:67 on 2024-02-19 11:07_

Nit: It could make sense to define a custom `Prompt` enum instead of using `Option` that is explicit about the `.` meaning:

```rust
enum Prompt {
	// Use the name of the directory containing the `.venv` as prompt
	DirectoryName,

	// Use this fixed name
	Static(Strring),

	// Don't use a prompt
	None
}

---

_@methane reviewed on 2024-02-19 12:54_

---

_Review comment by @methane on `crates/uv/src/main.rs`:1022 on 2024-02-19 12:54_

Where is the best place for the const?

<img width="227" alt="image" src="https://github.com/astral-sh/uv/assets/199592/d75cf8bb-c354-4471-940b-1378b4242974">


---

_@dhruvmanila reviewed on 2024-02-19 19:04_

---

_Review comment by @dhruvmanila on `crates/uv/src/main.rs`:654 on 2024-02-19 19:04_

I've expanded the CLI docs with the default behavior which depends on the venv name and also with the list of possible values.

---

_Comment by @dhruvmanila on 2024-02-19 19:13_

@methane Thanks a lot for initiating this effort! I made some small tweaks at the end to accommodate Micha's recommendation:
1. Expanding the CLI flag documentation
2. Using a custom enum `Prompt`
3. Updated the PR description to list down the workflow

Congrats on landing your first PR and welcome to Astral! 🥳 

---

_Merged by @dhruvmanila on 2024-02-19 19:13_

---

_Closed by @dhruvmanila on 2024-02-19 19:13_

---

_Branch deleted on 2024-02-20 04:58_

---

_Comment by @frague59 on 2024-02-21 21:00_

Great job guys !

---
