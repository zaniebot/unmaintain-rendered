```yaml
number: 1190
title: Should ty  support the Type Server Protocol?
type: issue
state: open
author: MichaReiser
labels:
  - question
assignees: []
created_at: 2025-09-16T06:27:44Z
updated_at: 2025-10-06T23:20:40Z
url: https://github.com/astral-sh/ty/issues/1190
synced_at: 2026-01-10T02:06:25Z
```

# Should ty  support the Type Server Protocol?

---

_Issue opened by @MichaReiser on 2025-09-16 06:27_


### Discussed in https://github.com/astral-sh/ruff/discussions/20424

<div type='discussions-op-text'>

<sup>Originally posted by **rchiodo** September 15, 2025</sup>
I proposed this idea back in May:
https://github.com/microsoft/pylance-release/discussions/7180

And I think we're (Pylance) getting close to being able to actually ship something that uses TSP. We'll be using it to launch Pyright (and soon Pyrefly) as our type server.

Would you guys be open to me submitting PRs to add TSP support to Ty? I already have a skeleton working here:
https://github.com/rchiodo/ruff/tree/rchiodo/tsp_investigation

I'd love to submit that work and finish implementing all of the TSP requests in the protocol.</div>

Posted by @rchiodo

---

_Label `question` added by @MichaReiser on 2025-09-16 06:28_

---

_Renamed from "Should 'Ty'  support the Type Server Protocol?" to "Should ty  support the Type Server Protocol?" by @MichaReiser on 2025-09-16 06:28_

---

_Comment by @rchiodo on 2025-09-16 16:11_

My hope is that the TSP work can sit on the side of the LSP. It delegates to an LSP server and just adds handlers on top of it. That should mean any work done to the LSP server won't cause any changes in the TSP server (and therefore the TSP work won't interfere with other work going on).

---

_Comment by @MichaReiser on 2025-09-22 07:17_

Hi @rchiodo, and thank you for reaching out. 

We're currently focusing on the Beta release of ty, which is why our resources are a bit tight right now, but that shouldn't prevent us from moving this discussion forward. But it may mean that we're slower in responding.

Like you, we want Python users to have a great experience when using VS Code, and Pylance plays a key role in the VS Code Python experience, so we're interested in figuring out how to provide a great experience when using ty and Pylance side-by-side. I have some questions regarding the user experience, and I would like to clarify them before we begin any implementation.

* Can users use ty and Pylance together, or is it one or the other? If users can use both extensions together, is there any resource sharing between them, or are both using their own ty instance?
* Our goal is to make ty a full-featured type checker LSP that provides a great experience across all editors, and we've made significant progress towards this goal in the last few months. Pylance can provide users with a more uniform experience if it acts as an intermediary for the LSP operations, but it's unclear whether this is worth the memory, latency, and performance overhead that an intermediary introduces. Is it the goal that Pylance wraps all operations, or would the TSP mode allow some requests to go directly to ty, and Pylance only hooks into/takes over operations where it significantly enhances the experience? What are examples where Pylance can provide a better experience that we can't do in a generic LSP, i.e., that would lead to a better experience when using ty and Pylance together that we wouldn't get when using ty alone?
* I haven't thought about this deeply, but each type checker uses a slightly different type system (even type variants). It seems tricky to map them all to a unified type system in Pylance. For example, ty supports intersections and uses them extensively for type narrowing and other operations. Does this require an extension to the TSP protocol and Pylance? 
* Related to the above: ty's type system will evolve, which may require changing existing types or introducing entirely new types (such as intersections) that necessitate changes to the TSP (and Pylance). Would ty have to maintain backward compatibility to avoid breaking older Pylance extensions (this seems non-trivial to me)? Would Pylance support multiple TSP versions to support both old and new ty verisons? 


I'm curious to hear your thoughts on this.

---

_Comment by @rchiodo on 2025-09-22 16:11_

Hi @MichaReiser,

Let me try and answer your questions:

> Can users use ty and Pylance together, or is it one or the other? If users can use both extensions together, is there any resource sharing between them, or are both using their own ty instance?

I think we'd have one or the other. Pylance would be the default experience, but in the default experience it can use Ty as the provider of types. So Pylance itself would ship with the Ty TSP server (or download it on the fly). User's could pick 'Ty' or 'Ruff' as their source of errors and then Pylance would change to use Ty as the TSP server instead of Pyright.

The Ty LSP extension would be separate and wouldn't run at the same time as Pylance (Pylance would likely disable itself). It would provide all of the LSP support for Python and Pylance wouldn't be needed. 

Alternatively, we could enable both at the same time. Pylance and Ty LSP would share the same server. But that seems trickier to manage.

> Is it the goal that Pylance wraps all operations, or would the TSP mode allow some requests to go directly to ty, and Pylance only hooks into/takes over operations where it significantly enhances the experience? 

That's the goal. The idea is that Pylance is an LSP wrapper on top of other Type Providers (and Language Servers). For example our hover provider currently supports parsing restructured text and generating Sphinx like markdown from it. It can do this with any TSP provider. This table here outlines the things we did on top of Pyright that were extra:

https://github.com/microsoft/pylance-release/discussions/7180 (table in first entry).

But for example, we added nothing to diagnostics. Diagnostics would be a straight LSP request and wouldn't change the output.

Not sure yet about other things. Internally our hover provider has 4 or 5 different providers, we might use LSP to get hover from Ty and then apply the other 3 or 4 on top of that. 

I think the goal is to provide the best Python support possible in the IDE, so whatever that takes is what we'll try. I don't think there's a clear - this request goes all the way through to the TSP server, and that request is wrapped. I imagine we'll have to try it a bunch and see what the difference is. 

> I haven't thought about this deeply, but each type checker uses a slightly different type system (even type variants). It seems tricky to map them all to a unified type system in Pylance. For example, ty supports intersections and uses them extensively for type narrowing and other operations. Does this require an extension to the TSP protocol and Pylance?

My hope is that all type handling is done on the Type Server side of the TSP. That Pylance doesn't need to know about intersections (although that sounds like it might). That's what the 'handle' was for in the current TSP protocol. The Type Server would be able to abstract its internal state and just return opaque handles to the client. 

I think this part is going to be tricky though. So far, we have semantic tokens and hover working through TSP. And as we go farther along, we're finding we need more information that's more specific to Pyright to figure out results. I don't know yet if we can abstract out all of this information and still provide the same level of detail in our LSP responses. 

I guess this part is wait and see.

> Related to the above: ty's type system will evolve, which may require changing existing types or introducing entirely new types (such as intersections) that necessitate changes to the TSP (and Pylance). Would ty have to maintain backward compatibility to avoid breaking older Pylance extensions (this seems non-trivial to me)? Would Pylance support multiple TSP versions to support both old and new ty verisons?

The TSP has a concept of a 'supported protocol version'. It's the first message that Pyrefly is handling:
https://github.com/facebook/pyrefly/pull/1029

We should be able to use that to determine if the Type server supports the protocol version that the Pylance extension is currently using. If the Type Server doesn't support the same version, Pylance would fall back to use Pyright that ships with it. 


> We're currently focusing on the Beta release of ty, which is why our resources are a bit tight right now, but that shouldn't prevent us from moving this discussion forward. But it may mean that we're slower in responding.

I just wanted to add that my goal is that the Pylance team maintains the TSP interface for Pyrefly and Ty. That nothing we add to Pyrefly or Ty prevents your teams from moving forward on any other work. Hopefully we can make it separate enough that it's not a large burden on you changing anything. (I expect some of the unit tests might break from time to time as the output of types changes).

Thanks so much for considering this and responding.



---

_Comment by @DetachHead on 2025-10-05 08:56_

> What are examples where Pylance can provide a better experience that we can't do in a generic LSP, i.e., that would lead to a better experience when using ty and Pylance together that we wouldn't get when using ty alone?

i think this is an important question that you didn't really answer.

as i said in https://github.com/microsoft/pylance-release/discussions/7180#discussioncomment-13227687, i think the idea of a "type server protocol" just adds a large amount of complexity to language servers for no good reason. what problems does it actually solve, and for who? (the problems that you created for yourselves by keeping pylance closed-source and unusable outside of vscode doesn't count) i mean what problems does this solve for the ty developers or its users?

what's the point of all of this complexity if ty aims to be a fully featured language server (ie. does everything pylance can do)? a ty maintainer can correct me if i'm wrong but i don't think their goal is to create a language server with barely any features and force users to use vscode with pylance for all the missing functionality (like pyright does). so once ty is feature complete, if a user wants to switch to ty in vscode, they would just switch entirely. what's the point in running it alongside pylance?

i don't see any reason to support this protocol.

---

_Comment by @rchiodo on 2025-10-06 16:46_

> what's the point of all of this complexity if ty aims to be a fully featured language server (ie. does everything pylance can do)?

There were really two points:

1) Share implementation. 
2) Ship in the box with VS code.

The idea for 1 was that other type checkers could share the functionality we added on top of Pyright. I agree this really only makes sense for open source type checkers if Pylance was also open source and perhaps split into parts that could be reused. We hope to someday make Pylance open source, but I don't think anybody would want to rely on that. 

For number 2, we didn't want to ship an LSP that had fewer features. We felt that the TSP would make it possible for any type checker to support everything that Pylance does. I'm not sure this is necessary though. If an LSP (that we shipped with VS code) was missing things, users should be able to switch easily. 

There is a 3rd potential reason for these new messages = add something for an LLM. These new messages are an addition to LSP that provide extra type inference. It's to be seen whether or not LLMs need this or not. 


---

_Comment by @DetachHead on 2025-10-06 21:52_

> The idea for 1 was that other type checkers could share the functionality we added on top of Pyright. I agree this really only makes sense for open source type checkers if Pylance was also open source and perhaps split into parts that could be reused

> For number 2, we didn't want to ship an LSP that had fewer features. We felt that the TSP would make it possible for any type checker to support everything that Pylance does. I'm not sure this is necessary though. If an LSP (that we shipped with VS code) was missing things, users should be able to switch easily.

I feel like these motivations are short sighted, even if pylance was open source. I agree that this new protocol would be useful right now, while these new language servers (ty and pyrefly) are incomplete, but once they come closer to feature parity with pylance, there will be less of a need to use pylance alongside them. 

My concern is that the amount of complexity this will add to the language server spec is not worth the short term benefit of a slightly better experience using these language servers while they're still under development (and it also seems very python-specific. Do any other languages need this?). Besides, this is already doable anyway because you can already turn on both language servers and set `ty.disableLanguageServices` to `true` to only use its diagnostics. 

My opinion here comes from my first hand experience developing my own language server towards pylance feature parity. before we had reimplemented most of pylance's functionality, I used both pylance and basedpyright the same way (using `basedpyright.disableLanguageServices`). But nowadays I see no need to use pylance at all, and I think most people using my language server feel the same way

> We hope to someday make Pylance open source, but I don't think anybody would want to rely on that.

I'm glad that's something you guys are considering! I think many users will be happy to hear that too

> There is a 3rd potential reason for these new messages = add something for an LLM. These new messages are an addition to LSP that provide extra type inference. It's to be seen whether or not LLMs need this or not.

I can't really speak on this because I don't really use any LLM integration with my IDE, but I'm curious as to what's stopping LLMs from being able to get type information using the existing language server protocol

---
