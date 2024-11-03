# AmpleGCG: Learning a Universal and Transferable Generator of Adversarial Attacks on Both Open and Closed LLM
This is the official repo of AmpleGCG ([https://arxiv.org/abs/2404.07921](https://arxiv.org/abs/2404.07921)). Please kindly 🌟star🌟 it and cite our paper 📜 if you find them useful.

<a href="https://github.com/OSU-NLP-Group/AmpleGCG?tab=readme-ov-file" target="_blank">
  <img src="https://img.shields.io/badge/AmpleGCG-black?style=flat&logo=python&logoColor=rgb" alt="AmpleGCG Badge">
</a>


## 🚨Updates🚨
- Nov 2th: Our technical paper of [AmpleGCG-plus](https://arxiv.org/abs/2410.22143) has officially arXived! Check it out!
- August 27th: 2024: Release of our extensive collection of **millions** of suffixes generated through GCG, along with their corresponding evaluation results.

  In light of the importance of building trustworthy AI systems that should be robust in both **natural** and **gibberish** language spaces, we have decided to release the raw datasets that are used to develop AmpleGCG and AmpleGCG-plus series of models to better contribute to the community. For more reasons why we believe these gibberish suffixes are important, please check the [Tweet Thread](https://x.com/LiaoZeyi/status/1828613837756490112) here. Please apply for it at [here](https://huggingface.co/osunlp/AmpleGCG-llama2-sourced-llama2-7b-chat#request-for-datasets).

- August 1st, 2024: Release of **AmpleGCG-plus**

  We are excited to announce the release of **AmpleGCG-plus**, an enhanced version of AmpleGCG designed to produce customized GCG suffixes. This upgrade introduces two significant improvements:

  1. **Enhanced Data Quality**: We've utilized a more effective and cost-efficient evaluator, harmbench-cls, in our OTF pipeline to collect higher-quality training datasets.

  2. **Enhanced Data Quantity**: Instead of sampling 200 suffixes for each query, **AmpleGCG-plus** now utilizes all available collected training pairs.

  Given that, we've developed two specialized versions of **AmpleGCG-plus**, tailored for Llama-2-chat and GPT-series models with more details in 🤗 [**AmpleGCG-plus**](https://huggingface.co/osunlp/AmpleGCG-plus-llama2-sourced-llama2-7b-chat).

  Both AmpleGCG-**plus** variants demonstrate superior performance compared to the original AmpleGCG when evaluated on AdvBench.

- July 20th: Acceptance to COLM

  We are thrilled to anounce that our [paper](https://arxiv.org/abs/2404.07921) is accepted at [COLM 2024](https://colmweb.org/)

---

## Reproducibility and Codes
This repository hosts the source code of **Augmented GCG**, which extends the capabilities of GCG by overgenerating samples alongside the GCG optimizations. Our work builds upon the foundational [GCG](https://github.com/llm-attacks/llm-attacks) work, and we express our deep appreciation for their open-source release.


Due to safety and ethical considerations, we have decided not to publicly release the trained **AmpleGCG**, our adversarial suffix generator in the wild. There exists a significant risk that, if used maliciously, AmpleGCG could rapidly compromise the safety of both open-source and proprietary models. Such a scenario could lead to widespread dissemination of harmful content, a risk we aim to mitigate by restricting access to the trained model.

 However, one can apply for our trained **AmpleGCG** via 🤗 [AmpleGCG-series models](https://huggingface.co/osunlp/AmpleGCG-llama2-sourced-llama2-7b-chat) and generated adversarial suffixes  via this [Google Form](https://docs.google.com/forms/d/1P8hxsR5_ROE1-J1pyKCqT1GBuIa0RqkwRc3opCAvQ0Y/edit) for research purposes only. Once approval, we will release the suffixes generated by AmpleGCG on [AdvBench](https://arxiv.org/abs/2307.15043) and [MaliciousIntruct](https://arxiv.org/abs/2310.06987). Access to the model and data is granted on a provisional basis and is subject to the sole discretion of the authors.



## Licensing Information
The code under this repo is licensed under an [OPEN RAIL-S License](https://www.licenses.ai/ai-pubs-open-rails-vz1).

The data under this repo is licensed under an [OPEN RAIL-D License](https://huggingface.co/blog/open_rail).

The model weight and parameters under this repo are licensed under an [OPEN RAIL-M License](https://www.licenses.ai/ai-pubs-open-railm-vz1).

## Introduction
**TL;DR** We further amplify the effectiveness of GCG, achieving increased ASR, more comprehensive identification of vulnerabilities, and improved efficiency across both open-source and closed-source models.

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

Augmented GCG simply extends GCG by overgenerating the suffix candidates during the optimizations.
To obtain the suffixes with augmented GCG under either individual query or multi queries settings, please first:

```bash
cd llmattack/experiments/launch_scripts
```

We provide the scripts for four settings of augmented GCG.
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
> [!NOTE]
> Notice that for multiple queries settings, we only save the suffixes with the lowest loss at each step, which is different from the individual query setting of saving all available sampled candidates at each step.

For individual query and multiple queries settings, we save the potential suffixes with the key `step_cands` and `controls` respectively. Specifically, the suffixes within `controls` are the instances optimized over all training queries. For the suffixes under individual setting, we save them as the format
```
query:
    ...,

    step_N-1:[
        control: <suffix>,
        loss: <loss>
    ],

    step_N:[
        control: <suffix>,
        loss: <loss>
    ],

    ...
```

For more details on optimizing over different models and setups, please refer to the [GCG repo](https://github.com/llm-attacks/llm-attacks/tree/main)

### Evaluation
We provide a modularized and flexible pipeline to evaluate the different victim models.

Take the multiple queries settings for an example.

If you have gotten the results from the augmented GCG above, you need to first deduplicate the generated suffixes and place them under the `myconfig/prompt_own_list.json` with the key (e.g. **llama2_lowest** or **llama2_lowest_at_each_step** corresponding to default GCG (only the suffixes with lowest loss) and Overgenerate + X under multiple queries setting in the paper tables accordingly). Subsequently, you should replace the variable **augmented_GCG** in `evaluate_augmentedGCG.sh` with your defined keys and run
```bash
cd <project_workspace>
bash evaluate_augmentedGCG.sh
```

You can easily swap to other victim models and the generation configs of victim models under `myconfig/target_lm` by utilizing [hydra](https://hydra.cc/docs/intro/).


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
Due to considered ethical issues, we don't publicly release the models in the wild. However, researchers could access to three different versions of AmpleGCG via 🤗 [AmpleGCG-series models](https://huggingface.co/osunlp/AmpleGCG-llama2-sourced-llama2-7b-chat) or train your own AmpleGCG-like adversarial suffixes generator based on the data collected from [individual query settings](#individual-query). For more details of training, please refer to the [paper](arxivlink) about the *overgenerate-then-filter* pipeline for collecting training data of either individual model or multiple models and the figure below.

![figure below](pipeline.png "overgenerate-then-filter")


You could evaluate your trained generator in `evaluate_augmentedGCG.sh` as well once you obtain your own generator. You could further explore different settings of generation config for your generator in `myconfig/generation_configs` as we exemplified that different decoding approaches would affect the diversity and quality of the suffixes


### Citation
```bash
@article{liao2024amplegcg,
  title={AmpleGCG: Learning a Universal and Transferable Generative Model of Adversarial Suffixes for Jailbreaking Both Open and Closed LLMs},
  author={Liao, Zeyi and Sun, Huan},
  journal={arXiv preprint arXiv:2404.07921},
  year={2024}
}

@article{kumar2024amplegcg,
  title={AmpleGCG-Plus: A Strong Generative Model of Adversarial Suffixes to Jailbreak LLMs with Higher Success Rates in Fewer Attempts},
  author={Kumar, Vishal and Liao, Zeyi and Jones, Jaylen and Sun, Huan},
  journal={arXiv preprint arXiv:2410.22143},
  year={2024}
}
```
