## Biomedical question answering robustness benchmark

The repo includes code to assess the robustness of large language models (LLMs) in biomedical question answering (QA). A collection of generalist and specialist LLMs are assessed.

This repo contains only the code for sampling-based attack, for running the benchmarks using the variant of [discreteZOO](https://github.com/StatNLP/discretezoo), the zeroth-order optimization-based attack used in the manuscript, please see [discretezoo-span](https://github.com/RealPolitiX/discretezoo-span).

### Installation

Clone the code repo. Go to the `qstab-main` folder, install using the following from the command line

<pre><code class="console"> pip install . </code></pre>

### Run baseline (no attack)

The baseline assesses the zero-shot performance of LLMs for the specified datasets. Under the `qstab-main` folder, type the following from the command line

Run [Flan-T5-large](https://huggingface.co/google/flan-t5-large) model on [MedQA-USMLE](https://huggingface.co/datasets/GBaker/MedQA-USMLE-4-options-hf-MPNet-IR) drug-mention questions
<pre><code class="console"> python ./scripts/run_hf_entity.py -grp="drugs" -nq=6000 </code></pre>

Run [MedAlpaca-7B](https://huggingface.co/medalpaca/medalpaca-7b) model on MedQA-USMLE disease-mention questions
<pre><code class="console"> python ./scripts/run_medalpaca_entity.py -grp="diseases" -nq=6000 </code></pre>

Here 6000 is just a large enough number such that all instances are run, a smaller number evaluates only a subset of the data.

### Run entity substitution attack

The substitution attack targets entities in the distractors of a question by constructing **adversarial distractors**. For each data instance, the attacker selects a replacement entity from a perturbations set using a sampling-based approach to substitute the original entity in the distractor.

* **Random sampling**

An example to attack the MedAlpaca-7B model on MedQA-USMLE drug-mention questions using random sampling.
<pre><code class="console"> python ./scripts/run_medalpaca_entity.py -ptb=True -ptb_samp="random" -grp="drugs" -nq=6000 </code></pre>

* **Powerscaled distance-weighted sampling (PDWS)**

An example to attack the MedAlpaca-7B model on MedQA-USMLE drug-mention questions using PDWS.
<pre><code class="console"> python ./scripts/run_medalpaca_entity.py -ptb=True -ptb_samp="distance" -grp="drugs" -ndist=20 -nq=6000 </code></pre>

* **Hyperparameter tuning for PDWS**

An example to tune the PDWS attacker's hyperparameter for the MedAlpaca-7B model on MedQA-USMLE drug-mention questions
<pre><code class="console"> bash ./scripts/medalpaca_tune.sh </code></pre>