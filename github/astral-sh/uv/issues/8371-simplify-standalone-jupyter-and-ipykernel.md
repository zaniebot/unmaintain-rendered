---
number: 8371
title: "Simplify standalone Jupyter and ipykernel environments setup: nb_conda_kernels but for uv"
type: issue
state: open
author: rbavery
labels: []
assignees: []
created_at: 2024-10-19T20:23:32Z
updated_at: 2024-10-27T19:44:18Z
url: https://github.com/astral-sh/uv/issues/8371
synced_at: 2026-01-07T13:12:17-06:00
---

# Simplify standalone Jupyter and ipykernel environments setup: nb_conda_kernels but for uv

---

_Issue opened by @rbavery on 2024-10-19 20:23_

Thank you for the detailed docs on using uv with jupyter. I read through them and was wondering if the setup process could be simplified for what I think is the most common use of jupyter as a standalone tool.

https://docs.astral.sh/uv/guides/integration/jupyter/

At least for me most of my use of Jupyter is as a standalone tool in the base environment where I am constantly switching from kernel to kernel depending on the project (which usually won't have a pyproject.toml). I think documenting how to register all uv project environments automatically would be helpful, so that users don't need to manually register kernels with ipykernel for every new environment.

I like the simplicity of conda's approach, where any conda environment with ipykernel installed is registered and made available to the base environment jupyter if the `nb_conda_kernels` package is installed in the base environment. https://github.com/anaconda/nb_conda_kernels

I think conda's approach fits in the category of running jupyter as a standalone tool, where the standalone tool has access to all projects (environments) as kernels. And the user has read/write access to each of these environments. 

Could the docs lead with this case in the instructions? And is it possible to run something like `nb_conda_kernels` in the background whenever `--with jupyter` is run to register all uv environments that have ipykernel installed?

---

_Comment by @bluss on 2024-10-19 20:48_

I have a solution for almost exactly that: Jupyterlab as a standalone tool, and switching between uv-defined kernels environments without having to register them, but that -- currently -- requires `pyproject.toml`. That was chosen as a project marker and dependency definition for my notebook kernels but it could also be done in other ways.

If you allow it, I'll link my project here. It supports multiple project managers but uv is the primary way it's intended to be used.

https://bluss.github.io/pyproject-local-kernel/

I'm not a huge believer in registering environments as kernelspecs, because environments come and go and move around, so the list gets stale if it is not cleaned up. A solution like nb_conda_kernels for uv would be fine, since it is dynamic, but I think the kernel provisioner method that pyproject-local-kernel is using is more modular - plays nice with other installed tools. (You can see that nb_conda_kernel mentions “A new kernel discovery system is being developed” - and this is what became kernel provisioners, long after they wrote that.)



---

_Comment by @bluss on 2024-10-20 21:27_

It looked like it would be simple, so here's a PoC level implementation of "nb_conda_kernels" but for uv. It's based on scanning pyproject.tomls under a designated directory.

https://github.com/bluss/uv-kernels

![bild](https://github.com/user-attachments/assets/b64189e0-df00-4dae-9646-c264c965e5db)


---

_Comment by @bluss on 2024-10-27 19:42_

Feedback on both of those is welcome.

My elevator pitch for why to use the former solution instead of the latter.

- pyproject-local-kernel keeps the configuration in the repository. You can check it out anywhere and if you have the right software installed (which is 'pyproject-local-kernel' as an "addon" to jupyterlab) you can just work on the notebook project.

- nb_conda_kernels or the uv-kernels PoC rely on "kernels installed elsewhere". Notebooks depend on some setup you have done some other place, not next to the notebook. If two people work on the same notebook in a git repo using that method, they need to agree on the name and location of that installed kernel environment.

---
