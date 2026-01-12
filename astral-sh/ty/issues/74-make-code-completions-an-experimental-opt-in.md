```yaml
number: 74
title: make code completions an experimental opt-in before the alpha release
type: issue
state: closed
author: BurntSushi
labels:
  - server
  - completions
assignees: []
created_at: 2025-05-01T12:33:24Z
updated_at: 2025-10-03T12:20:16Z
url: https://github.com/astral-sh/ty/issues/74
synced_at: 2026-01-12T15:54:22Z
```

# make code completions an experimental opt-in before the alpha release

---

_@BurntSushi_

              We could do something similar to what r-a does with their "experimental" capabilities which requires explicit opt-in from client side. Refer to the first paragraph in https://github.com/rust-lang/rust-analyzer/blob/master/docs/book/src/contributing/lsp-extensions.md#lsp-extensions:

> All capabilities are enabled via the experimental field of `ClientCapabilities` or `ServerCapabilities`.

Specifically, [here](https://github.com/rust-lang/rust-analyzer/blob/5d66d45005fef79751294419ab9a9fa304dfdf5c/editors/code/src/client.ts#L290-L330) they send the capabilities that the VS Code extension supports.

So, for us it could look like below which would be sent when during the server initialization process:
```json
"capabilities": {
	"experimental": {
		"completions": {
			"enabled": true
		}
	}
} 
```

_Originally posted by @dhruvmanila in https://github.com/astral-sh/ruff/issues/17744#issuecomment-2843213742_
            

---

_Comment by @BurntSushi on 2025-05-01 12:34_

Specifically motivated by https://github.com/astral-sh/ruff/pull/17744#pullrequestreview-2808094461, where it's probably a good idea to _not_ have code completion until it means some minimal standard of quality. In astral-sh/ruff#17744, I just built a very small end-to-end MVP to make completions actually work via LSP.

---

_Label `server` added by @BurntSushi on 2025-05-01 12:34_

---

_Comment by @MichaReiser on 2025-05-01 12:37_

At least, before the LSP alpha, which might not be part of the initial CLI

---

_Comment by @MichaReiser on 2025-05-06 07:02_

CC: @charliermarsh in case you're interested in taking this on

---

_Comment by @charliermarsh on 2025-05-06 12:32_

Yeah I can do this.

---

_Assigned to @charliermarsh by @charliermarsh on 2025-05-06 13:08_

---

_Comment by @charliermarsh on 2025-05-07 03:07_

Looking into this more, I think these should just be `initializationOptions`, maybe? It seems like the whole `experimental` thing is more for servers to indicate to clients that they support non-standard extensions. \cc @dhruvmanila

---

_Comment by @dhruvmanila on 2025-05-07 13:04_

The `initializationOptions` just says that the server supports completions and what all specific features of completions that the server supports. This is mainly used by the client to make sure it asks for completions at the right time e.g., after the `.` character in `module.`.

What we want here is that, by default, the completions should be off as it's pretty barebones but we don't want to block users / us from trying out completions. And, we also don't want to be competing with other language servers that will still be required for full completion support. This is why we want it to be an opt-in feature rather than opt-out for now.

Does that make sense?

---

_Comment by @dhruvmanila on 2025-05-07 13:08_

We basically want the equivalent of `--preview` for the language server. If we used the `--preview` flag from the command-line, that would make it harder for users because it would require them to understand how to override the command used by the editor to start the server. And, that would be different for each editor. Another option if it's possible would be to make it a server option instead of relying on a specific namespace in the capabilities.

---

_Comment by @charliermarsh on 2025-05-07 13:12_

Makes sense, I'm just not sure that we want _exactly_ what RA is doing, since `experimental` is actually part of the LSP spec and has a specific meaning, and it's not clear to me that we actually want that meaning.

---

_Comment by @MichaReiser on 2025-05-07 13:39_

I searched the node lsp client code to see how and where to pass the experimental capabilities but I didn't see a way how it could be customized when creating the client:

https://github.com/microsoft/vscode-languageserver-node/blob/c02c91cb63c8524a476e32ff56343c2370881262/client/src/common/client.ts#L1404-L1405

I'd suggest to go with `initializationOptions` for now. We can migrate away from it in the future.

---

_Comment by @MichaReiser on 2025-05-07 13:42_

Nevermind, I think I found the relevant code:

1. https://github.com/rust-lang/rust-analyzer/blob/15d87419f1a123d8f888d608129c3ce3ff8f13d4/editors/code/src/client.ts#L291-L292
2. https://github.com/rust-lang/rust-analyzer/blob/15d87419f1a123d8f888d608129c3ce3ff8f13d4/editors/code/src/client.ts#L297-L340

---

_Comment by @charliermarsh on 2025-05-07 13:43_

Thanks, I'll look into it today.

---

_Label `server` added by @MichaReiser on 2025-05-07 15:08_

---

_Renamed from "[red-knot] make code completions an experimental opt-in before the alpha release" to "make code completions an experimental opt-in before the alpha release" by @MichaReiser on 2025-05-07 15:13_

---

_Comment by @charliermarsh on 2025-05-07 15:34_

I'm fairly certain that we don't want to use the experimental capabilities stuff, which is intended for non-standard extensions. I think we want to pass initialization options, then have the server _not_ return completion support unless the initialization options contain the relevant setting.

---

_Comment by @dhruvmanila on 2025-05-07 15:40_

> I think we want to pass initialization options, then have the server _not_ return completion support unless the initialization options contain the relevant setting.

I think it's fine to make it a server setting as well. Should we have a "preview" namespace for it? For example, the following in VS Code?
```json
{
	"ty.preview.completions": "true"
}
```

---

_Comment by @dhruvmanila on 2025-05-07 15:42_

I'm seeing precedence in other settings using "experimental" instead:
```jsonc
"typescript.tsserver.experimental.enableProjectDiagnostics": false,

// Enable the command to toggle the experimental notebook inline diff editor.
"notebook.diff.experimental.toggleInline": false,

"ruff.enableExperimentalFormatter": false,

// Use ESLint version 8.57 or later and `eslint.useFlatConfig` instead.
// Enables support of experimental Flat Config (aka eslint.config.js). Requires ESLint version >= 8.21 < 8.57.0).
"eslint.experimental.useFlatConfig": false,

// Try prettier's [new ternary formatting](https://github.com/prettier/prettier/pull/13183) before it becomes the default behavior.
"prettier.experimentalTernaries": false,
```

---

_Closed by @charliermarsh on 2025-05-07 16:39_

---

_Label `completions` added by @AlexWaygood on 2025-10-03 12:20_

---
