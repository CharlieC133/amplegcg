# AmpleGCG: Learning a Universal and Transferable Generator of Adversarial Attacks on Both Open and Closed LLM

## Reproducibility and Codes
This repository hosts the source code for the **Augmented GCG**, which extends the capabilities of GCG by overgenerating samples alongside the GCG optimizations. Our work builds upon the foundational [GCG](https://github.com/llm-attacks/llm-attacks) work, and we express our deep appreciation for their open-source release.


Due to safety and ethical considerations, we have decided not to publicly release the trained **AmpleGCG**, our adversarial suffix generator. There exists a significant risk that, if used maliciously, AmpleGCG could rapidly compromise the integrity of both open-source and proprietary models. Such a scenario could lead to widespread dissemination of harmful effects, a risk we aim to mitigate by restricting access to the trained model.

We believe in responsible disclosure and the ethical use of AI technologies.  However, for research studies, applications could be submitted via this [Google Form](https://docs.google.com/forms/d/1P8hxsR5_ROE1-J1pyKCqT1GBuIa0RqkwRc3opCAvQ0Y/edit). Once approval, we will research the suffixes generated by the AmpleGCG on [AdvBench](https://arxiv.org/abs/2307.15043) and [MaliciousIntruct](https://arxiv.org/abs/2310.06987). If researchers want to apply AmpleGCG on the new Benchmark, please directly contact us and we will help to generate customized adversarail suffixes accordingly.
Access to the Software is granted on a provisional basis and is subject to the sole discretion of the authors. The authors reserve the right to deny or restrict access to the Software to any individual or entity at any time, without notice and without liability.

## Introduction
**TL;DR** We further amplify the effectiveness of GCG on higher ASR, broader coverage of vulnerabilities and higher efficiency.

As large language models (LLMs) become increasingly prevalent and integrated into autonomous systems, ensuring their safety is imperative.
Despite significant strides toward safety alignment, recent work GCG (Zou
et al., 2023) successfully produces a single suffix for each query to jailbreak
LLMs. In this work, we first identify the overlooked opportunities by
solely picking the suffix with the lowest loss during GCG optimization, and consequently, uncover many other missed successful suffixes
in the middle steps. Moreover, we utilize them as training data to learn
a generator named AmpleGCG, which captures the distribution of adversarial suffixes given a harmful query. This generator facilitates the rapid
generation of hundreds of suffixes for any harmful query in minutes. AmpleGCG achieves near 100% attack success rate (ASR) on two aligned LLMs
(Llama-2-7B-Chat and Vicuna-7B), surpassing two strongest existing attack
baselines. Interestingly, AmpleGCG also transfers effectively to attack different models, including closed-source LLMs, achieving a 99% ASR on the
latest GPT-3.5. To summarize, our work amplifies the impact of GCG by
training a generator of adversarial suffixes that is universal to any harmful
query and is transferable from attacking open-source LLMs to closed-source
LLMs. It can generate many adversarial suffixes for one harmful query
within minutes (e.g., 200 suffixes in 6 mins with an ASR of 99% when
attacking Llama-2-7B-Chat), rendering it more challenging to defend


## Setup

```bash
conda create --name AmpleGCG python=3.11.4

conda activate AmpleGCG

pip install -r requirements.txt
```

## Experiments

### Augmented GCG

Augmented GCG simply extends GCG by overgenerating the suffixes candidates during the optimizations.
To obtain the suffixes with augmented GCG under either individual query or multi queries settings, please first:

```bash
cd llmattack/experiments/launch_scripts
```

We provide four settings for augmented GCG.
<a name="individual-query"></a>
1. Individual Query

    1.1 Individual Model

    ```bash
    bash run_overgenerate_indiv_query_indiv_model_llama2-chat.sh
    ```

    1.2 Multiple Models

    ```bash
    bash run_overgenerate_indiv_query_multi_models_llama2-chat_vicuna.sh
    ```

2. Multiple Queries

    2.1 Individual Model

    ```bash
    bash run_overgenerate_mutli_queries_indiv_model.sh
    ```

    2.2 Multiple Models

    ```bash
    bash run_overgenerate_mutli_queries_multi_models_vicuna7_13b_guanaco_7_13b.sh
    ```

> Notice that for multiple queries settings, we only save the suffixes with the lowest loss at each step, which is different from the individual query setting of saving all available sampled candidates at each step.

For more details on optimizing over different models and setups, please refer to the [GCG repo](https://github.com/llm-attacks/llm-attacks/tree/main)

### Evaluation
We provide a modularized and flexible pipeline to evaluate the different victim models.

Take the multiple queries settings for an example.

If you have gotten the results from the augmented GCG above, you need to first depopulate the generated suffixes and place them under the `myconfig/prompt_own_list.json` with the key (e.g. **llama2_lowest** or **llama2_lowest_at_each_step** corresponding to default GCG (only the suffixes with lowest loss) and Overgenerate + X under multiple queries setting). Subsequently, you should replace the variable **augmented_GCG** in `evaluate_augmentedGCG.sh` with your defined keys and run
```bash
cd ../../..
bash evaluate_augmentedGCG.sh
```

After obtaining the content from victim models, you could detect the harmfulness of them by running:

```bash
bash add_reward.sh sequence
```
which would utilize [Beavor-Cost](https://huggingface.co/PKU-Alignment/beaver-7b-v1.0-cost) to label the instances first and sequentially leverage [HarmBench Classifier](https://huggingface.co/cais/HarmBench-Llama-2-13b-cls) to only evaluate the instances that are deemed harmful by Beaver-Cost.

You could use a more advanced GPT4 evaluator by
```bash
bash add_reward.sh gpt4
```


### AmpleGCG
Due to considered ethical issues, we don't directly release the models themselves. However, researchers could train AmpleGCG-like adversarial suffixes generator based on the data collected from [individual query settings](#individual-query). For more details, please refer to the [paper](arxivlink) about the *overgenerate-then-filter* pipeline for collecting training data of either individual model or multiple models and the figure below.

![figure below](pipeline.png "overgenerate-then-filter")


We could evaluate your trained generator in `evaluate_augmentedGCG.sh` as well once you obtain your own generator. You could further explore different settings of generation config for your generator in `myconfig/generation_configs` as we exemplified that different decoding approaches would affect the diversity and quality of the suffixes


### Citation

### License