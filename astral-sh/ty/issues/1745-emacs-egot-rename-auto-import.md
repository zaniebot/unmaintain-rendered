```yaml
number: 1745
title: emacs egot rename/auto import
type: issue
state: closed
author: danielpopescu
labels:
  - server
assignees: []
created_at: 2025-12-03T16:03:23Z
updated_at: 2025-12-04T13:40:17Z
url: https://github.com/astral-sh/ty/issues/1745
synced_at: 2026-01-10T01:56:40Z
```

# emacs egot rename/auto import

---

_Issue opened by @danielpopescu on 2025-12-03 16:03_

### Summary

Hello... excited about this new type checker/LSP server. Testing with emacs+eglot, already looks pretty good (very snappy!). I did noticed that the rename variable is not working with latest version... Here's my config:

```
  (setq-default eglot-workspace-configuration
                `(
                :ty (
                     :diagnosticMode "workspace"
                     :experimental (:rename t
                                    :autoImport t)
                     :inlayHints (:variableTypes t
                                  :callArgumentNames t)
                     )
                    )
                )
```

Is this a known issue?


Thanks


### Version

_No response_

---

_Comment by @MichaReiser on 2025-12-03 16:51_

Can you share a specific example of what you tried to rename. There are some known limitations with the rename feature

---

_Label `needs-info` added by @MichaReiser on 2025-12-03 16:51_

---

_Label `server` added by @AlexWaygood on 2025-12-03 16:58_

---

_Comment by @danielpopescu on 2025-12-03 17:03_

Really, just rename anything (variable, parameter, etc). For instance, in this function:

```py
def list_split[T](lst: list[T], k: int) -> list[list[T]]:
    assert k > 0
    num = len(lst) // k
    return [
        lst[(i * num) : ((i + 1) * num if i < k - 1 else len(lst))] for i in range(k)
    ]
```


attempts to rename lst or num, fail with: eglot--error: [eglot] Unsupported or ignored LSP capability ‘:renameProvider’






---

_Comment by @MichaReiser on 2025-12-03 17:34_

I don't know emacs but could you try removing the `:ty` prefix from the settings? 

If eglot has a way to log the client/server communication, could you enable it and share the initial messages between client/server?

---

_Comment by @danielpopescu on 2025-12-03 18:40_

I cannot remove ty, as that is how the LSP config is defined for eglot: https://elpa.gnu.org/devel/doc/eglot.html#Project_002dspecific-configuration

Here's the beginning of the log:

```
[jsonrpc] D[13:35:33.125] Running language server: /Users/xyz/.venv/bin/ty server
[jsonrpc] e[13:35:33.125] --> initialize[1] {"jsonrpc":"2.0","id":1,"method":"initialize","params":{"processId":14322,"clientInfo":{"name":"Eglot","version":"1.19.0.20251130.135230"},"rootPath":"/Users/xyz/tmp/","rootUri":"file:///Users/xyz/tmp","initializationOptions":{},"capabilities":{"workspace":{"applyEdit":true,"executeCommand":{"dynamicRegistration":false},"workspaceEdit":{"documentChanges":true},"didChangeWatchedFiles":{"dynamicRegistration":true},"symbol":{"dynamicRegistration":false},"semanticTokens":{"refreshSupport":true},"configuration":true,"workspaceFolders":true},"textDocument":{"synchronization":{"dynamicRegistration":false,"willSave":true,"willSaveWaitUntil":true,"didSave":true},"completion":{"dynamicRegistration":false,"completionItem":{"snippetSupport":true,"deprecatedSupport":true,"resolveSupport":{"properties":["documentation","details","additionalTextEdits"]},"tagSupport":{"valueSet":[1]},"insertReplaceSupport":true},"contextSupport":true},"hover":{"dynamicRegistration":false,"contentFormat":["markdown","plaintext"]},"signatureHelp":{"dynamicRegistration":false,"signatureInformation":{"parameterInformation":{"labelOffsetSupport":true},"documentationFormat":["markdown","plaintext"],"activeParameterSupport":true}},"references":{"dynamicRegistration":false},"definition":{"dynamicRegistration":false,"linkSupport":true},"declaration":{"dynamicRegistration":false,"linkSupport":true},"implementation":{"dynamicRegistration":false,"linkSupport":true},"typeDefinition":{"dynamicRegistration":false,"linkSupport":true},"documentSymbol":{"dynamicRegistration":false,"hierarchicalDocumentSymbolSupport":true,"symbolKind":{"valueSet":[1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26]}},"documentHighlight":{"dynamicRegistration":false},"codeAction":{"dynamicRegistration":false,"resolveSupport":{"properties":["edit","command"]},"dataSupport":true,"codeActionLiteralSupport":{"codeActionKind":{"valueSet":["quickfix","refactor","refactor.extract","refactor.inline","refactor.rewrite","source","source.organizeImports"]}},"isPreferredSupport":true},"formatting":{"dynamicRegistration":false},"rangeFormatting":{"dynamicRegistration":false},"rename":{"dynamicRegistration":false},"semanticTokens":{"dynamicRegistration":false,"requests":{"quote":{"full":{"delta":true}}},"overlappingTokenSupport":true,"multilineTokenSupport":true,"tokenTypes":["namespace","type","class","enum","interface","struct","typeParameter","parameter","variable","property","enumMember","event","function","method","macro","keyword","modifier","comment","string","number","regexp","operator","decorator"],"tokenModifiers":["declaration","definition","readonly","static","deprecated","abstract","async","modification","documentation","defaultLibrary"],"formats":["relative"]},"inlayHint":{"dynamicRegistration":false},"callHierarchy":{"dynamicRegistration":false},"typeHierarchy":{"dynamicRegistration":false},"publishDiagnostics":{"relatedInformation":false,"versionSupport":true,"codeDescriptionSupport":false,"tagSupport":{"valueSet":[1,2]}}},"window":{"showDocument":{"support":true},"showMessage":{"messageActionItem":{"additionalPropertiesSupport":true}},"workDoneProgress":true},"general":{"positionEncodings":["utf-32","utf-8","utf-16"]},"experimental":{}},"workspaceFolders":[{"uri":"file:///Users/xyz/tmp","name":"~/tmp/"}]}}
[stderr]  2025-12-03 13:35:33.130330000  INFO Version: 0.0.1-alpha.29 (0c3cae494 2025-11-28)
[jsonrpc] e[13:35:33.133] <-- initialize[1] {"jsonrpc":"2.0","id":1,"result":{"capabilities":{"codeActionProvider":{"codeActionKinds":["quickfix"]},"completionProvider":{"triggerCharacters":["."]},"declarationProvider":true,"definitionProvider":true,"diagnosticProvider":{"identifier":"ty","interFileDependencies":true,"workDoneProgress":true,"workspaceDiagnostics":true},"documentHighlightProvider":true,"documentSymbolProvider":true,"executeCommandProvider":{"commands":["ty.printDebugInformation"],"workDoneProgress":false},"hoverProvider":true,"inlayHintProvider":{},"notebookDocumentSync":{"notebookSelector":[{"cells":[{"language":"python"}]}],"save":false},"positionEncoding":"utf-8","referencesProvider":true,"selectionRangeProvider":true,"semanticTokensProvider":{"full":true,"legend":{"tokenModifiers":["definition","readonly","async"],"tokenTypes":["namespace","class","parameter","selfParameter","clsParameter","variable","property","function","method","keyword","string","number","decorator","builtinConstant","typeParameter"]},"range":true},"signatureHelpProvider":{"retriggerCharacters":[")"],"triggerCharacters":["(",","]},"textDocumentSync":{"change":2,"openClose":true},"typeDefinitionProvider":true,"workspaceSymbolProvider":true},"serverInfo":{"name":"ty","version":"0.0.1-alpha.29 (0c3cae494 2025-11-28)"}}}
[jsonrpc] e[13:35:33.133] --> initialized {"jsonrpc":"2.0","method":"initialized","params":{}}
```

---

_Comment by @MichaReiser on 2025-12-04 07:39_

Thanks, that's useful. The interesting bit from the logs is:

```
"rename":{"dynamicRegistration":false},
```


Eglot doesn't support dynamically registering the rename capability, but ty doesn't know whether you've enabled renaming when using workspace configuration during startup. 

Can you try to pass the `experimental.rename` setting as part of the `initializationOptions` options?

@dhruvmanila What do you think of changing the server capabilities to always register the rename provider if the client doesn't support dynamic registration and then checking the option in `can_rename` (we could even promote users to enable the experimental option).

---

_Comment by @dhruvmanila on 2025-12-04 08:15_

> What do you think of changing the server capabilities to always register the rename provider if the client doesn't support dynamic registration and then checking the option in `can_rename` (we could even promote users to enable the experimental option).

This already happens:

https://github.com/astral-sh/ruff/blob/4c86300091de5b9eafecbdd364299ebb43752b95/crates/ty_server/src/capabilities.rs#L366-L376

Or, do you mean something else?



---

_Comment by @dhruvmanila on 2025-12-04 08:21_

Oh, are these workspace configuration?

> Can you try to pass the `experimental.rename` setting as part of the `initializationOptions` options?

I think this should work but I think it should be fine to do what you've suggested which is to always advertise support for rename capability and return early during the `prepareRename` request similar to how it is if language services are disabled.

---

_Comment by @MichaReiser on 2025-12-04 08:21_

We only announce rename support if it is enabled in initialize options. I suggest to always announce the rename capability for clients not supporting dynamic registration and bail out in can_rename if the setting's turned off

---

_Comment by @dhruvmanila on 2025-12-04 08:25_

That seems reasonable, I'd prefer that the check be in `prepareRename` request handler instead of the `can_rename` function.

---

_Label `needs-info` removed by @dhruvmanila on 2025-12-04 08:26_

---

_Comment by @danielpopescu on 2025-12-04 12:49_

Just a quick update... I did augment initializationOptions with the experimental options: 

  (add-to-list 'eglot-server-programs
               '((python-ts-mode) . ("/Users/dpo/.venv/bin/ty" "server" :initializationOptions (
                     :experimental (:rename t
                                    :autoImport t)
                                                                                                ))))


and rename works!

Nonetheless, this is confusing  as which options can be in the workspace configuration cand which ones need to be in  initializationOptions?

For reference, I use clangd, rust_analyzer, haskell language server, pyright, etc.... and entire configuration is done via workspace definition... this is the first LSP that I needed to touch initializationOptions... 

Thank you!

---

_Comment by @MichaReiser on 2025-12-04 12:56_

Yes I agree and this is what I and Dhruv were discussing. [#21789](https://github.com/astral-sh/ruff/pull/21789) changes the behavior so that you can use `initialize_options` or workspace options. 

---

_Comment by @danielpopescu on 2025-12-04 13:01_

Excellent. Just to be sure, this will apply not only to rename but also to any other existing or future options... one can set it up via workspace definition and initialize_options?

Thank you.

---

_Comment by @MichaReiser on 2025-12-04 13:20_

Yes, as far as I know this already works for the other experimental settings

---

_Comment by @danielpopescu on 2025-12-04 13:33_

Ok. Thanks. Just fyi, I did type polars. and polars was not auto imported in my file... this was even after I added to initialize_options. Maybe auto import is not working, or not using it right?

---

_Comment by @BurntSushi on 2025-12-04 13:40_

@danielpopescu I don't think auto-import supports completion module names (outside of `import` statements) yet. See https://github.com/astral-sh/ty/issues/1530. I believe it's easy to add, just hasn't been done yet.

---

_Closed by @MichaReiser on 2025-12-04 13:40_

---
