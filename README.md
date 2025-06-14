# VisCA-Rebuttal

## C1: Threat model realism and motivation for stealthiness (1475A, 1475B)
### 1. Section IV-A: Threat model
* **Adversary Goal**: The adversary aims to manipulate the inputs fed into an LLM (i.e., the victim model such as ChatGPT), in order to induce incorrect or misleading outputs. These perturbed outputs may influence downstream users or decision-making systems. Importantly, the adversary seeks to achieve this goal while preserving the readability and semantic consistency of the original input, so that the perturbations remain imperceptible to human reviewers or automatic detectors. Therefore, instead of overtly altering key concepts (e.g., changing “independent” to “dependent”), the adversary focuses on stealthy perturbations that subtly interfere with the model's behavior while minimizing user awareness.
* **Adversary Knowledge**: We assume a black-box threat model. The adversary has no access to the internal architecture, training data, or parameters of the victim model (e.g., ChatGPT), but can interact with it through input-output queries. This allows the adversary to observe how output responses change in response to crafted inputs.
* **Adversary Capability**: The adversary is assumed to have control over the final form of the user input received by the LLM, for example by intercepting the prompt in transit or injecting imperceptible perturbations at the interface layer (e.g., clipboard, browser extensions, proxy APIs). However, the adversary is constrained in that they cannot drastically change the prompt content; the modified input must remain semantically and syntactically plausible to avoid suspicion. In addition, the adversary has sufficient computational resources to perform various computational tasks, such as representing large amounts of texts as vectors and performing various computational operations.
  
### 2. Section VI-D: Real-world VisCA deployment
* VisCA's low-cost, non-iterative, and visually imperceptible perturbation strategy makes it particularly suitable for real-world threat models, such as Man-In-The-Middle (MITM) attacks or malicious client-side manipulations. In practice, user queries sent to LLMs can pass through multiple components-including web browsers, third-party applications, proxy APIs, or cloud services—before reaching the model. These intermediary components may be compromised or malicious, enabling attackers to subtly inject perturbations into user inputs without altering their appearance to the user. Such attacks are especially viable in scenarios involving AI-assisted healthcare, legal, or financial services, where inputs are often pre-processed or forwarded by upstream systems before reaching the LLM. In these cases, the attacker may not wish to dramatically alter the input (which could trigger user suspicion or validation mechanisms), but instead employ stealthy, character-level perturbations that influence the LLM's output in a targeted way. Unlike traditional white-box attacks or semantically disruptive manipulations, VisCA enables lightweight and stealthy perturbations that work in real-time and preserve the original input's surface form. This makes it a realistic and dangerous attack vector in deployed black-box systems, emphasizing the urgent need for robust defenses against such invisible manipulations.

## C2: Choice of GPT-2 (1475A, 1475B)
### 1. Prior work
[29] J. Shi, Y. Liu, P. Zhou, and L. Sun, “Poster: Badgpt: Exploring security vulnerabilities of chatgpt via backdoor attacks to instructgpt,” in 30th Annual Network and Distributed System Security Symposium, NDSS 2023, 2023.

### 2. Comparison of GPT-2 against LLaMA-2, Gemma-2, and E5-base in VisCA’s candidate selection
<img src="./img/embedding_choice.png" width="100%">

## C3: ASR interpretation of Table V and Table XV (1475A, 1475B)
<img src="./img/TableV.png" width="80%">
<img src="./img/TableXV.png" width="80%">


## 1475A_Response
### 1. Spelling Correction Resilience
VisCA’s Eye-only strategy transposes adjacent letters to mimic natural typing or visual perception errors. Most spell checkers avoid correcting such patterns, especially when the result still forms a plausible word (e.g., “lvoe” instead of “love”). In some cases, the transposition even results in another valid word with a different meaning (e.g., “form” $\rightarrow$ “from”, “calm” $\rightarrow$ “clam”, “stop” $\rightarrow$ “spot”), which are syntactically correct and unlikely to be corrected. These subtle yet valid changes reduce the chance of detection and can lead LLMs to incorrect interpretations. In contrast, EvilText uses homoglyph substitutions that retain word structure but bypass Unicode-based detection, while Bad Characters injects invisible symbols, which are ignored by spell checkers. Thus, VisCA, like these baselines, remains robust against standard spelling correction, while uniquely leveraging human-like errors to mislead LLMs.


## 1475B_Response
### 1. Robustness under Task-agnostic Word Selection
To evaluate the robustness and adaptability of VisCA in scenarios where task type is unknown or mixed, we conducted an additional experiment using a task-agnostic word selection strategy. Instead of relying on task-specific heuristics (e.g., using sentiment words for SST-2), we employed KeyBERT, an unsupervised keyword extractor, to identify salient words in each input without referring to task labels. We constructed a hybrid evaluation set by randomly sampling 50 instances from each of the six tasks used in our main experiments, resulting in a total of 300 diverse examples. For each input, we extracted the top-3 keywords using KeyBERT and apply character transposition and substitution on them. All other attack configurations (e.g., perturbation rules, query protocols, and target model) remain remain identical to those in the task-aware setup. Table XIII presents the ASR of VisCA under both strategies. The task-agnostic variant achieves an ASR of 19.00%, with a relative drop of 11.67 percentage points compared to the task-aware version. Despite this reduction, the results demonstrate that VisCA retains strong effectiveness even without access to task-specific information, and remains robust under general-purpose word selection strategies.

<img src="./img/Task-agnostic.png" width="50%">

### 2. Each character’s Unicode code point
<img src="./img/friend_table.png" width="50%">















