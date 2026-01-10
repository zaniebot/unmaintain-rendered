```yaml
number: 5397
title: Improve copy of console command examples
type: pull_request
state: merged
author: eth3lbert
labels:
  - documentation
  - preview
assignees: []
merged: true
base: main
head: doc-copy-console
created_at: 2024-07-24T04:36:45Z
updated_at: 2024-07-31T14:52:18Z
url: https://github.com/astral-sh/uv/pull/5397
synced_at: 2026-01-10T13:37:23Z
```

# Improve copy of console command examples

---

_Pull request opened by @eth3lbert on 2024-07-24 04:36_

## Summary

This PR improves the copy of the console command example by:

- Preventing the selection of generic prompts and generic output
- Lazily setting copy content by leveraging intersection observer

Most of the changes are inspired by https://github.com/opensafely/documentation/pull/1461

Some other useful refs:
- https://github.com/squidfunk/mkdocs-material/issues/3647
- https://mkdocstrings.github.io/recipes/#prevent-selection-of-prompts-and-output-in-python-code-blocks

Resolves #5355

## Test Plan

- 
  ``` shell-session
  mkdocs serve -f mkdocs.public.yml
  ``` 
- Navigate to http://localhost:8000/uv/first-steps/#viewing-the-version
- Try clicking the copy button
- Try copying by selecting the content



---

_@eth3lbert reviewed on 2024-07-24 04:42_

Noted: The current implementation is not restricted to `console` code blocks at this time.
If we find it too general, we could restrict it to specific languages by following the warning guidelines in https://mkdocstrings.github.io/recipes/#prevent-selection-of-prompts-and-output-in-python-code-blocks.

---

_Assigned to @zanieb by @zanieb on 2024-07-24 14:42_

---

_Comment by @zanieb on 2024-07-24 14:42_

Thanks! I'll play with this a bit.

---

_Comment by @zanieb on 2024-07-24 14:52_

Huh this isn't working for me on Vivaldi, it's working for you?

---

_Comment by @eth3lbert on 2024-07-24 14:57_

> Huh this isn't working for me on Vivaldi, it's working for you?

Oh, really? I've tested it on Firefox and Arc without any issues.
I'll get Vivaldi and see what's going on.

---

_Comment by @zanieb on 2024-07-24 15:01_

Vivaldi is Chromium based fwiw. I can try Firefox too.

---

_Comment by @zanieb on 2024-07-24 15:05_

Doesn't work on Firefox for me either, let me see if it's something in my setup.

---

_Comment by @zanieb on 2024-07-24 15:06_

It's not my ad-blocker (uBlock Origin)

---

_Comment by @eth3lbert on 2024-07-24 16:06_

> Vivaldi is Chromium based fwiw. I can try Firefox too.

Yes, I can reproduce the issue as well and have found a solution.

Instead of using
```js
document.addEventListener("DOMContentLoaded", () => {
  setCopyText();
});
```
we should use the following instead
``` js
document$.subscribe(function() {
  setCopyText();
})
```
Follow the `How to integrate with third-party JavaScript libraries` guideline: https://squidfunk.github.io/mkdocs-material/customization/?h=javascript#additional-javascript

---

_Comment by @eth3lbert on 2024-07-30 19:18_

@zanieb A friendly ping, in case you missed the fix.

---

_Comment by @zanieb on 2024-07-30 19:49_

I was on vacation :) I'll take a look at it this week though!

---

_Label `documentation` added by @zanieb on 2024-07-30 19:49_

---

_Label `preview` added by @zanieb on 2024-07-30 19:49_

---

_Comment by @zanieb on 2024-07-30 20:09_

This still doesn't work for me in Vivaldi or Firefox on macOS :/

---

_Comment by @eth3lbert on 2024-07-30 20:21_

> This still doesn't work for me in Vivaldi or Firefox on macOS :/

Weird, I just tested it in Vivaldi (6.8.3381.48), Firefox (128.0.3) and Arc (1.52.0) on macOS without any issue.

Have you seen any js errors in the developer tools?


---

_Comment by @zanieb on 2024-07-30 20:27_

Weird.. no errors in the developer tools console, just the "Enabled live reload" message. I can see `extra.js` was loaded in the network tab.

---

_Comment by @zanieb on 2024-07-30 20:35_

(Sorry I don't have the extra time to dig into what's going on right now)

---

_Comment by @eth3lbert on 2024-07-30 20:53_

No worries, we can revisit this later anytime.

---

_Comment by @eth3lbert on 2024-07-31 05:11_

The following is a non-lazy patch that has been modified to allow work with instant mode, which you might also like to try:

```js
function getTextWithoutPromptAndOutput(targetSelector) {
  const targetElement = document.querySelector(targetSelector);

  // exclude "Generic Prompt" and "Generic Output" spans from copy
  const excludedClasses = ["gp", "go"];

  const clipboardText = [];
  [...targetElement.childNodes].map((node) => {
    // If the element does not contain the matching class,
    // add to the clipboard text array
    if (
      !excludedClasses.some((className) => node?.classList?.contains(className))
    ) {
      return clipboardText.push(node.textContent);
    }

    return null;
  });

  return clipboardText.join("").trim();
}

function patchCopyCodeButtons() {
  // select all "copy" buttons whose target selector is a <code> element
  [
    ...document.querySelectorAll(
      `button.md-clipboard[data-clipboard-target$="code"]`,
    ),
  ].map((btn) =>
    btn.setAttribute(
      "data-clipboard-text",
      getTextWithoutPromptAndOutput(btn.dataset.clipboardTarget),
    ),
  );
}

document$.subscribe(function () {
  patchCopyCodeButtons();
});
```

Although both lazy and non-lazy work for me.

---

_Comment by @zanieb on 2024-07-31 13:28_

Hey so uhh have we been talking about different things the whole time?

I'm pressing the "copy text" button (which includes the "$" in the clipboard).

I just realized if I try to _highlight_ and copy the text it works as described here ("$" is excluded) :)

---

_Comment by @eth3lbert on 2024-07-31 13:36_

Hmmm, the results are not as expected.
The expected behavior would be to enable both regardless of whether
1. the user selects and copies the text. (css part)
2. clicks the copy button. (js part)

---

_Comment by @zanieb on 2024-07-31 13:39_

Ah tragic. I don't understand why the JS isn't working for me... I'll see if someone else on our side can reproduce.

---

_Comment by @eth3lbert on 2024-07-31 13:54_

Oh, and I think there's one more thing we could check first. Check the data attribute `data-clipboard-text` of the copy button. It's supposed to have the text without the `$`.

---

_Comment by @zanieb on 2024-07-31 14:01_

I don't see that attribute. I feel like I'm doing something wrong 😭 

```html
<button class="md-code__button" title="Copy to clipboard" data-clipboard-target="#__code_0 > code" data-md-type="copy"></button>
```

If I add

```diff
diff --git a/docs/js/extra.js b/docs/js/extra.js
index a8058688..8b08034c 100644
--- a/docs/js/extra.js
+++ b/docs/js/extra.js
@@ -25,6 +25,7 @@ function setCopyText() {
   const elements = document.querySelectorAll(
     'button.md-clipboard[data-clipboard-target$="code"]',
   );
+  console.log(elements);
   const observer = new IntersectionObserver((entries) => {
     entries.forEach((entry) => {
       // target in the viewport that have not been patched
```

I see `NodeList []` in the console.

---

_Comment by @zanieb on 2024-07-31 14:04_

Doing something sloppy like this makes it work

```diff
diff --git a/docs/js/extra.js b/docs/js/extra.js
index a8058688..dd49ab48 100644
--- a/docs/js/extra.js
+++ b/docs/js/extra.js
@@ -23,8 +23,9 @@ function setCopyText() {
   const attr = "clipboardText";
   // all "copy" buttons whose target selector is a <code> element
   const elements = document.querySelectorAll(
-    'button.md-clipboard[data-clipboard-target$="code"]',
+    "#__code_0 > nav > button"
   );
+  console.log(elements);
   const observer = new IntersectionObserver((entries) => {
     entries.forEach((entry) => {
       // target in the viewport that have not been patched
```

---

_Comment by @zanieb on 2024-07-31 14:07_

(Is it possible it's a difference from the insiders build?)

---

_Comment by @eth3lbert on 2024-07-31 14:08_

Indeed, I can reproduce the issue on https://docs.astral.sh/uv/. There is also no button with the `md-clipboard` class.  I'll change the class name.

---

_Comment by @zanieb on 2024-07-31 14:14_

Victory!

---

_Comment by @zanieb on 2024-07-31 14:14_

Thanks :)

---

_@eth3lbert reviewed on 2024-07-31 14:15_

---

_Review comment by @eth3lbert on `docs/js/extra.js`:26 on 2024-07-31 14:15_

Could also consider just use
```suggestion
 'button[data-clipboard-target$="code"]',
 ```

---

_@zanieb reviewed on 2024-07-31 14:16_

---

_Review comment by @zanieb on `docs/js/extra.js`:26 on 2024-07-31 14:16_

Seems reasonable too.

---

_Comment by @eth3lbert on 2024-07-31 14:16_

Thanks for your time and the review :)

---

_Comment by @zanieb on 2024-07-31 14:27_

Is there a compelling argument for the more complicated lazy-version or should we do the second patch you suggested?

---

_Comment by @eth3lbert on 2024-07-31 14:40_

The lazy version only patches elements within the viewport via `IntersectionObserver`, while the non-lazy version patches all elements. Therefore, for long pages or pages with a large number of copy buttons and content, the lazy version would be preferable.

The only drawback of the lazy version is browser compatibility.

You can find more information on browser compatibility here: https://developer.mozilla.org/en-US/docs/Web/API/IntersectionObserver/IntersectionObserver#browser_compatibility

Currently, `IntersectionObserver` enjoys baseline wide availability.  If you have concerns and need to support even older browsers, then I would suggest using the non-lazy version.

---

_@zanieb approved on 2024-07-31 14:43_

---

_Comment by @eth3lbert on 2024-07-31 14:47_

@zanieb Could you also indent the suggestion before merging :)

---

_@zanieb reviewed on 2024-07-31 14:48_

---

_Review comment by @zanieb on `docs/js/extra.js`:26 on 2024-07-31 14:48_

```suggestion
   'button[data-clipboard-target$="code"]',
```

---

_Review comment by @zanieb on `docs/js/extra.js`:26 on 2024-07-31 14:49_

```suggestion
     'button[data-clipboard-target$="code"]',
```

---

_@zanieb reviewed on 2024-07-31 14:49_

---

_Comment by @zanieb on 2024-07-31 14:50_

Thanks !

---

_Merged by @zanieb on 2024-07-31 14:52_

---

_Closed by @zanieb on 2024-07-31 14:52_

---

_Branch deleted on 2024-07-31 14:52_

---
