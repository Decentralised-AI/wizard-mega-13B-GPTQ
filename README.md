---
datasets:
- anon8231489123/ShareGPT_Vicuna_unfiltered
- ehartford/wizard_vicuna_70k_unfiltered
- ehartford/WizardLM_alpaca_evol_instruct_70k_unfiltered
language:
- en
library_name: transformers
pipeline_tag: text-generation
license: other
---

# Wizard Mega 13B GPTQ

This repo contains 4bit GPTQ format quantised models of [OpenAccess AI Collective's Wizard Mega 13B](https://huggingface.co/openaccess-ai-collective/wizard-mega-13b).

It is the result of quantising to 4bit using [GPTQ-for-LLaMa](https://github.com/qwopqwop200/GPTQ-for-LLaMa).

## Repositories available

* [4-bit GPTQ models for GPU inference](https://huggingface.co/TheBloke/wizard-mega-13B-GPTQ).
* [4-bit, 5-bit 8-bit GGML models for llama.cpp CPU (+CUDA) inference](https://huggingface.co/TheBloke/wizard-mega-13B-GGML).
* [OpenAccess AI Collective's original float16 HF format repo for GPU inference and further conversions](https://huggingface.co/openaccess-ai-collective/wizard-mega-13b).

## How to easily download and use this model in text-generation-webui

Open the text-generation-webui UI as normal.

1. Click the **Model tab**.
2. Under **Download custom model or LoRA**, enter `TheBloke/wizard-mega-13B-GPTQ`.
3. Click **Download**.
4. Wait until it says it's finished downloading.
5. Click the **Refresh** icon next to **Model** in the top left.
6. In the **Model drop-down**: choose the model you just downloaded, `wizard-mega-13B-GPTQ`.
7. If you see an error in the bottom right, ignore it - it's temporary.
8. Fill out the `GPTQ parameters` on the right: `Bits = 4`, `Groupsize = 128`, `model_type = Llama`
9. Click **Save settings for this model** in the top right.
10. Click **Reload the Model** in the top right.
11. Once it says it's loaded, click the **Text Generation tab** and enter a prompt!

## Provided files

**`wizard-mega-13B-GPTQ-4bit-128g.no-act-order.safetensors`**

This will work with all versions of GPTQ-for-LLaMa. It has maximum compatibility.

It was created without `--act-order` to ensure compatibility with all UIs out there.

* `wizard-mega-13B-GPTQ-4bit-128g.safetensors`
  * Works with all versions of GPTQ-for-LLaMa code, both Triton and CUDA branches
  * Works with text-generation-webui one-click-installers
  * Parameters: Groupsize = 128. No act-order.
  * Command used to create the GPTQ:
    ```
    python llama.py /workspace/models/openaccess-ai-collective_wizard-mega-13b c4 --wbits 4 --true-sequential --groupsize 128  --save_safetensors /workspace/wizard-mega-13b/gptq/wizard-mega-13B-GPTQ-4bit-128g.no-act-order.safetensors
    ```


# Original Wizard Mega 13B model card

Wizard Mega is a Llama 13B model fine-tuned on the [ShareGPT](https://huggingface.co/datasets/anon8231489123/ShareGPT_Vicuna_unfiltered), [WizardLM](https://huggingface.co/datasets/ehartford/WizardLM_alpaca_evol_instruct_70k_unfiltered), and [Wizard-Vicuna](https://huggingface.co/datasets/ehartford/wizard_vicuna_70k_unfiltered) datasets. These particular datasets have all been filtered to remove responses where the model responds with "As an AI language model...", etc or when the model refuses to respond.

## Release (Epoch Two)

The Wizard Mega 13B SFT model is being released after two epochs as the eval loss increased during the 3rd (final planned epoch). Because of this, we have preliminarily decided to use the epoch 2 checkpoint as the final release candidate. https://wandb.ai/wing-lian/vicuna-13b/runs/5uebgm49

## Build

Wizard Mega was built with [Axolotl](https://github.com/OpenAccess-AI-Collective/axolotl) on 8xA100 80GB for 15 hours. The configuration to duplicate this build is provided in this repo's [/config folder](https://huggingface.co/openaccess-ai-collective/wizard-mega-13b/tree/main/configs).

## Examples

````
### Instruction: write Python code that returns the first n numbers of the Fibonacci sequence using memoization.

### Assistant: 
```python
def fib(n):
    if n < 2:
        return n
    elif n in cache:
        return cache[n]
    else:
        a, b = 0, 1
        for i in range(2, n+1):
            cache[i] = a
            a, b = b, a + b
        return cache[n]
    
def first_n(n):
    fib_list = [fib(i) for i in range(n+1)]
    return fib_list[:n]
```
This function uses memoization to store the values of previous Fibonacci numbers in a cache. This way, if the same number is requested again, it can be returned immediately without recalculating it. 
The `first_n` function takes an integer `n` as input, and calculates the first n numbers of the Fibonacci sequence using memoization. It returns a list of those numbers.
````

```
### Instruction: Finish the joke, a mechanic and a car salesman walk into a bar...  

### Assistant:  ...and the bartender says "Hey guys, what can I get for you?" The mechanic replies, "I'll have a beer, but make it a quick one. I have to fix this guy's car before he finds out I
 fiddled with his brakes." The salesman quips, "And I'll have a martini, shaken not stirred. After all, I have to sell this guy a car that doesn't break down on him within the first year of ownership."
```


