---
title: "Complete Guide: Setting Up Ollama on Intel GPU with Intel Graphics Package Manager"
date: 2026-02-11
draft: false
categories: ["technical"]
tags: ["ollama", "intel", "gpu", "wsl", "ai"]
description: "A step-by-step guide to setting up Ollama to use an Intel GPU on WSL/Ubuntu, including driver installation and verification."
---

I remember using ChatGPT for the first time to write a reply when I received appreciation from the leadership team for my work in my previous company. Nowadays, it is part of day-to-day life; AI has made my life easier. I was wondering what if we can run LLM locally on my laptop. I installed Ollama desktop for Windows on my Laptop. My laptop with just 16 GB RAM was working fine with small models with basic email writing tasks. Using a model with 1b parameters and my regular apps like Teams, Chrome, etc., my laptop was frequently becoming unresponsive. On my another Laptop with a dedicated graphics card, I was able to run models up to 8b parameters smoothly.

I thought why can’t we use Intel GPU to perform the GPU heavy tasks on my laptop. I started exploring and found a reference to [Intel Ipex-llm](https://github.com/intel/ipex-llm/blob/main/docs/mddocs/Quickstart/ollama_portable_zip_quickstart.md) project on Github. You will get a zip file which you can extract and Ollama locally using Intel GPU. I did this setup on Ubuntu 24.04 running on Windows WSL. Here is step by step process:

1.  **Update GPU driver on machine**

    Follow below steps to install packages from Intel:

    A. Refresh the package index and install package manager

    ```bash
    sudo apt-get update
    sudo apt-get install -y software-properties-common
    ```

    B. Add intel-graphics Personal Package Archive (PPA)

    ```bash
    sudo add-apt-repository -y ppa:kobuk-team/intel-graphics
    ```

    C. Install compute related packages

    ```bash
    sudo apt-get install -y libze-intel-gpu1 libze1 intel-metrics-discovery intel-opencl-icd clinfo intel-gsc
    ```

    D. Install media related packages

    ```bash
    sudo apt-get install -y intel-media-va-driver-non-free libmfx-gen1 libvpl2 libvpl-tools libva-glx2 va-driver-all vainfo
    ```

    E. Verify installation

    ```bash
    clinfo | grep "Device Name"
    ```

    [![result of running command clinfo | grep “Device Name”](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fcdn-images-1.medium.com%2Fmax%2F1024%2F1%2AEdC9FM9mOBbjMNLwzdFkrA.png)](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fcdn-images-1.medium.com%2Fmax%2F1024%2F1%2AEdC9FM9mOBbjMNLwzdFkrA.png)

    If you do not see the result like above, there could be some issue with the user you are using, run below commands to add your user.

    ```bash
    sudo gpasswd -a ${USER} render
    newgrp render
    ```

    Using the above steps, we have installed Intel graphics packages in Ubuntu running in WSL.

2.  Download the file from this [link](https://github.com/ipex-llm/ipex-llm/releases/tag/v2.3.0-nightly).
3.  Extract the file

    ```bash
    tar -xvf [Downloaded tgz file path]
    ```

4.  Go to the extracted folder and run start-ollama.sh

    ```bash
    cd PATH/TO/EXTRACTED/FOLDER
    ./start-ollama.sh
    ```

    [![screenshot of ollama running after running command](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F69ubb8nigwsbl4g8dzh7.png)](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F69ubb8nigwsbl4g8dzh7.png)

5.  Open another terminal and run your model

    ```bash
    cd PATH/TO/EXTRACTED/FOLDER
    ./ollama run llama3.2:1b
    ```

    [![sample run for running ollama from ubuntu terminal](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F88aisuznd3z5awu68dt4.png)](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F88aisuznd3z5awu68dt4.png)

6.  You can verify the GPU usage from task manager.

    [![Screenshot of GPU usage from task manager.](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F3jy0ms08qddbdp1zua85.png)](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F3jy0ms08qddbdp1zua85.png)

**Conclusion**

I was able to run small models like qwen3:1.7b, qwen3:0.6b, llama3.2:1b, and gemma3:1b smoothly. Running deepseek model deepseek-r1:1.5b was giving garbage response. Somehow managed to run gemma3:4b only once after that it was getting failed. What more I can expect on a machine running on 16 GB RAM with an i5 processor. It was good learning, I connected the locally running Ollama with Librechat and played with it.

**References:**

1.  [https://github.com/intel/ipex-llm/blob/main/docs/mddocs/Quickstart/ollama_portable_zip_quickstart.md](https://github.com/intel/ipex-llm/blob/main/docs/mddocs/Quickstart/ollama_portable_zip_quickstart.md)
2.  [https://github.com/ipex-llm/ipex-llm/releases/tag/v2.3.0-nightly](https://github.com/ipex-llm/ipex-llm/releases/tag/v2.3.0-nightly)
3.  [https://dgpu-docs.intel.com/driver/client/overview.html](https://dgpu-docs.intel.com/driver/client/overview.html)
