---
number: 16880
title: "[Bug] Gradio package installed via uv reports correct version (6.0.1) but shows runtime behavior of older version"
type: issue
state: closed
author: coffeedrunkpanda
labels:
  - question
assignees: []
created_at: 2025-11-28T13:59:50Z
updated_at: 2025-11-28T15:00:12Z
url: https://github.com/astral-sh/uv/issues/16880
synced_at: 2026-01-07T13:12:19-06:00
---

# [Bug] Gradio package installed via uv reports correct version (6.0.1) but shows runtime behavior of older version

---

_Issue opened by @coffeedrunkpanda on 2025-11-28 13:59_

### Summary

### **Issue**

 Uv reports that the gradio 6.0.1 was installed (the correct target version), but the behaviour
is of older gradio versions. There were breakable changes in [https://www.gradio.app/main/guides/gradio-6-migration-guide](gradio 6), where the parameter "type" is now needed for the `gradio.Chatbot` initialisation. 
The error encountered reports **unexpected keyword argument 'type'**, thus the package installed must be
an older version. I confirmed using google colab that the code is correct for gradio 6 and should accept the 
new parameter. 

### **Environment Information**

The behaviour occurred with two different environments. Installing gradio 6.0.1 (target version) through:  
* Docker: ghcr.io/astral-sh/uv:python3.12-bookworm-slim
* Local: uv 0.9.13 (Homebrew 2025-11-26)

Additional: 
Operating system: MacOS

### **Error Traceback:** 

Traceback (most recent call last):
  File "/app/app.py", line 158, in <module>
    main()
  File "/app/app.py", line 144, in main
    chatbot = gr.Chatbot([], type="messages" , elem_id="chatbot", label="Conversation")
              ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/app/.venv/lib/python3.12/site-packages/gradio/component_meta.py", line 194, in wrapper
    return fn(self, **kwargs)
           ^^^^^^^^^^^^^^^^^^
TypeError: Chatbot.__init__() got an unexpected keyword argument 'type'




### Platform

macOS (Darwin 24.6.0 arm64)

### Version

uv 0.9.13 (Homebrew 2025-11-26), and Docker Image ghcr.io/astral-sh/uv:python3.12-bookworm-slim

### Python version

Python 3.13.5

---

_Label `bug` added by @coffeedrunkpanda on 2025-11-28 13:59_

---

_Comment by @konstin on 2025-11-28 14:16_

Can you provide a reproducible example (https://github.com/astral-sh/uv/issues/9452) that shows how you install gradio and the verbose (`-v`) logs from the installation?

---

_Label `needs-mre` added by @konstin on 2025-11-28 14:17_

---

_Label `bug` removed by @charliermarsh on 2025-11-28 14:28_

---

_Comment by @coffeedrunkpanda on 2025-11-28 14:40_

Hi konstin, I added a reproducible example with the docker container in my repository: https://github.com/coffeedrunkpanda/tandem-buddy/

The commands I used to build and run the docker container are in the README: https://github.com/coffeedrunkpanda/tandem-buddy/blob/main/README.md

I will try to get more information with the verbose command now. 


---

_Comment by @konstin on 2025-11-28 14:57_

gradio self-reports as 6.0.1, so I don't think that's the problem here:

```
$ docker run --rm -it tandem-buddy-app python -c "import gradio; print(gradio.__version__)"
6.0.1
```

---

_Comment by @coffeedrunkpanda on 2025-11-28 14:59_

Hey konstin, I am now thinking it is an issue with gradio itself, not uv. We can close the issue. 

I am testing in colab with different versions, the 5.50.0 works for my repository. The 6.0.1 one does not. 


---

_Closed by @konstin on 2025-11-28 15:00_

---

_Label `needs-mre` removed by @konstin on 2025-11-28 15:00_

---

_Label `question` added by @konstin on 2025-11-28 15:00_

---
