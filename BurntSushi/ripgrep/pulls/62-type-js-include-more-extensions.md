```yaml
number: 62
title: "--type js: include more extensions"
type: pull_request
state: merged
author: martinlindhe
labels: []
assignees: []
merged: true
base: master
head: js-wc
created_at: 2016-09-24T12:45:07Z
updated_at: 2016-09-25T15:16:59Z
url: https://github.com/BurntSushi/ripgrep/pull/62
synced_at: 2026-01-12T18:23:12Z
```

# --type js: include more extensions

---

_@martinlindhe_

Includes more commonly used extensions to the js type.
- coffe, used by coffescript
- ts, tsx, used by typescript
- jsx, used by react.js
- vue, used by vue.js


---

_Comment by @Keats on 2016-09-24 16:02_

TypeScript also uses `.tsx` for ts files with JSX in them


---

_Comment by @martinlindhe on 2016-09-24 17:16_

@Keats thanks for the suggestion. Patch updated to include .tsx


---

_Comment by @BurntSushi on 2016-09-24 18:06_

Hmm... I'm not an especially seasoned Javascript programmer, so I could be wrong about this, but it seems weird to lump things like CoffeeScript under the Javascript type. Was that intentional? Should CoffeeScript be given its own type? Same for TypeScript and JSX?


---

_Comment by @martinlindhe on 2016-09-25 06:25_

To me it was intentional to include coffescript (I saw there already is a separate coffescript type too).

Maybe js is a bit special, but I've many times messed with js project where more than one of these extensions are used.

For example right now i have a golang app, with a js frontend. The js frontend is built with vue.js so it is a mix of .js and .vue files. So when i am searching for a "javascript related" snippet, i don't want the mental burden of figuring "may the code be in a .vue component, or in a .js file?" (it's all javascript). And while coffescript might look differently, it compiles down to js and is used in projects together with other parts of js code, which might be in a .js file.

It is not uncommon to find coffescript stuff in your node_modules dependencies.

Regarding typescript, it is a superscript of javascript and is getting more popular as well.

So this patch address my use case (searching for "js stuff" in the "js-like files")

Regarding not lumping things together, I am thinking one could make it even better if it were possible to specify a type that consisted of multiple types. So we could have a "js" type, that for now could include *.js", coffescript, typescript, react and vue types.
This would require more changes to the code however, and I am not well suited to resolve it atm.


---

_Comment by @martinlindhe on 2016-09-25 06:42_

Also related, similar functionality in ag:
https://github.com/ggreer/the_silver_searcher/blob/31e1f8cb856736df254dccb62cf15e937002ef48/src/lang.c#L42

`{ "js", { "js", "jsx", "vue" } },`


---

_Comment by @BurntSushi on 2016-09-25 13:17_

I am pretty torn on this issue. It feels really wrong to lump entirely separate languages into the same file type.

Your use case does sound important, and I can see why you want this. I may request that you define your own type for this (which can be done conveniently with an alias until we get a config file).


---

_Comment by @BurntSushi on 2016-09-25 14:02_

This feature suggestion may make having a custom type a little easier: https://github.com/BurntSushi/ripgrep/issues/83


---

_Comment by @martinlindhe on 2016-09-25 14:06_

Okay, so to progress this patch, could it be accepted if we drop the coffescript stuff entirely? Or should i rather just drop it and live happy with some rg alias?

Regarding #83, interesting!


---

_Comment by @BurntSushi on 2016-09-25 14:39_

Well, the problem is that even JSX claims to be its own language. From https://jsx.github.io/:

> JSX is a statically-typed, object-oriented programming language designed to run on modern web browsers.

However, from what I know about JSX, in practice, I can see how it makes sense to lump it under the `js` type.

I looked for information about `.vue` files but couldn't find any. (I skimmed through the Vue project web site.) Are they claiming to be their own programming language too?


---

_Comment by @BurntSushi on 2016-09-25 14:41_

But the [React docs say](https://facebook.github.io/react/docs/jsx-in-depth.html) that it's not its own language, but a syntax extension:

> JSX is a JavaScript syntax extension that looks similar to XML.


---

_Comment by @Keats on 2016-09-25 14:42_

I think that's a different `jsx`, I could be wrong but I think @martinlindhe refers to the React syntax, not this language (last release was in 2014 so I guess the one from dena is not an active project anymore)


---

_Comment by @BurntSushi on 2016-09-25 14:43_

@Keats Oh. Thanks for the clarification!

I think my feeling here is that we should drop TypeScript and CoffeeScript from being designated as Javascript, but keep JSX.

I don't know about Vue though. Are `.vue` files template files? Wouldn't they be predominantly HTML?


---

_Comment by @Keats on 2016-09-25 14:47_

Looking at files like https://github.com/ElemeFE/vue-desktop/blob/master/src/basic/alert.vue, it seems .vue is a mix of html/css and js (similar to a react component with inline css).
Can `-t` and `-T` take lists?
I.e. to use `--type js,ts`


---

_Comment by @BurntSushi on 2016-09-25 14:52_

> Can -t and -T take lists?

Just specify `-t` multiple times. `-tjs -tts`.


---

_Comment by @martinlindhe on 2016-09-25 15:00_

@BurntSushi with regards to .vue files (vue components), they contain sections: one for html template, one for css, one for script (js), looks like ![https://vuejs.org/guide/application.html](https://vuejs.org/images/vue-component.png)

From experience, a vue component usually has the majority of code in javascript.


---

_Comment by @BurntSushi on 2016-09-25 15:02_

OK, I guess I'm fine with that.

So I'd say to take out TypeScript and CoffeeScript, keep jsx and vue, and let's get this merged.

Thank you so much for your patience and sticking with me. :-)


---

_Comment by @martinlindhe on 2016-09-25 15:07_

No problems! Reworked the patch according to comments.

I left out the following in order to stay on track for this PR, but I think this should be added to types.rs as well:

``` rust
("typescript", &["*.ts", "*.tsx"]),
```


---

_Merged by @BurntSushi on 2016-09-25 15:10_

---

_Closed by @BurntSushi on 2016-09-25 15:10_

---

_Comment by @BurntSushi on 2016-09-25 15:11_

Poifect. Adding TypeScript would be great. (I do kind of think the name should be `ts` though? `typescript` feels a bit long to type, but I think someone who uses TypeScript should weigh in on whether `ts` is acceptable or not.)


---

_Comment by @Keats on 2016-09-25 15:12_

`ts` is the preferred name yes


---

_Comment by @martinlindhe on 2016-09-25 15:16_

Addressed ts in #84


---
