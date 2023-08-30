# DEP
[![DOI](https://zenodo.org/badge/626436024.svg)](https://zenodo.org/badge/latestdoi/626436024)

This repo contains data of DEP: Approximating Online Human Evaluation of Social Chatbots with Prompting.

## Introduction
As conversational models become increasingly available to the general public, users are engaging with this technology in social interactions. Such unprecedented interaction experiences may pose considerable social and psychological risks to the users unless the technology is properly controlled. This highlights the need for scalable and robust evaluation metrics for conversational chatbots. Existing evaluation metrics aim to automate offline user evaluation and approximate human judgment of pre-curated dialogs. However, they are limited in their ability to capture subjective perceptions of users who actually interact with the bots and might not generalize to real-world settings. To address this limitation, we propose an approach to approximate online human evaluation leveraging large language models (LLMs) from the GPT family. We introduce a new Dialog system Evaluation framework based on Prompting (DEP), which enables a fully automatic evaluation pipeline that replicates live user studies and achieves an impressive correlation with human judgment (up to Pearson r=0.95 on a system level). The DEP approach involves collecting synthetic chat logs of evaluated bots with an LLM in the other-play setting, where the LLM is carefully conditioned to follow a specific scenario. We further explore different prompting approaches to produce evaluation scores with the same LLM. The best-performing prompts, which contain few-shot demonstrations and instructions, show outstanding performance on the tested dataset and demonstrate the ability to generalize to other dialog corpora.

## Datasets
This repository contains three datasets used in our paper with all additional annotations. The data format for each of the datasets, iEval [1], FED [2], and DSTC9 [3], is described below in the respective order.

### iEval
iEval dataset with additional data and annotations is available in file [`ieval_dep_steps12_eval.json`](ieval_dep_steps12_eval.json).

Additional data include freshly generated dialog logs between the chatbots and an LLM (`chat_log_pr1`)
as well as ratings for these dialogs produced by the same LLM (`Rating_pr`). Annotations provide
emotion/intent labels for each dilog turn that were generated using emotion/intent classifier from [4]
(`labels_b_b` and `labels_h_b`). The file contains a list of objects, with each object corresponsing to
a single experimental task of the following structure:

- `part` (int): numerical id of the experimantal task
- `elapsed_sec` (int): overall number of seconds elapsed per given task for human worker (not used in our work)
- `elapsed_min` (float): overall number of minutes elapsed per given task for human worker (not used in our work)
- `TEQ_raw` (list(int)): worker's responses to the questions of the Toronto Empathy Questionnaire (not used in our work)
- `TEQ_agg` (int): worker's total score for Toronto Empathy Questionnaire (not used in our work)
- `<VALENCE>` (obj): emotional polarity of the grounding scenario; taken values: `positive`, `negative`
  - `conv_id` (str): id of a conversation from the EmpatheticDialogues dataset [5] that was used for the grounding scenario
  - `emotion` (str): emotion label from the EmpatheticDialogues dataset [5] that was used for the grounding scenario
  - `prompt` (str): situation prompt from the EmpatheticDialogues dataset [5] that was used for the grounding scenario
  - `<COLOR>` (obj): color code of a chatbot; taken values: `Pink`, `Purple`, `Yellow`, `Green`
    - `chat_log` (str): chat log between the worker and the chatbot with the assigned \<COLOR\> for the given emotional \<VALENCE\>
    - `P` (int): worker's Likert-type score assessing the statement: "The chatbot was polite throughout our conversation."
    - `^P` (int): worker's Likert-type score assessing the statement: "Some of the chatbot's responses were insulting."
    - `A` (int): worker's Likert-type score assessing the statement: "The chatbot was paying attention to what I was saying."
    - `^A` (int): worker's Likert-type score assessing the statement: "The chatbot was inattentive to my messages."
    - `EU` (int): worker's Likert-type score assessing the statement: "The chatbot understood my emotions."
    - `ER` (int): worker's Likert-type score assessing the statement: "The chatbot responded to my emotions appropriately."
    - `L` (int): worker's Likert-type score assessing the statement: "The chatbot is likable."
    - `R` (int): worker's Likert-type score assessing the statement: "The chatbot was repetitive."
    - `S` (int): worker's Likert-type score assessing the statement: "The chabot's responses made sense given my inputs."
    - `Rating` (int): overall rating of the chatbot (inverse of the rank, i.e., the higher, the better)
    - `chat_log_pr1` (str): prompted chat log between the LLM and the chatbot with the assigned \<COLOR\> for the given emotional \<VALENCE\>
    - `Rating_pr` (str): overall rating produced by promting the LLM to rate the prompted chat log
    - `labels_b_b` (list): emotion/intent labels for turns in LLM-to-bot dialog (chat_log_pr1)
    - `labels_h_b` (list): emotion/intent labels for turns in human-to-bot dialog (chat_log)
   
### FED
FED dataset with additional annotations is available in file [`fed_dep_step2_eval.csv`](fed_dep_step2_eval.csv).

Annotations include ratings for dialogs produced by prompting an LLM (`openai_response`).
The file contains a table with the following columns:

- `context` (str): chat log between the human and the system
- `response` (str): n/a
- `system` (str): system participating in the chat; taken values: `Meena`, `Mitsuku`, `Human`
- `annotations` (dict): fine-grained dialog annotations provided by human workers
- `number_of_turns` (int): number of turn in the dialog
- `diag_raw_length` (int): number of works in the dialog
- `overall_avg` (float): overall average rating of the dialog based on human annotations
- `expected_overall_rating` (str): overall average rating of the dialog based on human annotations mapped to textual format; taken values: `Very bad`, `Bad`, `Okay`, `Good`, `Very good`
- `openai_response` (str): overall rating produced by promting the LLM to rate the dialog in textual format

### DSTC9
DSTC9 dataset with additional annotations is available in file [`dstc9_dep_step2_eval.csv`](dstc9_dep_step2_eval.csv).

Annotations include ratings for dialogs produced by prompting an LLM.
The file contains a table with the following columns:

- `context` (str): chat log between the human and the system
- `human (overall)` (float): overall average rating of the dialog based on human annotations
- `consistent` (float): average score of `consistent` dimension of the dialog based on human annotations
- `likeable` (float): average score of `likeable` dimension of the dialog based on human annotations
- `diverse` (float): average score of `diverse` dimension of the dialog based on human annotations
- `informative` (float): average score of `informative` dimension of the dialog based on human annotations
- `coherent` (float): average score of `coherent` dimension of the dialog based on human annotations
- `understanding` (float): average score of `understanding` dimension of the dialog based on human annotations
- `flexible` (float): average score of `flexible` dimension of the dialog based on human annotations
- `topic depth` (float): average score of `topic depth` dimension of the dialog based on human annotations
- `error recovery` (float): average score of `error recovery` dimension of the dialog based on human annotations
- `inquisitive` (float): average score of `inquisitive` dimension of the dialog based on human annotations
- `model` (str): id of chatbot participating in the chat; taken values from `chatbot1.json` to `chatbot11.json`
- `dialogue_number_of_words` (int): number of words in the dialog
- `dialogue_number_of_tokens` (int): number of tokens in the dialog
- `openai_response` (str): overall rating produced by promting the LLM to rate the dialog in textual format
- `openai_score` (float): overall rating produced by promting the LLM to rate the dialog in numerical format on 0-4 scale

[1] E. Svikhnushina, A. Filippova, and P. Pu. 2022. iEval: Interactive Evaluation Framework for Open-Domain Empathetic Chatbots. [Paper](https://aclanthology.org/2022.sigdial-1.41/), [github](https://github.com/Sea94/ieval)

[2] S. Mehri and M. Eskenazi. 2020. Unsupervised Evaluation of Interactive Dialog with DialoGPT. [Paper](https://aclanthology.org/2020.sigdial-1.28/), [github](https://github.com/shikib/fed)

[3] C. Gunasekara et al. 2020. Overview of the Ninth Dialog System Technology Challenge: DSTC9. [Paper](https://arxiv.org/abs/2011.06486), data accessible [here](https://github.com/exe1023/DialEvalMetrics/tree/main#dstc9-data) and [here](https://github.com/ictnlp/DialoFlow/tree/main/FlowScore/data).

[4] A. Welivita and P. Pu. 2020. A Taxonomy of Empathetic Response Intents in Human Social Conversations. [Paper](https://aclanthology.org/2020.coling-main.429/), [github](https://github.com/anuradha1992/EmpatheticIntents)

[5] H. Rashkin, E. M. Smith, M. Li, and Y. Boureau. 2019. Towards Empathetic Open-domain Conversation Models: a New Benchmark and Dataset. [Paper](https://aclanthology.org/P19-1534/), [github](https://github.com/facebookresearch/EmpatheticDialogues)

## References

Please cite [*] if you found the resources in this repository useful.

[*] E. Svikhnushina, P. Pu. *Approximating Online Human Evaluation of Social Chatbots with Prompting*

```
@inproceedings{Svikhnushina2023Approximating,
  title = {Approximating Online Human Evaluation of Social Chatbots with Prompting},
  author = {Ekaterina Svikhnushina and Pearl Pu},
  booktitle = {SIGDIAL},
  year = {2023},
}
```
## License
See the [LICENSE](LICENSE) file in the root repo folder for more details.
