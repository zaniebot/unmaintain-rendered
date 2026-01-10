```yaml
number: 18629
title: "[ty] Improve support for `object.<CURSOR>` completions"
type: pull_request
state: merged
author: BurntSushi
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: ag/completion-object-v2
created_at: 2025-06-11T15:00:07Z
updated_at: 2025-06-12T11:43:10Z
url: https://github.com/astral-sh/ruff/pull/18629
synced_at: 2026-01-10T18:45:04Z
```

# [ty] Improve support for `object.<CURSOR>` completions

---

_Pull request opened by @BurntSushi on 2025-06-11 15:00_

This makes it work for a number of additional cases, like nested
attribute access and things like `[].<CURSOR>`.

The basic idea is that instead of selecting a covering node closest to a
leaf that contains the cursor, we walk up the tree as much as we can.
This lets us access the correct `ExprAttribute` node when performing
nested access.

This also adds a number of tests covering some interesting cases.

Ref astral-sh/ty#86


---

_Review requested from @carljm by @BurntSushi on 2025-06-11 15:00_

---

_Review requested from @MichaReiser by @BurntSushi on 2025-06-11 15:00_

---

_Review requested from @AlexWaygood by @BurntSushi on 2025-06-11 15:00_

---

_Review requested from @sharkdp by @BurntSushi on 2025-06-11 15:00_

---

_Review requested from @dcreager by @BurntSushi on 2025-06-11 15:00_

---

_Review request for @dcreager removed by @BurntSushi on 2025-06-11 15:00_

---

_Review request for @carljm removed by @BurntSushi on 2025-06-11 15:00_

---

_Review request for @sharkdp removed by @BurntSushi on 2025-06-11 15:00_

---

_Review requested from @dhruvmanila by @BurntSushi on 2025-06-11 15:00_

---

_Comment by @github-actions[bot] on 2025-06-11 15:03_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Label `server` added by @MichaReiser on 2025-06-11 16:02_

---

_Label `ty` added by @MichaReiser on 2025-06-11 16:02_

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/completion.rs`:86 on 2025-06-11 16:18_

This is probably out of scope for this PR. But for something like `import collections.<CURSOR>` or `from collections.<CURSOR>`, do we want to be able to provide autocomplete suggestions for the `collections.abc` submodule of `collections` in due course? (That would be cool!)

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/completion.rs`:1 on 2025-06-11 16:24_

Both appear to work as expected, but is it also worth adding tests for `3.14.<CURSOR>` (should provide completions for attributes on `float` instances) and `(1).<CURSOR>` (should provide completions for attributes on `int` instances)?

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/find_node.rs`:81 on 2025-06-11 16:28_

slightly simpler?

```suggestion
        self.nodes.iter().rev().nth(1).copied()
```

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/find_node.rs`:72 on 2025-06-11 16:29_

```suggestion
        *self.nodes.last().expect("`CoveringNode::nodes` should always be non-empty")
```

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/completion.rs`:120 on 2025-06-11 16:36_

this appears to work great for `class Foo: ...` and `def bar(): ...` at the top level. But in nested contexts, I still seem to be getting "unwelcome" autocompletions after ellipses when I run the playground locally using this branch:

![image](https://github.com/user-attachments/assets/78c1fa06-12cc-4dcb-aafc-b05da289ad62)

![image](https://github.com/user-attachments/assets/28c5d870-a724-47b8-b351-0fcb81a700a9)



---

_Review comment by @AlexWaygood on `crates/ty_ide/src/completion.rs`:1481 on 2025-06-11 16:40_

Also probably not something for this PR, but consider something like this:

<details>
<summary>Playground screenshot</summary>

![image](https://github.com/user-attachments/assets/4dd6f097-192c-48f0-a414-d1c6f04cf9be)

</details>

Eventually, it would be great if `bar` and `mro` were positioned above `a` in the completion suggestions in this screenshot. `Foo.bar` and `Foo.mro` both have a `__defaults__` attribute (they're both `FunctionType` instances), but `Foo.a` does not (it's an `int`)!

---

_@AlexWaygood approved on 2025-06-11 16:41_

Great work!

---

_Review comment by @BurntSushi on `crates/ty_ide/src/completion.rs`:86 on 2025-06-11 17:57_

Definitely. Working on import auto-completions (`from module import <CURSOR>`) is next on my list. I think it would make sense to put `import module.<CURSOR>` in that bucket too.

---

_@BurntSushi reviewed on 2025-06-11 17:57_

---

_@BurntSushi reviewed on 2025-06-11 18:02_

---

_Review comment by @BurntSushi on `crates/ty_ide/src/completion.rs`:1 on 2025-06-11 18:02_

Ah yeah, I tested both of those cases ad hoc, but looks like I didn't add tests for them. Done!

---

_Review comment by @BurntSushi on `crates/ty_ide/src/find_node.rs`:81 on 2025-06-11 18:04_

Ehhhhh. I think I very slightly prefer what I wrote since what I want is random access and not iteration (even though [your snippet almost certainly compiles down to random access anyway](https://doc.rust-lang.org/src/core/slice/iter/macros.rs.html#205)).

---

_@BurntSushi reviewed on 2025-06-11 18:04_

---

_Review comment by @BurntSushi on `crates/ty_ide/src/completion.rs`:1481 on 2025-06-11 18:10_

Yeah that's a tricky one. Not just taking the prefix context into account, but the suffix context as well. I added that to the completion notes.

---

_@BurntSushi reviewed on 2025-06-11 18:10_

---

_@BurntSushi reviewed on 2025-06-11 18:13_

---

_Review comment by @BurntSushi on `crates/ty_ide/src/completion.rs`:120 on 2025-06-11 18:13_

Odd. I can't repro that in neovim. That is, if I have this:

```
class Foo:
    def bar(self): ...<CURSOR>
```

Then I don't get completions, even if I specifically ask for them.

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/completion.rs`:120 on 2025-06-11 18:16_

Huh... FWIW, the way I tested this PR locally with the playground was to checkout this branch, navigate to the `playground` directory, then run `npm start --workspace ty-playground`

---

_@AlexWaygood reviewed on 2025-06-11 18:16_

---

_@AlexWaygood reviewed on 2025-06-11 18:16_

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/completion.rs`:120 on 2025-06-11 18:16_

(no idea why the playground might be doing something different to neovim -- @MichaReiser or @dhruvmanila might be able to help there!)

---

_Comment by @BurntSushi on 2025-06-11 18:16_

Demo:

https://github.com/user-attachments/assets/27c89b16-50a3-49e3-b931-2bb201676589

(I still haven't setup VS Code.)

---

_@BurntSushi reviewed on 2025-06-11 18:22_

---

_Review comment by @BurntSushi on `crates/ty_ide/src/completion.rs`:120 on 2025-06-11 18:22_

I got this initially when running `npm start --workspace ty-playground`:

```
[INFO]: ⬇️  Installing wasm-bindgen...
[INFO]: License key is set in Cargo.toml but no LICENSE file(s) were found; Please add the LICENSE file(s) to your project directory
[INFO]: ✨   Done in 23.87s
[INFO]: 📦   Your wasm pkg is ready to publish at ../../playground/ty/ty_wasm.

> ty-playground@0.0.0 start
> vite

sh: line 1: vite: command not found
npm error Lifecycle script `start` failed with error:
npm error code 127
npm error path /home/andrew/astral/ruff/pr1/playground/ty
npm error workspace ty-playground@0.0.0
npm error location /home/andrew/astral/ruff/pr1/playground/ty
npm error command failed
npm error command sh -c vite
npm notice
npm notice New minor version of npm available! 11.3.0 -> 11.4.1
npm notice Changelog: https://github.com/npm/cli/releases/tag/v11.4.1
npm notice To update run: npm install -g npm@11.4.1
npm notice
```

I then went to install `vite` via the AUR and got this build error:

```
CMake Error at CMakeLists.txt:237 (message):
  libglm-dev package is required, you might specify the include directory
  where to find glm/glm.hpp through -DGLM_INC=/path/to/glm
```

I then installed `glm` and tried again and got more build errors:

```
CMake Error in src/CMakeLists.txt:
  Found relative path while evaluating include directories of "vite":

    "GLEW_INCLUDE_PATH-NOTFOUND"



CMake Error in src/CMakeLists.txt:
  Found relative path while evaluating include directories of "vite":

    "GLEW_INCLUDE_PATH-NOTFOUND"



CMake Error in src/CMakeLists.txt:
  Found relative path while evaluating include directories of "vite":

    "GLEW_INCLUDE_PATH-NOTFOUND"
```

I'll try futzing with this later.

---

_@BurntSushi reviewed on 2025-06-11 18:24_

---

_Review comment by @BurntSushi on `crates/ty_ide/src/completion.rs`:120 on 2025-06-11 18:24_

`vite` appears to be a bunk AUR package. Trying with `vite-git` now.

---

_@AlexWaygood reviewed on 2025-06-11 18:24_

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/completion.rs`:120 on 2025-06-11 18:24_

Hah, I haven't got a clue when it comes to Javascript/typescript -- @MichaReiser's _definitely_ your guy there 😆

---

_@BurntSushi reviewed on 2025-06-11 18:29_

---

_Review comment by @BurntSushi on `crates/ty_ide/src/completion.rs`:120 on 2025-06-11 18:29_

Okay I got it running... Does it serve it as a web site somewhere? How do I get to it?

---

_Comment by @BurntSushi on 2025-06-11 18:31_

Going to bring this in even with the unknown playground issue (which isn't a regression) since it improves a lot of other cases. I'm happy to do follow-ups if folks have them.

---

_Merged by @BurntSushi on 2025-06-11 18:31_

---

_Closed by @BurntSushi on 2025-06-11 18:31_

---

_Branch deleted on 2025-06-11 18:31_

---

_@carljm reviewed on 2025-06-11 18:32_

---

_Review comment by @carljm on `crates/ty_ide/src/completion.rs`:120 on 2025-06-11 18:32_

Should be serving at `localhost:5173` -- it should also say this on its terminal output, if it's working correctly?

---

_@AlexWaygood reviewed on 2025-06-11 18:37_

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/completion.rs`:120 on 2025-06-11 18:37_

Yeah I usually get to this state after running the command above:

![image](https://github.com/user-attachments/assets/f069505c-ffbc-4f51-a474-9a84ec4d270c)

And then I can just navigate to `localhost:5173` in a browser to use my local playground deploy. Which I can usually get to immediately by holding down `command` and then double-clicking on the `http://localhost:5173/` line in my terminal

---

_@BurntSushi reviewed on 2025-06-11 18:37_

---

_Review comment by @BurntSushi on `crates/ty_ide/src/completion.rs`:120 on 2025-06-11 18:37_

This is what I see:

```
[andrew@duff playground]$ npm start --workspace ty-playground

> ty-playground@0.0.0 prestart
> npm run dev:wasm


> ty-playground@0.0.0 dev:wasm
> wasm-pack build ../../crates/ty_wasm --dev --target web --out-dir ../../playground/ty/ty_wasm

[INFO]: 🎯  Checking for the Wasm target...
[INFO]: 🌀  Compiling to Wasm...
   Compiling ruff_python_ast v0.0.0 (/home/andrew/astral/ruff/pr1/crates/ruff_python_ast)
   Compiling ty_wasm v0.0.0 (/home/andrew/astral/ruff/pr1/crates/ty_wasm)
   Compiling ruff_python_parser v0.0.0 (/home/andrew/astral/ruff/pr1/crates/ruff_python_parser)
   Compiling ruff_python_literal v0.0.0 (/home/andrew/astral/ruff/pr1/crates/ruff_python_literal)
   Compiling ruff_db v0.0.0 (/home/andrew/astral/ruff/pr1/crates/ruff_db)
   Compiling ty_python_semantic v0.0.0 (/home/andrew/astral/ruff/pr1/crates/ty_python_semantic)
   Compiling ty_vendored v0.0.0 (/home/andrew/astral/ruff/pr1/crates/ty_vendored)
   Compiling ruff_python_formatter v0.0.0 (/home/andrew/astral/ruff/pr1/crates/ruff_python_formatter)
   Compiling ty_ide v0.0.0 (/home/andrew/astral/ruff/pr1/crates/ty_ide)
   Compiling ty_project v0.0.0 (/home/andrew/astral/ruff/pr1/crates/ty_project)
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 10.36s
[INFO]: ⬇️  Installing wasm-bindgen...
[INFO]: License key is set in Cargo.toml but no LICENSE file(s) were found; Please add the LICENSE file(s) to your project directory
[INFO]: ✨   Done in 13.45s
[INFO]: 📦   Your wasm pkg is ready to publish at ../../playground/ty/ty_wasm.

> ty-playground@0.0.0 start
> vite
```

A "vite" window opens, but it's blank. And:

```
[andrew@duff playground]$ curl localhost:5173
curl: (7) Failed to connect to localhost port 5173 after 0 ms: Could not connect to server
```

I am doing this on a remote box... And I'm guessing the vite window is opening via X11 forwarding. So I wonder if that's messing with things.

---

_@BurntSushi reviewed on 2025-06-11 18:39_

---

_Review comment by @BurntSushi on `crates/ty_ide/src/completion.rs`:120 on 2025-06-11 18:39_

Yeah it looks like things are hanging right after running `vite`. So this is probably an X11 forwarding issue. Boooooo.

I'll try this directly on my laptop tomorrow.

---

_@dhruvmanila reviewed on 2025-06-12 07:46_

This looks good!

I haven't been able to reproduce the nested context issue that Alex pointed out, I've asked him to see if it's reproducible now otherwise we can ignore that for now.

I think it might be useful to add couple of tests when the dot (`.`) character is inside a string and f-string. Following are couple of examples that already work correctly:
```py
foo = 1
bar = 2

class Foo:
    def method(self): ...

f = Foo()

# String, this is not an attribute access
"f.<CURSOR>
# F-string, this is an attribute access
f"{f.<CURSOR>
```

---

_Comment by @AlexWaygood on 2025-06-12 11:01_

> I haven't been able to reproduce the nested context issue that Alex pointed out, I've asked him to see if it's reproducible now otherwise we can ignore that for now.

This still reproduces for me on https://play.ty.dev/:

https://github.com/user-attachments/assets/c5703c90-af9b-415c-bf6e-325fc84a64f1


---

_Comment by @dhruvmanila on 2025-06-12 11:37_

> This still reproduces for me on [play.ty.dev](https://play.ty.dev/):

Interesting! Thanks, I'm able to reproduce it on the playground and in VS Code but not in Neovim!

<img width="624" alt="Screenshot 2025-06-12 at 17 03 44" src="https://github.com/user-attachments/assets/47cbf1e3-f8de-434a-9cd2-4d4aeffa5e00" />

And, the trace logs:
```
[Trace - 5:06:24 PM] Sending request 'textDocument/completion - (146)'.
Params: {
    "textDocument": {
        "uri": "file:///Users/dhruv/playground/ty_server/completions.py"
    },
    "position": {
        "line": 1,
        "character": 16
    },
    "context": {
        "triggerKind": 2,
        "triggerCharacter": "."
    }
}


[Trace - 5:06:24 PM] Received response 'textDocument/completion - (146)' in 1ms.
Result: [
    {
        "label": "Foo"
    }
]
```

The above trace logs is for the following snippet along with the cursor position:
```py
class Foo:
    class Bar: .<CURSOR>
```

---

_Comment by @BurntSushi on 2025-06-12 11:43_

Interesting. I've recorded that in my notes to try and address later. Once I get around to setting up VS Code.

---
