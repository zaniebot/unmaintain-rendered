```yaml
number: 3082
title: "rg -v * breaks the code"
type: issue
state: closed
author: sonisnehal
labels:
  - invalid
assignees: []
created_at: 2025-07-02T03:59:16Z
updated_at: 2025-07-02T09:34:58Z
url: https://github.com/BurntSushi/ripgrep/issues/3082
synced_at: 2026-01-12T16:13:25Z
```

# rg -v * breaks the code

---

_@sonisnehal_

### Please tick this box to confirm you have reviewed the above.

- [x] I have a different issue.

### What version of ripgrep are you using?

ripgrep 14.1.1

### How did you install ripgrep?

brew install ripgrep

### What operating system are you using ripgrep on?

macOs

### Describe your bug.

When you run rg -v * command it goes in to infinite loop and prints random code

3220:function endTagInForeignContent(p, token) {
3221:    if (token.tagID === html_js_1.TAG_ID.P || token.tagID === html_js_1.TAG_ID.BR) {
3222:        popUntilHtmlOrIntegrationPoint(p);
3223:        p._endTagOutsideForeignContent(token);
3224:        return;
3225:    }
3226:    for (let i = p.openElements.stackTop; i > 0; i--) {
3227:        const element = p.openElements.items[i];
3228:        if (p.treeAdapter.getNamespaceURI(element) === html_js_1.NS.HTML) {
3229:            p._endTagOutsideForeignContent(token);
3230:            break;
3231:        }
3232:        const tagName = p.treeAdapter.getTagName(element);
3233:        if (tagName.toLowerCase() === token.tagName) {
3234:            //NOTE: update the token tag name for `_setEndLocation`.
3235:            token.tagName = tagName;
3236:            p.openElements.shortenToLength(i);
3237:            break;
3238:        }
3239:    }
3240:}

### What are the steps to reproduce the behavior?

run "rg -v *"

### What is the actual behavior?

Prints endless code

3220:function endTagInForeignContent(p, token) {
3221:    if (token.tagID === html_js_1.TAG_ID.P || token.tagID === html_js_1.TAG_ID.BR) {
3222:        popUntilHtmlOrIntegrationPoint(p);
3223:        p._endTagOutsideForeignContent(token);
3224:        return;
3225:    }
3226:    for (let i = p.openElements.stackTop; i > 0; i--) {
3227:        const element = p.openElements.items[i];
3228:        if (p.treeAdapter.getNamespaceURI(element) === html_js_1.NS.HTML) {
3229:            p._endTagOutsideForeignContent(token);
3230:            break;
3231:        }
3232:        const tagName = p.treeAdapter.getTagName(element);
3233:        if (tagName.toLowerCase() === token.tagName) {
3234:            //NOTE: update the token tag name for `_setEndLocation`.
3235:            token.tagName = tagName;
3236:            p.openElements.shortenToLength(i);
3237:            break;
3238:        }
3239:    }
3240:}

### What is the expected behavior?

it should return text which dont contain *

---

_Comment by @ltrzesniewski on 2025-07-02 08:50_

The `*` gets expanded by the shell to a list of files, which changes the meaning of the command. You need to escape it, like `'*'`, and since it's a regex quantifier, also add `-F` to disable regexes. 

---

_Closed by @BurntSushi on 2025-07-02 09:34_

---

_Label `invalid` added by @BurntSushi on 2025-07-02 09:34_

---
